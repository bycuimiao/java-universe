##前言
        最近了解了一下设计模式，起初看的是《大话设计模式》，这本书是用C#语言写的，觉着挺有意思，其实很多模式我们都已经在用了，却不知道这就是设计模式。所以后来买了本GOF的《设计模式——可复用面向对象软件的基础》打算好好钻研下。这本书是设计模式的鼻祖，相当权威，书名中说的是“可复用面向对象软件的基础”，这是基础，我对此表示比较震撼。用了三年的面向对象语言居然不了解设计模式，不知道这是基础，看来也是白活了。我暂时了解到的在此基础上还有重构等等很多比较高级概念。但这本书显然超出了我的理解范围，首先他用的语言是C++，我之前没接触过。还有就是这本书仅有248页，可见语言之简练，很多地方写的特别晦涩难懂，没有坚实的基础的人还是不推荐这本书了。为了继续深入的学习，我又买了一本阎宏的《Java与模式》，共1024页，讲述的可谓详尽。这本书不仅对GOF的23种设计模式做了比较详细的讲解还讲了几个之后出现的设计模式。但对于一些复杂的模式我还是不能很好的理解，或是对一些比较简单模式也只是知道结构，却不知道如何应用。总结下来，设计模式对我暂时的工作毫无帮助。但我却对面对对象有了更加深入的认识，比如之前我一直使用接口，却不知道为什么，我现在仍然在使用，但我知道我为什么这么做了，之前是无脑的模仿，但在我了解了设计模式之后，我学会了思考。这种提高我觉着是我最大的收获。

我觉着设计模式光凭死读书是学不会的，实践中总结，再实践，再总结，终有一天我相信我会掌握设计这门艺术，而现在，我需要做的是把理论基础打好，为了将来更好的实践它。

该笔记是编者读过《大话设计模式》、《Java与模式》和部分《设计模式——可复用面向对象软件的基础》之后总结出来的初学者笔记，里面大部分摘自书记中的关键性语句，也有部分自己浅显的见解，不足之处，还请见谅。

##鸣谢
特别鸣谢资深程序员盖超为本笔记提出宝贵意见，完成了审校工作，确保了本笔记的正确性，并为不足和缺陷的地方添加了批注。



###1、我们为什么需要设计模式？
	每一个模式描述了一个在我们周围不断重复发生的问题，以及该问题的解决方案的核心，这样，你就能一次又一次地使用该方案而不必做重复的劳动。[GOF]
	上面这句话指的是城市和建筑模式，但他的思想也同样适用于面向对象的设计模式。所以架构师的英文和建筑师是一样的——Architect。我对上面这句话的理解是这样的：设计模式就是为了达到代码复用的目的。（批注：模式是代码复用的一种方式。具体问题具体分析，需要根据情况选择对应的模式。比如回收站要用单例模式实现。具体规则的定义可能会用到模板方法。这些需要依照具体是情况来定。）
	软件并不是做好了就永远不变的，它需要随时间而增加新的功能，或改变功能，所以需要设计模式来使改动最小却能实现我们新的功能。所以我们所有的设计模式都是为了更好的复用之前的代码，在最小程度改变原有代码的情况下把新的功能加进去，或把某些需要的功能改变。这就是设计模式的初衷。

 

###2、设计模式基于哪些设计原则？
	设计模式是基于一定的原则的，这些原则就是设计原则。所有设计模式都在基于这些设计原则的基础上去实现代码复用。所以我认为设计原则是设计模式的基础。（批注：同时也是设计的目的）

 

####1）开-闭原则（Open-Closed Principle 或 OCP）：一个软件实体应当对扩展开放，对修改关闭。[Java与模式]
	对于OCP通俗的解释就是：软件实体可以被扩展，但不可以被修改。（批注：这个原则非常重要，是设计的核心所在。）
	每个软件推出后都会不定时的进行更新，增加新的功能，若每次增加新的功能时都要大量的修改之前的代码的话很有可能讲之前好用的功能改坏，所以好的设计就是遵循开闭原则，每次增加新功能时只是增加新的类，尽可能少的去修改之前的代码。
	实现开闭原则抽象化是其中的关键。[Java与模式]
	我认为，开闭原则是总的原则，之后所有的设计原则都是为了更好的实现开闭原则的。因为OCP正是设计模式所追求的达到更好的代码复用。在尽量少的修改原有的代码的基础上，可以增加新的功能、新的模块。

 

####2）单一职责原则（Single Responsibility Principle 或 SRP）：就一个类而言，应该仅有一个引起它变化的原因。[大话设计模式]（批注：我的理解是面向对象后，能高度抽象        的尽量抽象出来，这样才符合自然。）
	我对于SRP的理解就是，一个类只负责一个功能，这样可以使程序更利于维护。但“一个功能”我认为是一个模糊的概念，“登陆”是一个功能，“登陆”里面的“输入密码”又是一个功能，输入密码之后的“密码加密”又是一个功能，但怎么样才算单一功能呢？这大概需要程序员自己去把握这个度。这个度，我认为就是程序的“粒度”。这里涉及到一个专业的术语——“粒度”。对于粒度暂且这样理解，因为我并没有找到相关资料。

 

####3）对可变性的封装原则（Principle of Encapsulation of Variation 或者 EVP）：找到一个系统的可变因素，将之封装起来。[Java与模式]
	[GOF]中对于EVP的解释：考虑你的设计中什么可能会发生变化。与通常将焦点放到什么会导致设计改变的思考方式正好相反，这一思路考虑的不是什么会导致设计改变，而是考虑你允许什么发生变化而不让这一变化导致重新设计。
	该原则意味着两点[Java与模式]：
	    a、一种可变性不应当散落在代码的很多角落里，而应当被封装到一个对象里。
		b、一种可变性不应当与另一种可变性混合在一起。

 

