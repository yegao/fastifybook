Request
----
处理函数的第一个参数是Request.
Request是一个包含以下属性的Fastify核心对象:

query - 解释之后的查询字符串
body - 请求体
params - 对应URL的参数
headers - 请求头
req - Node.js传入的HTTP请求
log - 传入请求的日志记录器
```javascript
fastify.post('/:params', options, function (request, reply) {
  console.log(request.body)
  console.log(request.query)
  console.log(request.params)
  console.log(request.headers)
  console.log(request.req)
  request.log.info('some info')
})
```

```javascript
fastify.route(options)
```
###options
```javascript
method:  //DELETE', 'GET', 'HEAD', 'PATCH', 'POST', 'PUT' , 'OPTIONS'  也可以是前面这些method组成的数组
url:     //匹配当前路由的path
schema:  //包含请求和响应的纲要(schemas)的对象
    body://如果请求是以POST和PUT方式发送过来的,将会验证请求体
    querystring://验证请求的querystring(查询字符串),这可以是一个完整的JSON Schema对象,包含type属性和properties属性,也可以直接是包含在properties的值 ①options①
    params://验证请求的params,就是冒号之后的那个(/:params)
    response://过滤和生成响应的纲要(schema),可以增加10-20%的吞吐量( 个人的理解就是reply.send(xxx)的参数xxx的纲要 )
beforeHandler(request,reply,done)://在执行请求(handler)处理之前执行的函数,比如您可以在路由级别执行身份验证,它也可以是函数数组
handler(request,reply)://处理请求的函数
schemaCompiler(schema)://创建纲要(schema)的函数
```
```javascript
fastify.route({
  method: 'GET',
  url: '/',
  schema: {
    querystring: {
      name: { type: 'string' },
      excitement: { type: 'integer' }
    },
    response: {
      200: {
        type: 'object',
        properties: {
          hello: { type: 'string' }
        }
      }
    }
  },
  handler: function (request, reply) {
    reply.send({ hello: 'world' })
  }
})
```
```javascript
fastify.route({
  method: 'GET',
  url: '/',
  schema: { ... },
  beforeHandler: function (request, reply, done) {
    // your authentication logic
    done()
  },
  handler: function (request, reply) {
    reply.send({ hello: 'world' })
  }
})
```













######①options①
个人理解'也可以直接是包含在properties的值'意思就是
```javascript
{
    type:'object',
    properties{
        name:{type:'string'},
        age:{type:'integer'}
    }
}
```
等价于
```
{
    name:{type:'string'},
    age:{type:'integer'}
}
```
