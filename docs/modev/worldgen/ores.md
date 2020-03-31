#在世界中添加矿石
许多mod会添加自己的矿石，您需要一种将它们放置在现有生物群系中以便玩家寻找的方法。 在本教程中，我们将研究将矿石添加到现有生物群系以及其他mod添加的生物群系。 将矿石添加到生物群系需要执行2个步骤。

- 遍历生物群系注册表以将您的矿石添加到现有生物群系中。
- 使用RegistryEntryAddedCallback以确保您的矿石被添加到mod添加的任何生物群系中。

我们假设您此时已经创建了自己的矿石。 石英矿石将替代我们，我们的目标是在整个世界生物群系中生成它。 适当时用您的矿石替换对石英矿石的引用。

##在生物群系中添加矿石
首先，我们需要创建一种方法来处理生物群系，检查其是否为有效的生物群系，然后添加矿石。

    private void handleBiome(Biome biome) {
    	if(biome.getCategory() != Biome.Category.NETHER && biome.getCategory() != Biome.Category.THEEND) {
    		biome.addFeature(
            	    GenerationStep.Feature.UNDERGROUND_ORES,
            	    Feature.ORE.configure(
    			new OreFeatureConfig(
    			    OreFeatureConfig.Target.NATURAL_STONE,
    			    Blocks.NETHER_QUARTZ_ORE.getDefaultState(),
    			    8 // 矿脉大小
    		   )).createDecoratedFeature(
    			Decorator.COUNT_RANGE.configure(new RangeDecoratorConfig(
    			    8, // 每个区块的矿脉数
    			    0, // 底部偏移
    			    0, // 最低y高度
    			    64 // 最高y高度
    		))));
    	}
    }
    
此方法通过提供的生成设置将您的矿石添加到整个世界。 随意更改值以适合您的mod。

##迭代生物群系注册表
接下来，我们需要处理已注册的所有生物群系以及将来将要注册的所有生物群落（由其他模组添加）。 我们首先遍历当前注册表，然后注册一个侦听器，以供将来添加。

    @Override
    public void onInitialize() {
    	// 遍历现有生物群系
    	Registry.BIOME.forEach(this::handleBiome);
     
    	// 监听其它生物群系的注册
    	RegistryEntryAddedCallback.event(Registry.BIOME).register((i, identifier, biome) -> handleBiome(biome));
    }
    
##结论
您应该会在整个世界中看到石英矿石的生成：
![image.png](https://i.loli.net/2020/03/31/iFolfLa8Qwn9hm7.png)