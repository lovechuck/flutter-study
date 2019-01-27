# 05-基础widgets

## 介绍

Widget的功能是“描述一个UI元素的配置数据”，Widget并不是表示最终绘制在设备屏幕上的显示元素，而只是显示元素的一个配置数据。Flutter中真正代表屏幕上显示元素的类是Element，

```dart
@immutable
abstract class Widget extends DiagnosticableTree {
  const Widget({ this.key });
  final Key key;

  @protected
  Element createElement();

  @override
  String toStringShort() {
    return key == null ? '$runtimeType' : '$runtimeType-$key';
  }

  @override
  void debugFillProperties(DiagnosticPropertiesBuilder properties) {
    super.debugFillProperties(properties);
    properties.defaultDiagnosticsTreeStyle = DiagnosticsTreeStyle.dense;
  }

  static bool canUpdate(Widget oldWidget, Widget newWidget) {
    return oldWidget.runtimeType == newWidget.runtimeType
        && oldWidget.key == newWidget.key;
  }
}
```

在Flutter开发中，我们一般都不用直接继承Widget类来实现Widget，相反，我们通常会通过继承StatelessWidget和StatefulWidget来间接继承Widget类来实现，而StatelessWidget和StatefulWidget都是直接继承自Widget类，而这两个类也正是Flutter中非常重要的两个抽象类，它们引入了两种Widget模型。

## StatelessWidget

StatelessWidget相对比较简单，它继承自Widget，重写了createElement()方法：

```dart
@override
StatelessElement createElement() => new StatelessElement(this);
```

StatelessWidget用于不需要维护状态的场景，它通常在build方法中通过嵌套其它Widget来构建UI，在构建过程中会递归的构建其嵌套的Widget。

## StatefulWidget

和StatelessWidget一样，StatefulWidget也是继承自widget类，并重写了createElement()方法，不同的是返回的Element 对象并不相同；另外StatefulWidget类中添加了一个新的接口createState()，下面我们看看StatefulWidget的类定义：

```dart
abstract class StatefulWidget extends Widget {
  const StatefulWidget({ Key key }) : super(key: key);

  @override
  StatefulElement createElement() => new StatefulElement(this);

  @protected
  State createState();
}
```

- StatefulElement 间接继承自Element类，与StatefulWidget相对应（作为其配置数据）。StatefulElement中可能会多次调用createState()来创建状态(State)对象。

- createState() 用于创建和Stateful widget相关的状态，它在Stateful widget的生命周期中可能会被多次调用。例如，当一个Stateful widget同时插入到widget树的多个位置时，Flutter framework就会调用该方法为每一个位置生成一个独立的State实例，其实，本质上就是一个StatefulElement对应一个State实例

## Flutter widget库介绍

Flutter提供了一套丰富、强大的基础widget，在基础widget库之上Flutter又提供了一套Material风格（Android默认的视觉风格）和一套Cupertino风格（iOS视觉风格）的widget库。要使用基础widget库，需要先导入：

```dart
import 'package:flutter/widgets.dart';
```

下面我们介绍一下常用的widget。

基础widget:

- Text：该 widget 可让您创建一个带格式的文本。
- Row、 Column： 这些具有弹性空间的布局类Widget可让您在水平（Row）和垂直（Column）方向上创建灵活的布局。其设计是基于web开发中的Flexbox布局模型。
- Stack： 取代线性布局 (译者语：和Android中的FrameLayout相似)，Stack允许子 widget 堆叠， 你可以使用 Positioned 来定位他们相对于Stack的上下左右四条边的位置。Stacks是基于Web开发中的绝对定位（absolute positioning )布局模型设计的。
- Container： Container 可让您创建矩形视觉元素。container 可以装饰一个BoxDecoration, 如 background、一个边框、或者一个阴影。 Container 也可以具有边距（margins）、填充(padding)和应用于其大小的约束(constraints)。另外， Container可以使用矩阵在三维空间中对其进行变换。

## Material widget

