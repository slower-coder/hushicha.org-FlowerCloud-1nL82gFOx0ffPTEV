
Graphql是什么？先来一段AI给的回答：
GraphQL是一种为API设计的查询语言，与REST相比，它提供了更高效、强大和灵活的方法来与数据交互。GraphQL由Facebook于2012年开发，并于2015年开源。其主要的优势在于能够允许客户端精确地指定他们需要的数据，从而避免了过度获取或数据不足的问题。
**主要特性**
1. 精确获取需要的数据：

2. 单一端点：

3. 类型系统：

4. 查询与修改：

5. 实时数据（Subscription）：

**优势和局限**
优势：

* 减少数据传输：只返回客户端请求的数据。
* 减少请求数：多个数据需求可以在单一查询中解决。
* 灵活性高：客户端可以自由构造查询，无需服务器频繁更新API。



局限：

* 复杂查询性能问题：如果不加限制地进行深度查询或大规模的数据嵌套，可能会对服务器性能造成影响。
* 缓存策略：相比于REST的URL级别缓存，GraphQL需要更复杂的缓存策略来优化性能。
* 学习曲线：对于开发者来说，需要学习新的查询语法及其底层实现。



 
其他内容就不过多介绍了，大家感兴趣可以自行去搜索有关理论或说明。接下来我直接提供实战入门演示。
 
以下开始正式演示正文：
先创建一个webapi项目作为服务端和一个控制台项目作为客户端，用来测试使用。以及对应的引用包，如下图所示：
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125039946-562170969.png)
 
新建Quries文件夹，用来存放查询使用的类和方法。以及新增一个测试用的类和string类型返回值的方法 Hello()
 
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040030-1879945922.png)
在启动项或Program里面，添加Graphql服务，并添加Query的类型注册：
 
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040278-1315082896.png)
 
最后还要记得映射端点：
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040391-1731526555.png)
然后运行程序，例如我默认运行起来端口是5264，则打开url（根据自己情况更改url地址）：[http://localhost:5264/graphql/](https://github.com)
然后输入查询语句：query:{hello}就可以查出对应的返回内容。
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040595-924841858.png)
客户端里面，创建graphql的客户端请求，并输入查询的方法为hello的query语句，以及输出的结果，如下图所示。结果和上面的一样，只是我只输出data里面的数据，data里面的数据就是我们需要的结果。
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040569-1212029463.png)
 
接着做个拓展演示，创建一个嵌套实体类，用来模拟多种情况：
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040152-1206913786.png)
 
创建一个测试使用的服务，模拟具体查询业务使用。
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040358-731169711.png)
 
注册服务和接口以后，运行程序，并在graphql里面进行运行测试。当前测试的是输出所有字段。
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040609-1834575144.png)
 
现在，例如我把子集合去掉不要，那查询出来也就不会带有子集合的任何内容：
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040328-1588821654.png)
 
或者只需要指定的其他字段，删掉了描述、子集合的城市字段：
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040573-172940860.png)
 
同样的，把查询语句丢到客户端程序里面进行查询，也可以查出指定字段的内容：
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040633-1381823204.png)
 
上面演示的是查询效果，也可以做增删改等其他操作。
在测试服务类新增一个业务操作，模拟接收到参数以后进行了业务操作，最终返回一个代表成功的数据。例如：
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040090-1031785025.png)
 
新建一个Mutations文件夹，用来存放增删改操作的类等。例如此处的测试使用的TestMutation.然后创建一个模拟传入参数进行操作的方法，该方法返回上面服务类里面的测试方法。
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125039975-402575664.png)
 
需要添加对修改有关操作的注册：
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040504-665591731.png)
 
然后启动，做个测试。使用mutation语句进行操作，操作指定方法，方法里面指定参数和字段数据。可以看到服务端进入了前面预设的业务方法内，并且返回的true被客户端成功接收。
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040635-2071358298.png)
 
在控制台客户端，也执行一下mutation操作，也能够成功调用：
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040633-1669515200.png)
 
以上是查询和修改操作的例子，graphql还可以做数据推送和订阅，用于实现websocket的效果。
新建一个subscriptions文件夹，用来存放所有的消息推送和订阅有关的定义类。例如TestSub,里面定义了一个推送方法OnTestPublish
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040421-649059192.png)
 
在前面的测试服务里面，新增ITopicEventSender事件接口的注入，以及新增一个方法，用来触发推送功能。并且推送的主题，使用刚才定义的OnTestPublish
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040546-279606840.png)
 
