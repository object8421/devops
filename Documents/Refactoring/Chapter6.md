# 重新组织函数

## 1. Extract Method 提炼函数

你有一段代码可以被组织在一起并独立出来。

* 将这段代码放进一个独立函数中，并让函数名称解释该函数的用途。

```
// 重构前的代码
void printOwing(double amount) {
  printBanner();
  
  // print details
  System.out.println("name: " + _name);
  System.out.println("amount: " + amount); 
}

// 重构后的代码
void printOwing(double amount) {
  printBanner();
  printDetails(amount);
}

void printDetails(double amount) {
  System.out.println("name: " + _name);
  System.out.println("amount: " + amount); 
}
```

### 动机

* 简短而命名良好的函数。
* 如果每个函数的粒度都很小，那么函数被利用的机会就更大。
* 这会使高层函数读起来就像一系列注释。
* 如果函数都是细粒度，那么函数的覆写也会更容易些。

### 做法

* 创建一个新函数，根据函数的意图来命名。
* 将提炼出的代码从源函数复制到新建的目标函数中。
* 仔细检查提炼出的代码，看看其中是否引用了“作用域限于源函数”的变量。
* 检查是否有“仅用于被提炼代码段”的临时变量。如果有，在目标函数中将它们声明为临时变量。
* 检查被提炼代码段，看看是否有任何局部变量的值被它改变。（一定要注意！）
* 将被提炼代码段中需要读取的局部变量，当作参数传给目标函数。
* 处理完所有局部变量之后，进行编译。
* 在源函数中，将被提炼代码段替换为对目标函数的调用。
* 编译，测试。

## 2. Inline Method 内联函数

一个函数的本体和名称同样清楚易懂。

* 在函数调用点插入函数本体，然后移除该函数。

```
// 重构前
int getRating() {
  return (moreThanFiveLateDeliveries) ? 2: 1;
}

boolean moreThanFiveLateDeliveries() {
  return _numberOfLateDeliveres > 5;
}

// 重构后
int getRating() {
  return (_numberOfLateDeliveres > 5) ? 2: 1;
}
```

### 动机

以简短的函数表现动作意图，这样会使代码更加清晰易读。

### 做法

* 检查函数，确定它不具多态性。

*　找出这个函数的所有被调用点。

* 将这个函数的所有被调用点都替换为函数本体。

* 编译，测试。

* 删除该函数的定义。

## 3. Inline Temp 内联临时变量

你有一个临时变量，只被一个简单表达式赋值一次，而它妨碍了其他重构手法。

* 将所有对该变量的引用动作，替换为对它赋值的那个表达式自身。

```
// 重构前
double basePrice = anOrder.basePrice();
return (basePrice > 1000);

// 重构后
return anOrder.basePrice() > 1000;
```

### 动机

一般来说，这样的临时变量不会有任何危害，可以放心地把它留在那儿。如果这个临时变量妨碍了其他的重构手法，就应该将它内联化。

### 做法

* 检查给临时变量赋值的语句，确保等号右边的表达式没有副作用。

* 如果这个临时变量并未被声明为final, 那就将它声明为final，然后编译。

* 找到该临时变量的所有引用点，将它们替换为“为临时变量赋值”的表达式。

* 每次修改后，编译并测试。

* 修改完所有引用点之后，删除该临时变量的声明和赋值语句。

* 编译，测试。

## 4. Replace Temp with Query 以查询取代临时变量

你的程序以一个临时变量保存某个表达式的运算结果。

* 将这个表达式提炼到一个独立函数中。将这个临时变量的所有引用点替换为对新函数的调用。以后，新函数就可被其他函数使用。

```
// 重构前
double basePrice = _quantity * _itemPrice;
if(basePrice > 1000)
  return basePrice * 0.95;
else 
  return basePrice * 0.98;
  
// 重构后
if(basePrice() > 1000)
  return basePrice() * 0.95;
else 
  return basePrice() * 0.98;
  
double basePrice() {
  return _quantity * _itemPrice;
}
```

### 动机

临时变量的问题在于：它们只是暂时的，而且只能在所属函数内使用。

### 做法

* 找出只被财会一次的临时变量：（如果某个临时变量被赋值超过一次，考虑将它分割成多个变量）

* 将该临时变量声明为final

* 编译：这可确保该临时变量的确只被赋值一次。

