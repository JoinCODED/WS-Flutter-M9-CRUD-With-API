In your `widgets` folder, create `add_form.dart` widget and inside it initialize a stateFul widget:

```dart
class AddBookForm extends StatefulWidget {
  @override
  AddBookFormState createState() {
    return AddBookFormState();
  }
}

class AddBookFormState extends State<AddBookForm> {

  @override
  Widget build(BuildContext context) {

  }
}
```

Firstly, we need to create a global key that uniquely identifies the Form widget and allows validation of the form.

```dart
class AddBookFormState extends State<AddBookForm> {
  final _formKey = GlobalKey<FormState>();
  @override
  Widget build(BuildContext context) {

  }
}
```

Now we will return a `Form` widget and assign it the `key` we just created:

```dart
Widget build(BuildContext context) {
return Form(
      key: _formKey,
 )
}
```

Inside this `Form` widget, we will create `TextFormField`s instead of regular `TextField`s for each of our properties except our `image` for now:

```dart
    return Form(
      key: _formKey,
    child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: <Widget>[
          TextFormField(
            decoration: const InputDecoration(
              hintText: 'Book title',
            ),
          ),
          TextFormField(
            decoration: const InputDecoration(
              hintText: 'Book Description',
            ),
            maxLines: null,
          ),
          TextFormField(
            decoration: const InputDecoration(
              hintText: 'Book price',
            ),
          ),
          Center(
            child: ElevatedButton(
              onPressed: () {},
              child: const Text("Add Book"),
            ),
          )
        ],
      ),
    );
```

Now import your widget in the `add_page.dart`.

```dart
class AddPage extends StatelessWidget {
  AddPage({Key? key}) : super(key: key);
  final titleController = TextEditingController();
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Add a book"),
      ),
      resizeToAvoidBottomInset: false,
      body: Padding(
        padding: const EdgeInsets.all(20.0),
        child: Column(
          children: [
            const Text("Fill those field to add a book"),
            AddBookForm(),
          ],
        ),
      ),
    );
  }
}
```

So what's special about it? let's add a `validator` property to our first field:

```dart
          TextFormField(
            decoration: const InputDecoration(
              hintText: 'Book title',
            ),
            validator: (value) {
              if (value!.isEmpty) {
                return "please fill out this field";
              } else {
                return null;
              }
            },
          ),
```

This `validator` property will give us the current `value` of the field, and if we returned something, it means the value the user entered is not valid, and if we returned `null` it means everything is ok.

But when does this `validator` runs? when we call `_formKey.currentState!.validate()`. so let's call it in our button:

```dart
ElevatedButton(
    onPressed: () {
                _formKey.currentState!.validate()
    },
        child: const Text("Add Book"),
    ),
```

Now leave the field empty and click the button, an error should appear.

Let's add `validator`s to our other fields, specially our `price` field, we want to make sure the user enters a valid number:

```dart
TextFormField(
            decoration: const InputDecoration(
              hintText: 'Book Description',
            ),
            maxLines: null,
            validator: (value) {
              if (value!.isEmpty) {
                return "please fill out this field";
              } else {
                return null;
              }
            },
          ),
          TextFormField(
            decoration: const InputDecoration(
              hintText: 'Book price',
            ),
            validator: (value) {
              if (value == null) {
                return "please enter a price";
              } else if (double.tryParse(value) == null) {
                return "please enter a number";
              }
              return null;
            },
          ),
```

Now were is our controllers? we will replace them with normal variables and save the values using the `onSaved` property.

Create a variable for each property we have:

```dart
  String title = "";
  String description = "";
  double price = 0;
```

Then on each field, use the `onSaved` to store the `value` in the respective variable:

```dart
Form(
      key: _formKey,
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: <Widget>[
          TextFormField(
            decoration: const InputDecoration(
              hintText: 'Book title',
            ),
            validator: (value) {
              if (value!.isEmpty) {
                return "please fill out this field";
              } else {
                return null;
              }
            },
            onSaved: (value) {
              title = value!;
            },
          ),
          TextFormField(
            decoration: const InputDecoration(
              hintText: 'Book Description',
            ),
            maxLines: null,
            validator: (value) {
              if (value!.isEmpty) {
                return "please fill out this field";
              } else {
                return null;
              }
            },
            onSaved: (value) {
              description = value!;
            },
          ),
          TextFormField(
            decoration: const InputDecoration(
              hintText: 'Book price',
            ),
            validator: (value) {
              if (value == null) {
                return "please enter a price";
              } else if (double.tryParse(value) == null) {
                return "please enter a number";
              }
              return null;
            },
            onSaved: (value) {
              price = double.parse(value!);
            },
          ),
          Center(
            child: ElevatedButton(
              onPressed: () {
                if (_formKey.currentState!.validate()) {
                  _formKey.currentState!.save();
                }
              },
              child: const Text("Add Book"),
            ),
          )
        ],
      ),
    );
```

`_formKey.currentState!.save()` will run all our `onSaved` methods we created and that happens only if all fields are valid because we checked for `_formKey.currentState!.validate()` first which will return `true` if no errors is there.
