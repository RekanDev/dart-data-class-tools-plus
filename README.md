# Dart Data Class Tools Plus

![Visual Studio Marketplace Version](https://img.shields.io/visual-studio-marketplace/v/RekanDev.dart-data-class-tools-plus.svg?style=flat-square)
![Visual Studio Marketplace Installs](https://img.shields.io/visual-studio-marketplace/i/RekanDev.dart-data-class-tools-plus.svg?style=flat-square)
![Visual Studio Marketplace Downloads](https://img.shields.io/visual-studio-marketplace/d/RekanDev.dart-data-class-tools-plus.svg?style=flat-square)
![Visual Studio Marketplace Rating](https://img.shields.io/visual-studio-marketplace/r/RekanDev.dart-data-class-tools-plus.svg?style=flat-square)
[![GitHub](https://img.shields.io/github/stars/RekanDev/dart-data-class-tools-plus.svg?style=flat-square)](https://github.com/RekanDev/dart-data-class-tools-plus)

Create dart data classes easily, fast and without writing boilerplate or running code generation.

> This project is an enhanced fork of [Dart Data Class Generator](https://github.com/ricardoemerson/dart-data-class-tools) with additional bug fixes and features.

# What's new in Dart Data Class Tools Plus 0.9.0 🎉

## Major Fixes

- **Fixed collection equality operators** - Now correctly uses `ListEquality().equals`, `MapEquality().equals`, `SetEquality().equals` for non-Flutter projects (fixes [issues #6, #23](https://github.com/ricardoemerson/dart-data-class-tools/issues/6))
- **Fixed null checks for collections** - Added proper null handling for nullable collections in `fromMap()` (fixes [issue #13](https://github.com/ricardoemerson/dart-data-class-tools/issues/13))
- **Fixed LinkedMap type errors** - Properly converts `LinkedMap` to `Map<String, dynamic>` in `fromMap` and `fromJson` (fixes [issue #19](https://github.com/ricardoemerson/dart-data-class-tools/issues/19))
- **Improved DateTime serialization** - Now uses ISO 8601 format (`toUtc().toIso8601String()`) instead of milliseconds (fixes [issue #22](https://github.com/ricardoemerson/dart-data-class-tools/issues/22))
- **Fixed enum serialization** - Enums now serialize as string names instead of indices (fixes [issue #7](https://github.com/ricardoemerson/dart-data-class-tools/issues/7))
- **Fixed copyWith for sealed classes** - copyWith now works correctly for subclasses of sealed classes (fixes [issue #8](https://github.com/ricardoemerson/dart-data-class-tools/issues/8))
- **Fixed JSON file separation** - Cross-platform path handling for separating JSON into multiple files (fixes [issue #20](https://github.com/ricardoemerson/dart-data-class-tools/issues/20))
- **Separated serialization methods** - toMap, fromMap, toJson, and fromJson are now separate quick fix options (fixes [issue #1](https://github.com/ricardoemerson/dart-data-class-tools/issues/1))

## New Features

- **Automatic const constructors** - Constructors are automatically marked as `const` when all properties are `final`
- **Blank line before constructor** - Added blank line between properties and constructor for better code formatting
- **Improved comment handling** - Comments are now properly ignored when detecting property modifiers (e.g., `int count; // Not final`)

## Features

The generator can generate the constructor, copyWith, toMap, fromMap, toJson, fromJson, toString, operator == and hashCode methods for a class based on [class properties](#create-data-classes-based-on-class-properties) or [raw JSON](#create-data-classes-based-on-json-beta).

Additionally the generator has a couple of useful quick fixes to speed up your development process. See the [Additional Features Section](#additional-features) for more.

If this extension is helpful to you, consider giving it a star on [GitHub](https://github.com/RekanDev/dart-data-class-tools-plus) or leave a review on the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=RekanDev.dart-data-class-tools-plus) :heart:

## Create Data Classes Based on Class Properties

### **Usage**

You can generate data classes either by the quick fix dialog or by running a command. In the quick fix dialog you have the option to not only generate whole data classes but also only specific methods. The command has the advantage of being able to generate multiple classes at the same time.

#### **Quick fix**

- Create a class with properties.
- Place your cursor on the first line of the class, the constructor or a field.
- Hit **CTRL + .** (or **CMD + .** on Mac) to open the quick fix dialog.
- Choose one of the available options:
  - Generate data class
  - Generate constructor
  - Generate copyWith
  - Generate toMap
  - Generate fromMap
  - Generate toJson
  - Generate fromJson
  - Generate toString
  - Generate equality
  - Generate Equatable

#### **Command**

- Create a class with properties.
- Hit **CTRL + P** (or **CMD + P** on Mac) to open the command dialog.
- Search for **Dart Data Class Tools Plus: Generate from class properties** and hit enter.
- When there are multiple classes in the current file, choose the ones you'd like to create data classes of in the dialog.

It is also possible to run the generator on an existing data class (e.g. when some parameters changed). The generator will then try to find the changes and replace the class with its updated version. **Note that custom changes to generated functions may be overridden**.

You can also customize the generator for example to use [Equatable](https://pub.dev/packages/equatable) for value equality. See the [Settings](#-settings) section for more options.

#### **Automatic const Constructors**

If all properties in a class are `final`, the constructor will automatically be marked as `const`:

```dart
class User {
  final String name;
  final int age;

  // Generated constructor will be:
  const User({
    required this.name,
    required this.age,
  });
}
```

#### **Enums**

In order for `enums` to be correctly serialized from and to JSON, please annotate them using a comment like so:
```dart
// enum
final Status status;
```

Enums are now serialized as string names (e.g., `status.name`) instead of indices for better readability and robustness.

#### Usage with Equatable

Although using the generator is fast, it still doesn't spare you from all the boiler plate necessary, which can be visually distracting. To reduce the amount of boiler plate needed, the generator works with **Equatable**. Just extend the class with `Equatable` or mix with `EquatableMixin` and the generator will use `Equatable` for value equality.

You can also use the setting `dart-data-class-generator.useEquatable`, if you always want to use `Equatable` for value equality.

#### Sealed Classes

The generator now supports generating `copyWith` methods for subclasses of sealed classes:

```dart
sealed class BaseEvent {}

class UserCreatedEvent extends BaseEvent {
  final String userId;
  final String email;
  
  // copyWith will be generated correctly
}
```

## Create Data Classes Based on JSON (Beta)

### **Usage**

- Create an **empty dart** file.
- Paste the **raw JSON without modifying it** into the otherwise empty file.
- Hit **CTRL + P** (or **CMD + P** on Mac) to open the command dialog.
- Search for **Dart Data Class Tools Plus: Generate from JSON** and hit enter.
- Type in a class name in the input dialog. This will be the name of the **top level class** if the JSON contains nested objects, all other class names will be inferred from the JSON keys.
- When there are nested objects in the JSON, a dialog will appear if you want to separate the classes into multiple files or if all classes should be in the same file.

> **Note:**
> **This feature is still in beta!**
> **Many API's return numbers like 0 or 1 as an integer and not as a double even when they otherwise are. Thus the generator may confuse a value that is usually a double as an int.**

## Additional Features

The extension includes some additional quick fixes that might be useful to you:

### Import refactoring

Sort imports alphabetically and bring them into the correct format easily.

## Settings

You can customize the generator to only generate the functions you want in your settings file.

* `dart-data-class-generator.json.key_format`: Whether to use snake_case or camelCase for the json keys. Options: `variable`, `camelCase`, `snake_case`.
* `dart-data-class-generator.quick_fixes`: If true, enables quick fixes to quickly generate data classes or specific methods only.
* `dart-data-class-generator.useEquatable`: If true, uses Equatable for value equality and hashCode.
* `dart-data-class-generator.fromMap.default_values`: If true, checks if a field is null when deserializing and provides a non-null default value.
* `dart-data-class-generator.constructor.default_values`: If true, generates default values for the constructor.
* `dart-data-class-generator.constructor.required`: If true, generates @required annotation for every constructor parameter. Note: The generator wont generate default values for the constructor if enabled!
* `dart-data-class-generator.json.separate`: Whether to separate a JSON into multiple files, when the JSON contains nested objects. Options: `ask` (choose manually every time), `separate` (always separate into multiple files), `current_file` (always insert all classes into the current file).
* `dart-data-class-generator.override.manual`: If true, asks, when overriding a class (running the command on an existing class), for every single function/constructor that needs to be changed whether the generator should override the function or not. This allows you to preserve custom changes you made to the function/constructor that would be otherwise overwritten by the generator.
* `dart-data-class-generator.constructor.enabled`: If true, generates a constructor for a data class.
* `dart-data-class-generator.copyWith.enabled`: If true, generates a copyWith function for a data class.
* `dart-data-class-generator.copyWith.usesValueGetter`: If true, uses ValueGetter for nullable types when generates copyWith.
* `dart-data-class-generator.toMap.enabled`: If true, generates a toMap function for a data class.
* `dart-data-class-generator.fromMap.enabled`: If true, generates a fromMap function for a data class.
* `dart-data-class-generator.toJson.enabled`: If true, generates a toJson function for a data class.
* `dart-data-class-generator.fromJson.enabled`: If true, generates a fromJson function for a data class.
* `dart-data-class-generator.toString.enabled`: If true, generates a toString function for a data class.
* `dart-data-class-generator.equality.enabled`: If true, generates an override of the == (equals) operator for a data class.
* `dart-data-class-generator.hashCode.enabled`: If true, generates a hashCode function for a data class.
* `dart-data-class-generator.hashCode.use_jenkins`: If true, uses the Jenkins SMI hash function instead of bitwise operator from dart:ui.

## Technical Improvements

### Collection Equality
- **Flutter projects**: Uses `listEquals`, `mapEquals`, `setEquals` from `package:flutter/foundation.dart`
- **Non-Flutter projects**: Uses `ListEquality().equals`, `MapEquality().equals`, `SetEquality().equals` from `package:collection/collection.dart`

### DateTime Serialization
- Serializes to ISO 8601 format: `toUtc().toIso8601String()`
- Deserializes from ISO 8601: `DateTime.parse().toLocal()`

### Enum Serialization
- Serializes as string names: `enumValue.name`
- Deserializes from string names using `firstWhere`

### Null Safety
- Proper null checks for nullable collections in `fromMap()`
- Default empty collections for non-nullable collections

### Cross-Platform Support
- Fixed file path handling for Windows, macOS, and Linux
- Proper handling of `LinkedMap` from JSON decoding

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

See [LICENSE.md](LICENSE.md) for details.
