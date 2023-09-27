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

**Avoid Functional Widgets**

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

**Specify Types for variables**

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

**Use `is` instead of `as`**

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

**Use `ItemExtent` in `ListView` if you have longer lists**

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

**Use `??` and `.?.` operators**

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
**Use Raw Strings**

Raw strings allow you to write strings in your code without having to escape certain characters, such as backslashes and dollar signs. This can make your code more readable and easier to maintain.

To use a raw string, simply prefix the string with the `r` character. For example:

```
var s = r'This is a raw string. I don\'t have to escape backslashes \ or dollar signs $.';
```

**Avoid `print()` calls**

The `print()` function is used to print output to the console. However, it is generally not recommended to use `print()` in production code. This is because `print()` can be slow and can also pollute the console with output that is not relevant to the user.

Instead of using `print()`, you should use a logging library such as `debugPrint()` or `log()`. These libraries provide more control over how and when output is logged.

To use `debugPrint()`, simply pass the string you want to print to the `debugPrint()` function. For example:

```
debugPrint('This is a debug message.');
```

To use `log()`, you need to first import the `dart:developer` library. Then, you can call the `log()` function with the string you want to log. For example:

```
import 'dart:developer';

log('This is a log message.');
```

**Use `print` Statements only in Debug Mode**

If you do need to use `print()` statements in your code, it is a good practice to only use them in debug mode. This will help to prevent them from polluting the console in production code.

To check if your code is running in debug mode, you can use the `kDebugMode` constant. For example:

```
if (kDebugMode) {
  print('This is a debug message.');
}
```

**Use leading underscore (‘_’) for private variables**

Private variables are variables that are only accessible to the class in which they are defined. To make a variable private, simply prefix the variable name with an underscore (`_`).

For example:

```
class MyClass {
  final int _myPrivateVariable = 10;

  int get myPublicVariable {
    return _myPrivateVariable;
  }
}
```

In this example, the `_myPrivateVariable` variable is private. It can only be accessed by methods within the `MyClass` class. The `myPublicVariable` getter method can be used to access the `_myPrivateVariable` variable from outside the `MyClass` class.

**Use `final` / `const` keyword for unmodified variables**

The `final` keyword is used to declare a variable that can only be assigned a value once. The `const` keyword is used to declare a constant variable, which is a variable whose value cannot be changed.

You should use the `final` or `const` keyword for any variable that will not be modified after it is assigned a value. This can make your code more readable and easier to maintain.

For example:

```
final String myName = 'John Doe';
const int myAge = 30;
```

In this example, the `myName` variable is final and the `myAge` variable is constant. The value of neither variable can be changed after it is assigned a value.




Here is a more detailed explanation of the Flutter best practices you mentioned:

**Use `final`/`const` keyword for unmodified variables**

The `final` keyword is used to declare a variable that can only be assigned a value once. The `const` keyword is used to declare a constant variable, which is a variable whose value cannot be changed.

You should use the `final` or `const` keyword for any variable that will not be modified after it is assigned a value. This can make your code more readable and easier to maintain.

For example:

```
final String myName = 'John Doe';
const int myAge = 30;
```

In this example, the `myName` variable is final and the `myAge` variable is constant. The value of neither variable can be changed after it is assigned a value.

**Don't use `+` for concatenating strings, use string interpolation**

String concatenation using the `+` operator can be slow and inefficient, especially when concatenating long strings. Instead of using `+`, you should use string interpolation.

To use string interpolation, simply enclose the variables you want to concatenate in double curly braces (`{{}}`). For example:

```
final String firstName = 'John';
final String text = '$firstName Doe';
```

The `text` variable will contain the string `John Doe`.

**Don't create a lambda when a tear-off will do**

A tear-off is a way of calling a method on an object without having to explicitly create the object. Tear-offs are more efficient than lambdas, so you should use them whenever possible.

For example, the following code uses a lambda to call the `print()` method on each element of a list:

```
List<String> names = [];

// Don't
names.forEach((name) {
  print(name);
});
```

This can be rewritten using a tear-off as follows:

```
List<String> names = [];

// Do
names.forEach(print);
```

The `forEach()` method expects a function that takes a single argument, which is the element of the list. In this case, we are passing the `print()` method as a tear-off.

**Using `async`/`await` more readable way**

The `async`/`await` keywords can make your code more readable and easier to maintain by allowing you to write asynchronous code in a sequential style.

For example, the following code uses callbacks to get the number of users and then handle any errors:

```
Future<int> getUsersCount() {
  return getUsers().then((users) {
    return users?.length ?? 0;
  }).catchError((e) {
    log.error(e);
    return 0;
  });
}
```

