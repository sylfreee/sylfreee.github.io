---
layout: post
title: "const" keyword in Dart
categories: [Dart]
---

***const*** keyword for constructors is for creating compile-time constant instances which helps Dart/Flutter application to be optimized.

Type ***const*** first when creating an object! 

## A compile-time constant value

A compile-time constant value is computed at compile-time. 

When you start a Dart/Flutter application, the application is loaded into memory and all constant values come into existence. these const instances are never garbage collected.

Dart/Flutter application gets a constant instance on the special cache like a hash map that stores objects by canonicalizing them.

## Benefits

Even if you create a class multiple times with ***const*** keyword and the same arguments, only one instance is created.

```dart
class UserId {
  final String value;
  const UserId(this.value);
}

void main() {
  print(identical(const UserId('example'), const UserId('example')));
  print(identical(UserId('example'), UserId('example')));
  print(identical(UserId('example'), const UserId('example')));
  print(identical(const UserId('example'), const UserId('hello')));
}
```

```shell
true
false
false
false
```

"*only one instance is created*" helps Flutter to rebuild only widgets that should be updated.

```dart
class _MyWidgetState extends State<MyWidget> {
  String title = "Title";

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(title), // Flutter rebuilds only this Text title!
      ),
      body: Column(
        children: <Widget>[
          const Text("Text 1"),
          const Padding(
            padding: const EdgeInsets.all(8.0),
            child: const Text("Another Text widget"),
          ),
          const Text("Text 3"),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        child: const Icon(Icons.add),
        onPressed: () {
          setState(() => title = 'New Title');
        },
      ),
    );
  }
}
```

### References

- ***[constant_reader.cc](https://github.com/dart-lang/sdk/blob/a24d875ff6aa7caf00bb4bbc11975cc206d9265b/runtime/vm/compiler/frontend/constant_reader.cc)***, in Dart SDK written in c++

- [dart - How does the const constructor actually work? - Stack Overflow](https://stackoverflow.com/questions/21744677/how-does-the-const-constructor-actually-work)
