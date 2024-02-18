# DART Package
If a folder contains a pubsec.yaml file then only we can call that folder as a package.
Dart Package is a dart project that can be used by other dart projects.

![pubsec](images/pubsec.png)


If you want to use any package in your project then add that package as a dependency in pubsec file and run pub get command to fetch the package.
pubse.lock contains all the details of your downlaoded packages.

# **Dependencies in DART**

There are 4 types of dependencies in dart:
1. Immediated Dependency
2. Transitive Dependency
3. Regular Dependencies: These dependencies are used during bot development and production phase 
4. Dev Dependencies: These dependencies are used only during the development phase.

![Immediate and Transitive Dependency](images/dependency.png)