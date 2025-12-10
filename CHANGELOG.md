# Change Log

## 0.11.0

### New Features
- **Auto Enum Detection**: Enums are detected automatically; no `// enum` comment needed.
- **Case-Insensitive Enum Matching**: `fromMap` now matches enum names regardless of casing (e.g., `ACTIVE`, `Active`).
- **`fromMap.use_as_cast` Default On**: `as` casting is enabled by default for cleaner null-safe code.

### Improvements
- **Nullable Enum Safety**: Nullable enums use `firstWhereOrNull` and auto-import `package:collection/collection.dart` when needed.
- **Cleaner `fromMap`**: Removed the extra `convertedMap` indirection; direct map access is used everywhere.
- **Enum Serialization**: Uses the non-nullable enum type for lookups and only applies `?.name` when the enum itself is nullable.

## 0.10.0

### New Features
- **Customizable Quick Fixes**: Added `quick_fixes.show` setting to customize which quick fixes appear in the quick fix menu
  - Select only the quick fixes you need (e.g., just `constructor` and `copyWith`)
  - Prevents duplicate selections in VS Code settings
  - Options: `dataClass`, `constructor`, `copyWith`, `toMap`, `fromMap`, `toJson`, `fromJson`, `toString`, `stringify`, `equality`, `useEquatable`
- **Smart Quick Fixes**: Quick fixes now automatically hide for methods that already exist in the class
  - If `toString()` already exists, "Generate toString" won't appear
  - Keeps the quick fix menu clean and relevant
- **Equatable Stringify Support**: Added `toString.useStringify` setting
  - When enabled and Equatable is used, generates `@override bool? get stringify => true;` instead of `toString()` method
  - Quick fix "Generate stringify" available when Equatable is detected
  - Automatically switches between `toString()` and `stringify` based on setting and Equatable usage

### Improvements
- **Enhanced fromMap Default Values**: `fromMap.default_values` setting now controls default values for both nullable and required (non-nullable) properties
  - When `false` (default): Required properties won't get default values if missing from map
  - When `true`: Required properties will get default values if missing from map

### Bug Fixes
- Fixed duplicate quick fix selections in VS Code settings UI
- Quick fixes now respect method existence - won't show for already-generated methods

## 0.9.0

### Major Fixes
- Fixed collection equality operators for non-Flutter projects ([issues #6, #23](https://github.com/ricardoemerson/dart-data-class-tools/issues/6))
  - Now uses `ListEquality().equals`, `MapEquality().equals`, `SetEquality().equals` from `package:collection/collection.dart`
  - Flutter projects continue using `listEquals`, `mapEquals`, `setEquals` from `package:flutter/foundation.dart`
- Fixed null checks for collections in `fromMap()` ([issue #13](https://github.com/ricardoemerson/dart-data-class-tools/issues/13))
  - Added proper null handling for nullable collections
  - Non-nullable collections now default to empty collections when null
- Fixed LinkedMap type errors ([issue #19](https://github.com/ricardoemerson/dart-data-class-tools/issues/19))
  - Added `Map<String, dynamic>.from()` conversion in both `fromMap` and `fromJson`
  - Handles `LinkedMap` returned by `json.decode()`
- Improved DateTime serialization ([issue #22](https://github.com/ricardoemerson/dart-data-class-tools/issues/22))
  - Now uses ISO 8601 format: `toUtc().toIso8601String()` instead of milliseconds
  - Deserialization uses `DateTime.parse().toLocal()`
- Fixed enum serialization ([issue #7](https://github.com/ricardoemerson/dart-data-class-tools/issues/7))
  - Enums now serialize as string names (`.name`) instead of indices
  - Deserialization uses `firstWhere` to parse from string names
- Fixed copyWith for sealed class subclasses ([issue #8](https://github.com/ricardoemerson/dart-data-class-tools/issues/8))
  - Added detection for sealed classes and their subclasses
  - copyWith now works correctly for subclasses of sealed classes
- Fixed JSON file separation ([issue #20](https://github.com/ricardoemerson/dart-data-class-tools/issues/20))
  - Cross-platform path handling using Node.js `path` module
  - Works correctly on Windows, macOS, and Linux
- Separated serialization methods in quick fixes ([issue #1](https://github.com/ricardoemerson/dart-data-class-tools/issues/1))
  - toMap, fromMap, toJson, and fromJson are now separate quick fix options

### New Features
- Automatic const constructors
  - Constructors are automatically marked as `const` when all properties are `final`
- Blank line before constructor
  - Added blank line between properties and constructor for better code formatting
- Improved comment handling
  - Comments are now properly ignored when detecting property modifiers

### Technical Improvements
- Better null safety handling for collections
- Improved DateTime field type handling
- Enhanced enum detection and serialization
- Cross-platform file path handling

## 0.8.1

- Revert the previous behavior.

## 0.8.0

- Simplify use of ValueGetter using `x?.call() ?? this.x` instead `x != null ? x() : this.x`. Thanks to [Petr Nymsa](https://github.com/petrnymsa).

## 0.7.0

- Update `List<Object> props = []` to `List<Object?> props = []` when any type of attribute has a nullable value when generates Equatable.

## 0.6.0

- Added support for uses ValueGetter for nullable types when generates copyWith.
- Added the setting `copyWith.usesValueGetter` to enable/disable uses of ValueGetter for nullable types when generates copyWith.

## 0.5.7

- Updated the badges in README.md.

## 0.5.2

Added support for snake_case json keys
Generate toString() when converting with Equatable
Other improvements

## 0.5.0

Added support for enums
Use factory constructors instead of static methods for json serialization

### 0.3.17

Added support for value equality on `Lists`, `Maps` and `Sets`.

### 0.3.16

Class fields can now also be declared after the constructor.
Minor improvements.

### 0.3.6 - 0.3.15

Fixed some bugs.

### 0.3.5

Added support for equatable by setting dart-data-class-generator.useEquatable to true.

Changed setting `dart-data-class-generator.constructor` to `dart-data-class-generator.constructor.enabled`
Changed setting `dart-data-class-generator.copyWith` to `dart-data-class-generator.copyWith.enabled`
Changed setting `dart-data-class-generator.toMap` to `dart-data-class-generator.toMap.enabled`
Changed setting `dart-data-class-generator.fromMap` to `dart-data-class-generator.fromMap.enabled`
Changed setting `dart-data-class-generator.toJson` to `dart-data-class-generator.toJson.enabled`
Changed setting `dart-data-class-generator.fromJson` to `dart-data-class-generator.fromJson.enabled`
Changed setting `dart-data-class-generator.toString` to `dart-data-class-generator.toString.enabled`
Changed setting `dart-data-class-generator.equality` to `dart-data-class-generator.equality.enabled`
Changed setting `dart-data-class-generator.hashCode` to `dart-data-class-generator.hashCode.enabled`

## 0.3.0

Added quick fixes.

## 0.2.0

Added support for @required annotation.
Changed the default hashCode implementation to bitwise operator.

## 0.1.0

Initial release (Beta).