Flutter提供了一套丰富的Material widget，可帮助您构建遵循Material Design的应用程序。要使用Material widget，需要先引入它：

```dart
import 'package:flutter/material.dart';
```

## 文本和字体

### Text

Text用于显示简单样式文本，它包含一些控制文本显示样式的一些属性，一个简单的例子如下：

```dart
Text("Hello world",
  textAlign: TextAlign.center,
  maxLines: 1,
  overflow: TextOverflow.ellipsis,
  textScaleFactor: 1.5,
);
```

- textAlign：文本的对齐方式；可以选择左对齐、右对齐还是居中。
- maxLines：指定文本显示的最大行数，
- overflow来指定截断方式，默认是直接截断，本例中指定的截断方。TextOverflow.ellipsis，它会将多余文本截断后以省略符“...”表示；
- textScaleFactor：代表文本相对于当前字体大小的缩放因子，相对于去设置文本的样式style属性的fontSize，它是调整字体大小的一个快捷方式。

### TextStyle

TextStyle用于指定文本显示的样式如颜色、字体、粗细、背景等。示例：

```dart
Text("Hello world",
  style: TextStyle(
    color: Colors.blue,
    fontSize: 18.0,
    height: 1.2,  
    fontFamily: "Courier",
    background: new Paint()..color=Colors.yellow,
    decoration:TextDecoration.underline,
    decorationStyle: TextDecorationStyle.dashed
  ),
);
```

### TextSpan

在上面的例子中，Text的所有文本内容只能按同一种样式，如果我们需要对一个Text内容的不同部分按照不同的样式显示，这时就可以使用TextSpan，它代表文本的一个“片段”。我们看看TextSpan的定义:

```dart
const TextSpan({
  TextStyle style, 
  Sting text,
  List<TextSpan> children,
  GestureRecognizer recognizer,
});
```

其中style 和 text属性代表该文本片段的样式和内容。 children是一个TextSpan的数组，也就是说TextSpan可以包括其他TextSpan。而recognizer用于对该文本片段上用于手势进行识别处理。

## 按钮

Material widget库中提供了多种按钮Widget如RaisedButton、FlatButton、OutlineButton等，它们都是直接或间接对RawMaterialButton的包装定制，所以他们大多数属性都和RawMaterialButton一样.

按钮都有如下相同点：

- 按下时都会有“水波动画”。
- 有一个onPressed属性来设置点击回调，当按钮按下时会执行该回调，如果不提供该回调则按钮会处于禁用状态，禁用状态不响应用户点击。

```dart
RaisedButton(
  child: Text("normal"),
  onPressed: () => {},
);

FlatButton(
  child: Text("normal"),
  onPressed: () => {},
)

OutlineButton(
  child: Text("normal"),
  onPressed: () => {},
)

IconButton(
  icon: Icon(Icons.thumb_up),
  onPressed: () => {},
)
```

## 图片和icon

### 图片

Flutter中，我们可以通过Image来加载并显示图片，Image的数据源可以是asset、文件、内存以及网络。

```dart
Image(
  image: AssetImage("images/avatar.png"),
  width: 100.0
);

Image(
  image: NetworkImage(
      "https://avatars2.githubusercontent.com/u/20411648?s=460&v=4"),
  width: 100.0,
)
```

#### 参数

Image在显示图片时定义了一系列参数，通过这些参数我们可以控制图片的显示外观、大小、混合效果等。我们看一下Image的主要参数：

```dart
const Image({
  ...
  this.width, //图片的宽
  this.height, //图片高度
  this.color, //图片的混合色值
  this.colorBlendMode, //混合模式
  this.fit,//缩放模式
  this.alignment = Alignment.center, //对齐方式
  this.repeat = ImageRepeat.noRepeat, //重复方式
  ...
})
```

### icon

Flutter中，可以像web开发一样使用iconfont，iconfont即“字体图标”，它是将图标做成字体文件，然后通过指定不同的字符而显示不同的图片。

