# 代码的坏味道

## 1. Duplicated Code 重复代码

设计将它们合而为一，程序会变得更好。

## 2. Long Method 过长函数

拥有短函数的对象会活得比较好、比较长。程序越长越难理解，应该尽量使用简单的函数。

## 3. Large Class 过大的类 [单一职能原则]

如果想利用单个类做太多事情，其内往往就会出现太多实例变量，一旦如此，Duplicated Code也就接踵而至了。


## 4. Long Parameter List 过长参数列

太长的参数列难以理解，可以使用对象作为参数。（需要注意的是：如果参数列是可控的，应该明确定义参数列而非使用过大的对象）

## 5. Divergent Change 发散式变化

如果某个类经常因为不同的原因在不同的方向上发生变化，Divergent Change就出现了。为此，你应该找出某些特定原因而造成的所有变化，然后运用Extract Class将它们提炼到另一个类中。

## 6. Shotgun Surgery 霰弹式修改

与Divergent Change正好相反。如果每遇到某种变化，都必须在许多不同的类做出许多修改，你所面临的坏味道就是Shotgun Surgery.

## 7. Feature Envy 依恋情结

某个函数为了计算某个值，从另一个对象那儿调用几乎半打的取值函数。就应该把这个函数移至另一个地点，减少依恋。

## 8. Data Clumps 数据泥团

两个类中相同参数、许多函数签名中相同的参数。这些总是绑在一起出现的数据，真应该拥有属于它们自己的对象，将它们提炼到一个独立的对象中。

## 9. Primitive Obsession 基本类型偏执

大多数编程环境都有两种数据：基本类型和引用类型。对象的一个极大价值在于：它们模糊了横亘于基本数据和体积较大的类之间的界限。你可以轻松编写出一些与语言内置类型无异的小型类。

## 10. Swith Statements switch惊悚现身

大多数时候，一看到switch语句，你就应该考虑以多态来替换它。

## 11. Parallel Inheritance Hierarcies 平行继承体系

其实就是Shotgun Surgery 的特殊情况。在这种情况下，每当你为某个类增加一个子类，必须也为另一个类相应增加一个子类。消除策略是：让一个继承体系的实例引用另一个继承体系的实例。

## 12. Lazy Class 冗赘类

你所创建的每一个类，都得有人去理解它、维护它，这些工作都是要花钱的。如果一个类的所得不值其身价，它就应该消失。

## 13. Speculative Generality 夸夸其谈未来性

不要企图为未来可能用到的情况而做设计。如果用不到，就不值得。用不上的装置，只会挡你的路。

## 14. Temporary Field 令人迷惑的暂时字段

对象内某个实例变量仅为某种特定情况而设。这样的代码让人不易理解，因为你通常认为对象在所有时候都需要它的所有变量。在变量未被使用的情况下猜测当初期其设置目的，会让你发疯的。

## 15. Message Chains 过度耦合的消息链

如果你看到用户向一个对象请求另一个对象，然后再向后者请求另一个对象，然后再请求另一个对象……这就是消息链。采取这种方式，意味着代码更加耦合，一旦对象间的关系发生任何变化，就不得不做出相应修改。

## 16. Middle Man 中间人

对象的基本特征之一就是封装——对外部世界隐藏其内部细节。封装往往伴随委托。但是人们可能过度运用委托，你也许会看到某个类接口有一半的函数都委托给其他类，这样就是过度运用。

## 17. Inappropriate Intimacy 狎昵关系

有时候你会看到两个类过于亲密，花费太多时间 去探究彼此的private成分。过分狎昵的类必须拆散。

## 18. Alternative Classes with Different Interfaces 异曲同工的类

如果两个函数做同一件事，却有着不同的签名。应该减少这种函数的存在。

## 19. Incomplete Library Class 不完美的库类

复用常被视为对象的终极目的，不过我们认为，利用的意义经常被高估 —— 大多数对象只要够用就好。但是无可否认，许多编程技术都建立在程序库的基础上。

## 20. Data Class 纯稚的数据类

它们拥有一些字段，以及用于访问这些字段的函数，除此一无长物。应该尽可能地使用封装，让它们参与整个系统的工作，必须承担一定责任。

## 21. Refused Bequest 被拒绝的遗赠

子类应该继承超类的函数和数据。所有的超类都应该是抽象的。如果子类复用了超类的行为，却又不愿意支持超类的接口，Refused Bequest的坏味道就会变得浓烈。