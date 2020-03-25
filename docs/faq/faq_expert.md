#常见问题（专家）
这是常见问题（用户）的“专家/技术”扩展。
##互操作性
###对Bukkit/Spigot/Paper支持方面有什么进展？
在Loader的0.5.0开发分支中，有一些实验性的功能可以在Paper上运行Fabric Loader，但是有许多关于Bukkit API缺陷的注意事项（例如在枚举中硬编码方块和物品的列表，使得很难支持修改方块或物品）。这可能永远不会得到官方的支持，应该更多地将其视为一种好奇或特例场景。
###能让Fabric和Sponge一起用吗？
这里最好的方法是将SpongeCommon和SpongeVanilla的修改版本移植为实现Sponge API的Fabric mod。由于Fabric在其核心使用了一个SpongePowered的Mixin叉，这变得有些简单了 - 但是可以这么说，映射的差异弥补了这一点。
###为什么Fabric API会破坏OptiFine光影？
渲染修补程序Fabric API使用Indigo假设（出于性能和代码简单性的原因）原版vertex格式保持不变。Mods通常不会改变它，但是ShadersMod和类似的Mods是一个常见的例外。因此，Indigo不能很好地照原样发挥。
##互操作性（复古）
###Fabric的实际运行版本是什么？
理论上，没有什么能阻止你在任何版本的Minecraft上运行Fabric的mod loader，在任何混淆层下，一直运行到c0.0.11a。但是，这些版本中的大多数都不存在Yarn映射-因此，使mods变得…有点复杂。
###Fabric可以与mods一起运行在旧的Minecraft版本吗？
可以！一般来说，所有的JAR模组（比如老版本的OptiFine，或者Better Than Wolves）都应该工作得很好，并允许同样程度的可修改性，你会得到一个未经修改的版本的Minecraft。不过，还有一些特别的注意事项：

对于大多数1.13之前版本的mod加载器，必须通过添加JVM参数-Dfabric.Loader.useCompatibilityClassLoader=true在Fabric加载器中启用兼容模式。

关于运行Minecraft Forge：

- 1.6~1.12.2：虽然Fabric过去运行在LaunchWrapper的顶部，但该功能目前尚未维护，需要重新设计。
- 1.3.1~1.5.2：目前没有计划。
- 1.2.5及以下：功能性，就像ModLoader。

关于运行ModLoader：没有已知的问题。

##哲学
###为什么要创建自己的映射，而不是使用MCP或Spigot的现有映射？
关于Mod Coder Pack，MCP：

- MCP的映射在旧的MCP许可证（[保存在Minecraft Gamepedia上](https://minecraft.gamepedia.com/Programs_and_editors/Mod_Coder_Pack#License_and_terms_of_use)）和新的MCPConfig存储库之间没有明确的许可条款。相反，Yarn是[CC0授权](https://github.com/FabricMC/yarn/blob/1.14.4/LICENSE) 并可根据需要自由使用、修改和重新分发。
    - 而其他项目如OptiFine和Sponge利用MCP，重新分配MCP映射 - 未经明确许可禁止 - 必须支持SRG混淆的mods，这[只能由Forge完成](https://github.com/MinecraftForge/MinecraftForge/blob/1.14.x/LICENSE.txt#L32-L35)。
    - 即使我们获得了许可，我们也不太可能自由地将此许可扩展到其他开发人员。这将违背Fabric发展自由和透明的核心价值观。
- MCP的映射并不总是更新到每个非“release”的Minecraft版本，也不允许其他人自己更新。这将禁止Fabric更新到快照或其他实验版本。
    - 此外，MCP的更新工具链 - 最近几年 - 依赖于私有的Mojang信息，普通用户无法访问，以及没有明确许可条款的软件。相反，Fabric的更新工具链仅仅依赖于自由软件工具（Stitch、Matcher、Tiny-Remapper、Enigma）和过程的工件 - 就像映射一样 - 是公开可用的。
- MCP的映射有一个更新过程，我们认为这个过程对代码评审不够开放，我们认为，在合并请求系统上使用[IRC bot提交](http://mcpbot.bspk.rs/)。（不过，这在很大程度上是一个偏好问题。）

关于Spigot：

- Spigot的映射只覆盖服务端，而且非常不完整，
- Spigot的映射与MCP有类似的许可问题。