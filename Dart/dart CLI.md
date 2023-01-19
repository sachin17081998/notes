# Dart CLI

The dart tool (bin/dart) is a command-line interface to the Dart SDK. The tool is available no matter how you get the Dart SDK—whether you download the Dart SDK explicitly or download only the Flutter SDK.
> when your **dart** coammnd in terminal you will see all available CLI tool offered by dart

```dart
E:\codes\dart code>dart
A command-line utility for Dart development.

Usage: dart <command|dart-file> [arguments]

Global options:
-h, --help                 Print this usage information.
-v, --verbose              Show additional command output.
    --version              Print the Dart SDK version.
    --enable-analytics     Enable analytics.
    --disable-analytics    Disable analytics.

Available commands:
  analyze    Analyze Dart code in a directory.
  compile    Compile Dart to various formats.
  create     Create a new Dart project.
  devtools   Open DevTools (optionally connecting to an existing application).
  doc        Generate API documentation for Dart projects.
  fix        Apply automated fixes to Dart source code.
  format     Idiomatically format Dart source code.
  migrate    Perform null safety migration on a project.
  pub        Work with packages.
  run        Run a Dart program.
  test       Run tests for a project.

Run "dart help <command>" for more information about a command.
See https://dart.dev/tools/dart-tool for detailed documentation
```

>when you run **dart create** yoou can see additional options that are available with create command

while creating any dart project we need to select a template.
> dart create -t template-name file-name

```dart
E:\codes\dart code>dart create -t console first_console_dart_app
Creating first_console_dart_app using template console...

  .gitignore
  analysis_options.yaml
  CHANGELOG.md
  pubspec.yaml
  README.md
  bin\first_console_dart_app.dart
  lib\first_console_dart_app.dart
  test\first_console_dart_app_test.dart
  ```

  this will create a folder with name **first_console_dart_app** containg above listed files.

  ### How to run and debug the project

  1. To run the project first go into the project and then run the project
  ```dart
    cd first_console_dart_app
    dart run
  ```


  ![Dart Project Structure](images/dart_file_structure.png 'Dart File Structure')