然后需要提供对推送服务的注册，以及持久化选择。
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040383-115551943.png)
使用默认的持久化，该持久化选择不建议上生产。具体原因，我去AI一下：
1. 可扩展性问题：AddInMemorySubscriptions 存储订阅信息是在内存中进行的。这意味着订阅数据仅存在于单个进程中。如果你的应用程序需要在多个服务器实例之间进行扩展，每个实例的内存中都会有独立的订阅状态，从而导致状态不一致。因此，在大型应用或高负载环境中，这种方法不能很好地扩展。
2. 持久性缺失：使用内存存储的另一个主要问题是数据的持久性。服务器重启或发生故障时，所有在内存中的订阅数据将丢失。这对于生产环境来说是不可接受的，因为需要保证服务的稳定性和数据的持久性。
3. 资源使用效率：随着订阅数量的增加，内存的使用量也会随之上升。在内存资源有限的环境中，这可能会影响应用程序的整体性能和响应速度。
4. 故障恢复：在内存中的订阅管理缺乏有效的故障恢复机制。如果系统崩溃或需要进行维护，恢复订阅状态将非常困难，可能需要从客户端重新建立订阅。

为了解决这些问题，生产环境中通常建议使用持久化和可扩展的订阅存储方案，比如基于 Redis 的 AddRedisSubscriptions 方法。大佬们感兴趣可以自己去拓展下。
 
现在缺少一个触发条件，由于咱们创建的是webapi项目，自带控制器，那我把控制器做个改造，通过swagger来调用进行触发数据推送，直接在请求里面，调用推送方法：
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040370-675123501.png)
 
最后，由于推送使用了websocket，所以也需要添加对websocket的注册：
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040381-2066407061.png)
 
然后启动程序，使用subscription进行订阅onTestPublish主题消息。运行以后，会一直监听，除非我们取消监听。
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040601-468108989.png)
 
打开swagger，直接调用并测试，可以看到面板接收到了测试推送的数据。
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040642-1804929209.png)
 
客户端要实现订阅，需要做一些改动。订阅的事件是字符串类型，所以需要创建一个字符串类型的属性，用来接收数据：
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040409-1951089640.png)
 
然后客户端创建时候，需要使用websocket端点。然后再创建订阅语句
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040632-625245454.png)
 
接下来是订阅的具体实现演示：
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040601-2017448084.png)
 
允许，并通过swagger调用两次测试，都可以被监听到。
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040664-270961278.png)
 
同时，之前打开的graphql演示面板，也可以看到能够收到后续消息，说明支持多客户端接收，符合websocket的推送效果。
![0](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017125040550-1406350421.png)
 
有关实现的核心代码。服务端注册有关：



```
 // 添加GraphQL服务
 builder.Services
     .AddGraphQLServer()
     .AddQueryType()
     .AddMutationType()
     .AddSubscriptionType()
     .AddInMemorySubscriptions(); // 默认消息持久化(生产情况建议更换)

 var app = builder.Build();

 app.UseWebSockets();

 // 映射GraphQL端点
 app.MapGraphQL();
```


 



 
客户端实现：



```
 var option = new GraphQLHttpClientOptions
 {
     EndPoint = new Uri("http://localhost:5264/graphql"),
     // 设置 WebSocket 端点以支持订阅
     WebSocketEndPoint = new Uri("ws://localhost:5264/graphql")
 };

 using var client = new GraphQLHttpClient(/*"http://localhost:5264/graphql"*/ option , new NewtonsoftJsonSerializer());
 //            var request = new GraphQLRequest
 //            {
 //                Query = @"mutation{
 //  otherOperation(info:{address:""龙岗区宝龙街道"",city:""大大大深圳"",phone:""10100011""})
 //}"
 //            };

 //            var response = await client.SendQueryAsync(request);
 //            Console.WriteLine(response.Data);

 // 定义订阅请求
 var subscriptionRequest = new GraphQLRequest
 {
     Query = @"
     subscription {
         onTestPublish
     }"
 };

 // 创建订阅流
 var subscriptionStream = client.CreateSubscriptionStream(subscriptionRequest);

 // 订阅消息流
 var subscription = subscriptionStream.Subscribe(
     response =>
     {
         if (response.Errors != null)
         {
             Console.WriteLine("Error occurred: " + response.Errors);
         }
         else
         {
             Console.WriteLine($"Received message: {response.Data.OnTestPublish}");
         }
     },
     error => Console.WriteLine($"Subscription error: {error.Message}"),
     () => Console.WriteLine("Subscription completed."));

 // 模拟其他逻辑（例如，在某个时刻取消订阅,这儿通过输入任意按键触发取消和释放）
 Console.WriteLine("Press any key to exit...");
 Console.ReadKey();

 // 取消订阅并关闭 WebSocket 连接
 subscription.Dispose();
 client.Dispose();
```


 



 如果需要我本地演示的代码项目，可以在本人公众号【**Dotnet Dancer**】回复: **代码演示**  即可获取开源项目地址。如果需要其他咨询或合作，可V：WeskyNet001
![](https://img2024.cnblogs.com/blog/1995789/202410/1995789-20241017124812185-190663260.jpg)
 


**如果以上内容对你有帮助，欢迎大佬们点赞、关注公众号或转发。谢谢大家！**


 



 
 本博客参考[楚门加速器](https://shexiangshi.org)。转载请注明出处！
