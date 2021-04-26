# Introduction



## java技术体系

Java不仅仅是一门编程语言，还是一个由一系列计算机软件和规范形成的技术体系，这个技术体系提供了完整的用于软件开发和跨平台部署的支持环境，并广泛应用于嵌入式系统、移动终端、企业服务器、大型机等各种场合。

### 组成

* 功能划分

    Sun官方所定义的Java技术体系根据各个组成部分的功能来进行划分，包括以下几个组成部分：

    1. Java程序设计语言
    2. 各种硬件平台上的Java虚拟机
    3. Class文件格式
    4. Java API类库
    5. 来自商业机构和开源社区的第三方Java类库

    JDK（Java Development Kit）是用于支持Java程序开发的最小环境。

    > 我们可以把Java程序设计语言、Java虚拟机、Java API类库这三部分统称为JDK。

    JRE（Java RuntimeEnvironment）是支持Java程序运行的标准环境。

    > 可以把Java API类库中的JavaSE API子集和Java虚拟机这两部分统称为JRE。

* 技术服务领域

    如果按照技术所服务的领域来划分，或者说按照Java技术关注的重点业务领域来划分，Java技术体系可以分为4个平台：

    1. Java Card

       支持一些Java小程序（Applets）运行在小内存设备（如智能卡）上的平台。

    2. Java ME（Micro Edition）

       支持Java程序运行在移动终端（手机、PDA）上的平台，对Java API有所精简，并加入了针对移动终端的支持，这个版本以前称为J2ME。

    3. Java SE（Standard Edition）

       支持面向桌面级应用（如Windows下的应用程序）的Java平台，提供了完整的Java核心API，这个版本以前称为J2SE。

    4. Java EE（Enterprise Edition）

       支持使用多层架构的企业应用（如ERP、CRM应用）的Java平台，除了提供Java SE API外，还对其做了大量的扩展并提供了相关的部署支持，这个版本以前称为J2EE。

Java开发技术在虚拟机层面隐藏了底层技术的复杂性以及机器与操作系统的差异性。Java虚拟机则在千差万别的物理机上建立了统一的运行平台，实现了在任意一台虚拟机上编译的程序都能在任何一台虚拟机上正常运行。