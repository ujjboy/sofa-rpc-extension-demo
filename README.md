# 为什么要扩展

SOFARPC 是一个可扩展性较高的 RPC 框架，当官方实现或者第三方实现没有满足自身需求的时候，进行扩展。

我们强烈推荐您将您的各种扩展贡献给 SOFARPC 官方。

但是如果由于组件的特殊性（例如内部组件）、或者编译级别（例如要求JDK8以上）不同，我们建议您开启一个工程单独存放逻辑，就像本例一样。
 
 
# 如何扩展 SOFARPC

本文将通过一个扩展一个本地的注册中心的实现作为例子，简单介绍下 SOFARPC 扩展能力的步骤。

## 步骤一： 编写实现

我们新建一个 `org.howtimeflies.sofa.rpc.registry.MyRegistry`，继承自一个可扩展接口或者抽象类 `com.alipay.sofa.rpc.registry.Registry`，具体实现不多介绍。

注意，在类名上增加如下注解，指定别名

```java
package org.howtimeflies.sofa.rpc.registry;
@Extension("myRegistry")
public class MyRegistry extends com.alipay.sofa.rpc.registry.Registry {
}
```

## 步骤二：编写扩展配置

新建：`src/main/resources/META-INF/services/sofa-rpc/com.alipay.sofa.rpc.registry.Registry`
内容为：
```
myRegistry=org.howtimeflies.sofa.rpc.registry.MyRegistry
```

## 步骤三：测试

可以打包测试，也可以直接写测试用例测试。例如 `org.howtimeflies.sofa.rpc.MyRegistryTest`里的。

```java
public class MyRegistryTest {
    public static void main(String[] args) {
        // 指定自己的注册中心
        RegistryConfig registryConfig = new RegistryConfig()
                .setProtocol("myRegistry");
        
    }
}
```