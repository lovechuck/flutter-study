# helloworld

使用vscode命令行创建flutter应用，ctril+alt+p打开命令行，选择Flutter：New Project创建应用，修改对应目录文件下的lib/main.dart文件如下所示：
```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
```
命令行运行 flutter run 执行，可以看到在ISO simulator中出现hello world。