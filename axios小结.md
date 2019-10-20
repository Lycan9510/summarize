# axios小结
>本文主要是翻译[axios文档](https://github.com/axios/axios/blob/master/README.md)，存在个人删减。

## 安装（Installing）

使用 npm:

```bash
$ npm install axios
```

使用 cdn:

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

## 案例

执行 `GET` 请求

```js
const axios = require('axios');

// 向user接口发出携带ID的请求
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    // 处理成功
    console.log(response);
  })
  .catch(function (error) {
    // 处理失败
    console.log(error);
  })
  .finally(function () {
    // 总是执行
  });  

// 是否要使用异步/等待？ 将`async`关键字添加到您的外部函数/方法中。
async function getUser() {
  try {
    const response = await axios.get('/user?ID=12345');
    console.log(response);
  } catch (error) {
    console.error(error);
  }
}
```

> **注意:** `async/await` 是ECMAScript 2017的一部分，并且Internet Explorer和较旧的浏览器不支持async / await，因此请谨慎使用。

执行 `POST` 请求

```js
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

执行多个并发请求

```js
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    // 现在两个请求都已完成
  }));
```

## axios API

可以通过将相关配置传递给 `axios` 来发出请求。

##### axios(config)

```js
// 发送一个POST请求
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
});
```

```js
// 发送一个GET请求
axios({
  method: 'get',
  url: 'http://bit.ly/2mTM3nY',
  responseType: 'stream'
});
```

##### axios(url[, config])

```js
// 发送GET请求（默认方法）
axios('/user/12345');
```

### 请求方法别名

为了方便起见，已为所有受支持的请求方法提供了别名。

##### axios.request(config)
##### axios.get(url[, config])
##### axios.delete(url[, config])
##### axios.head(url[, config])
##### axios.options(url[, config])
##### axios.post(url[, data[, config]])
##### axios.put(url[, data[, config]])
##### axios.patch(url[, data[, config]])

###### 注意
当使用别名方法`url`，`method`和`data`属性时，不需要在config中指定。

### 并发

帮助程序功能，用于处理并发请求。

##### axios.all(iterable)
##### axios.spread(callback)

### 创建实例

可以使用自定义配置新建一个 axios 实例。

##### axios.create([config])

```js
const instance = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});
```

### 实例方法

可用的实例方法在下面列出。 指定的配置将与实例配置合并。

##### axios#request(config)
##### axios#get(url[, config])
##### axios#delete(url[, config])
##### axios#head(url[, config])
##### axios#options(url[, config])
##### axios#post(url[, data[, config]])
##### axios#put(url[, data[, config]])
##### axios#patch(url[, data[, config]])
##### axios#getUri([config])

## 请求配置

这些是发出请求的可用配置选项。 只需要 `url` 。 如果未指定 `method` ，则请求将默认为 `GET` 。

```js
{
  baseURL: 'https://some-domain.com/api/',
  url: '/user',
  method: 'get', // default

  // `transformRequest`允许在将请求数据发送到服务器之前对其进行更改
  // 这仅适用于请求方法 'PUT', 'POST', 'PATCH' 和 'DELETE'
  // 数组中的最后一个函数必须返回 字符串 或 ArrayBuffer，
  // FormData 或 Stream
  // 可以修改标头对象。
  transformRequest: [function (data, headers) {
    // Do whatever you want to transform the data

    return data;
  }],

  // `transformResponse`允许在更改响应数据之前
  // 它传递给then / catch
  transformResponse: [function (data) {
    // Do whatever you want to transform the data

    return data;
  }],

  // `headers` 是要发送的自定义标头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` 是与请求一起发送的URL参数
  // 必须是 普通对象 或 URLSearchParams 对象
  params: {
    ID: 12345
  },

  // `paramsSerializer` i是一个可选功能，负责序列化 `params`
  // (例如：https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function (params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },

  // `data` 是要作为请求主体发送的数据
  // 仅适用于请求方法 'PUT', 'POST'，和 'PATCH'
  // 如果未设置`transformRequest`，则必须为以下类型之一：
  // - 字符串，普通对象，ArrayBuffer，ArrayBufferView，URLSearchParams
  // - 浏览器专属: FormData, File, Blob
  // - Node 专属: Stream, Buffer
  data: {
    firstName: 'Fred'
  },

  // `timeout` 指定请求超时之前的毫秒数。
  // 如果请求花费的时间超过 `timeout` ，则请求将被中止。
  timeout: 1000, // default is `0` (no timeout)

  // `withCredentials` 指示是否跨站点访问控制请求
  // 应该使用凭据进行
  withCredentials: false, // default

  // `adapter` 允许自定义处理请求，这使测试更加容易。
  // 返回承诺并提供有效的响应（请参见lib / adapters / README.md）。
  adapter: function (config) {
    /* ... */
  },

  // `auth` 指示应使用HTTP Basic auth，并提供凭据。
  // 这将设置一个 `Authorization` 标头，以覆盖您使用 `headers` 设置的所有现有 `Authorization` 自定义标头。
  // 请注意，只能通过此参数配置HTTP Basic身份验证。
  // 对于Bearer令牌等，请改用 `Authorization` 自定义标头。
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

  // `responseType` 表示服务器将使用以下选项响应的数据类型：'arraybuffer'，'document'，'json'，'text'，'stream'
  //  仅限浏览器：“ blob”
  responseType: 'json', // default

  // `responseEncoding` 表示用于解码响应的编码
  // 注意：忽略 'stream' 或客户端请求的 `responseType` 
  responseEncoding: 'utf8', // default

  // `xsrfCookieName` 是用作xsrf令牌值的cookie的名称
  xsrfCookieName: 'XSRF-TOKEN', // default

  // `xsrfHeaderName` 是带有xsrf令牌值的http标头的名称
  xsrfHeaderName: 'X-XSRF-TOKEN', // default

  // `onUploadProgress` 允许处理上传进度事件
  onUploadProgress: function (progressEvent) {
    // 对本机进度事件执行任何操作
  },

  // `onDownloadProgress` 允许处理下载进度事件
  onDownloadProgress: function (progressEvent) {
    // 对本机进度事件执行任何操作
  },

  // `maxContentLength` 定义http响应内容的最大大小（以字节为单位）
  maxContentLength: 2000,

  // `validateStatus` 定义是为给定的HTTP响应状态代码解析还是拒绝承诺。
  // 如果validateStatus返回true（或设置为null或undefined），则承诺将得到解决；否则，诺言将被拒绝。
  validateStatus: function (status) {
    return status >= 200 && status < 300; // default
  },

  // `maxRedirects` 定义了node.js中要遵循的最大重定向数。
  // 如果设置为0，则不会进行重定向。
  maxRedirects: 5, // default

  // `socketPath` 定义要在node.js中使用的UNIX套接字。
  // 例如：'/var/run/docker.sock'将请求发送到Docker守护程序。
  // 只能指定 `socketPath` 或 `proxy`。
  // 如果两者都指定，则使用`socketPath`。
  socketPath: null, // default

  // `httpAgent`和`httpsAgent`定义了在node.js中分别执行http和https请求时要使用的自定义代理。 
  // 这样可以添加默认情况下未启用的选项，如 `keepAlive`。
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 'proxy' 定义代理服务器的主机名和端口。
  // 可以使用常规的 `http_proxy` 和 https_proxy` 环境变量。
  // 如果将环境变量用于代理配置，则还可以将no_proxy环境变量定义为以逗号分隔的不应代理的域列表。
  // 使用`false`禁用代理，忽略环境变量。
  // auth表示应该使用HTTP Basic auth连接到代理，并提供凭据。
  // 这将设置一个“ Proxy-Authorization”标头，覆盖您使用“ headers”设置的任何现有“ Proxy-Authorization”自定义标头。
  
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // `cancelToken` 指定一个取消令牌，可用于取消请求
  // （有关详细信息，请参见下面的“取消”部分）
  cancelToken: new CancelToken(function (cancel) {
  })
}
```

## 响应模式

请求的响应包含以下信息。

```js
{
  // `data` is the response that was provided by the server
  data: {},

  // `status` is the HTTP status code from the server response
  status: 200,

  // `statusText` is the HTTP status message from the server response
  statusText: 'OK',

  // `headers` the headers that the server responded with
  // All header names are lower cased
  headers: {},

  // `config` is the config that was provided to `axios` for the request
  config: {},

  // `request` is the request that generated this response
  // It is the last ClientRequest instance in node.js (in redirects)
  // and an XMLHttpRequest instance in the browser
  request: {}
}
```

使用 `then` 时，您将收到如下响应：

```js
axios.get('/user/12345')
  .then(function (response) {
    console.log(response.data);
    console.log(response.status);
    console.log(response.statusText);
    console.log(response.headers);
    console.log(response.config);
  });
