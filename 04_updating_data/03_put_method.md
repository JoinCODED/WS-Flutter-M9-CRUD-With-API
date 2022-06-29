Here is the endpoint for updating a book:

```
Put, https://coded-books-api-crud.herokuapp.com/books/{bookId}
```

**Note:** The put method requires data which is the updated book, as well as the `id` of the book we want to update.

7. Back to the `dio` file, create a function named `updateBook` similar to the `createBook` function.

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

8. In the provider file, create a future function that takes the updated book as an argument and passes it to the `updateBook` function:

```dart
  Future<void> updateBook(Book book) async {
    await BooksServices().updateBook(book: book);
  }
```

**Reminder:** In order to see the changes in the frontend, you need to update the book there as well.

9. Inside the `updateBook` function, use the `indexWhere` method to look for the index of the book that has an id that matches the received book's id. Replace the found book with the updated book that we received as a response from the backend.

```dart
  void updateBook(Book book) async {
    Book newBook = await BooksServices().updateBook(book: book);
    int index = books.indexWhere((book) => book.id == newBook.id);
    books[index] = newBook;
    notifyListeners();
  }
```

10. Go to the `Update Book` button inside the form and call the `updateBook` method:

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

**Note:** This time we provided an `id` because the backend needs it to know which book to update.
