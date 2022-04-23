In our application, we will need a server that will provide us with the books data we need, and the server that we will use is the `https://coded-books-api-crud.herokuapp.com`. This server will provide us with the data we need and will accept some operations on the data.

To communicate with this server, we will use the HTTP requests.

![img](https://miro.medium.com/max/1400/1*2UbC5pSRyjGmF1ezB9hvYg.png)

We will use the `dio` library in order to request the date. We will tell the server we need the list of books with their price, title, image etc..., and then the server will send us back the data we need.

The format that the server will give it to us is called `JSON`. But what is `JSON`?

`JSON`, or JavaScript Object Notation, is a minimal, readable format for structuring data. It is used primarily to transmit data between a server and web application, no matter what the programming language used on either side,they can communicate and transmit data using `JSON`.

How does `JSON` structure looks?

```json
{
  "id": 1,
  "title": "Six of Crows",
  "price": 45.5,
  "image": "https://images-na.ssl-images-amazon.com/images/I/71pX+JTU8EL.jpg",
  "description": "Six of Crows is a fantasy novel written by the Israeli-American author Leigh Bardugo published by Henry Holt and Co. in 2015. The story follows a thieving crew and is primarily set in the city of Ketterdam, loosely inspired by Dutch Republic–era Amsterdam."
}
```

As you can see, it's a key-value pairs, very similar to dart maps.

However, we still have to convert the `JSON` data to dart `maps`. and we can do this in our model using a factory constructor:

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

And here is our API end points and we will go through them one by one:

```
Get, https://coded-books-api-crud.herokuapp.com/books
Post, https://coded-books-api-crud.herokuapp.com/books
Put, https://coded-books-api-crud.herokuapp.com/books/{bookId}
Delete, https://coded-books-api-crud.herokuapp.com/books/{bookId}
```
