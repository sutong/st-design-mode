包结构说明
	**.nouse 未使用模式的代码
	**.rewrite 使用模式的代码
	**.template 模式的示例代码
	**.template.extend 模式扩展的示例代码

简单工厂
	问题
		客户端只知接口，而不知实现
	定义
		提供一个创建对象实例的功能，而无需关心其具体实现
	优点
		帮助封装
			实现组件封装，让组件外部能真正面向接口编程
		解耦
			实现客户端和具体实现类的解耦
			客户端根本就不知道具体由谁来实现，也不知道具体是如何实现的
			客户端只通过工厂获取它需要的接口对象
	缺点
		增加客户端复杂度
			如果通过客户端的参数来选择具体的实现类，那么客户端必须理解各个参数代表的具体含义，
			增加了使用难度，也部分暴露了内部实现，
			可以通过选用可配置的方式实现
		不方便扩展子工厂
			私有化构造方法，使用静态方法创建接口，就不能通过子类来改变创建接口的方法的行为
	本质
		在于选择
		为客户端选择相应的实现，使得客户端与实现类之间解耦。
		
	使用
		完全封装隔离具体实现，让外部通过接口操作封装体
		把对外创建对象责任集中管理和控制

外观模式(Facade)
	场景
		组装电脑
			DIY
				自己电子市场购买配件，自己组装，但需要对各配件比较熟悉
			找装机公司
				找一家专业的装机公司，提出自己的要求，等待拿电脑就可以
		代码生成
			根据配置信息，控制表现层、逻辑层、数据层3层代码的生成与否		
	问题
		为了生成代码，客户端需要与系统的多个模块交互	
	定义
		为子系统中的一组接口提供一个一致的访问界面
	目的
		不是给系统增加新的功能接口，而是减少客户端与系统内多模块的交互，松散耦合，让客户端更简单的使用系统。
		包装已有的功能，主要负责组合已有功能实现客户需求，而不是添加新的实现。
	优点
		松散耦合
			松散了客户端与系统的耦合关系，让系统内部的模块能更容易扩展和维护。
		简单易用
			客户端不需要了解系统内部的实现，也不需要跟系统内部的模块进行交互，只需跟外观交互就可以了。
		更好地划分访问的层次
			有些方法是对系统外的，有些方法是系统内部使用的。把需要暴露给外部的功能集中到外观中，这样既方便客户端调用，同时也隐藏了内部的细节
	缺点
		过多的或不合理的Facade让人困惑，到底是调用Facade好呢，还是直接调用模块好。
	本质
		封装交互，简化调用
	使用
		为一个复杂的系统提供一个简单接口的时候
		让客户端和抽象类的实现部分松散耦合
		构建多层结构的系统

适配器模式(Adapter)
	场景
		装配电脑
			在旧的电脑上，添加新的硬盘（并口），旧电脑的电源只支持串口的，通过转接线解决电源与硬盘接口不匹配问题
		同时支持数据库和文件的日志管理
			第一版用户要求日志以文件形式记录（读、写）。
			第二版用户要求日志以数据库形式记录（增、删、改、查），同时支持数据库和文件的储存形式。
	问题
		两个版本的操作接口不一样，导致客户端在使用第二版的时候，无法以同样的方式直接使用第一版的实现。
		修改源码，可能会导致其他依赖这些实现的应用无法正常工作，或者根本拿不到源码（两个版本的开发功能可能不一样）
	定义
		将一个类的接口转化为客户希望的另一类接口
	目的
		复用已有的功能，而不是实现新的接口
	优点
		更好的复用性
			让功能（已有的，只是接口不兼容）得到更好的复用
		更好的扩展性
			调用自己开发的功能，自然扩展系统的功能
	缺点
		过多使用，让系统凌乱
			明明看到调用是A接口，其实内部被适配成了B接口来实现
	本质
		转换匹配，复用功能
	使用
		使用一个已经存在的类，但它的接口不符合你的需求
		创建一个可以复用的类，可能和一些不兼容的类一起工作

单例模式(Singleton)
	场景
		读取配置文件的内容	
	问题
		通过new创建对象，使得在系统运行过程中存在多个对象，造成内存资源的浪费（特别是配置内容较多的情况下）
		如果保证在系统运行期间，一个类只有一个实例
	定义
		保证一个类仅有一个实例，并提供一个访问它的全局访问点
	实现
		包括懒汉式和饿汉式
		懒汉
			体现了延迟加载和缓存思想
		饿汉
			利用了static的特性，是线程安全
	扩展
		lazy initialization holder class
			使用了java的类级内部类和多线程缺省同步锁，实现了延迟加载和线程安全
		单元素的枚举
			代码简洁，提供序列化机制，并由JVM提供保障，绝对防止多次实例化，更简洁、高效、安全
	目的
		保证这个类在运行期间只会被创建一个类实例，并提供唯一一个全局的访问点
	优缺点
		空间和时间
			懒汉式是时间换空间，每次获取实例都要判断是否需要创建实例，浪费时间。但如果一直没人使用，就不会创建实例，节约内存空间。
			饿汉式是空间换时间，类装载的时候就会创建类实例，不管用不用，但调用时候，就不需判断，节省运行时间。
		线程安全
			不加同步的懒汉式是线程不安全的
			饿汉式是线程安全的，在装载类的时候不会发生并发
	本质
		控制实例数目
	使用
		需要控制类的实例只能有一个，并且客户端只能从一个全局访问点访问