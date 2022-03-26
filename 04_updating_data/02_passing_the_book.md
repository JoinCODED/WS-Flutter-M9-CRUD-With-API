Like we did before, we will pass the book using route params.

In your `main.dart` define the route and a param called `bookId`.

```dart
GoRoute(
        path: '/update/:bookId',
        builder: (context, state) {
          return UpdatePage(book: book);
        },
      ),
```

Now we will call our provider, search for the book with the same `id` and pass it to the `UpdatePage`.

```dart
GoRoute(
        path: '/update/:bookId',
        builder: (context, state) {
          final book = Provider.of<BooksProvider>(context).books.firstWhere(
              (book) => book.id.toString() == (state.params['bookId']!));
          return UpdatePage(book: book);
        },
      ),
```

Now in the `book_card.dart` we have an edit icon, clicking on it should take us to the update page and pass the book `id` with it:

```dart
IconButton(
    onPressed: () {
    GoRouter.of(context).push('/update/${book.id}');
    },
    icon: const Icon(Icons.edit)),
```

In your form, we accepted the book argument in our parent widget, how can we access it within our child widget?

Under your build method, you can access the book using `widget.book`:

```dart
  Widget build(BuildContext context) {
    Book book = widget.book;
    return Form(
[...]
```

And now we can add `initialValue` property to all our fields using the book object we received:

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
            initialValue: book.title,
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
            initialValue: book.description,
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
            initialValue: book.price.toString(),
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
          Row(
            children: [
              GestureDetector(
                onTap: () async {
                  final XFile? image =
                      await _picker.pickImage(source: ImageSource.gallery);
                  setState(() {
                    _image = File(image!.path);
                  });
                },
                child: Container(
                  width: 100,
                  height: 100,
                  margin: const EdgeInsets.only(top: 20),
                  decoration: BoxDecoration(color: Colors.blue[200]),
                  child: _image != null
                      ? Image.file(
                          _image,
                          width: 200.0,
                          height: 200.0,
                          fit: BoxFit.fitHeight,
                        )
                      : Container(
                          decoration: BoxDecoration(color: Colors.blue[200]),
                          width: 200,
                          height: 200,
                          child: Icon(
                            Icons.camera_alt,
                            color: Colors.grey[800],
                          ),
                        ),
                ),
              ),
              const Padding(
                padding: EdgeInsets.all(8.0),
                child: Text("Image"),
              )
            ],
          ),
          Center(
            child: ElevatedButton(
              onPressed: () {
                if (_formKey.currentState!.validate()) {
                  // If the form is valid, display a Snackbar.
                  _formKey.currentState!.save();
                  GoRouter.of(context).pop();
                }
              },
              child: const Text("Update Book"),
            ),
          )
        ],
      ),
    );
```

We have been going from a file to file for a long time, we are almost done, phew..
