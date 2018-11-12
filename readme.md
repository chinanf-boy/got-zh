# sindresorhus/got [![explain]][source] [![translate-svg]][translate-list] 
    
<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name
    


「 简化的HTTP请求 」

[中文](./readme.md) | [english](https://github.com/sindresorhus/got) 


---

## 校对 ✅

<!-- doc-templite START generated -->
<!-- repo = 'sindresorhus/got' -->
<!-- commit = '7f18ef397341214d9f46d774f69e65d6cdd95494' -->
<!-- time = '2018-11-08' -->
翻译的原文 | 与日期 | 最新更新 | 更多
---|---|---|---
[commit] | ⏰ 2018-11-08 | ![last] | [中文翻译][translate-list]

[last]: https://img.shields.io/github/last-commit/sindresorhus/got.svg
[commit]: https://github.com/sindresorhus/got/tree/7f18ef397341214d9f46d774f69e65d6cdd95494

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)
        

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

<div align="center">
	<br>
	<br>
	<img width="360" src="https://github.com/sindresorhus/got/blob/master/media/logo.svg" alt="Got">
	<br>
	<br>
	<br>
	<p align="center">非常感谢 <a href="https://moxy.studio"><img src="https://sindresorhus.com/assets/thanks/moxy-logo.svg" width="150"></a> 赞助 me!
	</p>
	<br>
	<br>
</div>

> 简化的HTTP请求

