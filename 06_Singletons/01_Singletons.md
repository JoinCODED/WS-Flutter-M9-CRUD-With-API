In the books provider, notice how we are calling the `BooksServices` many times, for each time we are creating an instance and saving it in the memory, but we actually need one instance, saving our self from filling the device memory with useless data.

To achieve that we will use a `Singleton`.

A singleton is a pattern used in object-oriented programming which ensures that a class has only one instance and also provides a global point of access to it.

To create a singleton for our `services/books.dart`:

```dart
class BooksServices {
    [...]
    static final BooksServices shared = BooksServices();
    [...]
}
```

Then in your books provider, replace each `BooksServices()` with `BooksServices.shared`.

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
