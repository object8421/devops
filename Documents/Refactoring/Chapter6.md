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

临时变量的问题在于