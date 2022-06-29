In the book provider, notice how we are calling the `BooksServices` many times. Each time we are creating an instance and saving it in the memory, but we actually need one instance to save ourselves from filling the device memory with useless data.

To achieve that we will use a `Singleton`.

A `singleton` is a pattern used in object-oriented programming, which ensures that the class has only one instance. In addition to that, it provides global access to it.

1. In the `services/books.dart` file, paste the code below to create a singleton:

```dart
class BooksServices {
    [...]
    static final BooksServices shared = BooksServices();
    [...]
}
```

2. In the books provider, replace each `BooksServices()` with `BooksServices.shared`.

```dart
    Future<void> getBooks() async {
    books = await BooksServices.shared.getBooks();
    notifyListeners();
  }

  void createBook(Book book) async {
    Book newBook = await BooksServices.shared.createBook(book: book);
    books.insert(0, newBook);
    notifyListeners();
  }

  void updateBook(Book book) async {
    Book newBook = await BooksServices.shared.updateBook(book: book);
    int index = books.indexWhere((book) => book.id == newBook.id);
    books[index] = newBook;
    notifyListeners();
  }

  void deleteBook(int bookId) async {
    await BooksServices.shared.deleteBook(bookId: bookId);
    books.removeWhere((book) => book.id == bookId);
    notifyListeners();
  }
```
