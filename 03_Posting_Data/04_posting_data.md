Back to our `services/books.dart` file.

We will create a new function that takes a `Book` as an argument and returns a `Book` object.

```dart
  Future<Book> createBook({required Book book}) async {
  }
```

Then we will create a variable of type `Book` and we will mark it as `late` because we will initialize it later:

```dart
  Future<Book> createBook({required Book book}) async {
    late Book retrievedBook;
  }
```

But why will the backend return a `Book` to us since we already have all the data?

Remember when we used to generate an `id` for our object? now the backend takes handle of this, it will return to us the `Book` with a unique `id`.

And remember when I told you we use `JSON` to send and receive data? well I lied, there's one exception where we don't use `JSON` to send our data, you saw how `JSON` is a key-value pair of strings, you can't store files in it, and we have an image, that's why we will use `FormData` here.

Again, if we did'nt have an image here, we wouldn't have used `FormDate`, and we will go over this case later.

Add your try-catch block:

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

We will initialize a `FormData` Object and inside it we added all our values, but for the image we used `MultipartFile.fromFile`.

Now let's take a look at our endpoint:

```
Post, http://http://10.0.2.2:5000/books
```

So the type is `post` and rest is the same:

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

And as a third argument, we'll pass the data:

```dart
      Response response = await _dio.post(_baseUrl + '/books', data: data);
```

Lastly we'll assign the response to our `retrievedBook` like we did with the `get` request:

```dart
      Response response = await _dio.post(_baseUrl + '/books', data: data);
      retrievedBook = Book.fromJson(response.data);
```

Return the book from the function:

```dart
    [...]
    } on DioError catch (error) {
      print(error);
    }
    return retrievedBook;
}
```

Now Back to our provider, we'll make almost the same function:

```dart
  Future<void> createBook(Book book) async {
    await DioClient().createBook(book: book);
  }
```

And back to our form button, we will call this function from our provider:

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

We passed our values to the function, and then we we called the `pop` method to go to the previous page.

Wait, our book is not added.. let's restart our app, oh here it is, but why that happened? that's because we added the book to the backend, so we need to do the `get` request again to get the updates, a solution for that is to add the book to the front end after we finish our call, so in your provider:

```dart
    Book newBook = await DioClient().createBook(book: book);
    books.insert(0, newBook);
    notifyListeners();
```

Try now!