This can be rewritten using `async`/`await` as follows:

```
Future<int> getUsersCount() async {
  try {
    var users = await getActiveUser();
    return users?.length ?? 0;
  } catch (e) {
    log.error(e);
    return 0;
  }
}
```

This version of the code is more readable and easier to maintain because it is written in a sequential style.

**Use `ListView.builder` for a building list with same views.**

The `ListView.builder()` constructor creates a `ListView` that builds its rows on demand. This can be more efficient than using the `ListView()` constructor, especially for long lists.

The `ListView.builder()` constructor takes three arguments:

* The number of items in the list.
* A builder function that takes an index and returns a widget.
* An optional itemExtent, which is the height of each item in the list.

For example, the following code creates a `ListView` with 100 items:

```
ListView.builder(
  itemCount: 100,
  itemBuilder: (context, index) {
    return Text('Item $index');
  },
)
```

The `itemBuilder` function is called for each item in the list. It takes an index and returns a widget, which is displayed in the `ListView`.


**Don't explicitly initialize variables to null**

In Dart, variables are automatically initialized to null when their value is not specified, so explicitly initializing a variable to null is unnecessary and redundant.

**Example:**

```dart
// Don't
int _val = null;

// Do
int? _val;
```

The `int?` type indicates that the variable `_val` may be null. This is more descriptive and can help to prevent bugs.

**Use expression function bodies**

For functions that contain just one expression, you can use an expression function. The `=>` (arrow) notation is used for expression function bodies.

**Example:**

```dart
// Don't
get name {
  return '$firstName $lastName';
}
Widget getLoading() {
  return CircularProgressIndicator(
    valueColor: AlwaysStoppedAnimation<Color>(Colors.blue),
  );
}

// Do
get name => '$firstName $lastName';
Widget getLoading() => CircularProgressIndicator(
      valueColor: AlwaysStoppedAnimation<Color>(Colors.blue),
);
```

Expression function bodies are more concise and easier to read.

**Use cascading operator**

The cascading operator (`..`) allows you to perform a sequence of operations on the same object without having to repeat the object name each time.

**Example:**

```dart
// Don't
var path = Path();
path.lineTo(0, size.height);
path.lineTo(size.width, size.height);
path.lineTo(size.width, 0);
path.close();

// Do
var path = Path()
..lineTo(0, size.height)
..lineTo(size.width, size.height)
..lineTo(size.width, 0)
..close();
```

The cascading operator makes the code more concise and easier to read.

**Use spread collections**

Spread collection syntax allows you to easily add existing items to a new collection.

**Example:**

```dart
// Don't
var y = [4,5,6];
var x = [1,2];
x.addAll(y);

// Do
var y = [4,5,6];
var x = [1,2,...y]; // 1,2,4,5,6
```

Spread collection syntax makes the code more concise and easier to read.

**Use literal to initialize growable collections**

It is recommended to use literal syntax to initialize growable collections, such as lists and maps. This makes the code more concise and easier to read.

**Example:**

```dart
// Good
var points = [];
var addresses = {};

// Bad
var points = List();
var addresses = Map();
```

**Use Colors in a separate file**

It is a good practice to have all of the colors in your application in a single file. This makes it easier to manage and update your colors.

**Example:**

```dart
class AppColor {
  static const Color red = Color(0xFFFF0000);
  static const Color green = Color(0xFF4CAF50);
  static const Color errorRed = Color(0xFFFF6E6E);
}
```

**Use Dart Code Metrics**

Dart Code Metrics is a static code analysis tool that can help you to improve the quality of your Flutter code. It provides a number of metrics that you can use to identify and address potential problems.

To use Dart Code Metrics, simply run the following command in your terminal:

```
dart analyze
```

This will generate a report that lists any potential problems in your code.

**Flutter Security Best Practices**

Security is an important consideration for any mobile app. Flutter provides a number of security features, but it is important to use them correctly.

Here are some Flutter security best practices:

* **Use code obfuscation.** Code obfuscation makes it more difficult for attackers to reverse engineer your app and steal your code or data.
* **Prevent background snapshots.** Background snapshots can reveal sensitive information about your app's state. To prevent background snapshots, use the `secure_application` Flutter package.
* **Use the underscore (_).** The underscore (_) can be used to indicate that a variable is not used. This can help to prevent bugs and make your code more readable.

**Example:**

```dart
someFuture.then((_) => someFunc());
```

In this example, the underscore indicates that the variable `DATA_TYPE VARIABLE` is not used. This makes the code more readable and can help to prevent bugs.


