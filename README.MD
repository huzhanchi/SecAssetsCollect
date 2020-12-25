## 整体介绍

在保护目标层面：如果说 命令通道 服务化是安全自动化的重要前提，而 资产 梳理是我们整个网络安全防护的前提以及最终的目标对象。
在情报层面：情报分为 对内情报，和对外情报。资产是对内情报重要的组成部分。

* 网络信息安全中资产包括哪些

![image](https://github.com/huzhanchi/SecAssetsCollect/blob/main/images/asset.png)

* 梳理资产的途径

1. 内部运维设施

来源于cmdb，包括：域名，ip，主机，操作系统，数据库。
来源于CI/CD流程：业务服务进程，镜像，包管理。

以上每个资产都会有特有属性，特别指出的是镜像以及业务进程包是要分析出其依赖的组件

2. 网络空间的自我测绘

业界shodan，zoomeye是对互联网全网做资产梳理，作为甲方就需要聚焦在自家的资产上，我们同样也可以通过网络空间测绘去收集 “运维cmdb”之外模糊地带的资产。
这一块主要是针对 “不符运维规范” 的应用层资产，从安全角度不能够完全依赖cmdb，一是cmdb的信息丰富度问题，另外是“不规范操作”在过去和未来将永远都可能发生。

web指纹扫描：常见的js组件，常见的中间件等等。

* 资产信息的有效使用

常规的 （资产主体名，版本）匹配就不说了，这里主要谈下“资产威胁评估” 我们可以深入做的一些事情。
在自己给业务方做威胁评估的时候 前端框架 的危险其实可以通过web指纹扫描进行深度排查，比如近期一业务方使用react，我们本能的觉得现代的mvvm框架都会对xss这类有很好的防护，但如果使用不当被入侵的可能性还是很大。

![image](https://github.com/huzhanchi/SecAssetsCollect/blob/main/images/1.png)

![image](https://github.com/huzhanchi/SecAssetsCollect/blob/main/images/2.png)

造成xss执行的原因是 state 没有直接从后端接口获取塞进model中，而是先渲染到html首页后通过
```
window.__INITIAL_STATE__ = ${JSON.stringify(initialData)};
```

这个案例中的两个特征：react框架，window.__INITIAL_STATE__ = ${JSON.stringify(initialData)}; 可以结合web指纹库深度检查。
