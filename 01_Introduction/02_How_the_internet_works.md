In our application, we need a server that provides us with the books data. This server is the `https://coded-books-api-crud.herokuapp.com`.
This server will not only help us to get the data, but also to make some operations on it, such as adding, deleting, or updating data.

To communicate with this server, we will use HTTP requests.

![img](https://miro.medium.com/max/1400/1*2UbC5pSRyjGmF1ezB9hvYg.png)

In addition, we will use the `dio` library to request the data, and tell the server we need the list of books with their prices, titles, images, etc, and then the server will send us back the data we need.

The format that the server gives us is called `JSON`.

`JSON`, or JavaScript Object Notation, is a minimal, readable format for structuring data. It is used primarily to transmit data between a server and web application. No matter which programming language is used on either side, they can communicate and transmit data using `JSON`.

How does `JSON` structure look?

```json
{
  "id": 1,
  "title": "Six of Crows",
  "price": 45.5,
  "image": "https://images-na.ssl-images-amazon.com/images/I/71pX+JTU8EL.jpg",
  "description": "Six of Crows is a fantasy novel written by the Israeli-American author Leigh Bardugo published by Henry Holt and Co. in 2015. The story follows a thieving crew and is primarily set in the city of Ketterdam, loosely inspired by Dutch Republic–era Amsterdam."
}
```

As you can see, it is made of key-value pairs, similar to dart maps.

However, we still have to convert the `JSON` data to dart `maps`, and we can do this in our model using a factory constructor:

```dart
class Book {
  int? id;
  String title;
  String description;
  String image;
  double price;

  Book(
      {this.id,
      required this.title,
      required this.description,
      required this.image,
      required this.price});

  Book.fromJson(Map<String, dynamic> json)
      : id = json['id'] as int?,
        title = json['title'] as String,
        description = json['description'] as String,
        image = json['image'] as String,
        price = json['price'] as double.tryParse(json['price'].toString()) ?? 0;
}
```

The following URLs are the API endpoints that will be used in the app. We will go through them one by one:

```
Get, https://coded-books-api-crud.herokuapp.com/books
Post, https://coded-books-api-crud.herokuapp.com/books
Put, https://coded-books-api-crud.herokuapp.com/books/{bookId}
Delete, https://coded-books-api-crud.herokuapp.com/books/{bookId}
```
