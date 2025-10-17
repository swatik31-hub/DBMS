MongoDB - Map reduces operations:
Implement Map reduces operation with suitable example using MongoDB.
--------------------------------------------------------------------------------
ubuntu@ubuntu-OptiPlex-3090:~$ mongosh
Current Mongosh Log ID:	68afde9d276d8ba08fd861df
Connecting to:		mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.5.0
Using MongoDB:		8.0.8
Using Mongosh:		2.5.0

For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/

------
   The server generated these startup warnings when booting
   2025-08-28T09:16:30.300+05:30: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
   2025-08-28T09:16:30.849+05:30: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
   2025-08-28T09:16:30.849+05:30: For customers running the current memory allocator, we suggest changing the contents of the following sysfsFile
   2025-08-28T09:16:30.849+05:30: For customers running the current memory allocator, we suggest changing the contents of the following sysfsFile
   2025-08-28T09:16:30.849+05:30: We suggest setting the contents of sysfsFile to 0.
   2025-08-28T09:16:30.849+05:30: We suggest setting swappiness to 0 or 1, as swapping can cause performance problems.
------

test> use storeDB;
switched to db storeDB
storeDB> db.sales.insertMany([
...   { product: "Laptop", price: 800, quantity: 2 },
...   { product: "Laptop", price: 800, quantity: 1 },
...   { product: "Phone", price: 500, quantity: 4 },
...   { product: "Tablet", price: 300, quantity: 3 },
...   { product: "Phone", price: 500, quantity: 2 },
...   { product: "Tablet", price: 300, quantity: 1 }
... ]);
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('68afdeb2276d8ba08fd861e0'),
    '1': ObjectId('68afdeb2276d8ba08fd861e1'),
    '2': ObjectId('68afdeb2276d8ba08fd861e2'),
    '3': ObjectId('68afdeb2276d8ba08fd861e3'),
    '4': ObjectId('68afdeb2276d8ba08fd861e4'),
    '5': ObjectId('68afdeb2276d8ba08fd861e5')
  }
}
storeDB> var mapFunction = function() {
...   emit(this.product, this.price * this.quantity);
... };
... 

storeDB> var reduceFunction = function(key, values) {
...   return Array.sum(values);
... };

storeDB> db.sales.mapReduce(
...   mapFunction,
...   reduceFunction,
...   {
...     out: "total_sales_per_product"
...   }
... );
... 
DeprecationWarning: Collection.mapReduce() is deprecated. Use an aggregation instead.
See https://docs.mongodb.com/manual/core/map-reduce for details.
{ result: 'total_sales_per_product', ok: 1 }
storeDB> db.total_sales_per_product.find().pretty();
[
  { _id: 'Phone', value: 3000 },
  { _id: 'Laptop', value: 2400 },
  { _id: null, value: NaN },
  { _id: 'Tablet', value: 1200 }
]
storeDB> 

-----------------------------------------------------------------------------------------
switched to db textDB
textDB> db.documents.insertMany([
...   { text: "hello world hello" },
...   { text: "hello from MongoDB" },
...   { text: "MongoDB is great" },
...   { text: "hello world" }
... ]);
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId('68afe007276d8ba08fd861e6'),
    '1': ObjectId('68afe007276d8ba08fd861e7'),
    '2': ObjectId('68afe007276d8ba08fd861e8'),
    '3': ObjectId('68afe007276d8ba08fd861e9')
  }
}
textDB> var mapFunction = function() {
...   var words = this.text.split(" ");
...   words.forEach(function(word) {
...     emit(word.toLowerCase(), 1);
...   });
... };

textDB> var reduceFunction = function(key, values) {
...   return Array.sum(values);
... };

textDB> db.documents.mapReduce(
...   mapFunction,
...   reduceFunction,
...   {
...     out: "word_count_results"
...   }
... );
{ result: 'word_count_results', ok: 1 }
textDB> db.word_count_results.find().pretty();
[
  { _id: 'hello', value: 4 },
  { _id: 'from', value: 1 },
  { _id: 'mongodb', value: 2 },
  { _id: 'great', value: 1 },
  { _id: 'is', value: 1 },
  { _id: 'world', value: 2 }
]
textDB> 


