php代理解决跨域问题
==============

通过php代理解跨域问题（XMLHTTPRequest，如ajax）。
在一些场景中可以使用
``` PHP
header('Access-Control-Allow-Origin: *');
```
或者 `iframe` `jsonp` 能够解决部分问题。
本项目采用php代理的方式，在不改动代码原代码的情况下，解决跨域问题。
要求PHP 5.3+
有兴趣的同学可以看看 [怎么跨域](http://enable-cors.org/server.html) （我还没看）.

使用
--------------

请求的地址是 `proxy.php` 的地址。
需要带上 `csurl` 参数，也就是需要跨域的站点。
当然可以使用其他方式请求，只要带上了 `csurl` 参数即可。

``` JAVASCRIPT
$('#target').load(
	'http://{本地站点}/proxy.php', {
		csurl: 'http://{需要跨域的站点}/',
		param1: value1, 
		param2: value2
	}
);
```

可以根据 `X-Proxy-URL` 这个头部信息，使用js自行判断是否需要使用跨域代理, 如使用 `$.ajaxPrefilter` 前处理。

``` JAVASCRIPT
$.ajaxPrefilter(function(options, originalOptions, jqXHR) {
	if (options.url.match(/^https?:/)) {
		options.headers['X-Proxy-URL'] = options.url;
		options.url = '/proxy.php';
	}
});
```

设置
--------------

为了安全 `proxy.php` 文件最好设置下可以跨域的站点:

``` PHP
$valid_requests = array(
	'http://www.domainA.com/',
	'http://www.domainB.com/path-to-services/service-a'
);
```

 