使用Material Design字体图标

Flutter默认包含了一套Material Design的字体图标，在pubspec.yaml文件中的配置如下

```dart
flutter:
  uses-material-design: true
```

Flutter封装了一个IconData和Icon来专门显示字体图标

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.center,
  children: <Widget>[
    Icon(Icons.accessible,color: Colors.green,),
    Icon(Icons.error,color: Colors.green,),
    Icon(Icons.fingerprint,color: Colors.green,),
  ],
)
```

## 单选框和复选框

Material widgets库中提供了Material风格的单选开关Switch和复选框Checkbox，它们都是继承自StatelessWidget。当用户点击Switch或Checkbox时，它们会触发onChanged回调，我们可以在此回调中处理选中状态改变逻辑。我们看一个简单的示例：

```dart
class SwitchAndCheckBoxTestRoute extends StatefulWidget {
  @override
  _SwitchAndCheckBoxTestRouteState createState() => new _SwitchAndCheckBoxTestRouteState();
}

class _SwitchAndCheckBoxTestRouteState extends State<SwitchAndCheckBoxTestRoute> {
  bool _switchSelected=true; //维护单选开关状态
  bool _checkboxSelected=true;//维护复选框状态
  @override
  Widget build(BuildContext context) {
    return Column(
      children: <Widget>[
        Switch(
          value: _switchSelected,//当前状态
          onChanged:(value){
            //重新构建页面  
            setState(() {
              _switchSelected=value;
            });
          },
        ),
        Checkbox(
          value: _checkboxSelected,
          activeColor: Colors.red, //选中时的颜色
          onChanged:(value){
            setState(() {
              _checkboxSelected=value;
            });
          } ,
        )
      ],
    );
  }
}
```

上面代码中，由于要维护Switch和Checkbox状态，所以SwitchAndCheckBoxTestRoute继承自StatefulWidget 。在其build方法中分别构建了一个Switch和Checkbox，初始状态都为选中状态，当用户点击时，会将状态置反，然后回调用setState()通知framework重新构建UI

## 输入框和表单

### TextField

TextField用于文本输入，它提供了很多属性，主要属性的作用如下：

```dart
const TextField({
  TextEditingController controller,
  FocusNode focusNode,
  InputDecoration decoration = const InputDecoration(),
  TextInputType keyboardType,
  TextInputAction textInputAction,
  TextStyle style,
  TextAlign textAlign = TextAlign.start,
  bool autofocus = false,
  bool obscureText = false,
  int maxLines = 1,
  int maxLength,
  bool maxLengthEnforced = true,
  ValueChanged<String> onChanged,
  VoidCallback onEditingComplete,
  ValueChanged<String> onSubmitted,
  List<TextInputFormatter> inputFormatters,
  bool enabled,
  this.cursorWidth = 2.0,
  this.cursorRadius,
  this.cursorColor,
  ...
})
```

- controller：编辑框的控制器，通过它可以设置/获取编辑框的内容、选择编辑内容、监听编辑文本改变事件。大多数情况下我们都需要显式提供一个controller来与文本框交互。如果没有提供controller，则TextField内部会自动创建一个。
- focusNode：用于控制TextField是否占有当前键盘的输入焦点。它是我们和键盘交互的一个handle。
- InputDecoration：用于控制TextField的外观显示，如提示文本、背景颜色、边框等。
- keyboardType：用于设置该输入框默认的键盘输入类型，取值如下：

| TextInputType枚举值 |                        含义                         |
| ------------------- | :-------------------------------------------------: |
| text                |                    文本输入键盘                     |
| multiline           |   多行文本，需和maxLines配合使用(设为null或大于1)   |
| number              |                数字；会弹出数字键盘                 |
| phone               | 优化后的电话号码输入键盘；会弹出数字键盘并显示"* #" |
| datetime            |    优化后的日期输入键盘；Android上会显示“: -”     |
| emailAddress        |         优化后的电子邮件地址；会显示“@ .”         |
| url                 |         优化后的url输入键盘； 会显示“/ .”         |

- textInputAction：键盘动作按钮图标(即回车键位图标)，它是一个枚举值，有多个可选值，全部的取值列表读者可以查看API文档。
- style：正在编辑的文本样式。
- textAlign: 输入框内编辑文本在水平方向的对齐方式。
- autofocus: 是否自动获取焦点。
- obscureText：是否隐藏正在编辑的文本，如用于输入密码的场景等，文本内容会用“•”替换。
- maxLines：输入框的最大行数，默认为1；如果为null，则无行数限制。
- maxLength和maxLengthEnforced ：maxLength代表输入框文本的最大长度，设置后输入框右下角会显示输入的文本计数。maxLengthEnforced决定当输入文本长度超过maxLength时是否阻止输入，为true时会阻止输入，为false时不会阻止输入但输入框会变红。
- onChange：输入框内容改变时的回调函数；注：内容改变事件也可以通过controller来监听。
- onEditingComplete和onSubmitted：这两个回调都是在输入框输入完成时触发，比如按了键盘的完成键（对号图标）或搜索键（🔍图标）。不同的是两个回调签名不同，onSubmitted回调是`ValueChanged<String>`类型，它接收当前输入内容做为参数，而onEditingComplete不接收参数。
- inputFormatters：用于指定输入格式；当用户输入内容改变时，会根据指定的格式来校验。
- enable：如果为false，则输入框会被禁用，禁用状态不接收输入和事件，同时显示禁用态样式（在其decoration中定义）。
- cursorWidth、cursorRadius和cursorColor：这三个属性是用于自定义输入框光标宽度、圆角和颜色的。

示例

```dart
Column(
        children: <Widget>[
          TextField(
            autofocus: true,
            decoration: InputDecoration(
                labelText: "用户名",
                hintText: "用户名或邮箱",
                prefixIcon: Icon(Icons.person)
            ),
          ),
          TextField(
            decoration: InputDecoration(
                labelText: "密码",
                hintText: "您的登录密码",
                prefixIcon: Icon(Icons.lock)
            ),
            obscureText: true,
          ),
        ],
);
```

### 表单

Form继承自StatefulWidget对象，它对应的状态类为FormState。我们先看看Form类的定义：

```dart
Form({
  @required Widget child,
  bool autovalidate = false,
  WillPopCallback onWillPop,
  VoidCallback onChanged,
})
```

- autovalidate：是否自动校验输入内容；当为true时，每一个子FormField内容发生变化时都会自动校验合法性，并直接显示错误信息。否则，需要通过调用FormState.validate()来手动校验。
- onWillPop：决定Form所在的路由是否可以直接返回（如点击返回按钮），该回调返回一个Future对象，如果Future的最终结果是false，则当前路由不会返回；如果为true，则会返回到上一个路由。此属性通常用于拦截返回按钮。
- onChanged：Form的任意一个子FormField内容发生变化时会触发此回调。

#### FormField

Form的子孙元素必须是FormField类型，FormField是一个抽象类，定义几个属性，FormState内部通过它们来完成操作，FormField部分定义如下：

```dart
const FormField({
  ...
  FormFieldSetter<T> onSaved, //保存回调
  FormFieldValidator<T>  validator, //验证回调
  T initialValue, //初始值
  bool autovalidate = false, //是否自动校验。
})
```

Flutter提供了一个TextFormField widget，它继承自FormField类，也是TextField的一个包装类

#### FormState

FormState为Form的State类，可以通过Form.of()或GlobalKey获得。我们可以通过它来对Form的子孙FormField进行统一操作。

- FormState.validate()：调用此方法后，会调用Form子孙FormField的validate回调，如果有一个校验失败，则返回false，所有校验失败项都会返回用户返回的错误提示。
- FormState.save()：调用此方法后，会调用Form子孙FormField的save回调，用于保存表单内容
- FormState.reset()：调用此方法后，会将子孙FormField的内容清空。