[![Build Status: Linux](https://travis-ci.org/sindresorhus/got.svg?branch=master)](https://travis-ci.org/sindresorhus/got) [![Coverage Status](https://coveralls.io/repos/github/sindresorhus/got/badge.svg?branch=master)](https://coveralls.io/github/sindresorhus/got?branch=master) [![Downloads](https://img.shields.io/npm/dm/got.svg)](https://npmjs.com/got) [![Install size](https://packagephobia.now.sh/badge?p=got)](https://packagephobia.now.sh/result?p=got)

Got是一个人性化,且功能强大的HTTP请求库.

它的创建是因为常用的[`request`](https://github.com/request/request)包过臃肿: [![Install size](https://packagephobia.now.sh/badge?p=request)](https://packagephobia.now.sh/result?p=request)

Got是Node.js的请求库。对于浏览器的请求,我们建议[ky](https://github.com/sindresorhus/ky).

## Highlights

-  [x] [Promise 和 Streams API](#api)
-  [x] [请求取消](#aborting-the-request)
-  [x] [符合RFC的缓存](#cache-adapters)
-  [x] [遵循重定向](#followredirect)
-  [x] [失败时重试](#retry)
-  [x] [进展事件](#onuploadprogress-progress)
-  [x] [处理 gzip/deflate](#decompress)
-  [x] [超时处理](#timeout)
-  [x] [元数据错误](#errors)
-  [x] [JSON模式](#json)
-  [x] [WHATWG URL支持](#url)
-  [x] [钩子](https://github.com/sindresorhus/got#hooks)
-  [x] [具有自定义默认值的实例](#instances)
-  [x] [可组合](advanced-creation.zh.md#merging-instances)
-  [x] [Electron支持](#useelectronnet)
-  [由~2000包和~500K repos使用](https://github.com/sindresorhus/got/network/dependents)
-   积极维护

[了解Got,如何与其他HTTP库进行比较](#comparison)

## Install

```
$ npm install got
```

<a href="https://www.patreon.com/sindresorhus">
	<img src="https://c5.patreon.com/external/logo/become_a_patron_button@2x.png" width="160">
</a>

## Usage

```js
const got = require('got');

(async () => {
	try {
		const response = await got('sindresorhus.com');
		console.log(response.body);
		//=> '<!doctype html> ...'
	} catch (error) {
		console.log(error.response.body);
		//=> 'Internal server error ...'
	}
})();
```

###### Streams

```js
const fs = require('fs');
const got = require('got');

got.stream('sindresorhus.com').pipe(fs.createWriteStream('index.html'));

// For POST, PUT, and PATCH methods `got.stream` returns a `stream.Writable`
fs.createReadStream('index.html').pipe(got.stream.post('sindresorhus.com'));
```

### API

`GET`方法为默认情况下的请求,但可以使用不同的方法，或在`options`.

#### got(url, [options])


- 返回一个[`response`对象](#response)的Promise或者

- 一个[stream](#streams-1)，如果`options.stream`设置为true.


参数 | 
---|
[url](#url) |
[options](#options) |

##### url

类型: | `string` `Object` 
---|---

- 请求的URL，若是字符串, 一个[`https.request`选项对象](https://nodejs.org/api/https.html#https_https_request_options_callback)或者一个[WHATWG `URL`](https://nodejs.org/api/url.html#url_class_url).

- `options`的属性将覆盖这个`url`属性.

- 如果未指定协议,则默认为`https`.

##### options

类型: | `Object`
---|---

参数 |> [Streams](#streams) / [baseUrl](#baseurl) / [headers](#headers) / [stream](#stream) / [body](#body) / [cookieJar](#cookiejar) / [encoding](#encoding) / [form](#form) / [json](#json) / [query](#query) / [timeout](#timeout) / [retry](#retry) / [followRedirect](#followredirect) / [decompress](#decompress) / [cache](#cache) / [request](#request) / [useElectronNet](#useelectronnet) / [throwHttpErrors](#throwhttperrors) / [agent](#agent) / [hooks](#hooks)

任何一个[`https.request`](https://nodejs.org/api/https.html#https_https_request_options_callback)的选项.

###### baseUrl

类型:| `string` `Object`
---|---

- 指定时,`baseUrl`将作为`url`的前缀.<br>如果您指定绝对URL,它将跳过`baseUrl`。

使用`got.extend()`时非常有用，用于创建利基特定的Got实例.

可以是字符串或[WHATWG `URL`](https://nodejs.org/api/url.html#url_class_url).

削减`baseUrl`的结尾，拼接开始的`url`参数，且是可选的:

```js
await got('hello', {baseUrl: 'https://example.com/v1'});
//=> 'https://example.com/v1/hello'

await got('/hello', {baseUrl: 'https://example.com/v1/'});
//=> 'https://example.com/v1/hello'

await got('/hello', {baseUrl: 'https://example.com/v1'});
//=> 'https://example.com/v1/hello'
```

###### headers

类型:| `Object`
---|---
默认:|`{}`

请求标头.

- 现有标题将被覆盖。标题设置为`null`将被省略.

###### stream

类型:| `boolean`
---|---
默认:|`false`

返回一个`Stream`，而不是`Promise`。这相当于调用`got.stream(url, [options])`.

###### body

类型:| `string` `Buffer` `stream.Readable` [`form-data`实例](https://github.com/form-data/form-data)
---|---

*如果您提供此选项,`got.stream()`将是只读的.*

`POST`请求发送的主体.

如果存在`options`和`options.method`未设定,`options.method`将被设置为`POST`.

该`content-length`标题将自动设置，如果`body`是一个`string`/`Buffer`/`fs.createReadStream`实例/[`form-data`实例](https://github.com/form-data/form-data),和`content-length`和`transfer-encoding`就不再是`options.headers`的手动设置.

###### cookieJar

类型: | [`tough.CookieJar`实例](https://github.com/salesforce/tough-cookie#cookiejar)
---|---

Cookie支持.您不必关心解析或如何存储它们.[例子.](#cookies)

**注意:** `options.headers.cookie`将被覆盖.

###### encoding

类型:| `string` `null`
---|---
默认: | `'utf8'`

[编码-Encoding](https://nodejs.org/api/buffer.html#buffer_buffers_and_character_encodings)用于响应数据的`setEncoding`。如果`null`, 响应body会返回一个[`Buffer`](https://nodejs.org/api/buffer.html)(二进制数据).

###### form

类型:| `boolean`
---|---
默认: | `false`

*如果您提供此选项,`got.stream()`将是只读的.*

如果设置为`true`和`Content-Type`标头未设置,它将被设置为`application/x-www-form-urlencoded`.

`body`必须是一个普通的对象。它将使用[`(new URLSearchParams(object)).toString()`](https://nodejs.org/api/url.html#url_constructor_new_urlsearchparams_obj)转换为查询字符串.

###### json

类型:| `boolean`
---|---
默认: | `false`

*如果你使用`got.stream()`,此选项将被忽略.*

如果设置为`true`和`Content-Type`标头未设置,它将被设置为`application/json`.

用`JSON.parse`解析响应主体，并设置`accept`标题为`application/json`。如果与`form`选项一起使用,`body`将字符串化为查询字符串,并将响应解析为JSON.

`body`必须是普通对象或数组,和能对其进行字符串化.

###### query

类型: | `string` `Object<string, string\|number>` [URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)
---|---

添加到请求URL的查询字符串。这将覆盖`url`的查询字符串.

如果你需要传入一个数组,你可以使用一个`URLSearchParams`，例如:

```js
const got = require('got');

const query = new URLSearchParams([['key', 'a'], ['key', 'b']]);

got('https://example.com', {query});

console.log(query.toString());
//=> 'key=a&key=b'
```

如果你需要一个不同的数组格式,你可以使用[`query-string`](https://github.com/sindresorhus/query-string)包:

```js
const got = require('got');
const queryString = require('query-string');

const query = queryString.stringify({key: ['a', 'b']}, {arrayFormat: 'bracket'});

got('https://example.com', {query});

console.log(query);
//=> 'key[]=a&key[]=b'
```

###### timeout

类型:| `number` `Object`
---|---

在中止请求发生[`got.TimeoutError`](#gottimeouterror)错误(a.k.a.`request`属性)之前，等待服务器结束响应的毫秒数。默认情况下,没有超时.

这也会接受`object`，使用以下字段来约束请求生命周期的每个阶段的持续时间:

-   `lookup`在分配套接字时启动,在解析主机名时结束。使用Unix域名套接字时不适用.
-   `connect`从`lookup`完成开始(或如果lookup不适用于请求，分配套接字的时候)，并在连接套接字时结束.
-   `secureConnect`从`connect`完成时开始，和在握手过程完成后结束(仅限HTTPS).
-   `socket`在连接套接字时开始。看[request.setTimeout](https://nodejs.org/api/http.html#http_request_settimeout_timeout_callback).
-   `response`当请求已写入套接字时开始,并在收到响应头时结束.
-   `send`套接字连接时开始，并以请求结束写入套接字.
-   `request`在请求初始化后开始,在响应的结束事件触发时结束.

###### retry

类型:| `number` `Object`<br>
---|---
默认: | 重试:`2`次
方法: | `GET` `PUT` `HEAD` `DELETE` `OPTIONS` `TRACE`
statusCodes: | [`408`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408) [`413`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/413) [`429`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) [`500`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500) [`502`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/502) [`503`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/503) [`504`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/504)
maxRetryAfter: | `undefined`

- Object对象 : 代表
	- `retries`, 重试
	- `methods`, 允许的方法
	- `statusCodes`和 
	- `maxRetryAfter` 最大[`Retry-After`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After)时间。

如果`maxRetryAfter`被设置为`undefined`,它会用`options.timeout`.<br>如果[`Retry-After`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After)标头大于`maxRetryAfter`,它将取消请求.

重试之间的延迟计算是`1000 * Math.pow(2, retry) + Math.random() * 100`函数,这里的`retry`是尝试数(从1开始).

`retries`属性可以

- 是`number`或者
- 一个带`retry`和`error`参数的`function`.该函数必须以毫秒为单位的延迟返回(返回`0`值，会取消重试).

**注意:**它仅在指定的方法,状态代码和这些网络错误上重试:

-   `ETIMEDOUT`: 达到[超时](#timeout)其中一个限制.
-   `ECONNRESET`:连接被网络点强行关闭.
-   `EADDRINUSE`:无法绑定到任何自由端口.
-   `ECONNREFUSED`:连接被服务器拒绝.
-   `EPIPE`:正在写入的流的远程端已关闭.
-   `ENOTFOUND`:无法将主机名解析为IP地址.
-   `ENETUNREACH`: 没有网络连接.
-   `EAI_AGAIN`:DNS查找超时.

###### followRedirect

类型:| `boolean`
---|---
默认:|`true`

定义是否应自动遵循重定向响应.

请注意,如果服务器发送回来一个`303`，以响应任何请求类型(`POST`,`DELETE`等等),Got会自动通过`GET`请求，位置头中指向的资源。这符合[规范](https://tools.ietf.org/html/rfc7231#section-6.4.4).

###### decompress

类型:| `boolean`
---|---
默认:|`true`

自动解压响应。这将设置`accept-encoding`标题为`gzip, deflate`，除非你自己设定.

如果禁用此选项,则会将压缩响应作为一个`Buffer`返回。如果您想自己处理解压或stream式处理原始压缩数据, 这可能很有用.

###### cache

类型:| `Object`
---|---
默认:|`false`

[缓存适配器实例](#cache-adapters)用于存储缓存的数据.

###### request

类型:| `Function`<br>
---|---
默认: | `http.request` `https.request` *(取决于协议)*

自定义请求函数.这样做的主要目的是为了[通过包装HTTP2，以此支持这个协议](#experimental-http2-support).

###### useElectronNet

类型:| `boolean`
---|---
默认:|`false`

在Electron中使用时,Got会使用[`electron.net`](https://electronjs.org/docs/api/net/)替代Node.js`http`模块。根据Electron的文档,它应该是完全兼容的,但其实不是完全兼容.看到[#443](https://github.com/sindresorhus/got/issues/443)和[#461](https://github.com/sindresorhus/got/issues/461).

###### throwHttpErrors

类型:| `boolean`
---|---
默认:|`true`

确定是否抛出`got.HTTPError`错误响应(非2xx状态代码).

如果禁用此选项,在请求遇到错误状态代码，会使用`response` resolve，而不是抛出`got.HTTPError`。如果您正在检查资源可用性,并且期望出现错误响应,这可能很有用.

###### agent

对应`http.request`的[`agent`选项](https://nodejs.org/api/http.html#http_http_request_url_options_callback),但有一个额外的功能:

如果您需要针对不同协议的不同代理,则可以将代理映射传递给`agent`选项。这是必要的,因为对一个协议的请求可能会重定向到另一个协议。在这种情况下,Got将为您切换到正确的协议代理.

```js
const got = require('got');
const HttpAgent = require('agentkeepalive');
const {HttpsAgent} = HttpAgent;

got('sindresorhus.com', {
	agent: {
		http: new HttpAgent(),
		https: new HttpsAgent()
	}
});
```

###### hooks

类型:| `Object<string, Function[]>`
---|---

钩子允许在请求生命周期中进行修改。钩子函数可以是异步的，和也能串行运行的.

###### hooks.beforeRequest

类型:| `Function[]`
---|---
默认:|`[]`

调用[标准的](source/normalize-arguments.js) [请求选项](#options)。在发送请求之前,不会对请求进行进一步更改。这对结合[`got.extend()`](#instances)和[`got.create()`](advanced-creation.zh.md)使用，特别有用，当您想要创建一个API客户端时,例如,使用HMAC签名.

见[AWS部分](#aws)举的例子.

**注意**:如果你修改了`body`，你也需要修改`content-length`标题,因为它已经被计算和分配.

###### hooks.beforeRedirect

类型:| `Function[]`
---|---
默认:|`[]`

调用[标准的](source/normalize-arguments.js) [请求选项](#options)。Got的请求不会进一步更改.当您想要避免死站点时,这尤其有用.例:

```js
const got = require('got');

got('example.com', {
	hooks: {
		beforeRedirect: [
			options => {
				if (options.hostname === 'deadSite') {
					options.hostname = 'fallbackSite';
				}
			}
		]
	}
});
```

###### hooks.beforeRetry

类型:| `Function[]`
---|---
默认:|`[]`

调用[标准的](source/normalize-arguments.js) [请求选项](#options),错误和重试计数。Got的请求不会进一步更改。在下次尝试之前，需要一些额外的工作时,这尤其有用.例:

```js
const got = require('got');

got('example.com', {
	hooks: {
		beforeRetry: [
			(options, error, retryCount) => {
				if (error.statusCode === 413) { // Payload too large
					options.body = getNewBody();
				}
			}
		]
	}
});
```

###### hooks.afterResponse

类型:| `Function[]`
---|---
默认:|`[]`

调用[响应对象](#response)和重试函数.

每个函数都应该返回响应。当您想要刷新访问令牌时,这尤其有用.例:

```js
const got = require('got');

const instance = got.extend({
	hooks: {
		afterResponse: [
			(response, retryWithMergedOptions) => {
				if (response.statusCode === 401) { // Unauthorized
					const updatedOptions = {
						headers: {
							token: getNewToken() // Refresh the access token
						}
					};

					// Save for further requests
					instance.defaults.options = got.mergeOptions(instance.defaults.options, updatedOptions);

					// Make a new retry
					return retryWithMergedOptions(updatedOptions);
				}

				// No changes otherwise
				return response;
			}
		]
	},
	mutableDefaults: true
});
```

#### Response

响应对象通常是一个[Node.js HTTP响应流](https://nodejs.org/api/http.html#http_class_http_incomingmessage)但是,如果从缓存返回，那它会是一个[类似响应的对象](https://github.com/lukechilds/responselike)和行为方式相同.

**参数** |> [body](#body-1) /  [url](#url-1) /  [requestUrl](#requesturl) /  [timings](#timings) /  [fromCache](#fromcache) /  [redirectUrls](#redirecturls) /  [retryCount](#retrycount) / 



##### body

类型:| `string` `Object` *(取决于`options.json`)*
---|---

请求的结果.

##### url

类型:| `string`
---|---

重定向后的请求URL或最终URL.

##### requestUrl

类型:| `string`
---|---

原始请求网址.

##### timings

类型:| `Object`
---|---

该对象包含以下属性:

-   `start`- 请求开始的时间.
-   `socket`- 将套接字分配给请求的时间.
-   `lookup`-  DNS查找完成的时间.
-   `connect`- 套接字成功连接的时间.
-   `upload`- 请求完成上传的时间.
-   `response`- 请求解雇的时间`response`事件.
-   `end`- 响应解雇的时间`end`事件.
-   `error`- 请求解雇的时间`error`事件.
-   `phases` 		- `wait` - `timings.socket - timings.start` 		- `dns` - `timings.lookup - timings.socket` 		- `tcp` - `timings.connect - timings.lookup` 		- `request` - `timings.upload - timings.connect` 		- `firstByte` - `timings.response - timings.upload` 		- `download` - `timings.end - timings.response`-`total`-`timings.end - timings.start`要么`timings.error - timings.start`

**注意**:时间是`number`表示自UNIX纪元以来经过的毫秒数.

##### fromCache

类型:| `boolean`
---|---

是否从缓存中检索响应.

##### redirectUrls

类型:| `Array`
---|---

重定向网址.

##### retryCount

类型:| `number`
---|---

重试请求的次数.

#### Streams

**注意**:进度事件,重定向事件和请求/响应事件，也可以与promise一起使用.

#### got.stream(url, [options])

设`options.stream`为`true`.

返回带有其他活动的一个[双工流](https://nodejs.org/api/stream.html#stream_class_stream_duplex):

##### .on('request', request)

`request`事件获取请求的request对象.

**小费**: 您可以使用`request`事件,中止请求:

```js
got.stream('github.com')
	.on('request', request => setTimeout(() => request.abort(), 50));
```

##### .on('response', response)

`response`事件获取最终请求的response对象.

##### .on('redirect', response, nextOptions)

`redirect`事件获取重定向的response对象。第二个参数是下一个重定向位置请求的选项.

##### .on('uploadProgress', progress)

##### .on('downloadProgress', progress)

上传(发送请求)和下载(接收响应)的进度事件。该`progress`参数是一个像这样的对象:

```js
{
	percent: 0.1,
	transferred: 1024,
	total: 10240
}
```

如果无法检索大小(流式传输时，可能发生),`total`会是`null`.

```js
(async () => {
	const response = await got('sindresorhus.com')
		.on('downloadProgress', progress => {
			// Report download progress
		})
		.on('uploadProgress', progress => {
			// Report upload progress
		});

	console.log(response);
})();
```

##### .on('error', error, body, response)

`error`在协议错误的情况下，发出的事件(如`ENOTFOUND`等)或状态错误(4xx或5xx)。第二个参数是状态错误时服务器响应的主体。第三个参数是响应对象.

#### got.get(url, [options])

#### got.post(url, [options])

#### got.put(url, [options])

#### got.patch(url, [options])

#### got.head(url, [options])

#### got.delete(url, [options])

将`options.method`方法名称设置好，和做成请求.

### Instances

#### got.extend([options])

配置一个带默认`options`的新`got`实例。该`options`与父实例的`defaults.options`合并，合并是通过使用[`got.mergeOptions`](#gotmergeoptionsparentoptions-newoptions)。您可以使用在实例上的`.defaults`属性，访问已resolve的选项,.

```js
const client = got.extend({
	baseUrl: 'https://example.com',
	headers: {
		'x-unicorn': 'rainbow'
	}
});

client.get('/demo');

/* HTTP Request =>
 * GET /demo HTTP/1.1
 * Host: example.com
 * x-unicorn: rainbow
 */
```

```js
(async () => {
	const client = got.extend({
		baseUrl: 'httpbin.org',
		headers: {
			'x-foo': 'bar'
		}
	});
	const {headers} = (await client.get('/headers', {json: true})).body;
	//=> headers['x-foo'] === 'bar'

	const jsonClient = client.extend({
		json: true,
		headers: {
			'x-baz': 'qux'
		}
	});
	const {headers: headers2} = (await jsonClient.get('/headers')).body;
	//=> headers2['x-foo'] === 'bar'
	//=> headers2['x-baz'] === 'qux'
})();
```

*需要更多控制Got的行为? 看看[`got.create()`](advanced-creation.zh.md).*

#### got.mergeOptions(parentOptions, newOptions)

扩展父选项。避免使用[对象传播...](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#Spread_in_object_literals)，因为它不能递归工作:

```js
const a = {headers: {cat: 'meow', wolf: ['bark', 'wrrr']}};
const b = {headers: {cow: 'moo', wolf: ['auuu']}};

{...a, ...b}            // => {headers: {cow: 'moo', wolf: ['auuu']}}
got.mergeOptions(a, b)  // => {headers: {cat: 'meow', cow: 'moo', wolf: ['auuu']}}
```

<!-- HERE -->

Options会深度合并到新对象。每个字段名的确定步骤如下: 

> `a`为旧/父的，`b`为新的。

-   如果新属性设置为`undefined`，保留了旧的.
-   如果父属性是`URL`的实例，而新的值是`string`或者`URL`, 那就创建一个新的URL实例:[`new URL(new, parent)`](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL#Syntax).
-   如果新属性是普通`Object`: 
	- 如果父属性也是普通的`Object`, 那两个值递归合并为一个新的`Object`.
	- 否则,只会深拷贝新值.
-   如果新属性是`Array`,它用新属性的深拷贝覆盖旧的.
-   否则,新值分配给对应的字段就好了。

#### got.defaults

类型:| `Object`
---|---

默认的Got选项.

## Errors

每个错误包含(如果可用)的属性字段 |
---|
`body`,|
`statusCode`,|
`statusMessage`,|
`host`,|
`hostname`|
`method`,|
`path`,|
`protocol`|
`url`|

使调试更容易.

在Promise模式下,
`response`也附加到错误.

#### got.CacheError

例如,当缓存方法失败时,如果数据库出现故障或存在文件系统错误.

#### got.RequestError

请求失败时，包含一个具有错误类代码的`code`属性,如`ECONNREFUSED`.

#### got.ReadError

从响应流中读取失败.

#### got.ParseError

当`json`选项已启用,服务器响应代码为2xx,和`JSON.parse`失败.

#### got.HTTPError

当服务器响应代码不是2xx时，包括`statusCode`,`statusMessage`,和`redirectUrls`属性.

#### got.MaxRedirectsError

当服务器重定向您十次以上时，包括一个`redirectUrls`属性,这是在放弃之前，重定向的一个URL数组.

#### got.UnsupportedProtocolError

不支持的协议时.

#### got.CancelError

请求被`.cancel()`中止时.

#### got.TimeoutError

当请求因一个[超时](#timeout)而中止时

## Aborting the request

Got返回的Promise有一个[`.cancel()`](https://github.com/sindresorhus/p-cancelable)。当调用时,会中止请求.

```js
(async () => {
	const request = got(url, options);

	// …

	// In another part of the code
	if (something) {
		request.cancel();
	}

	// …

	try {
		await request;
	} catch (error) {
		if (request.isCanceled) { // Or `error instanceof got.CancelError`
			// Handle cancelation
		}

		// Handle other errors
	}
})();
```

<a name="cache-adapters"></a>

## Cache

Got实现了[RFC 7234](http://httpwg.org/specs/rfc7234.html)兼容的HTTP缓存,可以在内存中开箱即用,并且可以使用各种存储适配器轻松插入。直接从缓存提供新缓存项,并使用`If-None-Match`/`If-Modified-Since`头重新验证过时的缓存项。您可以在[`cacheable-request`文件](https://github.com/lukechilds/cacheable-request)中，阅读有关基本缓存行为的更多信息.

您可以使用JavaScript`Map`类型，作为内存缓存:

```js
const got = require('got');
const map = new Map();

(async () => {
		let response = await got('sindresorhus.com', {cache: map});
		console.log(response.fromCache);
		//=> false

		response = await got('sindresorhus.com', {cache: map});
		console.log(response.fromCache);
		//=> true
})();
```

Got 内部使用[Keyv](https://github.com/lukechilds/keyv)支持各种存储适配器。对于更多伸缩性,你可以使用[官方Keyv存储适配器](https://github.com/lukechilds/keyv#official-storage-adapters):

```
$ npm install @keyv/redis
```

```js
const got = require('got');
const KeyvRedis = require('@keyv/redis');

const redis = new KeyvRedis('redis://user:pass@localhost:6379');

got('sindresorhus.com', {cache: redis});
```

Got提供了Map API内容的支持,因此可以轻松编写自己的存储适配器或使用第三方解决方案.

例如,以下所有，都是有效的存储适配器:

```js
const storageAdapter = new Map();
// Or
const storageAdapter = require('./my-storage-adapter');
// Or
const QuickLRU = require('quick-lru');
const storageAdapter = new QuickLRU({maxSize: 1000});

got('sindresorhus.com', {cache: storageAdapter});
```

查看[keyv文档](https://github.com/lukechilds/keyv)有关如何使用存储适配器的更多信息.

## Proxies

你可以使用[`tunnel`](https://github.com/koichik/node-tunnel)包,加上`agent`与代理一起工作:

```js
const got = require('got');
const tunnel = require('tunnel');

got('sindresorhus.com', {
	agent: tunnel.httpOverHttp({
		proxy: {
			host: 'localhost'
		}
	})
});
```

查看下[`global-tunnel`](https://github.com/np-maintain/global-tunnel)，如果您想为应用程序中的所有HTTP/HTTPS流量配置代理支持.

## Cookies

你可以使用[`tough-cookie`](https://github.com/salesforce/tough-cookie)包:

```js
const got = require('got');
const {CookieJar} = require('tough-cookie');

const cookieJar = new CookieJar();
cookieJar.setCookie('foo=bar', 'https://www.google.com');

got('google.com', {cookieJar});
```

## Form data

你可以使用[`form-data`](https://github.com/form-data/form-data)，用表单数据创建POST请求:

```js
const fs = require('fs');
const got = require('got');
const FormData = require('form-data');
const form = new FormData();

form.append('my_file', fs.createReadStream('/foo/bar.jpg'));

got.post('google.com', {
	body: form
});
```

## OAuth

你可以使用[`oauth-1.0a`](https://github.com/ddo/oauth-1.0a)包，创建签名的OAuth请求:

```js
const got = require('got');
const crypto  = require('crypto');
const OAuth = require('oauth-1.0a');

const oauth = OAuth({
	consumer: {
		key: process.env.CONSUMER_KEY,
		secret: process.env.CONSUMER_SECRET
	},
	signature_method: 'HMAC-SHA1',
	hash_function: (baseString, key) => crypto.createHmac('sha1', key).update(baseString).digest('base64')
});

const token = {
	key: process.env.ACCESS_TOKEN,
	secret: process.env.ACCESS_TOKEN_SECRET
};

const url = 'https://api.twitter.com/1.1/statuses/home_timeline.json';

got(url, {
	headers: oauth.toHeader(oauth.authorize({url, method: 'GET'}, token)),
	json: true
});
```

## Unix Domain Sockets

请求也可以通过[UNIX域名套接字](http://serverfault.com/questions/124517/whats-the-difference-between-unix-socket-and-tcp-ip-socket)发送出去。 使用以下URL方案:`PROTOCOL://unix:SOCKET:PATH`.

-   `PROTOCOL` - `http`或`https` *(可选)*
-   `SOCKET`- 一个UNIX域名套接字的绝对路径,例如:`/var/run/docker.sock`
-   `PATH`- 请求路径,例如:`/v2/keys`

```js
got('http://unix:/var/run/docker.sock:/containers/json');

// Or without protocol (HTTP by default)
got('unix:/var/run/docker.sock:/containers/json');
```

## AWS

对AWS服务的请求，需要签署他们的标头(headers)。这可以通过使用[`aws4`](https://www.npmjs.com/package/aws4)包。这是一个用已签名的请求，查询["API网关"](https://docs.aws.amazon.com/apigateway/api-reference/signing-requests/)的示例..

```js
const AWS = require('aws-sdk');
const aws4 = require('aws4');
const got = require('got');

const chain = new AWS.CredentialProviderChain();

// Create a Got instance to use relative paths and signed requests
const awsClient = got.extend({
	baseUrl: 'https://<api-id>.execute-api.<api-region>.amazonaws.com/<stage>/',
	hooks: {
		beforeRequest: [
			async options => {
				const credentials = await chain.resolvePromise();
				aws4.sign(options, credentials);
			}
		]
	}
});

const response = await awsClient('endpoint/path', {
	// Request-specific options
});
```

## Testing

您可以通过使用[`nock`](https://github.com/node-nock/nock)包模拟端点:

```js
const got = require('got');
const nock = require('nock');

nock('https://sindresorhus.com')
	.get('/')
	.reply(200, 'Hello world!');

(async () => {
	const response = await got('sindresorhus.com');
	console.log(response.body);
	//=> 'Hello world!'
})();
```

如果需要真正的集成测试,可以使用[`create-test-server`](https://github.com/lukechilds/create-test-server):

```js
const got = require('got');
const createTestServer = require('create-test-server');

(async () => {
	const server = await createTestServer();
	server.get('/', 'Hello world!');

	const response = await got(server.url);
	console.log(response.body);
	//=> 'Hello world!'

	await server.close();
})();
```

## Tips

### User Agent

设置`'user-agent'`头是个好主意，因此提供者可以更容易地看到它们的资源是如何使用的。默认情况下,它是指向这个存储库的URL。当然您也可以设置为`null`禁用.

```js
const got = require('got');
const pkg = require('./package.json');

got('sindresorhus.com', {
	headers: {
		'user-agent': `my-package/${pkg.version} (https://github.com/username/my-package)`
	}
});

got('sindresorhus.com', {
	headers: {
		'user-agent': null
	}
});
```

### 304 Responses

记住,如果你发送一个`if-modified-since`标题。和接收到了`304 Not Modified`响应,主体-body就会是空的。缓存和检索主体内容是您的职责.

### Custom endpoints

使用`got.extend()`让它更好地与REST API一起工作。特别是你使用了`baseUrl`选项.

**注:**不要对[`got.create()`](advanced-creation.zh.md)感到疑惑,它没有默认值.

```js
const got = require('got');
const pkg = require('./package.json');

const custom = got.extend({
	baseUrl: 'example.com',
	json: true,
	headers: {
		'user-agent': `my-package/${pkg.version} (https://github.com/username/my-package)`
	}
});

// Use `custom` exactly how you use `got`
(async () => {
	const list = await custom('/v1/users/list');
})();
```

*需要将一些实例合并为单个实例吗? 查看[`got.mergeInstances()`](advanced-creation.zh.md#merging-instances).*

### Experimental HTTP2 support

GET提供了[`http2-wrapper`](https://github.com/szmarczak/http2-wrapper)包，使用HTTP2的实验支持:

```js
const got = require('got');
const {request} = require('http2-wrapper');

const h2got = got.extend({request});

(async () => {
	const {body} = await h2got('https://nghttp2.org/httpbin/headers');
	console.log(body);
})();
```

## Comparison

|                       |     `got`    |   `request`  | `node-fetch` |    `axios`   |
|-----------------------|:------------:|:------------:|:------------:|:------------:|
| HTTP/2 支持        |      ❔      |       ✖      |       ✖      |       ✖      |
| Browser 支持       |       ✖      |       ✖      |       ✔*     |       ✔      |
| Electron 支持      |       ✔      |       ✖      |       ✖      |       ✖      |
| Promise API           |       ✔      |       ✔      |       ✔      |       ✔      |
| Stream API            |       ✔      |       ✔      | Node.js only |       ✖      |
| Request 中止   |       ✔      |       ✖      |       ✖      |       ✔      |
| RFC compliant caching |       ✔      |       ✖      |       ✖      |       ✖      |
| Cookies (out-of-box)  |       ✔      |       ✔      |       ✖      |       ✖      |
| 跟踪 重定向网址     |       ✔      |       ✔      |       ✔      |       ✔      |
| 失败重试   |       ✔      |       ✖      |       ✖      |       ✖      |
| Progress 事件       |       ✔      |       ✖      |       ✖      | Browser only |
| 可控 gzip/deflate  |       ✔      |       ✔      |       ✔      |       ✔      |
| timeouts  优化   |       ✔      |       ✖      |       ✖      |       ✖      |
| Timings               |       ✔      |       ✔      |       ✖      |       ✖      |
| Errors 元数据  |       ✔      |       ✖      |       ✖      |       ✔      |
| JSON 模式             |       ✔      |       ✔      |       ✖      |       ✔      |
| Custom defaults       |       ✔      |       ✔      |       ✖      |       ✔      |
| Composable            |       ✔      |       ✖      |       ✖      |       ✖      |
| Hooks                 |       ✔      |       ✖      |       ✖      |       ✔      |
| Issues open           |   ![][gio]   |   ![][rio]   |   ![][nio]   |   ![][aio]   |
| Issues closed         |   ![][gic]   |   ![][ric]   |   ![][nic]   |   ![][aic]   |
| Downloads             |    ![][gd]   |    ![][rd]   |    ![][nd]   |    ![][ad]   |
| Coverage              |    ![][gc]   |    ![][rc]   |    ![][nc]   |    ![][ac]   |
| Build                 |    ![][gb]   |    ![][rb]   |    ![][nb]   |    ![][ab]   |
| Bugs                  |   ![][gbg]   |   ![][rbg]   |   ![][nbg]   |   ![][abg]   |
| Dependents            |   ![][gdp]   |   ![][rdp]   |   ![][ndp]   |   ![][adp]   |
| Install size          |   ![][gis]   |   ![][ris]   |   ![][nis]   |   ![][ais]   |

\*它几乎与浏览器`fetch` API兼容.<br> ❔ 实验支持.

<!-- ISSUES OPEN -->

[gio]: https://img.shields.io/github/issues/sindresorhus/got.svg

[rio]: https://img.shields.io/github/issues/request/request.svg

[nio]: https://img.shields.io/github/issues/bitinn/node-fetch.svg

[aio]: https://img.shields.io/github/issues/axios/axios.svg

<!-- ISSUES CLOSED -->

[gic]: https://img.shields.io/github/issues-closed/sindresorhus/got.svg

[ric]: https://img.shields.io/github/issues-closed/request/request.svg

[nic]: https://img.shields.io/github/issues-closed/bitinn/node-fetch.svg

[aic]: https://img.shields.io/github/issues-closed/axios/axios.svg

<!-- DOWNLOADS -->

[gd]: https://img.shields.io/npm/dm/got.svg

[rd]: https://img.shields.io/npm/dm/request.svg

[nd]: https://img.shields.io/npm/dm/node-fetch.svg

[ad]: https://img.shields.io/npm/dm/axios.svg

<!-- COVERAGE -->

[gc]: https://coveralls.io/repos/github/sindresorhus/got/badge.svg?branch=master

[rc]: https://coveralls.io/repos/github/request/request/badge.svg?branch=master

[nc]: https://coveralls.io/repos/github/bitinn/node-fetch/badge.svg?branch=master

[ac]: https://coveralls.io/repos/github/mzabriskie/axios/badge.svg?branch=master

<!-- BUILD -->

[gb]: https://travis-ci.org/sindresorhus/got.svg?branch=master

[rb]: https://travis-ci.org/request/request.svg?branch=master

[nb]: https://travis-ci.org/bitinn/node-fetch.svg?branch=master

[ab]: https://travis-ci.org/axios/axios.svg?branch=master

<!-- BUGS -->

[gbg]: https://badgen.net/github/label-issues/sindresorhus/got/bug/open

[rbg]: https://badgen.net/github/label-issues/request/request/Needs%20investigation/open

[nbg]: https://badgen.net/github/label-issues/bitinn/node-fetch/bug/open

[abg]: https://badgen.net/github/label-issues/axios/axios/bug/open

<!-- DEPENDENTS -->

[gdp]: https://badgen.net/npm/dependents/got

[rdp]: https://badgen.net/npm/dependents/request

[ndp]: https://badgen.net/npm/dependents/node-fetch

[adp]: https://badgen.net/npm/dependents/axios

<!-- INSTALL SIZE -->

[gis]: https://packagephobia.now.sh/badge?p=got

[ris]: https://packagephobia.now.sh/badge?p=request

[nis]: https://packagephobia.now.sh/badge?p=node-fetch

[ais]: https://packagephobia.now.sh/badge?p=axios

## Related

- [gh-got](https://github.com/sindresorhus/gh-got) - Got 便利包，与GitHub API交互
- [gl-got](https://github.com/singapore/gl-got) - Got 便利包，与Gitlab API交互
- [travis-got](https://github.com/samverschueren/travis-got) - Got 便利包，与Travis API交互
- [graphql-got](https://github.com/kevva/graphql-got) - Got 便利包，与GraphQL API交互
- [GotQL](https://github.com/khaosdoctor/gotql) - Got 便利包，与GraphQL API交互, 但是使用JSON解析替代字符串

## Maintainers

| [![Sindre Sorhus](https://github.com/sindresorhus.png?size=100)](https://sindresorhus.com) | [![Vsevolod Strukchinsky](https://github.com/floatdrop.png?size=100)](https://github.com/floatdrop) | [![Alexander Tesfamichael](https://github.com/AlexTes.png?size=100)](https://github.com/AlexTes) | [![Luke Childs](https://github.com/lukechilds.png?size=100)](https://github.com/lukechilds) | [![Szymon Marczak](https://github.com/szmarczak.png?size=100)](https://github.com/szmarczak) | [![Brandon Smith](https://github.com/brandon93s.png?size=100)](https://github.com/brandon93s) |
| ------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| [Sindre Sorhus](https://sindresorhus.com) | [Vsevolod Strukchinsky](https://github.com/floatdrop) | [Alexander Tesfamichael](https://alextes.me) | [Luke Childs](https://github.com/lukechilds) | [Szymon Marczak](https://github.com/szmarczak) | [Brandon Smith](https://github.com/brandon93s) |

## License

MIT