* 将“对该临时变量赋值”之语句的等号右侧部分提炼到一个独立函数中。

* 编译，测试。

```
// 重构前
double getPrice() {
  int basePrice = _quantity * _itemPrice;
  double discountFactor;
  if (basePrice > 1000) discountFactor = 0.95;
  else discountFactor = 0.98;
  return basePrice * discountFactor;
}

// 将临时变量声明为final，检查它们是否的确只被赋值一次。
// 如果编译器报告或警告，就说明临时变量不只被赋值一次。
double getPrice() {
  final int basePrice = _quantity * _itemPrice;
  final double discountFactor;
  if (basePrice > 1000) discountFactor = 0.95;
  else discountFactor = 0.98;
  return basePrice * discountFactor;
}

// 接下来，把赋值动作的右侧表达式提炼出来。
double getPrice() {
  final int basePrice = basePrice();
  final double discountFactor;
  if (basePrice > 1000) discountFactor = 0.95;
  else discountFactor = 0.98;
  return basePrice * discountFactor;
}

private int basePrice() {
  return _quantity * _itemPrice;
}

// 编译并测试，然后开始使用Inline Temp。首先把临时变量basePrice的第一个引用点替换掉：
double getPrice() {
  final int basePrice = basePrice();
  final double discountFactor;
  if (basePrice() > 1000) discountFactor = 0.95;
  else discountFactor = 0.98;
  return basePrice * discountFactor;
}

// 编译、测试、下一个。由于“下一个”已经是basePrice的最后一个引用点，所以我把basePrice临时变量的声明一并去掉。
double getPrice() {
  final double discountFactor;
  if (basePrice() > 1000) discountFactor = 0.95;
  else discountFactor = 0.98;
  return basePrice() * discountFactor;
}

// 搞定basePrice之后，再以类似办法提炼出discountFactor():
double getPrice() {
  final double discountFactor = discountFactor();
  return basePrice() * discountFactor;
}

private double discountFactor() {
  if (basePrice() > 1000) return 0.95;
  else return 0.98;
}

// 你看，如果没有把临时变量basePrice替换成一个查询式，将多么难以提炼discountFactor()!

// 最终，getPrice() 变成了这样：
double getPrice() {
  return basePrice() * discountFactor();
}
```

## 5. Introduce Explaining Varible 引入解释性变量

你有一个复杂的表达式。

将该复杂表达式（或其中一部分）的结果放进一个临时变量，以此变量名称来解释表达式用途。

```
if((platform.toUpperCase().indexOf("MAC") > -1) &&
  (browser.toUpperCase().indexOf("IE") > -1) && 
  wasInitialized() && resize > 0) {
  // do something   
}

final boolean isMacOs = platform.toUpperCase().indexOf("MAC") > -1;
final boolean isIEBrowser = browser.toUpperCase().indexOf("IE") > -1;
final boolean wasResized = resize > 0;

if (isMacOs && isIEBrowser && wasInitialized() && wasResized) {
  // do something
}
```

### 动机

表达式有可能非常复杂而难以阅读，在这种情况下，临时变量可以帮助你将表达式分解为比较容易管理的形式。

## 做法

* 声明一个final临时变量，将待分解之复杂表达式的一部分动作的运算结果赋值给它。

* 将表达式中的“运算结果”这一部分，替换为上述临时变量。

* 编译，测试

* 重复上述过程，处理表达式的其他部分。

```
// 范例
double price() {
  return _quantity * _itemPrice - Math.max(0, _quantity - 500) * _itemPrice * 0.05 + Math.min(_quantity * _itemPrice * 0.1, 100.0);
}

// 让代码更简单
double price() {
  final double basePrice = _quantity * _itemPrice;
  return basePrice - Math.max(0, _quantity - 500) * _itemPrice * 0.05 + Math.min(basePrice * 0.1, 100.0);
}

// 然后将批发折扣的计算提炼出来，将结果赋予临时变量。
double price() {
  final double basePrice = _quantity * _itemPrice;
  final double quantityDiscount = Math.max(0, _quantity-500) * _itemPrice * 0.05;
  return basePrice - quantityDiscount + Math.min(basePrice * 0.1, 100.0);
}

// 然后再把运费提炼出来
double price() {
  final double basePrice = _quantity * _itemPrice;
  final double quantityDiscount = Math.max(0, _quantity-500) * _itemPrice * 0.05;
  final double shipping = Math.min(basePrice * 0.1, 100.0);
  return basePrice - quantityDiscount + shipping;
}
```

