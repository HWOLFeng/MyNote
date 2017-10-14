# RESTful 简介
首先，了解什么是REST/RESTful
中文：具象状态传输 / （资源）表现层状态转化
> 维基百科
> https://zh.wikipedia.org/wiki/REST
```java
@RestController
public class GreetingController {
//确保HTTP request 的路径/greet 对应在greeting() 方法上 
//同时我们可以追加参数，来处理请求的post,delete,put,get
//当然我们也可以将@RequestMapping绑定在类上，表明这个类都是处理某路径上的请求
//@RequestMapping("/greet",method=GET)
@RequestMapping("/greet")
//@RequestParam绑定了request的name给greeting()方法的name，然后defalutValue设置默认绑定参数"World"
public Greeting greeting(@RequestParam(value="name", defaultValue="World") String name) {
        return new Greeting(counter.incrementAndGet(),
                            String.format(template, name));
    }
 }
```