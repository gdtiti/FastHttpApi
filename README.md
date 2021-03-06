### 简介
FastHttpApi是在Beetlex基础上实现的轻量级和高性能的HTTP服务组件，组件支持http和WebSocket协议。只需要配置证书文件即可让服务支持https的安全性访问。 通过测试组件在GET/POST这些数据交互的场景下性能优胜于Asp.net core！**[详情查看官网](http://www.ikende.com/)**.

**[master/PerformanceTest](https://github.com/IKende/FastHttpApi/tree/master/PerformanceTest)**
![](https://i.imgur.com/lyvQIhu.png)

### 安装包

```
Install-Package BeetleX.FastHttpApi -Version 0.9.9.7
```
### 配置 Server GC
`<ServerGarbageCollection>true</ServerGarbageCollection>`

```
    [Controller]
    class Program
    {
        private static BeetleX.FastHttpApi.HttpApiServer mApiServer;
        static void Main(string[] args)
        {
            mApiServer = new BeetleX.FastHttpApi.HttpApiServer();
            mApiServer.Debug();
            mApiServer.Register(typeof(Program).Assembly);
            mApiServer.Open();
            Console.Write(mApiServer.BaseServer);
            Console.Read();
        }
        // Get /hello?name=henry 
        // or
        // Get /hello/henry
        [RouteTemplate("{name}")]
        public object Hello(string name)
        {
            return $"hello {name} {DateTime.Now}";
        }
        // Get /GetTime  
        public object GetTime()
        {
            return DateTime.Now;
        }
        // Post /PostStream
        // name=aaa&value=bbb
        [Post]
        [NoDataConvert]
        public object PostStream(IHttpContext context)
        {
            Console.WriteLine(context.Data);
            string value = context.Request.Stream.ReadString(context.Request.Length);
            return value;
        }
        // Post /Post
        // {"name":"henry","value":"bbbb"}
        [Post]
        public object Post(string name, string value, IHttpContext context)
        {
            Console.WriteLine(context.Data);
            return $"{name}={value}";
        }
        
        // Post /PostForm
        // name=aaa&value=bbb
        [Post]
        [FormUrlDataConvert]
        public object PostForm(string name, string value, IHttpContext context)
        {
            Console.WriteLine(context.Data);
            return $"{name}={value}";
        }
    }
```

