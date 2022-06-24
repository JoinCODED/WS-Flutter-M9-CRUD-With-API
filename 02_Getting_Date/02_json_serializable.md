We will need two packages that will save us a lot of time and effort.

Install those packages:

```
flutter pub add json_serializable
flutter pub add build_runner
```

The first step is to import `json_serializable` package to our model file:

```dart
import 'package:json_annotation/json_annotation.dart';
```

Then we will add this import, and you will get the red underline that we all hate, but that's fine, it's because the file doesn't exist, it will be magically generated later!

```dart
part 'book.g.dart';
```

Notice that the `book` part should always be the name of the file you are in, for example, if we are doing this for the note model, it would be `note.g.dart`.

Just before the class definition, add this line:

```dart
@JsonSerializable()
class Book {
[...]
```

Then, at the end of your class add those lines:

```dart
  factory Book.fromJson(Map<String, dynamic> json) => _$BookFromJson(json);
  Map<String, dynamic> toJson() => _$BookToJson(this);
```

And like we mentioned, you will change the `Book` part to be the same as your model name.

Lastly run this command:

```shell
flutter pub run build_runner build
```

A new file magically appears in your `models` folder, it's called `book.g.dart` and you can find the generated code there.

You can also do this manually, but for large models with many field, this will save you a lot of time.
