# Dart 语言了解

## 概念

当您了解Dart语言时，请记住以下事实和概念：

- 您可以放在变量中的所有内容都是一个对象，每个对象都是一个类的实例。偶数，函数和 null对象。所有对象都从Object类继承。

- 尽管Dart是强类型的，但类型注释是可选的，因为Dart可以推断类型。在上面的代码中，number 推断为类型int。如果要明确说明不需要任何类型，请 使用特殊类型dynamic。

- Dart支持泛型类型，如`List<int>`（整数列表）或`List<dynamic>`（任何类型的对象列表）。

- Dart支持顶级函数（例如main()），以及绑定到类或对象的函数（分别是静态和实例方法）。您还可以在函数内创建函数（嵌套函数或本地函数）。

- 类似地，Dart支持顶级变量，以及绑定到类或对象的变量（静态和实例变量）。实例变量有时称为字段或属性。

- 与Java，飞镖不具备关键字public，protected和private。如果标识符以下划线（_）开头，则它对其库是私有的。有关详细信息，请参阅 库和可见性。

- 标识符可以以字母或下划线（_）开头，后跟这些字符加数字的任意组合。

- Dart有两个表达式（具有运行时值）和 语句（不具有）。例如，条件表达式 condition ? expr1 : expr2的值为expr1或expr2。将其与if-else语句进行比较，该语句没有任何值。语句通常包含一个或多个表达式，但表达式不能直接包含语句。

- Dart工具可以报告两种问题：警告和错误。警告只是表明您的代码可能无法正常工作，但它们不会阻止您的程序执行。错误可以是编译时或运行时。编译时错误会阻止代码执行; 运行时错误导致 代码执行时引发异常。

## 变量

这是创建变量并初始化它的示例：

```dart
var name = 'Bob';
```

变量存储引用。调用的变量name包含对String值为“Bob” 的对象的引用。

name推断变量的类型String，但您可以通过指定它来更改该类型。如果对象不限于单一类型，请按照设计指南指定Object或dynamic类型。

```dart
dynamic name = 'Bob' ;
```

另一种选择是显式声明可以推断出的类型：

```dart
String name = 'Bob' ;
```

## 默认值

未初始化的变量的初始值为null。即使是带有数字类型的变量最初也是null，因为数字也都是对象。

```dart
int lineCount ; assert （lineCount == null ）;
```

## 最终和常数

如果您从不打算更改变量，请使用final或const代替var或替代类型。最终变量只能设置一次; const变量是编译时常量。

以下是创建和设置最终变量的示例：

```dart
final name = 'Bob' ; //没有类型注释
final String nickname = 'Bobby' ;  
```

使用const为您要为变量的编译时间常数。如果const变量在类级别，请标记它static const。在声明变量的地方，将值设置为编译时常量，例如数字或字符串文字，const变量或常数上的算术运算结果：

```dart
const bar = 1000000; // Unit of pressure (dynes/cm2)
const double atm = 1.01325 * bar; // Standard atmosphere 
```

## 内置类型

- numbers 数字
- strings 字符串
- booleans 布尔
- lists 列表（也称为数组）
- maps 映射
- runes 符文（用于表示字符串中的Unicode字符）
- symbols 符号

## 数字

两种形式：

```
int
整数值不大于64位，具体取决于平台。

double
64位（双精度）浮点数，由IEEE 754标准规定。
```

## 字符串

Dart字符串是一系列UTF-16代码单元。您可以使用单引号或双引号来创建字符串：

```dart
var s1 = 'Single quotes work well for string literals.';
var s2 = "Double quotes work just as well.";
var s3 = 'It\'s easy to escape the string delimiter.';
var s4 = "It's even easier to use the other delimiter.";
```

## 布尔

类型bool。只有两个对象具有bool类型：true和false，它们都是编译时常量。

```dart
// Check for an empty string.
var fullName = '';
assert(fullName.isEmpty);

// Check for zero.
var hitPoints = 0;
assert(hitPoints <= 0);

// Check for null.
var unicorn;
assert(unicorn == null);

// Check for NaN.
var iMeantToDoThis = 0 / 0;
assert(iMeantToDoThis.isNaN);
```

## Lists

也许几乎每种编程语言中最常见的集合是数组或有序的对象组。在Dart中，数组是 List对象，因此大多数人只是将它们称为列表。

```dart
var list = [1, 2, 3];
assert(list.length == 3);
assert(list[1] == 2);

list[1] = 1;
assert(list[1] == 1);
```

## Maps

通常，映射是关联键和值的对象。键和值都可以是任何类型的对象。每个键只出现一次，但您可以多次使用相同的值。

```dart
var gifts = {
  // Key:    Value
  'first': 'partridge',
  'second': 'turtledoves',
  'fifth': 'golden rings'
};

var nobleGases = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};
```