####4）里氏代换原则（Liskov Substitution Principle 或者 LSP）：任何基类可以出现的地方，子类一定可以出现。[Java与模式] 注：基类就是父类（编者注）
	[大话设计模式]中的解释：一个软件实体如果使用的是一个父类的话，那么一定适用于其子类，而且它察觉不出父类对象和子类对象的区别。也就是说，在软件里面，把父类都替换成他的子类，程序的行为没有变化。简单的说，就是子类型必须能够替换掉它们的父类型。
	通俗的来讲就是：子类可以扩展父类的功能，但不能改变父类原有的功能。（批注：继承重写的过程是一种对父类或接口标准重新实现的过程，这个过程中必须要按照约定好的标准来做。父类的定义就是标准的定义。）
	它包含以下4层含义：
		子类可以实现父类的抽象方法，但不能覆盖父类的非抽象方法。
		子类中可以增加自己特有的方法。
		当子类的方法重载父类的方法时，方法的前置条件（即方法的形参）要比父类方法的输入参数更宽松。（编者注：这里宽松指的是类型）
		当子类的方法实现父类的抽象方法时，方法的后置条件（即方法的返回值）要比父类更严格。
#####那么LSP如何去实现尽量少的更改原有代码而增加新的功能的呢？
	[大话设计模式]是这样解释的：只有当子类可以替换掉父类，软件单元的功能不受影响时，父类才被真正的复用，而子类也能够在父类的基础上增加新的行为。由于这种可替换性才使得父类类型的模块在无需修改的情况下就可以扩展。
####5）依赖倒转原则（Dependency Inversion Principle 或者 DIP）：
	a、 抽象不应当依赖于细节，细节应当依赖于抽象[Java与模式]
	b、 要针对接口编程，不要针对实现编程[GOF]
	c、 高层模块不应该依赖底层模块。两个都应该依赖抽象[大话设计模式]
我对DIP的理解是这样的：比如声明一个加密接口Encryption，但它的实现类是MD5加密的EncryptionMD5Impl，也就是Encryption eMD5 = newEncryptionMD5Impl();然后在程序中调用的加密方法eMD5.encrypt();这里是MD5加密，但有一天我想用Base64加密了，这种针对于接口的编程就可以在改很少的原有代码下实现增加新的功能。也就是加一个EncryptionBase64Impl实现Encryption接口，然后其余的地方都不用改。我认为这就是DIP最基本的运用。
注：以抽象方式耦合是依赖倒转原则的关键，里氏代换原则是实现依赖倒转原则的基础。[Java与模式]

 

6）接口隔离原则（Interface Segregation Principle 或者 ISP）：使用多个专门的接口比使用单一的总接口要好。[Java与模式]

 

我对ISP没有什么深刻的认识，就是觉着要把一个大的接口划分成小的接口，把功能分离，就是ISP。

下图摘自[卡奴达摩的博客]

 

这个就是违反ISP的实例

 

这个是遵守ISP的实例

 

7）组合/聚合复用原则（Composition/Aggregation Reuse Principle 或者 CARP）：在一个新的对象里面使用一些已有的对象，使之成为新对象的一部分；新的对象通过向这些对象的委派达到复用已有功能的目的。[Java与模式]

 

简言之CARP就是：要尽量使用合成/聚合，尽量不要使用继承。[Java与模式]（这个是开发的一个准则。一般情况都是这样做的。除非面对的需求是有特殊性的，比如模板方法，这个就是把继承用到极致的一个典范。）

 

那么问题来了，为什么要尽量使用合成/聚合，不使用继承呢？

我认为，首先，合成/聚合是一种较弱的耦合关系，而继承确实一种强耦合，使得两个类之间父类改变子类也随之变化。其次，继承没有合成/聚合灵活，合成/聚合关系可以将功能委派给一个接口，而功能需要改变的时候只需要改变接口的实现类就好了。但继承的实现却是静态的，不能这样改变。最后继承破坏了封装性（书上是这么说的，但对于此我并没有太好的理解）。

 

8）迪米特法则（Law of Demeter 或者 LoD）：只与你直接的朋友们通信。[Java与模式]（这个在实际应用中需要看具体的情况来定，不可以一味的套用。在抽象的基础上降低耦合性是常见的做法。如果每一个类只有一个朋友类，反而达不到解耦的效果。）

对迪米特法则的解释：如果两个类不必彼此直接通信，那么这两个类就不应当发生直接的相互关系。如果其中的一个类需要调用另一个类的某一个方法的话，可以通过第三者转发这个调用。[Java与模式]

我认为LoD就是为了降低类与类之间的耦合，把功能分开，实现模块化，让模块之间通过另一个共通的“朋友”类进行通信，使得将来程序改动的时候只改动某一个模块，因为模块之间是通过他们共通的朋友通信的，所以耦合度低，不会影响其他模块的功能。[Java与模式]中说LoD的主要用意是控制信息的过载。我认为就是模块化这个意思。
--------------------- 
作者：忧伤的可乐鸡 
来源：CSDN 
原文：https://blog.csdn.net/u012817635/article/details/51077944 
版权声明：本文为博主原创文章，转载请附上博文链接！
(并为整理完，先整理到这，有时间勘误更新)