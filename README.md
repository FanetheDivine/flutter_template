# Flutter项目模板
* 入口 `lib/main.dart`
* 启动 `flutter run`
* 打包 `flutter build`
# 基础知识
* 关键字`final` 常量.变量和变量的每一个字段都不可更改
* 关键字`const` 此变量在编译时确定,编译器会提示.`const`修饰的组件,`state`变化相互独立
* `空值检测` `MyInheritedWidget? result`表示这个值可能为null.`result?.data`如果`result`是`null`则停止获取属性并返回null.`var ?? default`为变量在null时提供默认值
* `模板字符串` 字符串中引入变量.例如`'value:${value}'`
* `样式` 组件参数可以描述本组件.此外,样式组件也会修饰子组件,例如`Center`会使子组件居中
* `构造函数`
  ```dart
    const MyHomePage({super.key, required this.title});
  ```
  这是一个不需要写明的构造函数.
  * `const` 如果构造组件时提供的参数完全可以在编译时确定,那么本组件的值也可以在编译时确定
  * `super.key` 与`React`中组件的`key`类似,用于列表等.写着就行
  * `required this.title` required表示构造组件时必须提供title.`this.title`表示这个值会直接赋值给同名属性,类型与该属性相同,不需要直接写明.
  * 构造组件 
  ```dart
  const MyHomePage(title: 'Flutter Demo Home Page')//需要写明属性名称
  ```
  构造组件时所有参数都在编译时决定,且构造函数本身由`const`修饰,因此此语句可以用`const`修饰
* 组件函数`build` 类似`render`函数.接受一个参数`BuildContext`类型的参数`context`
* `BuildContext` 在组件的`build`函数中获取指定上下文提供者的值.与`React`的`context`类似.
    * 从主题中获取颜色  
    ```dart
     Theme.of(context).colorScheme.inversePrimary
    ```
    * 自定义上下文提供者
    ```dart
    class MyInheritedWidget extends InheritedWidget {//继承此类 可以为其包裹的组件提供值
        final MyData data;

        const MyInheritedWidget({
            Key? key,
            required this.data,
            required Widget child,
        }) : super(key: key, child: child);

        @override
        bool updateShouldNotify(MyInheritedWidget oldWidget) => data != oldWidget.data;//返回true则包裹的组件更新

        static MyData of(BuildContext context) {
            final MyInheritedWidget? result = context.dependOnInheritedWidgetOfExactType<MyInheritedWidget>();
            // 如果调用此函数的环境不被此组件包裹 则result为null
            return result?.data;
        }
    }
    ```
    在某个组件的`build`函数中获取值
    ```dart
    MyInheritedWidget.of(context)
    ```
* `更新机制` 与`React`不同,状态更新时`Flutter`只会更新订阅状态的组件.因此可以使用`InheritedWidget`进行全局状态管理.
# Hooks
利用[flutter_hooks](https://github.com/rrousselGit/flutter_hooks/blob/master/packages/flutter_hooks/resources/translations/zh_cn/README.md)库使用hook,限制与`React`类似.此外hooks只能在build函数中使用.
组件类需要继承`HookWidget`,可以减少`StateWidget`的使用难度.
使用例
```dart
  @override
  Widget build(BuildContext context) {
    final counter = useState(0);
    return ElevatedButton(
              onPressed: () => counter.value++,
              child: Row(
                children: [
                  Text('${counter.value}'),
                  const Icon(Icons.add)
                ],
              )
          )
  }
```
还有useEffect等
# 路由和导航

# 表单和表格
未完待续
