## 一：命名规范：

1.类(class),结构体(struct)命名：

访问级别：全级别通用(public、private、protected、static)
命名法则：帕斯卡
示例：GameObject、Vector3

备注：单词首字母全大写，要求单词能够准备表达结构体、类意思

2.变量(field)、属性(property))和委托(delegate)命名:

访问级别：private static
命名法则：驼峰式
示例： s_GameObject、s_OnClick

访问级别：private、protected
命名法则：驼峰式
示例：m_GameObject、m_OnClick

访问级别：public、public static、local（局部访问）
命名法则：驼峰式
示例：gameObject、onClick

备注：首字母小写，后续单词大写，私有静态前面添加“s_”,私有保护前添加“m_”

3.函数（function）命名:

访问级别：全级别通用(public、private、protected、static)
命名法则：帕斯卡
示例：Start()、Update()

备注：单词首字母全大写，要求单词能够准备表达函数意思

4.参数(parameter)命名:

访问级别：local（局部访问）
命名法则：驼峰式
示例：position、rotation

备注：首字母小写，后续单词大写

5.接口(interface)命名:

访问级别：public
命名法则：帕斯卡
示例：IReference

备注：单词首字母全大写,命名前添加大写“I”以明确接口类型

6.常量(const)命名:

访问级别：全级别通用(public、private、protected、static)
命名法则：其他
示例：LIST_MAX_COUNT

备注：全字母大写，每个单词之间以"_"分隔

7.补充命名规范：

命名空间（name space）：同1类(class)命名，示例：GameFrameWork

枚举项（enum items）：同6常量(const)命名，示例：MSG_DEFINED_LOGIN

抽象类（abstract class）： 抽象类命名使用Base开头，示例：BaseManager

测试类（test class）:测试示例以Test结尾，示例：MoveTest

派生类（derived class）：派生类要尽量明确基类功能实现，规范命名。示例：ProcedureChangeScene、LuaManager

## 二：代码规范：

1.避免频繁new object，尽可能缓存起来或者使用ReferencePool.Acquire<>(),减少垃圾回收频率。C#中所有的类都直接或间接继承自System.Object类，内存分配于托管堆中，由GC负责垃圾回收，GC是一种高耗性能的操作，同一帧中大量GC可能导致游戏卡顿，框架中已实现了引用池，所有继承IReference接口的类，可通过引用池重复利用，注意每次使用完后需手动回收引用对象。同时需要在Debugger的ReferencePool查看是否已经成功回收，避免使用不当引内存泄漏。

2.一个函数尽可能简短，函数中存在不同功能操作应多分几个函数实现。长篇大段的函数实现会使我们的视觉疲劳，撰写过长的代码使代码分析难度加大，修复bug和代码的维护成本也增大。代码行数尽可能少，过长的代码应思考代码是否能通过重构技巧缩减行数。

3.空Update()、Start()等生命周期函数需要删除，即使是空的也存在不必要的性能消耗。Unity在编译时候会根据Mono中相关函数名，将相关的函数指针添加到生命周期委托当中，从而导致该函数在生命周期当中被不必要的调用。

## 三：工具类代码规范：
工具类统一放置到Utils文件目录下，创建工具类脚本统一命名规范为Utils.XXX，XXX命名以模块功能划分，相同功能模块写在同一个脚本目录下，以partial关键字扩展Utils工具类。

示例：在Utils目录下新建一个Utils.Resource脚本， partial class Utils中创建公共静态类Resource,在然后内部实现LoadResource静态方法。

    public static partial class Utils
    {
        public static class Resource
        {
            public static void LoadResource(string path)
            {
                
            }
        }
    }
	
	## 四：资源规范：
	
	0.Prefab中组件命名：
	
		(1)文本(Text) 命名前缀：Txt_XXX 示例：Txt_Server
		
		(2)图片(Image) 命名前缀：Img_XXX 示例：Img_Player
		
		(3)原图(Raw Image) 命名前缀：Texture_XXX 示例：Texture_Bg
		
		(4)按钮(Button) 命名前缀：Btn_XXX 示例：Btn_Login
		
		(5)选框(Toggle) 命名前缀：Toggle_XXX 示例：Toggle_Server
		
		(6)滑动条(Slider) 命名前缀：Slider_XXX 示例：Slider_Progress
		
		(7)拖动条(Scrollbar) 命名前缀：Scrollbar_XXX 示例：Scrollbar_Sample
		
		(8)拖动框(ScrollView) 命名前缀：ScrollView_XXX 示例：ScrollView_Sample
		
		(9)复选框(Dropdown) 命名前缀：Dropdown_XXX 示例：Dropdown_Sample
		
		(10)输入框(InputField) 命名前缀：InputField_XXX 示例：InputField_Sample
	
	1.避免生成过于复杂的prefab，如果prefab中元素过多尝试切分prefab。Unity中prefab过于复杂在加载过程中容易阻塞主线程，导致短时间假死现象。应该尽量减小prefab的复杂度，个别能够复用的元素尽量复用。可尝试避免一口气加载全部元素，切分prefab分帧或切换状态后加载额外加载子类prefab。
	
	2.避免频繁Instantiate和Destroy对象，对于频繁实例化和销毁的对象应该使用ObjectPool将对象复用起来。当前场景状态切换时再回收全部对象，然后调用ReleaseAllUnused()方法释放全部对象。
	
	3.UI动静分离。动态UI会造成网格重构，网格重构也是一种高耗性能的操作，建议将UI以Canvas划分拆分为动态UI和静态UI，甚至复杂的UI可拆分为好几个Canvas。但是注意增加Canvas会导致Drawcall增加，虽然现在手机性能的提升几个Drawcall对于手机影响变小，但是尽量还是控制好Canvas不要超过10个。
	
	4.如果图集中存在较长UI元素，例如：16*1200的图片，需要将图片切割分为2张16*600图等，做到尽可能充分利用图集的空余空间。
	
	5.UI展示大图元素比较多无法打到一张1024*1024图集中，考虑将元素合并到一张背景大图中。例如：活动、充值界面，存在许多大图展示，可考虑让美术将大图元素整合到一张背景大图中。
	
	5.当打开多级界面，若界面被完全覆盖,可将被覆盖界面隐藏，防止OverDraw性能损耗，同时若仅仅存在UI界面无需渲染场景的状况，可关闭场景相机，减少性能损耗。
	
	6.不同的GPU对于处理像素块的速度是不同的，对于一些低端机型可适当考虑减少分辨率，减少像素块处理的量。
	
	7.血条、伤害显示等量大且变化频繁的UI，应当考虑使用SpriteRenderer，同时配合GPU Instancing和MaterialPropertyBlock对材质做操作合并dc。
	
	8.Animator设计应当简单明了，避免复杂的网状结构，尽量设计成可通过程序状态机能够快速切换的形式。
	
	9.常用颜色应该规范建立色值表，常用文字或公共UI图等应当通过色值表匹配当前适用的颜色。
	
	10.原则上UI图片资源应该关闭Mipmap、Read\Write Enable等选项，减少内存消耗。
	
	## 四：自检规范：
	
	1.代码自检：
	2.资源冗余自检：
	3.配置自检：
