# POST提交数据的请求类型

HTTP协议的POST请求一般用来向服务端提交数据，数据是需要经过编码才能提交给服务端的，同时服务端也需要解码，所以需要通过一个字段来告诉服务端编码方式，这个字段就是Content-Type。在前端的开发中，大部分时间不需要单独配置这个字段，因为大部分框架和依赖包已经有默认的配置，常见的数据编码方式有：
- application/json
这个配置告诉服务端，消息主体是序列化后的JSON字符串，会自动在POST请求加上application/json请求头。使用axios在默认情况下就是使用这个配置。
- application/x-www-form-urlencoded
这个是最普通的POST提交数据的方式，浏览器原生的<form>表单，如果不设置enctype属性，那么最终就会以application/x-www-form-urlencoded方式提交数据，这种方式会以键值对的形式传到后端，如果前端使用Ajax，默认就是这种方式。
- multipart/from-data
当使用表单上传文件的时候，需要让enctype属性等于multipart/from-data，这个配置会指定传输数据为二进制数据。