## Runes

在Dart中，符文是字符串的UTF-32代码点。

Unicode为世界上所有书写系统中使用的每个字母，数字和符号定义唯一的数值。由于Dart字符串是一系列UTF-16代码单元，因此在字符串中表示32位Unicode值需要特殊语法。

```dart
main() {
  var clapping = '\u{1f44f}';
  print(clapping);
  print(clapping.codeUnits);
  print(clapping.runes.toList());

  Runes input = new Runes(
      '\u2665  \u{1f605}  \u{1f60e}  \u{1f47b}  \u{1f596}  \u{1f44d}');
  print(new String.fromCharCodes(input));
}
```

## Symbols

符号对象表示达特程序声明的操作者或标识符。您可能永远不需要使用符号，但它们对于按名称引用标识符的API非常有用，因为缩小会更改标识符名称而不会更改标识符符号。

要获取标识符的符号，请使用符号文字， #后面跟着标识符：

```dart
#radix
#bar
```
符号文字是编译时常量。

## 功能

Dart是一种真正的面向对象语言，因此即使是函数也是对象并且具有类型Function。 这意味着函数可以分配给变量或作为参数传递给其他函数。您也可以像调用函数一样调用Dart类的实例。

以下是实现函数的示例：

```dart
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}

bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;

```

## main（）函数

每个应用程序都必须具有顶级main()功能，该功能用作应用程序的入口点。该main()函数返回void并具有List<String>参数的可选参数。

以下main()是Web应用程序的功能示例：

```dart
void main() {
  querySelector('#sample_text_id')
    ..text = 'Click me!'
    ..onClick.listen(reverseText);
}
```
注意：..前面代码中 的语法称为级联。使用级联，您可以对单个对象的成员执行多个操作。

## 作为第一类对象的函数

您可以将函数作为参数传递给另一个函数。例如：

```dart
void printElement(int element) {
  print(element);
}

var list = [1, 2, 3];

// Pass printElement as a parameter.
list.forEach(printElement);
```

## 控制流程语句

您可以使用以下任一方法控制Dart代码的流程：

```dart
if 和 else
for 循环
while 和 do-while 循环
break 和 continue
switch 和 case
assert
也可以使用 try-catch 和影响控制流 throw，如异常中所述。
```

示例

```dart

if (isRaining()) {
  you.bringRainCoat();
} else if (isSnowing()) {
  you.wearJacket();
} else {
  car.putTopDown();
}

var message = StringBuffer('Dart is fun');
for (var i = 0; i < 5; i++) {
  message.write('!');
}

while (!isDone()) {
  doSomething();
}
do {
  printLine();
} while (!atEndOfPage());

var command = 'OPEN';
switch (command) {
  case 'CLOSED':
    executeClosed();
    break;
  case 'PENDING':
    executePending();
    break;
  case 'APPROVED':
    executeApproved();
    break;
  case 'DENIED':
    executeDenied();
    break;
  case 'OPEN':
    executeOpen();
    break;
  default:
    executeUnknown();
}

try {
  breedMoreLlamas();
} catch (e) {
  print('Error: $e'); // Handle the exception first.
} finally {
  cleanLlamaStalls(); // Then clean up.
}
```

## 类

Dart是一种面向对象的语言，具有类和基于mixin的继承。每个对象都是一个类的实例，所有类都来自Object。 基于Mixin的继承意味着虽然每个类（除了Object）只有一个超类，但是类体可以在多个类层次结构中重用。

```dart
class Point {
  num x; // Declare instance variable x, initially null.
  num y; // Declare y, initially null.
  num z = 0; // Declare z, initially 0.

  Point(num x, num y) {
    // There's a better way to do this, stay tuned.
    this.x = x;
    this.y = y;
  }

  num distanceTo(Point other) {
    var dx = x - other.x;
    var dy = y - other.y;
    return sqrt(dx * dx + dy * dy);
  }

  class Rectangle {
        num left, top, width, height;

        Rectangle(this.left, this.top, this.width, this.height);

        // Define two calculated properties: right and bottom.
        num get right => left + width;
        set right(num value) => left = value - width;
        num get bottom => top + height;
        set bottom(num value) => top = value - height;
    }
}
```

## 抽象类和方法

抽象方法
实例，getter和setter方法可以是抽象的，定义一个接口，但将其实现留给其他类。抽象方法只能存在于抽象类中。

抽象类
使用abstract修饰符定义抽象类 - 无法实例化的类。抽象类对于定义接口非常有用，通常还有一些实现。如果希望抽象类看起来是可实例化的，请定义工厂构造函数。

