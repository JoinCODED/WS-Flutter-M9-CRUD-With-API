Back to our `dio` file, we'll create a function that is very similar to the `createBook` function.

So what is our endpoint?

```
Put, http://http://10.0.2.2:5000/books/{bookId}
```

The method is `put` and we need to also send the `{bookId}` in the url, how to do this?

```dart
  Future<Book> updateBook({required Book book}) async {
    late Book retrievedBook;
    try {
      FormData data = FormData.fromMap({
        "title": book.title,
        "description": book.title,
        "price": book.price,
        "image": await MultipartFile.fromFile(
          book.image,
        ),
      });
      print(book.id);

      Response response =
          await _dio.put(_baseUrl + '/books/${book.id}', data: data);
      retrievedBook = Book.fromJson(response.data);
    } on DioError catch (error) {
      print(error);
    }
    return retrievedBook;
  }
```

And in our provider:

```dart
  Future<void> updateBook(Book book) async {
    await DioClient().updateBook(book: book);
  }
```

Don't forget, in order to see the changes in the frontend too we need to update the book that is in our list too.

```dart
  Future<void> updateBook(Book book) async {
    Book newBook = await DioClient().updateBook(book: book);
    int index = books.indexWhere((book) => book.id == newBook.id);
    books[index] = newBook;
    notifyListeners();
  }
```

We found the `index` of our book that we want to update, then we replaced it with the book that came as a response from the backend.

Lastly, let's go to our form button and call our method:

```dart
ElevatedButton(
              onPressed: () {
                if (_formKey.currentState!.validate()) {
                  _formKey.currentState!.save();
                  Provider.of<BooksProvider>(context, listen: false).updateBook(
                      Book(
                          id: book.id,
                          title: title,
                          description: description,
                          image: _image.path,
                          price: price));
                  GoRouter.of(context).pop();
                }
              },
              child: const Text("Update Book"),
            ),
```

Pay attention, this time we provided an `id` because the backend need it to know which book to update!
