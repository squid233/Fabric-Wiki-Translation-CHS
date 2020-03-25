#经常问的问题 (用户)
这个是一个精简版的常见问题。关于技术类问题请移步专家页面。
##通用
###Fabric支持哪些Minecraft版本？
18w43a快照版以及以上。（1.14正式版及以上）
   
Fabric在微调后可以在旧Minecraft版本上运行。详见专家页面。
###有什么Fabric整合包吗？
一些整合包目前正在通过MultiMC导出进行分发，例如[All of Fabric](https://github.com/AllOfFabric/AOF/releases)。不用担心启动包-它们通常是“概念的证明”，不是用于生产游戏。
###什么启动器支持Fabric（及其整合包）？
截至6月30日，以下启动器支持：
- MultiMC - 这里提供了一个指南；最新的开发版本还包括一个“Install Fabric”按钮。
- 原版 - Fabric的下载页上提供了一个安装程序。

**请记住** Fabric API是一个独立的组件，必须单独下载 - 您可以在这里找到它。

我们推荐使用MultiMC，因为它在模组环境中具有优越的用户体验。
###什么启动器支持分发Fabric整合包？
截至6月30日，以下启动器支持：
- MCUpdater - guide
- Technic (Solder) - guide
- ATLauncher - guide
- MultiMC/Vanilla - 您始终可以导出整合包或使用ZIP文件！

请注意，我们没有关于Twitch启动器支持的信息或ETA。
##兼容性
###Fabric可以和Bukkit/Spigot/Paper一起运行吗？
现在不行。 这种情况可能会在年底改变，但不太可能得到正式支持。
###Fabric可以和Forge一起运行吗？
- 在当前的Minecraft版本上，Fabric当前无法在Forge之上运行。
- 从理论上讲，有可能创建一种方法，也就是说，实现这一目标没有已知的主要技术障碍。
- 在这方面进行了一些实验和讨论（截至6月30日），但最终用户或mod开发人员无法使用。

开发团队不认为Forge互操作是一个高优先级的目标，因为我们在有限的时间内致力于该项目的重点是为Fabric社区及其开发人员和用户提供支持。
###Fabric可以和OptiFine一起运行吗？
没有正式版。 但是，有非官方的mod来促进在Fabric上启动适用于1.14+的OptiFine。 但您的里程可能会有所不同。
###哦，不！我试过在OptiFine上启用光影，我的世界看起来很奇怪！
Fabric的渲染补丁虽然尽可能地具有最低的侵入性，但对原版渲染系统内部使用的数据格式做了一些假设。光影打破了这个假设，所以事情出了岔子。解决方案确实存在，并由上述非官方mods的最新版本实现。

**不要** 将Fabric API降级作为解决方法。这是个坏主意。
###Fabric可以和Sponge一起运行吗？
现在不行。 Sponge尚未为运行Fabric的Minecraft版本1.14提供现成的API或有效的实现。