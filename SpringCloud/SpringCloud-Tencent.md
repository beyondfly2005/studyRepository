## Spring Cloud Tencent


> https://baijiahao.baidu.com/s?id=1736312832828976725
> https://blog.csdn.net/qq_24950043/article/details/125581835
> 
> 
#### 北极星
注册中心、配置中心、路由、服务限流、熔断降级实际上都是通过Polaris服务来完成的，也就是说比起spring cloud alibaba，我们不需要再单独安装nacos,sentinel等服务，只需要安装一个Polaris即可

基础组件

- 注册中心 Spring Cloud Tencent Discovery

- 配置中心 Spring Cloud Tencent Config

- 网关/路由 Spring Cloud Tencent Router

- 服务限流 Spring Cloud Tencent Rate Limit

- 服务熔断 Spring Cloud Tencent CircuitBreaker
- 负载均衡 Spring cloud Tencent Loadbalancer
- 组间调用 Feign或RestTemplate，这点与spring cloud一致




##### 服务注册 发现与治理中心-Spring Cloud Tencent Discovery


> 

#### 服务限流—— Spring Cloud Tencent Rate Limit
