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
...
