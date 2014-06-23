# 关于本书 #

## 许可 ##
本书《 The Little MongoDB Book 》基于 Attribution-NonCommercial 3.0 Unported license. **你无须为本书付款。**

你可以自由的复制，分发，修改和传阅本书。但请认可该书属于作者 Karl Seguin，并请勿将本书用于任何商业目的。

你可以在以下链接查看完整的许可文档:

<http://creativecommons.org/licenses/by-nc/3.0/legalcode>

## 关于作者 ##
Karl Seguin 在多领域有着丰富经验，他是 .NET 和 Ruby 的开发专家。他也参与贡献 OSS 项目, 还是技术文档撰写人而且偶尔做做演讲。MongoDB 方面，他是 C# MongoDB 库 NoRM 的核心开发者，写有互动入门教程 [mongly](http://openmymind.net/mongly/) 和 [Mongo Web Admin](https://github.com/karlseguin/Mongo-Web-Admin)。他用 MongoDB，为休闲游戏开发者写了一个免费服务, [mogade.com](http://mogade.com/)。

Karl 还编写了 [The Little Redis Book](http://openmymind.net/2012/1/23/The-Little-Redis-Book/)

你可以在 <http://openmymind.net> 找到他的 Blog，或者通过 [@karlseguin](http://twitter.com/karlseguin) 在 Twitter 上关注他。

## 鸣谢 ##
特别感谢 [Perry Neal](http://twitter.com/perryneal), 赐予我你的视野，精神，和热情。你赐予了我无尽的力量。感恩。

## 最新版本 ##
最新的版本由 Asya Kamsky 更新到了 MongoDB 2.6 。本书最新代码可以在这里获得:

<http://github.com/karlseguin/the-little-mongodb-book>.

### 中文版本 ###
Karl 在 [the-little-mongodb-book](https://github.com/karlseguin/the-little-mongodb-book) 的 Github 链接中给出了 [justinyhuang](https://github.com/justinyhuang) 的 [the-little-mongodb-book-cn](https://github.com/justinyhuang/the-little-mongodb-book-cn) 链接。但貌似 justinyhuang 并没有同步更新到 MongoDB 2.6 。内容上也和原文稍微有点出入，并且由于本人水平有限，无法提交自信正确的内容。因此重开一项目。如果你被搜索引擎引导到本工程，在此向你致歉，并希望有能力者且有时间者一同完善和同步本工程。

# 简介 #
 > 这章那么短不是我的错，MongoDB 就真的很易学。

It is often said that technology moves at a blazing pace. It's true that there is an ever growing list of new technologies and techniques being released. However, I've long been of the opinion that the fundamental technologies used by programmers move at a rather slow pace. One could spend years learning little yet remain relevant. What is striking though is the speed at which established technologies get replaced. Seemingly overnight, long-established technologies find themselves threatened by shifts in developer focus.

Nothing could be more representative of this sudden shift than the progress of NoSQL technologies against well-established relational databases. It almost seems like one day the web was being driven by a few RDBMSs, and the next, five or so NoSQL solutions had established themselves as worthy solutions.

Even though these transitions seem to happen overnight, the reality is that they can take years to become accepted practice. The initial enthusiasm is driven by a relatively small set of developers and companies. Solutions are refined, lessons learned and seeing that a new technology is here to stay, others slowly try it for themselves. Again, this is particularly true in the case of NoSQL where many solutions aren't replacements for more traditional storage solutions, but rather address a specific need in addition to what one might get from traditional offerings.

Having said all of that, the first thing we ought to do is explain what is meant by NoSQL. It's a broad term that means different things to different people. Personally, I use it very broadly to mean a system that plays a part in the storage of data. Put another way, NoSQL (again, for me), is the belief that your persistence layer isn't necessarily the responsibility of a single system. Where relational database vendors have historically tried to position their software as a one-size-fits-all solution, NoSQL leans towards smaller units of responsibility where the best tool for a given job can be leveraged. So, your NoSQL stack might still leverage a relational database, say MySQL, but it'll also contain Redis as a persistence lookup for specific parts of the system as well as Hadoop for your intensive data processing. Put simply, NoSQL is about being open and aware of alternative, existing and additional patterns and tools for managing your data.

You might be wondering where MongoDB fits into all of this. As a document-oriented database, MongoDB is a more generalized NoSQL solution. It should be viewed as an alternative to relational databases. Like relational databases, it too can benefit from being paired with some of the more specialized NoSQL solutions. MongoDB has advantages and drawbacks, which we'll cover in later parts of this book.

As you may have noticed, we use the terms MongoDB and Mongo interchangeably.

# 开始 #
Most of this book will focus on core MongoDB functionality. We'll therefore rely on the MongoDB shell. While the shell is useful to learn as well as being a useful administrative tool, your code will use a MongoDB driver.

This does bring up the first thing you should know about MongoDB: its drivers. MongoDB has a [number of official drivers](http://docs.mongodb.org/ecosystem/drivers/) for various languages. These drivers can be thought of as the various database drivers you are probably already familiar with. On top of these drivers, the development community has built more language/framework-specific libraries. For example, [NoRM](https://github.com/atheken/NoRM) is a C# library which implements LINQ, and [MongoMapper](https://github.com/jnunemaker/mongomapper) is a Ruby library which is ActiveRecord-friendly. Whether you choose to program directly against the core MongoDB drivers or some higher-level library is up to you. I point this out only because many people new to MongoDB are confused as to why there are both official drivers and community libraries - the former generally focuses on core communication/connectivity with MongoDB and the latter with more language and framework-specific implementations.

As you read through this, I encourage you to play with MongoDB to replicate what I demonstrate as well as to explore questions that you might come up with on your own. It's easy to get up and running with MongoDB, so let's take a few minutes now to set things up.

1. Head over to the [official download page](http://www.mongodb.org/downloads) and grab the binaries from the first row (the recommended stable version) for your operating system of choice. For development purposes, you can pick either 32-bit or 64-bit.

2. Extract the archive (wherever you want) and navigate to the `bin` subfolder. Don't execute anything just yet, but know that `mongod` is the server process and `mongo` is the client shell - these are the two executables we'll be spending most of our time with.

3. Create a new text file in the `bin` subfolder named `mongodb.config`.

4. Add a single line to your mongodb.config: `dbpath=PATH_TO_WHERE_YOU_WANT_TO_STORE_YOUR_DATABASE_FILES`. For example, on Windows you might do `dbpath=c:\mongodb\data` and on Linux you might do `dbpath=/var/lib/mongodb/data`.

5. Make sure the `dbpath` you specified exists.

6. Launch mongod with the `--config /path/to/your/mongodb.config` parameter.

As an example for Windows users, if you extracted the downloaded file to `c:\mongodb\` and you created `c:\mongodb\data\` then within `c:\mongodb\bin\mongodb.config` you would specify `dbpath=c:\mongodb\data\`. You could then launch `mongod` from a command prompt via `c:\mongodb\bin\mongod --config c:\mongodb\bin\mongodb.config`.

Feel free to add the `bin` folder to your path to make all of this less verbose. MacOSX and Linux users can follow almost identical directions. The only thing you should have to change are the paths.

Hopefully you now have MongoDB up and running. If you get an error, read the output carefully - the server is quite good at explaining what's wrong.

You can now launch `mongo` (without the *d*) which will connect a shell to your running server. Try entering `db.version()` to make sure everything's working as it should. Hopefully you'll see the version number you installed.

# 第一章 - 基础知识 #
We begin our journey by getting to know the basic mechanics of working with MongoDB. Obviously this is core to understanding MongoDB, but it should also help us answer higher-level questions about where MongoDB fits.

To get started, there are six simple concepts we need to understand.

1. MongoDB has the same concept of a `database` with which you are likely already familiar (or a schema for you Oracle folks).  Within a MongoDB instance you can have zero or more databases, each acting as high-level containers for everything else.

2. A database can have zero or more `collections`. A collection shares enough in common with a traditional `table` that you can safely think of the two as the same thing.

3. Collections are made up of zero or more `documents`. Again, a document can safely be thought of as a `row`.

4. A document is made up of one or more `fields`, which you can probably guess are a lot like `columns`.

5. `Indexes` in MongoDB function mostly like their RDBMS counterparts.

6. `Cursors` are different than the other five concepts but they are important enough, and often overlooked, that I think they are worthy of their own discussion.  The important thing to understand about cursors is that when you ask MongoDB for data, it returns a pointer to the result set called a cursor, which we can do things to, such as counting or skipping ahead, before actually pulling down data.

To recap, MongoDB is made up of `databases` which contain `collections`. A `collection` is made up of `documents`. Each `document` is made up of `fields`. `Collections` can be `indexed`, which improves lookup and sorting performance. Finally, when we get data from MongoDB we do so through a `cursor` whose actual execution is delayed until necessary.

Why use new terminology (collection vs. table, document vs. row and field vs. column)? Is it just to make things more complicated? The truth is that while these concepts are similar to their relational database counterparts, they are not identical. The core difference comes from the fact that relational databases define `columns` at the `table` level whereas a document-oriented database defines its `fields` at the `document` level. That is to say that each `document` within a `collection` can have its own unique set of `fields`.  As such, a `collection` is a dumbed down container in comparison to a `table`, while a `document` has a lot more information than a `row`.

Although this is important to understand, don't worry if things aren't yet clear. It won't take more than a couple of inserts to see what this truly means. Ultimately, the point is that a collection isn't strict about what goes in it (it's schema-less). Fields are tracked with each individual document. The benefits and drawbacks of this will be explored in a future chapter.

Let's get hands-on. If you don't have it running already, go ahead and start the `mongod` server as well as a mongo shell. The shell runs JavaScript. There are some global commands you can execute, like `help` or `exit`. Commands that you execute against the current database are executed against the `db` object, such as `db.help()` or `db.stats()`. Commands that you execute against a specific collection, which is what we'll be doing a lot of, are executed against the `db.COLLECTION_NAME` object, such as `db.unicorns.help()` or `db.unicorns.count()`.

Go ahead and enter `db.help()`, you'll get a list of commands that you can execute against the `db` object.

A small side note: Because this is a JavaScript shell, if you execute a method and omit the parentheses `()`, you'll see the method body rather than executing the method. I only mention it so that the first time you do it and get a response that starts with `function (...){` you won't be surprised. For example, if you enter `db.help` (without the parentheses), you'll see the internal implementation of the `help` method.

First we'll use the global `use` helper to switch databases, so go ahead and enter `use learn`. It doesn't matter that the database doesn't really exist yet. The first collection that we create will also create the actual `learn` database. Now that you are inside a database, you can start issuing database commands, like `db.getCollectionNames()`. If you do so, you should get an empty array (`[ ]`). Since collections are schema-less, we don't explicitly need to create them. We can simply insert a document into a new collection. To do so, use the `insert` command, supplying it with the document to insert:

	db.unicorns.insert({name: 'Aurora',
		gender: 'f', weight: 450})

The above line is executing `insert` against the `unicorns` collection, passing it a single parameter. Internally MongoDB uses a binary serialized JSON format called BSON. Externally, this means that we use JSON a lot, as is the case with our parameters. If we execute `db.getCollectionNames()` now, we'll actually see two collections: `unicorns` and `system.indexes`. The collection `system.indexes` is created once per database and contains the information on our database's indexes.

You can now use the `find` command against `unicorns` to return a list of documents:

	db.unicorns.find()

Notice that, in addition to the data you specified, there's an `_id` field. Every document must have a unique `_id` field. You can either generate one yourself or let MongoDB generate a value for you which has the type `ObjectId`. Most of the time you'll probably want to let MongoDB generate it for you. By default, the `_id` field is indexed - which explains why the `system.indexes` collection was created. You can look at `system.indexes`:

	db.system.indexes.find()

What you're seeing is the name of the index, the database and collection it was created against and the fields included in the index.

Now, back to our discussion about schema-less collections. Insert a totally different document into `unicorns`, such as:

	db.unicorns.insert({name: 'Leto',
		gender: 'm',
		home: 'Arrakeen',
		worm: false})

And, again use `find` to list the documents. Once we know a bit more, we'll discuss this interesting behavior of MongoDB, but hopefully you are starting to understand why the more traditional terminology wasn't a good fit.

## 掌握选择器(Selector) ##
In addition to the six concepts we've explored, there's one practical aspect of MongoDB you need to have a good grasp of before moving to more advanced topics: query selectors. A MongoDB query selector is like the `where` clause of an SQL statement. As such, you use it when finding, counting, updating and removing documents from collections. A selector is a JSON object, the simplest of which is `{}` which matches all documents. If we wanted to find all female unicorns, we could use `{gender:'f'}`.

Before delving too deeply into selectors, let's set up some data to play with. First, remove what we've put so far in the `unicorns` collection via: `db.unicorns.remove({})`. Now, issue the following inserts to get some data we can play with (I suggest you copy and paste this):

	db.unicorns.insert({name: 'Horny',
		dob: new Date(1992,2,13,7,47),
		loves: ['carrot','papaya'],
		weight: 600,
		gender: 'm',
		vampires: 63});
	db.unicorns.insert({name: 'Aurora',
		dob: new Date(1991, 0, 24, 13, 0),
		loves: ['carrot', 'grape'],
		weight: 450,
		gender: 'f',
		vampires: 43});
	db.unicorns.insert({name: 'Unicrom',
		dob: new Date(1973, 1, 9, 22, 10),
		loves: ['energon', 'redbull'],
		weight: 984,
		gender: 'm',
		vampires: 182});
	db.unicorns.insert({name: 'Roooooodles',
		dob: new Date(1979, 7, 18, 18, 44),
		loves: ['apple'],
		weight: 575,
		gender: 'm',
		vampires: 99});
	db.unicorns.insert({name: 'Solnara',
		dob: new Date(1985, 6, 4, 2, 1),
		loves:['apple', 'carrot',
			'chocolate'],
		weight:550,
		gender:'f',
		vampires:80});
	db.unicorns.insert({name:'Ayna',
		dob: new Date(1998, 2, 7, 8, 30),
		loves: ['strawberry', 'lemon'],
		weight: 733,
		gender: 'f',
		vampires: 40});
	db.unicorns.insert({name:'Kenny',
		dob: new Date(1997, 6, 1, 10, 42),
		loves: ['grape', 'lemon'],
		weight: 690,
		gender: 'm',
		vampires: 39});
	db.unicorns.insert({name: 'Raleigh',
		dob: new Date(2005, 4, 3, 0, 57),
		loves: ['apple', 'sugar'],
		weight: 421,
		gender: 'm',
		vampires: 2});
	db.unicorns.insert({name: 'Leia',
		dob: new Date(2001, 9, 8, 14, 53),
		loves: ['apple', 'watermelon'],
		weight: 601,
		gender: 'f',
		vampires: 33});
	db.unicorns.insert({name: 'Pilot',
		dob: new Date(1997, 2, 1, 5, 3),
		loves: ['apple', 'watermelon'],
		weight: 650,
		gender: 'm',
		vampires: 54});
	db.unicorns.insert({name: 'Nimue',
		dob: new Date(1999, 11, 20, 16, 15),
		loves: ['grape', 'carrot'],
		weight: 540,
		gender: 'f'});
	db.unicorns.insert({name: 'Dunx',
		dob: new Date(1976, 6, 18, 18, 18),
		loves: ['grape', 'watermelon'],
		weight: 704,
		gender: 'm',
		vampires: 165});

Now that we have data, we can master selectors. `{field: value}` is used to find any documents where `field` is equal to `value`. `{field1: value1, field2: value2}` is how we do an `and` statement. The special `$lt`, `$lte`, `$gt`, `$gte` and `$ne` are used for less than, less than or equal, greater than, greater than or equal and not equal operations. For example, to get all male unicorns that weigh more than 700 pounds, we could do:

	db.unicorns.find({gender: 'm',
		weight: {$gt: 700}})
	//or (not quite the same thing, but for
	//demonstration purposes)
	db.unicorns.find({gender: {$ne: 'f'},
		weight: {$gte: 701}})

    	
The `$exists` operator is used for matching the presence or absence of a field, for example:

	db.unicorns.find({
		vampires: {$exists: false}})

should return a single document. The '$in' operator is used for matching one of several values that we pass as an array, for example:

    db.unicorns.find({
    	loves: {$in:['apple','orange']}})

This returns any unicorn who loves 'apple' or 'orange'.

If we want to OR rather than AND several conditions on different fields, we use the `$or` operator and assign to it an array of selectors we want or'd:

	db.unicorns.find({gender: 'f',
		$or: [{loves: 'apple'},
			  {weight: {$lt: 500}}]})

The above will return all female unicorns which either love apples or weigh less than 500 pounds.

There's something pretty neat going on in our last two examples. You might have already noticed, but the `loves` field is an array. MongoDB supports arrays as first class objects. This is an incredibly handy feature. Once you start using it, you wonder how you ever lived without it. What's more interesting is how easy selecting based on an array value is: `{loves: 'watermelon'}` will return any document where `watermelon` is a value of `loves`.

There are more available operators than what we've seen so far. These are all described in the [Query Selectors](http://docs.mongodb.org/manual/reference/operator/query/#query-selectors) section of the MongoDB manual. What we've covered so far though is the basics you'll need to get started. It's also what you'll end up using most of the time.

We've seen how these selectors can be used with the `find` command. They can also be used with the `remove` command which we've briefly looked at, the `count` command, which we haven't looked at but you can probably figure out, and the `update` command which we'll spend more time with later on.

The `ObjectId` which MongoDB generated for our `_id` field can be selected like so:

	db.unicorns.find(
		{_id: ObjectId("TheObjectId")})

## 小结 ##
我们还没有看到 `update` , 或是能拿来做更华丽事情的 `find`。不过，我们已经安装好 MongoDB 并运行起来了, 简略的介绍了一下 `insert` 和 `remove` 命令 (完整版也没比我们介绍的多什么)。 我们还介绍了 `find` 以及了解了 MongoDB `selectors` 是怎么一回事。 我们起了个很好的头，并为以后的学习奠定了坚实基础。 信不信由你，其实你已经掌握了学习 MongoDB 所必须的大多数知识 - 它真的是易学易用。 我强烈建议你在继续学习之前在本机上多试试多玩玩。 插入不同的文档，可以试试看在不同的集合中，习惯一下使用不同的选择器。试试 `find`, `count` 和 `remove`。 多试几次之后，你会发现原来看起来那么格格不入的东西，用起来居然水到渠成。

# 第二章 - 更新 #
In chapter 1 we introduced three of the four CRUD (create, read, update and delete) operations. This chapter is dedicated to the one we skipped over: `update`. `Update` has a few surprising behaviors, which is why we dedicate a chapter to it.

## Update: Replace Versus $set ##
In its simplest form, `update` takes two parameters: the selector (where) to use and what updates to apply to fields. If Roooooodles had gained a bit of weight, you might expect that we should execute:

	db.unicorns.update({name: 'Roooooodles'},
		{weight: 590})

(If you've played with your `unicorns` collection and it doesn't have the original data anymore, go ahead and `remove` all documents and re-insert from the code in chapter 1.)

Now, if we look at the updated record:

	db.unicorns.find({name: 'Roooooodles'})

You should discover the first surprise of `update`. No document is found because the second parameter we supplied didn't have any update operators, and therefore it was used to **replace** the original document. In other words, the `update` found a document by `name` and replaced the entire document with the new document (the second parameter). There is no equivalent functionality to this in SQL's `update` command. In some situations, this is ideal and can be leveraged for some truly dynamic updates. However, when you want to change the value of one, or a few fields, you must use MongoDB's `$set` operator. Go ahead and run this update to reset the lost fields:

	db.unicorns.update({weight: 590}, {$set: {
		name: 'Roooooodles',
		dob: new Date(1979, 7, 18, 18, 44),
		loves: ['apple'],
		gender: 'm',
		vampires: 99}})

This won't overwrite the new `weight` since we didn't specify it. Now if we execute:

	db.unicorns.find({name: 'Roooooodles'})

We get the expected result. Therefore, the correct way to have updated the weight in the first place is:

	db.unicorns.update({name: 'Roooooodles'},
		{$set: {weight: 590}})

## Update Operators ##
In addition to `$set`, we can leverage other operators to do some nifty things. All update operators work on fields - so your entire document won't be wiped out. For example, the `$inc` operator is used to increment a field by a certain positive or negative amount. If Pilot was incorrectly awarded a couple vampire kills, we could correct the mistake by executing:

	db.unicorns.update({name: 'Pilot'},
		{$inc: {vampires: -2}})

If Aurora suddenly developed a sweet tooth, we could add a value to her `loves` field via the `$push` operator:

	db.unicorns.update({name: 'Aurora'},
		{$push: {loves: 'sugar'}})

The [Update Operators](http://docs.mongodb.org/manual/reference/operator/update/#update-operators) section of the MongoDB manual has more information on the other available update operators.

## Upserts ##
One of the more pleasant surprises of using `update` is that it fully supports `upserts`. An `upsert` updates the document if found or inserts it if not. Upserts are handy to have in certain situations and when you run into one, you'll know it. To enable upserting we pass a third parameter to update `{upsert:true}`.

A mundane example is a hit counter for a website. If we wanted to keep an aggregate count in real time, we'd have to see if the record already existed for the page, and based on that decide to run an update or insert. With the upsert option omitted (or set to false), executing the following won't do anything:

	db.hits.update({page: 'unicorns'},
		{$inc: {hits: 1}});
	db.hits.find();

However, if we add the upsert option, the results are quite different:

	db.hits.update({page: 'unicorns'},
		{$inc: {hits: 1}}, {upsert:true});
	db.hits.find();

Since no documents exists with a field `page` equal to `unicorns`, a new document is inserted. If we execute it a second time, the existing document is updated and `hits` is incremented to 2.

	db.hits.update({page: 'unicorns'},
		{$inc: {hits: 1}}, {upsert:true});
	db.hits.find();

## Multiple Updates ##
The final surprise `update` has to offer is that, by default, it'll update a single document. So far, for the examples we've looked at, this might seem logical. However, if you executed something like:

	db.unicorns.update({},
		{$set: {vaccinated: true }});
	db.unicorns.find({vaccinated: true});

You might expect to find all of your precious unicorns to be vaccinated. To get the behavior you desire, the `multi` option must be set to true:

	db.unicorns.update({},
		{$set: {vaccinated: true }},
		{multi:true});
	db.unicorns.find({vaccinated: true});

## 小结 ##
本章中我们介绍了集合的基本 CRUD 操作。我们详细讲解了 `update` 及它的三个有趣的行为。 首先，如果你传 MongoDB 一个文档但是不带更新操作, MongoDB 的 `update` 会默认替换现有文档。因此，你通常要用到 `$set` 操作 (或者其他各种可用的用于修改文档的操作)。 其次， `update` 支持 `upsert` 操作，当你不知道文档是否存在的时候，非常有用。 最后，默认情况下， `update` 只更新第一个匹配文档，因此当你希望更新所有匹配文档时，你要用 `multi` 。

# 第三章 - 掌握查询 #
Chapter 1 provided a superficial look at the `find` command. There's more to `find` than understanding `selectors` though. We already mentioned that the result from `find` is a `cursor`. We'll now look at exactly what this means in more detail.

## 字段选择 ##
Before we jump into `cursors`, you should know that `find` takes a second optional parameter called "projection". This parameter is the list of fields we want to retrieve or exclude. For example, we can get all of the unicorns' names without getting back other fields by executing:

	db.unicorns.find({}, {name: 1});

By default, the `_id` field is always returned. We can explicitly exclude it by specifying `{name:1, _id: 0}`.

Aside from the `_id` field, you cannot mix and match inclusion and exclusion. If you think about it, that actually makes sense. You either want to select or exclude one or more fields explicitly.

## 排序(Ordering) ##
A few times now I've mentioned that `find` returns a cursor whose execution is delayed until needed. However, what you've no doubt observed from the shell is that `find` executes immediately. This is a behavior of the shell only. We can observe the true behavior of `cursors` by looking at one of the methods we can chain to `find`. The first that we'll look at is `sort`. We specify the fields we want to sort on as a JSON document, using 1 for ascending and -1 for descending. For example:

	//heaviest unicorns first
	db.unicorns.find().sort({weight: -1})

	//by unicorn name then vampire kills:
	db.unicorns.find().sort({name: 1,
		vampires: -1})

As with a relational database, MongoDB can use an index for sorting. We'll look at indexes in more detail later on. However, you should know that MongoDB limits the size of your sort without an index. That is, if you try to sort a very large result set which can't use an index, you'll get an error. Some people see this as a limitation. In truth, I wish more databases had the capability to refuse to run unoptimized queries. (I won't turn every MongoDB drawback into a positive, but I've seen enough poorly optimized databases that I sincerely wish they had a strict-mode.)

## 分页(Paging) ##
Paging results can be accomplished via the `limit` and `skip` cursor methods. To get the second and third heaviest unicorn, we could do:

	db.unicorns.find()
		.sort({weight: -1})
		.limit(2)
		.skip(1)

Using `limit` in conjunction with `sort`, can be a way to avoid running into problems when sorting on non-indexed fields.

## 计数(Count) ##
The shell makes it possible to execute a `count` directly on a collection, such as:

	db.unicorns.count({vampires: {$gt: 50}})

In reality, `count` is actually a `cursor` method, the shell simply provides a shortcut. Drivers which don't provide such a shortcut need to be executed like this (which will also work in the shell):

	db.unicorns.find({vampires: {$gt: 50}})
		.count()

## 小结 ##
使用 `find` 和 `cursors` 非常简单。还讲了一些我们后面章节会用到的或是非常特殊情况才用的命令，不过不管怎样，现在，你应该已经非常熟练使用 mongo shell 以及理解 MongoDB 的基本原则了。

# 第四章 - 数据建模 #
Let's shift gears and have a more abstract conversation about MongoDB. Explaining a few new terms and some new syntax is a trivial task. Having a conversation about modeling with a new paradigm isn't as easy. The truth is that most of us are still finding out what works and what doesn't when it comes to modeling with these new technologies. It's a conversation we can start having, but ultimately you'll have to practice and learn on real code.

Out of all NoSQL databases, document-oriented databases are probably the most similar to relational databases - at least when it comes to modeling. However, the differences that exist are important.

## No Joins ##
The first and most fundamental difference that you'll need to get comfortable with is MongoDB's lack of joins. I don't know the specific reason why some type of join syntax isn't supported in MongoDB, but I do know that joins are generally seen as non-scalable. That is, once you start to split your data horizontally, you end up performing your joins on the client (the application server) anyway. Regardless of the reasons, the fact remains that data *is* relational, and MongoDB doesn't support joins.

Without knowing anything else, to live in a join-less world, we have to do joins ourselves within our application's code. Essentially we need to issue a second query to `find` the relevant data in a second collection. Setting our data up isn't any different than declaring a foreign key in a relational database. Let's give a little less focus to our beautiful `unicorns` and a bit more time to our `employees`. The first thing we'll do is create an employee (I'm providing an explicit `_id` so that we can build coherent examples)

	db.employees.insert({_id: ObjectId(
		"4d85c7039ab0fd70a117d730"),
		name: 'Leto'})

Now let's add a couple employees and set their manager as `Leto`:

	db.employees.insert({_id: ObjectId(
		"4d85c7039ab0fd70a117d731"),
		name: 'Duncan',
		manager: ObjectId(
		"4d85c7039ab0fd70a117d730")});
	db.employees.insert({_id: ObjectId(
		"4d85c7039ab0fd70a117d732"),
		name: 'Moneo',
		manager: ObjectId(
		"4d85c7039ab0fd70a117d730")});


(It's worth repeating that the `_id` can be any unique value. Since you'd likely use an `ObjectId` in real life, we'll use them here as well.)

Of course, to find all of Leto's employees, one simply executes:

	db.employees.find({manager: ObjectId(
		"4d85c7039ab0fd70a117d730")})

There's nothing magical here. In the worst cases, most of the time, the lack of join will merely require an extra query (likely indexed).

## Arrays and Embedded Documents ##
Just because MongoDB doesn't have joins doesn't mean it doesn't have a few tricks up its sleeve. Remember when we saw that MongoDB supports arrays as first class objects of a document? It turns out that this is incredibly handy when dealing with many-to-one or many-to-many relationships. As a simple example, if an employee could have two managers, we could simply store these in an array:

	db.employees.insert({_id: ObjectId(
		"4d85c7039ab0fd70a117d733"),
		name: 'Siona',
		manager: [ObjectId(
		"4d85c7039ab0fd70a117d730"),
		ObjectId(
		"4d85c7039ab0fd70a117d732")] })

Of particular interest is that, for some documents, `manager` can be a scalar value, while for others it can be an array. Our original `find` query will work for both:

	db.employees.find({manager: ObjectId(
		"4d85c7039ab0fd70a117d730")})

You'll quickly find that arrays of values are much more convenient to deal with than many-to-many join-tables.

Besides arrays, MongoDB also supports embedded documents. Go ahead and try inserting a document with a nested document, such as:

	db.employees.insert({_id: ObjectId(
		"4d85c7039ab0fd70a117d734"),
		name: 'Ghanima',
		family: {mother: 'Chani',
			father: 'Paul',
			brother: ObjectId(
		"4d85c7039ab0fd70a117d730")}})

In case you are wondering, embedded documents can be queried using a dot-notation:

	db.employees.find({
		'family.mother': 'Chani'})

We'll briefly talk about where embedded documents fit and how you should use them.

Combining the two concepts, we can even embed arrays of documents:
	db.employees.insert({_id: ObjectId(
		"4d85c7039ab0fd70a117d735"),
		name: 'Chani',
		family: [ {relation:'mother',name: 'Chani'},
			{relation:'father',name: 'Paul'},
			{relation:'brother', name: 'Duncan'}]})


## Denormalization ##
Yet another alternative to using joins is to denormalize your data. Historically, denormalization was reserved for performance-sensitive code, or when data should be snapshotted (like in an audit log). However, with the ever-growing popularity of NoSQL, many of which don't have joins, denormalization as part of normal modeling is becoming increasingly common. This doesn't mean you should duplicate every piece of information in every document. However, rather than letting fear of duplicate data drive your design decisions, consider modeling your data based on what information belongs to what document.

For example, say you are writing a forum application. The traditional way to associate a specific `user` with a `post` is via a `userid` column within `posts`. With such a model, you can't display `posts` without retrieving (joining to) `users`. A possible alternative is simply to store the `name` as well as the `userid` with each `post`. You could even do so with an embedded document, like `user: {id: ObjectId('Something'), name: 'Leto'}`. Yes, if you let users change their name, you may have to update each document (which is one multi-update).

Adjusting to this kind of approach won't come easy to some. In a lot of cases it won't even make sense to do this. Don't be afraid to experiment with this approach though. It's not only suitable in some circumstances, but it can also be the best way to do it.

## Which Should You Choose? ##
Arrays of ids can be a useful strategy when dealing with one-to-many or many-to-many scenarios. But more commonly, new developers are left deciding between using embedded documents versus doing "manual" referencing.

First, you should know that an individual document is currently limited to 16 megabytes in size. Knowing that documents have a size limit, though quite generous, gives you some idea of how they are intended to be used. At this point, it seems like most developers lean heavily on manual references for most of their relationships. Embedded documents are frequently leveraged, but mostly for smaller pieces of data which we want to always pull with the parent document. A real world example may be to store an `addresses` documents with each user, something like:

	db.users.insert({name: 'leto',
		email: 'leto@dune.gov',
		addresses: [{street: "229 W. 43rd St",
		            city: "New York", state:"NY",zip:"10036"},
		           {street: "555 University",
		            city: "Palo Alto", state:"CA",zip:"94107"}]})

This doesn't mean you should underestimate the power of embedded documents or write them off as something of minor utility. Having your data model map directly to your objects makes things a lot simpler and often removes the need to join. This is especially true when you consider that MongoDB lets you query and index fields of an embedded documents and arrays.

## Few or Many Collections ##
Given that collections don't enforce any schema, it's entirely possible to build a system using a single collection with a mishmash of documents but it would be a very bad idea.  Most MongoDB systems are laid out somewhat similarly to what you'd find in a relational system, though with fewer collections. In other words, if it would be a table in a relational database, there's a chance it'll be a collection in MongoDB (many-to-many join tables being an important exception as well as tables that exist only to enable one to many relationships with simple entities).

The conversation gets even more interesting when you consider embedded documents. The example that frequently comes up is a blog. Should you have a `posts` collection and a `comments` collection, or should each `post` have an array of `comments` embedded within it? Setting aside the 16MB document size limit for the time being (all of *Hamlet* is less than 200KB, so just how popular is your blog?), most developers should prefer to separate things out. It's simply cleaner, gives you better performance and more explicit.  MongoDB's flexible schema allows you to combine the two approaches by keeping comments in their own collection but embedding a few comments (maybe the first few) in the blog post to be able to display them with the post.  This follows the principle of keeping together data that you want to get back in one query.

There's no hard rule (well, aside from 16MB). Play with different approaches and you'll get a sense of what does and does not feel right.

## 小结 ##
本章目标是提供一些对你在 MongoDB 中数据建模有帮助的指导, 一个新起点，如果愿意你可以这样认为。在一个面向文档系统中建模，和在面向关系世界中建模，是不一样的，但也没多少不同。你能得到更多的灵活性并且只有一个约束，而对于新系统，一切都很完美。你唯一会做错的就是你不去尝试。

# 第五章 - MongoDB 使用场景 #
By now you should have a feel for where and how MongoDB might fit into your existing system. There are enough new and competing storage technologies that it's easy to get overwhelmed by all of the choices.

For me, the most important lesson, which has nothing to do with MongoDB, is that you no longer have to rely on a single solution for dealing with your data. No doubt, a single solution has obvious advantages, and for a lot projects - possibly even most - a single solution is the sensible approach. The idea isn't that you *must* use different technologies, but rather that you *can*. Only you know whether the benefits of introducing a new solution outweigh the costs.

With that said, I'm hopeful that what you've seen so far has made you see MongoDB as a general solution. It's been mentioned a couple times that document-oriented databases share a lot in common with relational databases. Therefore, rather than tiptoeing around it, let's simply state that MongoDB should be seen as a direct alternative to relational databases. Where one might see Lucene as enhancing a relational database with full text indexing, or Redis as a persistent key-value store, MongoDB is a central repository for your data.

Notice that I didn't call MongoDB a *replacement* for relational databases, but rather an *alternative*. It's a tool that can do what a lot of other tools can do. Some of it MongoDB does better, some of it MongoDB does worse. Let's dissect things a little further.

## Flexible Schema ##
An oft-touted benefit of document-oriented database is that they don't enforce a fixed schema. This makes them much more flexible than traditional database tables. I agree that flexible schema is a nice feature, but not for the main reason most people mention.

People talk about schema-less as though you'll suddenly start storing a crazy mishmash of data. There are domains and data sets which can really be a pain to model using relational databases, but I see those as edge cases. Schema-less is cool, but most of your data is going to be highly structured. It's true that having an occasional mismatch can be handy, especially when you introduce new features, but in reality it's nothing a nullable column probably wouldn't solve just as well.

For me, the real benefit of dynamic schema is the lack of setup and the reduced friction with OOP. This is particularly true when you're working with a static language. I've worked with MongoDB in both C# and Ruby, and the difference is striking. Ruby's dynamism and its popular ActiveRecord implementations already reduce much of the object-relational impedance mismatch. That isn't to say MongoDB isn't a good match for Ruby, it really is. Rather, I think most Ruby developers would see MongoDB as an incremental improvement, whereas C# or Java developers would see a fundamental shift in how they interact with their data.

Think about it from the perspective of a driver developer. You want to save an object? Serialize it to JSON (technically BSON, but close enough) and send it to MongoDB. There is no property mapping or type mapping. This straightforwardness definitely flows to you, the end developer.

## Writes ##
One area where MongoDB can fit a specialized role is in logging. There are two aspects of MongoDB which make writes quite fast. First, you have an option to send a write command and have it return immediately without waiting for the write to be acknowledged. Secondly, you can control the write behavior with respect to data durability. These settings, in addition to specifying how many servers should get your data before being considered successful, are configurable per-write, giving you a great level of control over write performance and data durability.

In addition to these performance factors, log data is one of those data sets which can often take advantage of schema-less collections. Finally, MongoDB has something called a [capped collection](http://docs.mongodb.org/manual/core/capped-collections/). So far, all of the implicitly created collections we've created are just normal collections. We can create a capped collection by using the `db.createCollection` command and flagging it as capped:

	//limit our capped collection to 1 megabyte
	db.createCollection('logs', {capped: true,
		size: 1048576})

When our capped collection reaches its 1MB limit, old documents are automatically purged. A limit on the number of documents, rather than the size, can be set using `max`. Capped collections have some interesting properties. For example, you can update a document but it can't change in size. The insertion order is preserved, so you don't need to add an extra index to get proper time-based sorting.  You can "tail" a capped collection the way you tail a file in Unix via `tail -f <filename>` which allows you to get new data as it arrives, without having to re-query it.

If you want to "expire" your data based on time rather than overall collection size, you can use [TTL Indexes](http://docs.mongodb.org/manual/tutorial/expire-data/) where TTL stands for "time-to-live".

## Durability ##
Prior to version 1.8, MongoDB did not have single-server durability. That is, a server crash would likely result in lost or corrupt data. The solution had always been to run MongoDB in a multi-server setup (MongoDB supports replication). Journaling was one of the major features added in 1.8. Since version 2.0 MongoDB enables journaling by default, which allows fast recovery of the server in case of a crash or abrupt power loss.   

Durability is only mentioned here because a lot has been made around MongoDB's past lack of single-server durability. This'll likely show up in Google searches for some time to come. Information you find about journaling being a missing feature is simply out of date.

## Full Text Search ##
True full text search capability is a recent addition to MongoDB.  It supports fifteen languages with stemming and stop words. With MongoDB's support for arrays and full text search you will only need to look to other solutions if you need a more powerful and full-featured full text search engine.  

## Transactions ##
MongoDB doesn't have transactions. It has two alternatives, one which is great but with limited use, and the other that is cumbersome but flexible.

The first is its many atomic update operations. These are great, so long as they actually address your problem. We already saw some of the simpler ones, like `$inc` and `$set`. There are also commands like `findAndModify` which can update or delete a document and return it atomically.

The second, when atomic operations aren't enough, is to fall back to a two-phase commit. A two-phase commit is to transactions what manual dereferencing is to joins. It's a storage-agnostic solution that you do in code. Two-phase commits are actually quite popular in the relational world as a way to implement transactions across multiple databases. The MongoDB website [has an example](http://docs.mongodb.org/manual/tutorial/perform-two-phase-commits/) illustrating the most typical example (a transfer of funds). The general idea is that you store the state of the transaction within the actual document being updated atomically and go through the init-pending-commit/rollback steps manually.

MongoDB's support for nested documents and flexible schema design makes two-phase commits slightly less painful, but it still isn't a great process, especially when you are just getting started with it.

## Data Processing ##
Before version 2.2 MongoDB relied on MapReduce for most data processing jobs. As of 2.2 it has added a powerful feature called  [aggregation framework or pipeline](http://docs.mongodb.org/manual/core/aggregation-pipeline/), so you'll only need to use MapReduce in rare cases where you need complex functions for aggregations that are not yet supported in the pipeline. In the next chapter we'll look at Aggregation Pipeline and MapReduce in detail. For now you can think of them as feature-rich and different ways to `group by` (which is an understatement).  For parallel processing of very large data, you may need to rely on something else, such as Hadoop. Thankfully, since the two systems really do complement each other, there's a [MongoDB connector for Hadoop](http://docs.mongodb.org/ecosystem/tools/hadoop/).

Of course, parallelizing data processing isn't something relational databases excel at either. There are plans for future versions of MongoDB to be better at handling very large sets of data.

## Geospatial ##
A particularly powerful feature of MongoDB is its support for [geospatial indexes](http://docs.mongodb.org/manual/applications/geospatial-indexes/). This allows you to store either geoJSON or x and y coordinates within documents and then find documents that are `$near` a set of coordinates or `$within` a box or circle. This is a feature best explained via some visual aids, so I invite you to try the [5 minute geospatial interactive tutorial](http://mongly.openmymind.net/geo/index), if you want to learn more.

## Tools and Maturity ##
You probably already know the answer to this, but MongoDB is obviously younger than most relational database systems. This is absolutely something you should consider, though how much it matters depends on what you are doing and how you are doing it. Nevertheless, an honest assessment simply can't ignore the fact that MongoDB is younger and the available tooling around isn't great (although the tooling around a lot of very mature relational databases is pretty horrible too!). As an example, the lack of support for base-10 floating point numbers will obviously be a concern (though not necessarily a show-stopper) for systems dealing with money.

On the positive side, drivers exist for a great many languages, the protocol is modern and simple, and development is happening at blinding speeds. MongoDB is in production at enough companies that concerns about maturity, while valid, are quickly becoming a thing of the past.

## 小结 ##
本章要说的是，MongoDB，大多数情况下，可以取代关系型数据库。它更简单更直接；更快速并且通常对应用开发者的约束更少。不过缺乏事务支持也许值得慎重考虑。当人们说起 *MongoDB 在新的数据库阵营中到底处在什么位置？* 时，答案很简单: **中庸**(*1*)。

# 第六章 - 数据聚合 #

## 聚合管道(Aggregation Pipeline) ##
Aggregation pipeline gives you a way to transform and combine documents in your collection.  You do it by passing the documents through a pipeline that's somewhat analogous to the Unix "pipe" where you send output from one command to another to a third, etc.

The simplest aggregation you are probably already familiar with is the SQL `group by` expression.  We already saw the simple `count()`  method, but what if we want to see how many unicorns are male and how many are female?  

	db.unicorns.aggregate([{$group:{_id:'$gender',
		total: {$sum:1}}}])

In the shell we have the `aggregate` helper which takes an array of pipeline operators.  For a simple count grouped by something, we only need one such operator and it's called `$group`.   This is the exact analog of `GROUP BY` in SQL where we create a new document with `_id` field indicating what field we are grouping by (here it's `gender`) and other fields usually getting assigned results of some aggregation, in this case we `$sum` 1 for each document that matches a particular gender.  You probably noticed that the `_id` field was assigned `'$gender'` and not `'gender'` - the `'$'` before a field name indicates that the value of this field from incoming document will be substituted.

What are some of the other pipeline operators that we can use?  The most common one to use before (and frequently after) `$group` would be `$match` - this is exactly like the `find` method and it allows us to aggregate only a matching subset of our documents, or to exclude some documents from our result.

	db.unicorns.aggregate([{$match: {weight:{$lt:600}}},
		{$group: {_id:'$gender',  total:{$sum:1},
		  avgVamp:{$avg:'$vampires'}}},
		{$sort:{avgVamp:-1}} ])
		
Here we introduced another pipeline operator `$sort` which does exactly what you would expect, along with it we also get `$skip` and `$limit`.  We also used a `$group` operator `$avg`.

MongoDB arrays are powerful and they don't stop us from being able to aggregate on values that are stored inside of them.  We do need to be able to "flatten" them to properly count everything:

	db.unicorns.aggregate([{$unwind:'$loves'},
     	{$group: {_id:'$loves',  total:{$sum:1},
	 	unicorns:{$addToSet:'$name'}}},
	  	{$sort:{total:-1}}, 
	  	{$limit:1} ])
		
Here we will find out which food item is loved by the most unicorns and we will also get the list of names of all the unicorns that love it.  `$sort` and `$limit` in combination allow you to get answers to "top N" types of questions.

There is another powerful pipeline operator called [`$project`](http://docs.mongodb.org/manual/reference/operator/aggregation/project/#pipe._S_project) (analogous to the projection we can specify to `find`) which allows you not just to include certain fields, but to create or calculate new fields based on values in existing fields.  For example, you can use math operators to add together values of several fields before finding out the average, or you can use string operators to create a new field that's a concatenation of some existing fields.

This just barely scratches the surface of what you can do with aggregations.  In 2.6 aggregation got more powerful as the aggregate command returns either a cursor to the result set (which you already know how to work with from Chapter 1) or it can write your results into a new collection using the `$out` pipeline operator.  You can see a lot more examples as well as all of the supported pipeline and expression operators in the [MongoDB manual](http://docs.mongodb.org/manual/core/aggregation-pipeline/). 

## MapReduce ##
MapReduce 分两步进行数据处理。首先是 map，然后 reduce。在 map 步骤中，转换输入文档和输出一个 key=>value 对(key 和/或 value 可以很复杂)。然后, key/value 对以 key 进行分组，有同样的 key 的 value 会被收入一个数组中。在 reduce 步骤中，获取 key 和该 key 的 value 的数组，生成最终结果。map 和 reduce 方法用 JavaScript 来编写。

在 MongoDB 中我们对一个集合使用 `mapReduce` 命令。 `mapReduce` 执行 map 方法， reduce 方法和 output 指令。在我们的 shell 中，我们可以创建输入一个 JavaScript 方法。许多库中，支持字符串方法 (有点丑)。第三个参数设置一个附加参数，比如说我们可以过滤，排序和限制那些我们想要分析的文档。我们也可以提供一个 `finalize` 方法来处理 `reduce` 步骤之后的结果。

在你的大多数聚合中，也许无需用到 MapReduce , 但如果需要，你可以读到更多关于它的内容，从 [我的 blog](http://openmymind.net/2011/1/20/Understanding-Map-Reduce/) 和 [MongoDB manual](http://docs.mongodb.org/manual/core/map-reduce/)。

## 小结 ##
在这章中我们介绍了 MongoDB 的 [聚合功能(aggregation capabilities)](http://docs.mongodb.org/manual/aggregation/)。 一旦你理解了聚合管道(Aggregation Pipeline)的构造，它还是相对容易编写的，并且它是一个聚合数据的强有力工具。 MapReduce 更难理解一点，不过它强力无边，就像你用 JavaScript 写的代码一样。

# 第七章 - 性能和工具 #
在这章中，我们来讲几个关于性能的话题，以及在 MongoDB 开发中用到的一些工具。我们不会深入其中的一个话题，不过我们会指出每个话题中最重要的方面。

## 索引(Index) ##
首先我们要介绍一个特殊的集合 `system.indexes` ，它保存了我们数据库中所有的索引信息。索引的作用在 MongoDB 中和关系型数据库基本一致: 帮助改善查询和排序的性能。创建索引用 `ensureIndex` :

	// where "name" is the field name
	db.unicorns.ensureIndex({name: 1});

删除索引用 `dropIndex`:

	db.unicorns.dropIndex({name: 1});

可以创建唯一索引，这需要把第二个参数 `unique` 设置为 `true`:

	db.unicorns.ensureIndex({name: 1},
		{unique: true});

索引可以内嵌到字段中 (再说一次，用点号) 和任何数组字段。我们可以这样创建复合索引:

	db.unicorns.ensureIndex({name: 1,
		vampires: -1});

索引的顺序 (1 升序, -1 降序) 对单键索引不起任何影响，但它会在使用复合索引的时候有所不同，比如你用不止一个索引来进行排序的时候。

阅读 [indexes page](http://docs.mongodb.org/manual/indexes/) 获取更多关于索引的信息。

## Explain ##
需要检查你的查询是否用到了索引，你可以通过 `explain` 方法:

	db.unicorns.find().explain()

输出告诉我们，我们用的是 `BasicCursor` (意思是没索引), 12 个对象被扫描，用了多少时间，什么索引，如果有索引，还会有其他有用信息。

如果我们改变查询索引语句，查询一个有索引的字段，我们可以看到 `BtreeCursor` 作为索引被用到填充请求中去:

	db.unicorns.find({name: 'Pilot'}).explain()

## Replication ##
MongoDB 的复制在某些方面和关系型数据库的复制类似。所有的生产部署应该都是副本集，理想情况下，三个或者多个服务器都保持相同的数据。写操作被发送到单个服务器，也即主服务器，然后从它异步复制到所有的从服务器上。你可以控制是否允许从服务器上进行读操作，这可以让一些特定的查询从主服务器中分离出来，当然，存在读取到的数据陈旧的风险。如果主服务器异常关闭，从服务中的一个将会自动晋升为新的主服务器继续工作。另外，MongoDB 的复制不在本书的讨论范围之内。

## Sharding ##
MongoDB 支持自动分片。分片是实现数据扩展的一种方法，依靠在跨服务器或者集群上进行数据分区来实现。一个最简单的实现是把所有的用户数据，按照名字首字母 A-M 放在服务器 1 ，然后剩下的放在服务器 2。谢天谢地，MongoDB 的拆分能力远比这种分法要强。分片不在本书的讨论范围之内，不过你应当有分片的概念，并且，当你的需求增长超过了使用单一副本集的时候，你应该考虑它。

尽管复制有时候可以提高性能(通过隔离长时间查询到从服务器，或者降低某些类型的查询的延迟),它的主要目的是维护高可用性。分片是扩展 MongoDB 集群的主要方法。把复制和分片结合起来实现可扩展和高可用性是禁术。 

## Stats ##
你可以通过 `db.stats()` 查询数据库的状态。基本上都是关于数据库大小的信息。你还可以查询集合的状态，比如说 `unicorns` 集合，可以输入 `db.unicorns.stats()`。基本上都是关于集合大小的信息，以及集合的索引信息。

## Profiler ##
你可以这样执行 MongoDB profiler :

	db.setProfilingLevel(2);

启动之后，我们可以执行一个命令:

	db.unicorns.find({weight: {$gt: 600}});

然后检查 profiler:

	db.system.profile.find()

输出会告诉我们:什么时候执行了什么，有多少文档被扫描，有多少数据被返回。

你要停止 profiler 只需要再调用一次 `setProfilingLevel` ，不过这次参数是 `0`。指定 `1` 作为第一个参数，将会过滤统计超过 100 milliseconds 的任务. 100 milliseconds 是默认的阈值，你可以在第二个参数中，指定不同的阈值时间，以 milliseconds 为单位:

	//profile anything that takes
	//more than 1 second
	db.setProfilingLevel(1, 1000);

## 备份和还原 ##
在 MongoDB 的 `bin` 目录下有一个可执行文件 `mongodump` 。简单执行 `mongodump` 会链接到 localhost 并备份你所有的数据库到 `dump` 子目录。你可以用 `mongodump --help` 查看更多执行参数。常用的参数有 `--db DBNAME` 备份指定数据库和 `--collection COLLECTIONNAME` 备份指定集合。你可以用 `mongorestore` 可执行文件，同样在 `bin` 目录下，还原之前的备份。同样， `--db` 和 `--collection` 可以指定还原的数据库和/或集合。 `mongodump` 和 `mongorestore` 使用 BSON，这是 MongoDB 的原生格式。

比如，来备份我们的 `learn` 数据库导 `backup` 文件夹，我们需要执行(在控制台或者终端中执行该命令，而不是在 mongo shell 中):

	mongodump --db learn --out backup

如果只还原 `unicorns` 集合，我们可以这样做:

	mongorestore --db learn --collection unicorns \
		backup/learn/unicorns.bson

值得一提的是， `mongoexport` 和 `mongoimport` 是另外两个可执行文件，用于导出和从 JSON/CSV 格式文件导入数据。比如说，我们可以像这样导出一个 JSON:

	mongoexport --db learn --collection unicorns

CSV 格式是这样:

	mongoexport --db learn \
		--collection unicorns \
		--csv --fields name,weight,vampires

注意 `mongoexport` 和 `mongoimport` 不一定能正确代表数据。真实的备份中，只能使用 `mongodump` 和 `mongorestore` 。  你可以从 MongoDB 手册中读到更多的 [备份须知](http://docs.mongodb.org/manual/core/backups/) 。

## 小结 ##
在这章中我们介绍了 MongoDB 的各种命令，工具和性能细节。我们没有涉及所有的东西，不过我们已经把常用的都看了一遍。MongoDB 的索引和关系型数据库中的索引非常类似，其他一些工具也一样。不过，在 MongoDB 中，这些更易于使用。

# 总结 #
你现在应该有足够的能力开始在真实项目中使用 MongoDB 了。虽然 MongoDB 远不止我们学到的这些内容，但是你要作的下一步是，把学到的知识融会贯通，熟悉我们需要用到的功能。[MongoDB website](http://www.mongodb.org/) 有许多有用的信息。官网的 [MongoDB user group](http://groups.google.com/group/mongodb-user) 是个问问题的好地方。

NoSQL 不光是为需求而生，它同时还是不断尝试创新的成果。不得不承认，我们的领域是不断前行的。如果我们不尝试，一旦失败，我们就绝不会取得成功。就是这样的，我认为，这是让你在职业生涯一路走好的方法。

----------

*1* :参考 [justinyhuang][1] 的翻译。MongoDB 属于 NoSQL，但是和传统关系型数据库类似，且较为通用。


[1]:https://github.com/justinyhuang/the-little-mongodb-book-cn/blob/master/mongodb.md
