# Advanced creation

> 通过创建特殊`got`实例,可以更轻松地调用REST API。

#### got.create(settings)

例:[gh-got](https://github.com/sindresorhus/gh-got/blob/master/index.js)

提供设置，配置一个新的`got`实例。您可以使用实例上的`.defaults`属性，访问已解决的选项.

**注意:** 与`got.extend()`相反,此方法没有默认值.

##### [options](readme.md#options)

要从父级继承,请将其设置为`got.defaults.options`或使用[`got.mergeOptions(defaults.options, options)`](readme.md#gotmergeoptionsparentoptions-newoptions).<br>
**注意** : 避免使用[对象传播-...语法糖](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#Spread_in_object_literals),因为它不能递归工作.

##### mutableDefaults

类型: | `boolean`
---|---
默认: | `false`

默认值是对应可变的状态。当你需要时它非常有用[在生命周期，更新 headers](readme.md#hooksafterresponse).

##### handler

类型: | `Function`
---|---
默认: | `undefined`

对请求进行其他更改的函数.

要从父级继承,请将其设置为`got.defaults.handler`.<br>要使用默认处理程序,只需省略.

###### [options](readme.md#options)

**注意:** 这些是[规范的-normalized](source/normalize-arguments.js)选项.

###### next()

返回一个`Promise`或者一个`Stream`，取决于[`options.stream`](readme.md#stream).

```js
const settings = {
	handler: (options, next) => {
		if (options.stream) {
			// It's a Stream
			// We can perform stream-specific actions on it
			return next(options)
				.on('request', request => setTimeout(() => request.abort(), 50));
		}

		// It's a Promise
		return next(options);
	},
	options: got.mergeOptions(got.defaults.options, {
		json: true
	})
};

const jsonGot = got.create(settings);
```

```js
const defaults = {
	options: {
		retry: {
			retries: 2,
			methods: [
				'GET',
				'PUT',
				'HEAD',
				'DELETE',
				'OPTIONS',
				'TRACE'
			],
			statusCodes: [
				408,
				413,
				429,
				500,
				502,
				503,
				504
			]
		},
		headers: {
			'user-agent': `${pkg.name}/${pkg.version} (https://github.com/sindresorhus/got)`
		},
		hooks: {
			beforeRequest: [],
			beforeRedirect: [],
			beforeRetry: [],
			afterResponse: []
		},
		decompress: true,
		throwHttpErrors: true,
		followRedirect: true,
		stream: false,
		form: false,
		json: false,
		cache: false,
		useElectronNet: false
	},
	mutableDefaults: false
};

// Same as:
const defaults = {
	handler: got.defaults.handler,
	options: got.defaults.options,
	mutableDefaults: got.defaults.mutableDefaults
};

const unchangedGot = got.create(defaults);
```

```js
const settings = {
	handler: got.defaults.handler,
	options: got.mergeOptions(got.defaults.options, {
		headers: {
			unicorn: 'rainbow'
		}
	})
};

const unicorn = got.create(settings);

// Same as:
const unicorn = got.extend({headers: {unicorn: 'rainbow'}});
```

### Merging instances

支持组合多个实例。这非常强大.您可以创建一个限制下载速度的客户端,然后使用签署请求的实例进行组合。它就像没有任何插件混乱的插件。您只需创建实例,然后将它们组合在一起.

#### got.mergeInstances(instanceA, instanceB, ...)

将许多实例合并为一个实例:

-   选项合并，是使用[`got.mergeOptions()`](readme.md#gotmergeoptionsparentoptions-newoptions)(+钩子也会被合并),
-   handlers 存储在一个数组中.

## Examples

组合的不同实例，的例子:

#### Denying redirects that lead to other sites than specified

拒绝，指向其他网站的重定向

```js
const controlRedirects = got.create({
	options: got.defaults.options,
	handler: (options, next) => {
		const promiseOrStream = next(options);
		return promiseOrStream.on('redirect', resp => {
			const host = new URL(resp.url).host;
			if (options.allowedHosts && !options.allowedHosts.includes(host)) {
				promiseOrStream.cancel(`Redirection to ${host} is not allowed`);
			}
		});
	}
});
```

#### Limiting download & upload

如果您的机器只有少量RAM,这非常有用.

```js
const limitDownloadUpload = got.create({
    options: got.defaults.options,
    handler: (options, next) => {
        let promiseOrStream = next(options);
        if (typeof options.downloadLimit === 'number') {
            promiseOrStream.on('downloadProgress', progress => {
        		if (progress.transferred > options.downloadLimit && progress.percent !== 1) {
        			promiseOrStream.cancel(`Exceeded the download limit of ${options.downloadLimit} bytes`);
        		}
        	});
        }

        if (typeof options.uploadLimit === 'number') {
            promiseOrStream.on('uploadProgress', progress => {
        		if (progress.transferred > options.uploadLimit && progress.percent !== 1) {
        			promiseOrStream.cancel(`Exceeded the upload limit of ${options.uploadLimit} bytes`);
        		}
        	});
        }

        return promiseOrStream;
    }
});
```

#### No user agent

没有 user agent

```js
const noUserAgent = got.extend({
	headers: {
		'user-agent': null
	}
});
```

#### Custom endpoint

自定义 端点

```js
const httpbin = got.extend({
	baseUrl: 'https://httpbin.org/'
});
```

#### Signing requests

签名请求

```js
const crypto = require('crypto');
const getMessageSignature = (data, secret) => crypto.createHmac('sha256', secret).update(data).digest('hex').toUpperCase();
const signRequest = got.extend({
	hooks: {
		beforeRequest: [
			options => {
				options.headers['sign'] = getMessageSignature(options.body || '', process.env.SECRET);
			}
		]
	}
});
```

#### Putting it all together

> 把上面的都放一起

如果这些实例是不同的模块,并且您不想重写它们,请使用`got.mergeInstances()`.

**注意** : 而`noUserAgent`实例必须放在合并链的末尾，当按顺序合并时。因为其他都有`user-agent`.

```js
const merged = got.mergeInstances(controlRedirects, limitDownloadUpload, httpbin, signRequest, noUserAgent);

(async () => {
	// There's no 'user-agent' header :)
	await merged('/');
	/* HTTP Request =>
	 * GET / HTTP/1.1
	 * accept-encoding: gzip, deflate
	 * sign: F9E66E179B6747AE54108F82F8ADE8B3C25D76FD30AFDE6C395822C530196169
	 * Host: httpbin.org
	 * Connection: close
	 */

	const MEGABYTE = 1048576;
	await merged('http://ipv4.download.thinkbroadband.com/5MB.zip', {downloadLimit: MEGABYTE});
	// CancelError: Exceeded the download limit of 1048576 bytes

	await merged('https://jigsaw.w3.org/HTTP/300/301.html', {allowedHosts: ['google.com']});
	// CancelError: Redirection to jigsaw.w3.org is not allowed
})();
```
