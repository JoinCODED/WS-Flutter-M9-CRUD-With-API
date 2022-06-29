2. In the `main.dart`, define a route with a param called `bookId`.

```dart
GoRoute(
        path: '/update/:bookId',
        builder: (context, state) {
          return UpdatePage(book: book);
        },
      ),
```

1. Call the provider and use the `firstWhere` method to look for a book with the same `id`, then pass the book that we found to the `UpdatePage` widget.

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

4. In the edit icon button inside the `book_card.dart`, use the `GoRouter.push` method and pass the path along with the book `id` to it:

```dart
IconButton(
    onPressed: () {
    GoRouter.of(context).push('/update/${book.id}');
    },
    icon: const Icon(Icons.edit)),
```

We accepted the book argument in the parent widget, how can we access it within our child widget?

5. Under your build method, you can access the book using `widget.book`:

```dart
  Widget build(BuildContext context) {
    Book book = widget.book;
    return Form(
[...]
```

6. Add the `initialValue` property to all the fields using the received book object:

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

We have been going from file to file for a long time, we are almost done, phew..