### 运用Extract Method处理上述范例。
```
double price() {
  return _quantity * _itemPrice - Math.max(0, _quantity - 500) * _itemPrice * 0.05 + Math.min(_quantity * _itemPrice * 0.1, 100.0);
}

// 这一次把底价计算提炼到一个独立函数中：
double price() {
  return basePrice() - Math.max(0, _quantity - 500) * _itemPrice * 0.05 + Math.min(basePrice() * 0.1, 100.0);
}

private double basePrice() {
  return _quantity * _itemPrice;
}

// 继续提炼，每次提炼出一个新函数。最后得到下列代码：
double price() {
  return basePrice() - quantityDiscount() + shipping();
}

private double basePrice() {
  return _quantity * _itemPrice;
}

private double quantityDiscount() {
  return Math.max(0, _quantity - 500) * _itemPrice * 0.05;
}

private double shipping() {
  return Math.min(basePrice() * 0.1, 100.0);
}
```

## 6. Split Temporary Variable 分解临时变量

你的程序有某个临时变量被赋值超过一次，它既不是循环变量，也不被用于收集计算结果。

* 针对每次赋值，创造一个独立、对应的临时变量。

### 动机

临时变量有各种不同用途，其中某些用途会很自然地导致临时变量被多次赋值。

### 做法

* 在待分解临时变量的声明及期第一次被赋值处，修改其名称。

* 将新的临时变量声明为final。

* 以该临时变量的第二次赋值动作为界，修改此前对该临时变量的所有引用点，让它们引用新的临时变量。

* 在第二次赋值时，重新声明原先那个临时变量。

* 编译，测试

* 重复上述过程。

### 范例

```
double getDistanceTravelled(int time) {
  double result;
  double acc = _primaryForce / _mass;
  int primaryTime = Math.min(time, _delay);
  result = 0.5 * acc * primaryTime * primaryTime;
  int secondaryTime = time - _delay;
  
  if(secondaryTime > 0) {
    double primaryVel = acc * _delay;
    acc = (_primaryForce + _secondaryForce) / _mass;
    result += primaryVel * secondaryTime + 0.5 * acc * secondaryTime * secondaryTime;
    return result;
  }
}

// 变量被重复赋值。明白acc变量的两个责任：1.保存每一个力造成的初始加速度；2.保存两个力共同造成的加速度。
// 新的临时变量的名称指出，它只承担原先acc变量的第一个责任，并声明为final.
double getDistanceTravelled(int time) {
  double result;
  final double primaryAcc = _primaryForce / _mass;
  int primaryTime = Math.min(time, _delay);
  result = 0.5 * primaryAcc * primaryTime * primaryTime;
  int secondaryTime = time - _delay;
  
  if(secondaryTime > 0) {
    double primaryVel = primaryAcc * _delay;
    double acc = (_primaryForce + _secondaryForce) / _mass;
    result += primaryVel * secondaryTime + 0.5 * acc * secondaryTime * secondaryTime;
    return result;
  }
}
```

## 7. Remove Assignments to Parameters 移除对参数的赋值

代码对一个参数进行赋值

* 以一个临时变量取代该参数的位置。

```
// 重构前
int discount (int inputVal, int quantity, int yearToDate) {
  if (inputVal > 50) inputVal -= 2;
}

// 重构后
int discount (int inputVal, int quantity, int yearToDate) {
  int result = inputVal;
  if (inputVal > 50) result -= 2;
}
```

### 动机

对参数赋值，如果你把一个对象作为参数传给某个函数，那么有可能会改变这个对象。

### 做法

* 建立一个临时变量，把等处理的参数值赋予它。

* 以“对参数的赋值”为界，将其后所有对此参数的引用点，全部替换为“对此临时变量的引用”

* 修改赋值语句，使其改变对新建之临时变量赋值。

* 编译，测试。

## 8. Replace Method with Method Object 以函数对象取代函数

你有一个大型函数，其中对局部变量的使用使你无法采用Extract Method.

* 将这个函数放进一个单独对象中，如此一来局部变量就成了对象内的字段。然后你可以同一个对象中将这个大型函数分解为多个小型函数。

### 动机

只要将相对独立的代码从大型函数中提炼出来，就可以大大提高代码的可读性。

### 做法
