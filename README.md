## Flutter Best Practice Guidelines

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

Here is a nice presentation of the above Flutter best practices with examples:

**1. Avoid Functional Widgets**

**Why:** Functional widgets can make it difficult to inspect your widget tree and can be less performant than class widgets.

**Example:**

```
// Functional widget
Widget functionWidget({Widget child}) {
  return Container(child: child);
}

// Class widget
class ClassWidget extends StatelessWidget {
  final Widget child;

  const ClassWidget({Key? key, required this.child}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(child: child);
  }
}
```

**Usage:**

```
// Functional widget
functionWidget(
  child: functionWidget(),
);

// Class widget
ClassWidget(
  child: ClassWidget(),
);
```

**Widget tree:**

```
// Functional widget
Container
  Container

// Class widget
ClassWidget
  Container
    ClassWidget
      Container
```

As you can see, the widget tree for the functional widget is less nested than the widget tree for the class widget. This is because the functional widget is simply creating a new container to wrap the child widget. The class widget, on the other hand, is creating a new widget to wrap the child widget.

The extra nesting in the widget tree can make it more difficult to inspect and debug your code. It can also lead to performance issues, as the Flutter engine has to render each widget in the tree.

**Recommendation:** Use class widgets instead of functional widgets whenever possible.

**2. Specify Types for variables**

**Why:** Specifying types for variables can make your code more readable, maintainable, and less prone to errors.

**Example:**

```
// Without type specification
var count = 10;
final person = Person();
const timeOut = 6000;

// With type specification
final int count = 10;
final Person person = Person();
const int timeOut = 6000;
```

**Recommendation:** Specify types for variables whenever possible.

**3. Use `is` instead of `as`**

**Why:** Using the `is` operator to check if a variable is of a certain type is more robust than using the `as` operator to cast a variable to a certain type.

**Example:**

```
// Using `as`
(item as Person).name = 'John Doe';

// Using `is`
if (item is Person) {
  item.name = 'John Doe';
}
```

**Recommendation:** Use the `is` operator to check if a variable is of a certain type before casting it to that type.

**4. Use `ItemExtent` in `ListView` if you have longer lists**

**Why:** Specifying the `itemExtent` property of a `ListView` can improve its performance if it has a large number of items.

**Example:**

```
// Without `itemExtent`
ListView(
  children: List.generate(10000, (index) => Text('Index: $index')),
)

// With `itemExtent`
ListView(
  itemExtent: 600,
  children: List.generate(10000, (index) => Text('Index: $index')),
)
```

**Recommendation:** Specify the `itemExtent` property of a `ListView` if it has a large number of items.

**5. Use `??` and `.?.` operators**

**Why:** The `??` and `.?.` operators can be used to make your code more concise and less prone to errors.

**Example:**

```
// Without `??` and `.?.`
var done = null;

bool isValid = done != null ? true : false;

String name = person != null ? person.name : null;

// With `??` and `.?.`
bool isValid = done ?? false;

String name = person?.name;
```

**Recommendation:** Use the `??` and `.?.` operators to make your code more concise and less prone to errors.