```

When using `catch`, or passing a [rejection callback](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) as second parameter of `then`, the response will be available through the `error` object as explained in the [Handling Errors](#handling-errors) section.

## 配置默认值

可以指定将应用于每个请求的配置默认值。

### 全局axios默认值

```js
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```

### 自定义实例默认值

```js
// 创建实例时设置默认配置
const instance = axios.create({
  baseURL: 'https://api.example.com'
});

// 创建实例后更改默认值
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
```

### 配置优先顺序

Config 将以优先顺序合并.  顺序是在 [lib/defaults.js](https://github.com/axios/axios/blob/master/lib/defaults.js#L28), 中找到库默认值，然后是实例的 `defaults` 属性，最后是请求的 `config` 参数。后者将优先于前者。 这是一个例子。

```js
// 使用库提供的配置默认值创建实例
// 此时，超时配置值为 '0' ，这是库的默认值
const instance = axios.create();

// 库的默认超时被覆盖
// 现在，使用此实例的所有请求将等待2.5秒，然后再超时
instance.defaults.timeout = 2500;

// 覆盖此请求的超时
instance.get('/longRequest', {
  timeout: 5000
});
```

## 拦截器

可以先拦截请求或响应，然后再由 `then` 或 `catch` 处理。

```js
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前处理
    return config;
  }, function (error) {
    // 对请求错误进行处理
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 任何位于2xx范围内的状态码都会触发此功能
    // 对响应数据进行处理
    return response;
  }, function (error) {
    // 任何超出2xx范围的状态码都会触发此功能
    // 对响应错误进行处理
    return Promise.reject(error);
  });
