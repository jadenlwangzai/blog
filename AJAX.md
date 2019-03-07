#### 1.POST数据出现4开头错误,这些都是客户端错误

* 415：Unsupported Media Type
对于当前请求的方法和所请求的资源，请求中提交的实体并不是服务器中所支持的格式，因此请求被拒绝。

解决：

```javascript
$$.ajax({
	contentType: 'application/json;chartset=UTF-8';//设置请求内容类型
})
```

* 400：Bad Request。前端请求的格式错误
(1)语义有误，当前请求无法被服务器理解。除非进行修改，否则客户端不应该重复提交这个请求。
(2)请求参数有误。

在POST请求下需要对数据对象转成字符串

```javascript
	$$.ajax({
            url: url,
            method: 'POST',
            contentType: 'application/json;chartset=UTF-8',
            dataType: 'json',
            data: JSON.stringify(
                data
            ),//需要POST请求的类型的，数据对象转成字符串格式
            success: function(data) {}
         });
```

#### 2.AJAX设置headers来跨域

```javascript
$$.ajax({
        url: url,
        headers: { 
		authorization: loginToken 
	},
        method: 'POST',
        contentType: 'application/json;chartset=UTF-8',
        dataType: 'json',
        data: JSON.stringify(
            data
        ),//需要POST请求的类型的，数据对象转成字符串格式
        success: function(data) {}
});

```

具体前端和后端实现过程：

（1）服务端：设置响应头
`header('Access-Control-Allow-Origin:*');` //支持全域名访问，不安全，部署后需要固定限制为客户端网址
`header('Access-Control-Allow-Methods:POST,GET,OPTIONS,DELETE');` //支持的http 动作
`header('Access-Control-Allow-Headers:x-requested-with,content-type');`  //响应头 请按照自己需求添加。

（2）客户端前端：设置headers自定义参数的时候的 "预请求" (也就是我们为什么总是看到两次请求的地方)
A 第一步：发送预请求`OPTIONS`请求。此时,服务器端需要对于`OPTIONS`请求作出响应 一般使用`202`响应即可，不用返回任何内容信息。
B 第二步：服务器accepted第一步请求后，浏览器自动执行第二步`发送真正的请求`。

#### 3. 接口给了成功回调的数据，但是进了error回调

接口给了成功回调的数据，但是进了error回调，导致，访问接口成功的callback无法执行。初步觉得 JSON 格式不对，但是看了Charles 其实是对的 

JSON格式 
* 1）键名称：用双引号括起

* 2）字符串：用使用双引号括起 

* 3）数字，布尔类型不需要使用双引号括起　　