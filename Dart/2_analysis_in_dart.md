# Analysis In Dart

Place the analysis options file, analysis_options.yaml, at the root of the package, in the same directory as the pubspec file.

Here’s a sample analysis options file:

```dart
include: package:lints/recommended.yaml

analyzer:
  exclude: [build/**]
  language:
    strict-casts: true
    strict-raw-types: true

linter:
  rules:
    - cancel_subscriptions
```

1. **Use include:** url to bring in options from the specified URL—in this case, from a file in the lints package. Because YAML doesn’t allow duplicate keys, you can include at most one file.
2. **Use the analyzer:** entry to customize static
   analysis: enabling stricter type checks, excluding files, ignoring specific rules, changing the severity of rules, or enabling experiments.
3. **Use the linter:** entry to configure linter rules.

If the analyzer can’t find an analysis options file at the package root, it walks up the directory tree, looking for one. If no file is available, the analyzer defaults to standard checks.

Consider the following directory structure for a large project:
![Analysis](images/analysis.png)

The analyzer uses file #1 to analyze the code in my_other_package and my_other_other_package, and file #2 to analyze the code in my_package.

## **Enabling stricter type checks**

If you want stricter static checks than the Dart type system requires, consider enabling the strict-casts, strict-inference, and strict-raw-types language modes:

```dart
analyzer:
  language:
    strict-casts: true
    strict-inference: true
    strict-raw-types: true
```

You can use the modes together or separately; all default to false.

## strict-casts: bool

A value of true ensures that the type inference engine never implicitly casts from dynamic to a more specific type. The following valid Dart code includes an implicit downcast from the dynamic value returned by jsonDecode to List[String] that could fail at runtime. This mode reports the potential error, requiring you to add an explicit cast or otherwise adjust your code.

```dart
void foo(List<String> lines) {
  ...
}

void bar(String jsonText) {
  foo(jsonDecode(jsonText)); // Implicit cast
}
```

> error - The argument type 'dynamic' can't be assigned to the parameter type 'List<String>'. - argument_type_not_assignable

## strict-inference: bool

A value of true ensures that the type inference engine never chooses the dynamic type when it can’t determine a static type. The following valid Dart code creates a Map whose type argument cannot be inferred, resulting in an inference failure hint by this mode:

```dart
final lines = {}; // Inference failure
lines['Dart'] = 10000;
lines['C++'] = 'one thousand';
lines['Go'] = 2000;
print('Lines: ${lines.values.reduce((a, b) => a + b)}'); // Runtime error
```

> info - The type argument(s) of 'Map' can't be inferred - inference_failure_on_collection_literal
> The strict-inference mode can identify many situations which result in an inference failure.

See Conditions for strict inference failure for an exhaustive list of inference failure conditions.

## strict-raw-types: bool

A value of true ensures that the type inference engine never chooses the dynamic type when it can’t determine a static type due to omitted type arguments. The following valid Dart code has a List variable with a raw type, resulting in a raw type hint by this mode:

```dart
List numbers = [1, 2, 3]; // List with raw type
for (final n in numbers) {
  print(n.length); // Runtime error
}
```

> info - The generic type 'List[dynamic]' should have explicit type arguments but doesn't - strict_raw_type

# **Lint Rules**

The Dart team provides two sets of recommended linter rules in the lints package:

1. **Core rules :**
   Help identify critical issues that are likely to lead to problems when running or consuming Dart code. All code should pass these linter rules. Packages that are uploaded to pub.dev have a package score that’s based in part on passing these rules.
2. **Recommended rules:**
   Help identify additional issues that may lead to problems when running or consuming Dart code, and enforce a single, idiomatic style and format. We recommend that all Dart code use these rules, which are a superset of the core rules.

To enable either of the rules add lynt package in your pubsec file and then import the lynt package in your analysis file

> **NOTE:** If writing flutter code then instead of using lynt package use flutter_lynt.

### **How to add rules**

In analysis file add all the rules that you want inside **linter:**

```dart
int calculate() {
  print("hellow"); //warning:Only use double quotes for strings containing single quotes
  print('hellow');
  return 6 * 7;
}
```

### **How to disable a rule**

If you include an analysis options file such as the one in lints, you might want to disable some of the included rules. Disabling individual rules is similar to enabling them, but requires the use of a map rather than a list as the value for the rules: entry, so each line should contain the name of a rule followed by either : false or : true.

Here’s an example of an analysis options file that uses all the recommended rules from lints except avoid_shadowing_type_parameters. It also enables the lint await_only_futures:

```dart
include: package:lints/recommended.yaml

linter:
  rules:
    avoid_shadowing_type_parameters: false
    await_only_futures: true
```

## **Excluding files**

To exclude files from static analysis, use the exclude: analyzer option. You can list individual files, or use glob syntax:

```dart
analyzer:
  exclude:
    - lib/client.dart
    - lib/server/*.g.dart
    - test/_data/**
```

## **Suppressing rules for a file**

To ignore a specific non-error rule for a specific file, add an ignore_for_file comment to the file:

```dart
// ignore_for_file: unused_local_variable
```

This acts for the whole file, before or after the comment, and is particularly useful for generated code.

### To suppress more than one rule, use a comma-separated list:

```dart
// ignore_for_file: unused_local_variable, duplicate_ignore, dead_code
```

### To suppress all linter rules, add a type=lint specifier:

```dart
// ignore_for_file: type=lint
```

> Version note: Support for the type=lint specifier was added in Dart 2.15.

### Suppressing rules for a line of code

To suppress a specific non-error rule on a specific line of code, put an ignore comment above the line of code. Here’s an example of ignoring code that causes a runtime error, as you might do in a language test:

```dart
// ignore: invalid_assignment
int x = '';
```

### To suppress more than one rule, supply a comma-separated list:

```dart
// ignore: invalid_assignment, const_initialized_with_non_constant_value
const x = y;
```

Alternatively, append the ignore rule to the line that it applies to:

int x = ''; // ignore: invalid_assignment

# Resources

1. [Official Dart Documentation ](https://dart.dev/guides/language/analysis-options#the-analysis-options-file)
