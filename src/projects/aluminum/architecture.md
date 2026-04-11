(Needs proofreading! Written by: Dhruva)

# Codebase Overview

Aluminum is built using the [Flutter framework](https://flutter.dev/), a framework made by Google
for building modern cross-platform GUI apps. We only target Windows currently, but the app has been
shown to build successfully on MacOS and Linux and would likely be usable on other platforms as well.

Aluminum is currently split into several branches to keep game-specific code isolated, since we may
add specific displays to the dashboard or other changes which are only useful for one season.
A basic template and any utilities which should be shared across different games should be kept on the main branch.
Separate branches are created for the specific configurations used for each game (e.g. 2026-main).

## About Flutter

Flutter uses Dart as its primary scripting language, although it uses a C++ engine on the
backend. Dart is similar to Java as it is also run inside a virtual machine and compiles
to bytecode (well sort of, usually... see [here](https://dart.dev/tools/dart-compile) for more info),
although it has more modern syntax features such as
[null safety](https://dart.dev/null-safety/understanding-null-safety), optional support
for dynamic types, and is organized by "libraries" instead of classes, allowing you to
have functions or variables which are not associated with a class for more functional
programming. Dart also has better type inference---while Java allows you to declare local
variables with `var`, it is not very commonly used. In Dart, you will often see variables
declared with only `var` or `final` and the compiler will infer the variable's type. Dart
has similar object-oriented features, although it does not have explicit public or private
modifiers, however, prefixing a class or function with an underscore will make it inaccessible
outside of the current library.

For help getting started with Flutter, you should look at the [official docs](https://docs.flutter.dev),
where there are beginner tutorials, videos, API docs for the full library, and more.

Building apps with Flutter requires the Flutter SDK to be installed, which comes with a command-line tool
for building and running Flutter projects. As of right now, this is only installed on Laptop #8
but this may change in the future. There is also a VS Code extension for running flutter directly from
VS Code. A guide to installing the SDK can be found [here](https://docs.flutter.dev/install/quick) in
official documentation.

## NTCore Bindings and FFIGen

This project uses Dart FFI (Foreign Function Interface) bindings to interface with the NTCore library, part of WPILib.
This library has a C API
which can easily be called from other languages, like Dart. Dart bindings were automatically generated using the ffigen
package, creating lib/ntcore/ntcore.g.dart. The main app code shouldn't directly use these bindings---instead,
go through the classes provided in lib/ntcore/instance.dart, which have all been documented and have methods which can
safely and easily be called from dart without interacting with native memory. You can easily add extra methods to NTInstance
or create another class if you need to access other parts of the C library which do not currently have safe dart bindings written
for them.

In order to regenerate the bindings, run tool/ffigen.dart (`dart run tool/ffigen.dart`). You may need to provide the location
of the C standard library headers, which it for some reason can't find sometimes (linux error, no clue if this happens on windows :P ),
so locate wherever those headers are on your
system and set your CPATH environment variable to that or temporarily add it to the compiler arguments in tool/ffigen.dart.

You may need to update the bindings if WPILib changes or adds to the NTCore C API. To do this, download the headers (the easiest
place to get them is [wpilib's maven releases](https://frcmaven.wpi.edu/ui/native/release). Go to the ntcoreffi releases, where there
is a .zip file containing all the headers. Unzip all the NTCore headers into ntcore_headers/include, replacing the old files. Then,
follow the above instructions to regenerate the bindings, and make sure there are no new errors and implement any new functionality.

Dart bindings for NTCore have been made in `lib/ntcore`, which contain several classes and methods to perform
common functions without having to directly interface with native functions and handle things like memory
allocation. `lib/ntcore/instance.dart` contains the `NTInstance` and `NTValueNotifier` classes which handle most of
this functionality. `lib/ntcore/library_link.dart` contains code necessary to load NTCore at startup and
`lib/ntcore/values.dart` contains the `NTValue` class, which is a [sealed class](https://www.darttutorial.org/dart-tutorial/dart-sealed-classes/)
that can be used to handle the different types of values in NetworkTables.

NTCore uses pointers to the WPI_String struct for strings. You can convert to/from dart strings using toWpiString and wpiToDartString methods. If you don't have a pointer to a WPI_String struct you can also cast the str field to a Utf8 pointer and then call toDartString() with the length from the len field.

## Using the NTCore Bindings from Dart

The app should make a single instance of NTInstance (ntcore/instance.dart), which is the main class responsible for
communication with NetworkTables. Then, use updateConnectionSettings or updateServerNamePort to connect to a specific NT server.
You can either connect to the rio via team number and port 5810, or to a sim using localhost:5810.
The NTInstance will then keep track of any entry handles in use to publish/subscribe to avoid memory leaks and keep publishers alive.

The NetworkTablesValue class is a sealed class with subclasses for each value type in NT. You can use a switch statement to check what type a value is or use an if statement to check if it matches a certain type. Also, the toString method will return an appropriate string representation of the value, whatever type it is.

The NTValueNotifier class is used to provide a ChangeNotifierProvider object which notifies listeners any time the value at a certain path in NetworkTables changes. Internally, NTInstance polls listeners and updates them in a loop, and this is how these updates are handed out.
The easiest way to create new NTValueNotifiers is to use the .fromName factory, which either creates a new one or returns an existing listener for that entry.
We keep most of the paths and notifiers (and the NTInstance) used as global variables in one file (lib/ntreferences.dart).
There is also an NTPrefixNotifier class which tracks a map of all the values under a certain prefix.

## Where to find specific things

- lib/screens contains files for each one of the screens on the dashboard - the main dash, settings, motor tester panel, etc. Each just has a class extending Widget which contains all the logic for that screen.
- lib/widgets contains some specific widgets used such as the field view widget and the auto chooser widget.
- main.dart is the entry point and has the main app and scaffold as well as the side drawer. It also maintains a list of all the screens, labels, and icons for each one - if you're adding a new screen make sure to add it to that list.
- util.dart contains some random things
- settings.dart contains the logic for saving, loading, and accessing settings to/from json files.

## The settings system

This system is contained entirely inside lib/settings.dart.

Settings are stored in a settings.json file in the app's config directory (the appropriate
directory for the current platform is fetched by the app_dirs package). This is loaded into
the Settings class at startup inside an instane of the Settings class. In order to change the settings,
you can first make a copy of the current settings using `Settings.copyInstance()`, modify it,
and then overwrite the settings by calling `Settings.overwriteSettings()`. The Settings class
is converted using dart:convert from the standard library into JSON automatically.

Reading from settings is done using the static getters on Settings.
