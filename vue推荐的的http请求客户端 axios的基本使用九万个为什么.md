---
title: vue推荐的的http请求客户端 axios的基本使用
date: 2017-03-23 00:04:26
categories: "开发"
tags:
	- 技术
	- GitHub
	- Node.js
	- JSON
	- 数据结构

---

![vue推荐的的http请求客户端 axios的基本使用][vue_http_ axios]

vue更新到2.0之后，作者就宣告不再对vue-resource更新，而是推荐的 axios

基于 Promise 的 HTTP 请求客户端，可同时在浏览器和 node.js 中使用

版本

v0.15.3

功能特性

 *  在浏览器中发送
    
    XMLHttpRequests
    
    请求
 *  在
    
    node.js
    
    中发送 http请求
 *  支持
    
    Promise API
 *  拦截请求和响应
 *  转换请求和响应数据
 *  自动转换
    
    JSON
    
    数据
 *  客户端支持保护安全免受
    
    XSRF
    
    攻击

请求方式

    axios(config)axios.request(config)axios.get(url[, config])axios.delete(url[, config])axios.head(url[, config])axios.post(url[, data[, config]])axios.put(url[, data[, config]])axios.patch(url[, data[, config]])

get请求

    axios.get('/user',{ params:{id: 12} }).then(res=>{ console.log(res) }).catch(err=>{ console.log(err) })

post请求

    axios.post('/user',{id: 12}).then(res=>{ console.log(res) }).catch(err=>{ console.log(err) })

发送并发请求

    axios.all([axios.get('/profile'), axios.post('/user')]).then(axios.spread((res1, res2)=>{console.log(res1)console.log(res2)}))

axios.all(\[\])

返回的结果是一个数组，使用

axios.spread

可将数组

\[res1,res2\]

展开为

res1, res2

直接通过配置发送请求，类似于 $.ajax(config)

axios(config) / axios(url,\[config\])

    axios({url:'/user',method: 'post',data:{ id: 1 },})axios('/user/12')

axios实例

实例配置

使用自定义的配置创建一个axios实例

    var axiosIns = axios.create({baseURL: '',timeout: 60000,headers: {'X-Custom-Header': 'foo'}})

axiosIns.get/post/delete/put/patch/head 都可以共用实例配置

请求配置

    {// 请求地址url: '/user',// 请求类型method: 'get',// 请根路径baseURL: 'http://www.mt.com/api',// 请求前的数据处理transformRequest:[function(data){}],// 请求后的数据处理transformResponse: [function(data){}],// 自定义的请求头headers:{'x-Requested-With':'XMLHttpRequest'},// URL查询对象params:{ id: 12 },// 查询对象序列化函数paramsSerializer: function(params){ }// request bodydata: { key: 'aa'},// 超时设置stimeout: 1000,// 跨域是否带TokenwithCredentials: false,// 自定义请求处理adapter: function(resolve, reject, config){},// 身份验证信息auth: { uname: '', pwd: '12'},// 响应的数据格式 json / blob /document /arraybuffer / text / streamresponseType: 'json',// xsrf 设置xsrfCookieName: 'XSRF-TOKEN',xsrfHeaderName: 'X-XSRF-TOKEN',// 下传和下载进度回调onUploadProgress: function(progressEvent){Math.round( (progressEvent.loaded * 100) / progressEvent.total )},onDownloadProgress: function(progressEvent){},// 最多转发数，用于node.jsmaxRedirects: 5,// 最大响应数据大小maxContentLength: 2000,// 自定义错误状态码范围validateStatus: function(status){return status >= 200 && status < 300;},// 用于node.js httpAgent: new http.Agent({ keepAlive: treu }),httpsAgent: new https.Agent({ keepAlive: true }),// 用于设置跨域请求代理proxy: {host: '127.0.0.1',port: 8080,auth: {username: 'aa',password: '2123'}},// 用于取消请求cancelToken: new CancelToken(function(cancel){})}

响应的数据结构

    {data: {}, //服务器返回的数据status: 200,statusText: 'OK',headers: {},config: {}}

全局配置

应用于所有请求

axios.defaults.baseURL = ‘http://www.mt.com/api‘

axios.defaults.headers.post\[‘Content-Type’\] = ‘application/x-www-form-urlencoded’;

拦截请求与响应

在

then

或

catch

之前拦截处理

    // 请求拦截axios.interceptors.request.use(function (config) {// 处理请求之前的配置return config;}, function (error) {// 请求失败的处理return Promise.reject(error);});// 响应拦截axios.interceptors.response.use(function (response) {// 处理响应数据return response;}, function (error) {// 处理响应失败return Promise.reject(error);});

错误处理

    axios.get('/user/12345').catch(function (error) {if (error.response) {// 服务器返回正常的异常对象console.log(error.response.data);console.log(error.response.status);console.log(error.response.headers);} else {// 服务器发生未处理的异常console.log('Error', error.message);}console.log(error.config);});

取消请求

    var CancelToken = axios.CancelToken;var source = CancelToken.source();axios.get('/user/12345', {cancelToken: source.token}).catch(function(thrown) {if (axios.isCancel(thrown)) {console.log('Request canceled', thrown.message);} else {// handle error}});// 取消请求source.cancel('Operation canceled by the user.');

    var CancelToken = axios.CancelToken;var cancel;axios.get('/user/12345', {cancelToken: new CancelToken(function executor(c) {cancel = c;})});// 取消请求cancel();

qs模块

用于处理URL查询参数

    var qs = require('qs');axios.post('/foo', qs.stringify({ 'bar': 123 }));

更详细更新的文档请参考axios github


[vue_http_ axios]: /pro/os/crawler/FYIR-JFRV-MJ7J.jpg
 *  **原文作者：** 九万个为什么
 *  **原文链接：** https://www.toutiao.com/item/6400354534828278274/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。