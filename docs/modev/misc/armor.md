#添加盔甲
##简介
虽然添加盔甲要比添加普通的方块/物品要复杂一点，但是一旦你能理解它，它就变得很容易制作了。要添加盔甲，我们将首先创建一个自定义Material类，然后注册物品。
##创建一个盔甲Material类
由于新的盔甲需要设置一个新的名称（以及额外的东西，如护甲值和耐久度），我们将不得不创建一个新的自定义盔甲类。
这个类将实现ArmorMaterial，并且是枚举类型。它需要很多参数，主要是名称、耐久度等，所以现在我们只保留它为空。现在不要担心任何错误。

    public enum CustomArmorMaterial implements ArmorMaterial {
        CustomArmorMaterial() {
     
        }
    }
    
因为需要很多参数，这里有一个列表来解释其中的每一个。

1. 字符串名称。这将作为一种“盔甲标签”并在稍后使用。
2. 耐久度倍数。这将是用于根据基础值确定耐久度的数字。
3. 护甲值，或在原版代码中的“保护值”。这将是一个int数组。
4. 附魔等级。这就像是盔甲在附魔书中获得高等级附魔或附魔赠送的可能性。
5. 声音事件。原版盔甲使用的标准是`SoundEvents.ITEM_ARMOR_EQUIP_X`，X是盔甲的类型。
6. 韧性。这是第二个保护值，盔甲对攻击高时更耐用。
7. 修复材料。这将是一个`Supplier<Ingredient>`实例，而不是一个物品，这将在一段时间内完成。

有了这些参数，现在应该是这样的：

    public enum CustomArmorMaterial implements ArmorMaterial {
        CustomArmorMaterial(String name, int durabilityMultiplier, int[] armorValueArr, int enchantability, SoundEvent soundEvent, float toughness, Supplier<Ingredient> repairIngredient) {
     
        }
    }
    
我们还必须定义这些值并使其可用，因此现在将如下所示：

    public enum CustomArmorMaterial implements ArmorMaterial {
        private final String name;
        private final int durabilityMultiplier;
        private final int[] armorValues;
        private final int enchantability;
        private final SoundEvent equipSound;
        private final float toughness;
        private final Lazy<Ingredient> repairIngredient;
     
        CustomArmorMaterial(String name, int durabilityMultiplier, int[] armorValueArr, int enchantability, SoundEvent soundEvent, float toughness, Supplier<Ingredient> repairIngredient) {
            this.name = name;
            this.durabilityMultiplier = durabilityMultiplier;
            this.armorValues = armorValueArr;
            this.enchantability = enchantability;
            this.equipSound = soundEvent;
            this.toughness = toughness;
            this.repairIngredient = new Lazy(repairIngredient); // We'll need this to be a Lazy type for later.
        }
    }
    
`ArmorMaterial`也需要一些其他的方法，所以我们会很快添加到这里。

我们还需要添加基本的耐久度值，所以现在我们将使用原版值`[13，15，16，11]`

    public enum CustomArmorMaterial implements ArmorMaterial {
        private static final int[] baseDurability = {13, 15, 16, 11};
        private final String name;
        private final int durabilityMultiplier;
        private final int[] armorValues;
        private final int enchantability;
        private final SoundEvent equipSound;
        private final float toughness;
        private final Lazy<Ingredient> repairIngredient;
     
        CustomArmorMaterial(String name, int durabilityMultiplier, int[] armorValueArr, int enchantability, SoundEvent soundEvent, float toughness, Supplier<Ingredient> repairIngredient) {
            this.name = name;
            this.durabilityMultiplier = durabilityMultiplier;
            this.armorValues = armorValueArr;
            this.enchantability = enchantability;
            this.equipSound = soundEvent;
            this.toughness = toughness;
            this.repairIngredient = new Lazy(repairIngredient);
        }
     
        public int getDurability(EquipmentSlot equipmentSlot_1) {
            return BASE_DURABILITY[equipmentSlot_1.getEntitySlotId()] * this.durabilityMultiplier;
        }
     
        public int getProtectionAmount(EquipmentSlot equipmentSlot_1) {
            return this.protectionAmounts[equipmentSlot_1.getEntitySlotId()];
        }
     
        public int getEnchantability() {
            return this.enchantability;
        }
     
        public SoundEvent getEquipSound() {
            return this.equipSound;
        }
     
        public Ingredient getRepairIngredient() {
            // 我们需要做一个Lazy类型，这样我们就可以从Supplier那里得到真正的Ingredient。
            return this.repairIngredientSupplier.get();
        }
     
        @Environment(EnvType.CLIENT)
        public String getName() {
            return this.name;
        }
     
        public float getToughness() {
            return this.toughness;
        }
    }
    
现在你已经有了盔甲材料类的基础知识，你现在可以自己制作盔甲材料了。这可以在代码顶部完成，如下所示：

    public enum CustomArmorMaterial implements ArmorMaterial {
        WOOL("wool", 5, new int[]{1,3,2,1}, 15, SoundEvents.BLOCK_WOOL_PLACE, 0.0F, () -> {
            return Ingredient.ofItems(Items.WHITE_WOOL);
        });
        [...]
    }
    
随意更改值以适合你的盔甲。
##创建盔甲物品
回到主类，现在你可以像这样创建物品：

    public class ExampleMod implements ModInitializer {
        public static final Item WOOL_HELMET = new ArmorItem(CustomArmorMaterial.WOOL, EquipmentSlot.HEAD, (new Item.Settings().group(ItemGroup.COMBAT)));
        public static final Item WOOL_CHESTPLATE = new ArmorItem(CustomArmorMaterial.WOOL, EquipmentSlot.CHEST, (new Item.Settings().group(ItemGroup.COMBAT)));
        public static final Item WOOL_LEGGINGS = new ArmorItem(CustomArmorMaterial.WOOL, EquipmentSlot.LEGS, (new Item.Settings().group(ItemGroup.COMBAT)));
        public static final Item WOOL_BOOTS = new ArmorItem(CustomArmorMaterial.WOOL, EquipmentSlot.FEET, (new Item.Settings().group(ItemGroup.COMBAT)));
    }
    
##注册盔甲物品
像注册普通物品一样注册它们。

        [...]
        public void onInitialize() {
            Registry.register(Registry.ITEM,new Identifier("tutorial","wool_helmet"), WOOL_HELMET);
    	Registry.register(Registry.ITEM,new Identifier("tutorial","wool_chestplate"), WOOL_CHESTPLATE);
    	Registry.register(Registry.ITEM,new Identifier("tutorial","wool_leggings"), WOOL_LEGGINGS);
    	Registry.register(Registry.ITEM,new Identifier("tutorial","wool_boots"), WOOL_BOOTS);
        }
        
##材质
既然你已经知道如何制作物品模型和材质，我们就不在这里讨论了。（它们与物品完全相同。）盔甲材质做得有点不同，因为Minecraft认为这是一个原版盔甲物品。为此，我们将生成pack.mcmeta文件，以便我们的资源可以像资源包一样工作。

> src/main/resources/pack.pack.mcmeta  
> 1.14的pack_format为4  
> 1.15的pack_format为5

    {
        "pack":{
            "pack_format":4,
            "description":"Tutorial Mod"
        }
    }
    
现在你终于可以把你的材质放到`src/main/resources/assets/minecraft/textures/models/armor/`里了。请记住，这两张图片是分开的。（可以使用原版材质作为参考。）

如果你跟着一切做，你现在应该可以拥有一套完整的盔甲了！