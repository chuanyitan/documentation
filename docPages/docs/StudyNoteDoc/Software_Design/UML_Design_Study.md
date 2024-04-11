# UML usage study

This page will use the plantUML as design tool to study the UML study.
The [plantUML documentation](https://pdf.plantuml.net/PlantUML_Language_Reference_Guide_zh.pdf) is [platUML](https://www.plantuml.com/plantuml/uml/SyfFKj2rKt3CoKnELR1Io4ZDoSa70000)

UML图分为结构图，行为图和交互图:
* 结构图分为类图、轮廓图、组件图、组合结构图、对象图、部署图、包图。
* 行为图又分活动图、用例图、状态机图和交互图。
* 交互图又分为序列图、时序图、通讯图、交互概览图。

最近看到的时序图，包图，组件图，流程图等等。
https://zhuanlan.zhihu.com/p/520475069  中有列举。



1.时序图
详细的时序图的使用实例，参考  https://blog.csdn.net/youngyouth/article/details/88714047

下面两个链接，详细的介绍了时序图中会用到的组件，以及组合片段的字段的详细说明及使用，非常有参考价值
* https://zhuanlan.zhihu.com/p/681732997
* https://zhuanlan.zhihu.com/p/422509874

下面是会用到组合消息的关键字
```bash
alt/else ，类似于 if / else
opt 选择，
loop 循环
par
break 跳出循环
critical
group 组
```