```

可以按需要删除拦截器。

```js
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

可以将拦截器添加到axios的自定义实例中。

```js
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```

## 处理错误

```js
axios.get('/user/12345')
  .catch(function (error) {
    if (error.response) {
      // 发出了请求，服务器返回了状态码
      // 超出2xx的范围
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else if (error.request) {
      // 已提出请求，但未收到回复
      // 'error.request' 是浏览器中XMLHttpRequest的一个实例，也是 node.js 中 http.ClientRequest 的一个实例
      console.log(error.request);
    } else {
      // 设置触发错误的请求时的处理
      console.log('Error', error.message);
    }
    console.log(error.config);
  });
```

使用 `validateStatus` 配置选项，您可以定义应该抛出错误的HTTP代码。

```js
axios.get('/user/12345', {
  validateStatus: function (status) {
    return status < 500; // 仅在状态码大于或等于500时拒绝
  }
})
```

使用 `toJSON` ，将获得一个对象，其中包含有关HTTP错误的更多信息。

```js
axios.get('/user/12345')
  .catch(function (error) {
    console.log(error.toJSON());
  });
```

## 取消

可以使用 *cancel token* 取消请求。

> The axios cancel token API is based on the withdrawn [cancelable promises proposal](https://github.com/tc39/proposal-cancelable-promises).

You can create a cancel token using the `CancelToken.source` factory as shown below:

```js
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios.get('/user/12345', {
  cancelToken: source.token
}).catch(function (thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled', thrown.message);
  } else {
    // handle error
  }
});

axios.post('/user/12345', {
  name: 'new name'
}, {
  cancelToken: source.token
})

// 取消请求（message参数是可选的）
source.cancel('Operation canceled by the user.');
```

You can also create a cancel token by passing an executor function to the `CancelToken` constructor:

```js
const CancelToken = axios.CancelToken;
let cancel;

axios.get('/user/12345', {
  cancelToken: new CancelToken(function executor(c) {
    // An executor function receives a cancel function as a parameter
    cancel = c;
  })
});

// cancel the request
cancel();
```

> Note: you can cancel several requests with the same cancel token.

## 使用application / x-www-form-urlencoded格式

默认情况下，axios将JavaScript对象序列化为 `JSON` 。 要改为以 `application/x-www-form-urlencoded` 格式发送数据，可以使用以下选项之一。

### 浏览器

在浏览器中，可以如下使用 [`URLSearchParams`](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams) API：

```js
const params = new URLSearchParams();
params.append('param1', 'value1');
params.append('param2', 'value2');
axios.post('/foo', params);
```

> 请注意，并非所有浏览器都支持 `URLSearchParams` （请参见 [caniuse.com](http://www.caniuse.com/#feat=urlsearchparams) ），但是有可用的polyfill（确保对全局环境进行 [polyfill](https://github.com/WebReflection/url-search-params) ）。

另外，您可以使用 [`qs`](https://github.com/ljharb/qs) 库对数据进行编码：

```js
const qs = require('qs');
axios.post('/foo', qs.stringify({ 'bar': 123 }));
```

或以另一种方式（ES6），

```js
import qs from 'qs';
const data = { 'bar': 123 };
const options = {
  method: 'POST',
  headers: { 'content-type': 'application/x-www-form-urlencoded' },
  data: qs.stringify(data),
  url,
};
axios(options);
```

### Node.js

在node.js中，可以按以下方式使用 [`querystring`](https://nodejs.org/api/querystring.html) 模块：

```js
const querystring = require('querystring');
axios.post('http://something.com/', querystring.stringify({ foo: 'bar' }));
```

也能使用 [`qs`](https://github.com/ljharb/qs) 库.

###### 注意

如果需要对嵌套对象进行字符串化处理，则最好使用 `qs` 库，因为 `querystring` 方法存在该用例的已知问题（https://github.com/nodejs/node-v0.x-archive/issues/1665）。

## Semver

在axios达到 `1.0` 版本之前，重大更改将以新的次要版本发布。 
例如， `0.5.1`和 `0.5.4` 将具有相同的API，但是 `0.6.0` 将具有重大更改。

## Promises

axios取决于要 [支持](http://caniuse.com/promises) 的本机ES6 Promise实现。 
如果您的环境不支持ES6 Promises，则可以进行 [polyfill](https://github.com/jakearchibald/es6-promise)。

## TypeScript
axios包括 [TypeScript](http://typescriptlang.org) 定义。
```typescript
import axios from 'axios';
axios.get('/user?ID=12345');
```

## 资源

* [Changelog](https://github.com/axios/axios/blob/master/CHANGELOG.md)
* [Upgrade Guide](https://github.com/axios/axios/blob/master/UPGRADE_GUIDE.md)
* [Ecosystem](https://github.com/axios/axios/blob/master/ECOSYSTEM.md)
* [Contributing Guide](https://github.com/axios/axios/blob/master/CONTRIBUTING.md)
* [Code of Conduct](https://github.com/axios/axios/blob/master/CODE_OF_CONDUCT.md)

## Credits

axios is heavily inspired by the [$http service](https://docs.angularjs.org/api/ng/service/$http) provided in [Angular](https://angularjs.org/). Ultimately axios is an effort to provide a standalone `$http`-like service for use outside of Angular.

## 协议

[MIT](LICENSE)