```dart
abstract class Doer {
  // Define instance variables and methods...

  void doSomething(); // Define an abstract method.
}

class EffectiveDoer extends Doer {
  void doSomething() {
    // Provide an implementation, so the method is not abstract here...
  }
}
```

## 继承和实现

实现：

```dart
// A person. The implicit interface contains greet().
class Person {
  // In the interface, but visible only in this library.
  final _name;

  // Not in the interface, since this is a constructor.
  Person(this._name);

  // In the interface.
  String greet(String who) => 'Hello, $who. I am $_name.';
}

// An implementation of the Person interface.
class Impostor implements Person {
  get _name => '';

  String greet(String who) => 'Hi $who. Do you know who I am?';
}

String greetBob(Person person) => person.greet('Bob');

void main() {
  print(greetBob(Person('Kathy')));
  print(greetBob(Impostor()));
}
```

继承：

```dart
class Television {
  void turnOn() {
    _illuminateDisplay();
    _activateIrSensor();
  }
  // ···
}

class SmartTelevision extends Television {
  void turnOn() {
    super.turnOn();
    _bootNetworkInterface();
    _initializeMemory();
    _upgradeApps();
  }
  // ···
}
```

重写：

```dart
class SmartTelevision extends Television {
  @override
  void turnOn() {...}
  // ···
}
```

## 枚举类型

枚举类型（通常称为枚举或枚举）是一种特殊类，用于表示固定数量的常量值。

使用枚举
使用enum关键字声明枚举类型：

```dart
enum Color { red, green, blue }
```

## 静态类变量和方法

使用static关键字实现类范围的变量和方法。

静态变量:

```dart
class Queue {
  static const initialCapacity = 16;
  // ···
}

void main() {
  assert(Queue.initialCapacity == 16);
}
```

静态方法:

```dart
import 'dart:math';

class Point {
  num x, y;
  Point(this.x, this.y);

  static num distanceBetween(Point a, Point b) {
    var dx = a.x - b.x;
    var dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
  }
}

void main() {
  var a = Point(2, 2);
  var b = Point(4, 4);
  var distance = Point.distanceBetween(a, b);
  assert(2.8 < distance && distance < 2.9);
  print(distance);
}
```

## 泛型

如果您查看基本数组类型的API文档 List，您会看到该类型实际上是List<E>。<...>表示法将List标记为 通用（或参数化）类型 - 具有正式类型参数的类型。按照惯例，大多数类型变量都有单字母名称，例如E，T，S，K和V.

```dart
var names = <String>['Seth', 'Kathy', 'Lars'];
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};
```

## 使用库

使用import指定如何从一个库中的命名空间在另一个库的范围内使用。

唯一需要的参数import是指定库的URI。对于内置库，URI具有特殊dart:方案。对于其他库，您可以使用文件系统路径或package: 方案。该package:方案指定由包管理器（如pub工具）提供的库。

```dart
import 'dart:html';
import 'package:test/test.dart';
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;
```

## 异步支持

Dart库中包含许多返回Future或Stream对象的函数。这些函数是异步的：它们在设置可能耗时的操作（例如I/O）后返回，而不等待该操作完成。

### 处理Future

当您需要完成的Future的结果时，您有两个选择：

- 使用async和await。
- 使用Future API。

使用async和await异步的代码，但它看起来很像同步代码。

```dart
await lookUpVersion();
Future checkVersion() async {
  var version = await lookUpVersion();
  // Do something with version
}
```

使用try,catch和finally 处理使用await以下代码的错误和清理：

```dart
try {
  version = await lookUpVersion();
} catch (e) {
  // React to inability to look up the version
}
```

### 声明异步函数

一个异步函数是一个函数体标有async修改。
将async关键字添加到函数使其返回Future。

```dart
Future<String> lookUpVersion() async => '1.0.0';
```

如果您的函数没有返回有用的值，请设置其返回类型`Future<void>`。

### 处理Streams

当您需要从Stream获取值时，您有两个选择：

- 使用async和异步for循环（await for）。
- 使用Stream API。

注意： 在使用之前await for，请确保它使代码更清晰，并且您确实希望等待所有流的结果。

```dart
await for (varOrType identifier in expression) {
  // Executes each time the stream emits a value.
}
```

## Generators

当您需要延迟地生成一系列值时，请考虑使用生成器函数。Dart内置支持两种形式：

- 同步生成器：返回一个Iterable对象。
- 异步生成器：返回Stream对象。

要实现同步生成器函数，请将函数体标记为sync*，并使用yield语句来传递值。
要实现异步生成器函数，请将函数体标记为async*，并使用yield语句来传递值。

```dart
Iterable<int> naturalsTo(int n) sync* {
  int k = 0;
  while (k < n) yield k++;
}

Stream<int> asynchronousNaturalsTo(int n) async* {
  int k = 0;
  while (k < n) yield k++;
}
```