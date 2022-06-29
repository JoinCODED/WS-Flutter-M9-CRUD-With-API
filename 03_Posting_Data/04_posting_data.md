1. In the `services/books.dart` file, create a new function that takes a `Book` as an argument and returns a `Book` object.

```dart
  Future<Book> createBook({required Book book}) async {
  }
```

2. Create a variable of type `Book` and mark it as `late` because you will initialize it later:

```dart
  Future<Book> createBook({required Book book}) async {
    late Book retrievedBook;
  }
```

Why do we need the backend to return a `Book` if we already have all the data?

We used to generate an `id` manually for the object. However, now the backend will take care of that and return the `Book` with a unique `id`.

**Note:** We use `JSON` to send and receive data, except when we use data of type files, because we cannot store files in a `key-value` pair. Instead, we use `FormData` to store these types of data.

3. Add the try-catch block:

```dart
  Future<Book> createBook({required Book book}) async {
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
    } on DioError catch (error) {
      print(error);
    }
  }
```

We initialized a `FormData` object, and inside it, we passed all of our values, but for the image we passed `MultipartFile.fromFile`.

Now, let's take a look at our endpoint:

```
Post, https://coded-books-api-crud.herokuapp.com/books
```

As you can see, the type of our request is `post`:

```dart
  Future<Book> createBook({required Book book}) async {
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

     Response response = await _dio.post(_baseUrl + '/books');
    } on DioError catch (error) {
      print(error);
    }
  }
```

**Note:** When the type of the request is post or update, the backend will expect data from the frontend, and that data will be sent as a second argument along with the endpoint as follows:

```dart
      Response response = await _dio.post(_baseUrl + '/books', data: data);
```

4. Assign the response to the `retrievedBook`:

```dart
      Response response = await _dio.post(_baseUrl + '/books', data: data);
      retrievedBook = Book.fromJson(response.data);
```

5. Return the book:

```dart
    [...]
    } on DioError catch (error) {
      print(error);
    }
    return retrievedBook;
}
```

6. Back to the provider, create a void function that calls the `createBook` function inside, and pass the new book to it:

```dart
  void createBook(Book book) async {
    await BooksServices().createBook(book: book);
  }
```

7. Back to the button in the form, pass the `createBook` function from the provider:

```dart
ElevatedButton(
              onPressed: () {
                if (_formKey.currentState!.validate()) {
                  _formKey.currentState!.save();
                  Provider.of<BooksProvider>(context, listen: false).createBook(
                      Book(
                          title: title,
                          description: description,
                          image: _image.path,
                          price: price));
                  GoRouter.of(context).pop();
                }
              },
              child: const Text("Add Book"),
    ),
```

We passed the values to the function, and called the `pop` method to go back to the previous page.

Wait! Our book is not added... Let's restart our app. Oh! here it is, but why did that happen?

Because we added the book to the backend, and we need to make a `get` request again to get the updates. To do that, we have to add the book in the front end after we finish our call.

8. In the provider, add the following code:

```dart
    Book newBook = await BooksServices().createBook(book: book);
    books.insert(0, newBook);
    notifyListeners();
```

Try now!
