# Flutter Best Practice Guidelines

**Placeholder Widgets**

Use SizedBox instead of Container as Placeholder widgets.

```
// Good
return _loaded ? SizedBox() : YourWidget();

// Better
return _loaded ? SizedBox.shrink() : YourWidget();
```

**Define Theme**

Define the Theme of the app as well as a first priority to avoid the headache of changing the theme in future updates. Setting up Theme is surely confusing but one time task. Ask your designer to share all the Theme related data like, Colors, font sizes and weightage.

```
class AppTheme {
  AppTheme();

  static AppTheme? _current;

  // ignore: prefer_constructors_over_static_methods
  static AppTheme get current {
    _current ??= AppTheme();
    return _current!;
  }

  static ThemeData? lightTheme = ThemeData(
    scaffoldBackgroundColor: AppColors.screenBackground,
    primaryColor: AppColors.blue,
    colorScheme: const ColorScheme.light(secondary: Colors.white),
    cardColor: Colors.white,
    floatingActionButtonTheme: const FloatingActionButtonThemeData(
      backgroundColor: AppColors.blue,
    ),
  );
}

MaterialApp(
  title: appName,
  theme: AppTheme.current.lightTheme,
  home: const MyHomePage(
    title: appName,
  ),
);
```

**Using `if` instead of Ternary Operator**

Use dart’s built in `if` statement in the layout.

```
// Good
Row(
  children: [
    Text("Hello Flutter"),
    if (Platform.isIOS) Text("iPhone"),
  ]
);

// Better
Row(
  children: [
    Text("Hello Flutter"),
    if (Platform.isIOS) ...[
      Text("iPhone")
      Text("MacBook")
    ],
  ]
);
```

**Use const widgets whenever possible**

Here the TitleWidget doesn’t change on every build, so its better to make it a const widget.

Advantage: Faster build because TitleWidget will be skipped in the build since it wont change during the process.

```
class MyWidget extends StatelessWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Column(
        mainAxisSize: MainAxisSize.min,
        children: const [
          TitleWidget(),
        ],
      ),
    );
  }
}

class TitleWidget extends StatelessWidget {
  const TitleWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return const Padding(
      padding: EdgeInsets.all(10),
      child: Text('Home'),
    );
  }
}
```

**Naming Conventions**

Classes, enums, typedefs, and extensions name should in UpperCamelCase.

```
class MyWidget { ... }
enum Choice { .. }
typedef Predicate<T> = bool Function(T value);
extension ItemList<T> on List<T> { ... }
```

Libraries, packages, directories, and source files name should be in snake_case(lowercase_with_underscores).

```
library firebase_dynamic_links;
import 'my_lib/my_lib.dart';
```

Variables, constants, parameters, and named parameters should be in lowerCamelCase.

```
var item;
const cost = 3.14;
final urlScheme = RegExp('^([a-z]+):');
void sum(int price) {
 // ...
}
```

