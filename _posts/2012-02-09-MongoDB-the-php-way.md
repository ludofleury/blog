---
layout: post
title: MongoDB, the PHP way.
author: Ludovic Fleury
---

MongoDB, There is no spoon
--------------------------

### Epic benchmark
Maybe you're like Sauron (a really-dark lord fighting with troll) and you love epic battles from the tech WWF league like
* "php" vs "javascript"
* "nginx" vs "node"
* "microsoft" vs "apple"

So instead of reading this article, I recommend you to hit another time the lovely godwin point with some tasty geek fights
* "Stormtroopers" vs "300 spartans"
* "NY Giants" vs "Lakers"
* "My kitchen" vs "My toilets" (c) @futurecat.

### The NoWay
If you're still reading, you're one of this guy which do not compare pears and apples. Good point.
Yet, did you ever try to define a pears from an apple ? Never. This is why :
* Pear is a NoApple fruit.
* Website is a NoPrint media.
* MongoDB is a NoSQL storage.

Let's try to go further in this non-sense :
If you start eating a pear while defining it as a NoApple, that's could be a huge deception:
* It's not the same shape
* It's not the same taste
* It's not the same color
* It's not the same price
Yet, It's a fruit and it's grow on a tree.

If you want to embrace MongoDB, for a trial session or your next project, don't take the pessismist path:
* It's not an autoincremented id
* It's not a table
* It's not a row
* It's not a transaction
Yet, It's a tool and it's store data.

Convinced ? Let's start a different approach.

MongoDB, Simple freedom
-----------------------

In the old book of the ancestor, there is an unknown prophecy which says:

> One day, the one will bring back the equilibre to the force
> Once, the two legendary ennemies, developer and ops, will regroup them into a new order
> wich will be called [DevOps]().

## JSON is Freedom
MongoDB is a NoSchema designed storage. Sorry, MongoDB is a schema-free designed storage. To be explicit: you are not constrained by any rule in your data design.
So if you want to store anything anywhere, yes -we- you can. The schema is not owned by the DBAdmin anymore, it bellows to the dev team.

If you're developper, you should feel like when you left the parent's house to fly with your own wings:
* Day 1 (Enthusiast): "Woohoo life!".
* Month 7 (Lost): "I'm hungry, I have nothing to wear, I have no money".

Same could quickly apply to your first MongoDB project:
* Day 1 (Promoted): "Woohoo let's store what I want, where I want".
* Month 7 (fired): "Oh my god, I've got 16M documents whith 16M different versions".

Now, go watch [spiderman at 1:30]() and remember that: "with great power comes great responsabilities".

Schemaless feature is allowed thank to the format used by MongoDB. Basically you define your document in JSON, example of what you can store :

```javascript
{ "name": "The empire strike back", "price": 6,66, "store": ["virgin","fnac","wallmart"] }
```
You just have to insert this document with a query and voila.

## BSON is Speed

BSON is a binary representation of JSON, it's _a standalone format_, that means interoperability come into the game. As any format, it carry specifics:
* It allows fast lookup.
* Definition (schema) is bounded to the data.
* It wasn't designed to be compact.
* Eases support and perf for any langage drivers. (?????)

Tons of pretty cool informations we're spreaded on the net: [basics about the format](), some [advanced stuffs](), Dwight "The Great CEO" love [to talk about it]().
For your first MongoDB encounter, the only thing you need to know is MongoDB store BSON. So when you're saving data, that this format which is used.

## FTL Jump Demo

MongoDB is composed of Database < Collection < Document.
Example, I can use a database called "store", where I named a collections "products", which contains many documents.

MongoDB require only one thing in a document: an unique id(entifier) under a specific pattern "_id". If you're a rebel, you could send the document without id
yet MongoDB will add it automatically __(and many MongoDB native drivers, like the PHP one, will add it before sending the document to your MongoDB server)__.

```php
$data = json_decode({ "name": "The empire strike back", "price": 6,66, "store": ["virgin","fnac","wallmart"] });

new Mongo('mongodb://localhost:27017')
    ->SelectDB('store')
    ->SelectCollection('products')
    ->save($data);
```

[KISS]() notice: __to be able to execute this query and store this document, I needed only one step : start the MongoDB server__
_no configuration for the server, no query to create the database, no query to create the collection, no query to define a schema._

MongoDB, Clone wars outsider
----------------------------

Every dark tech lord fight for one of the most trending topic: scalability.
Good news, it's probably the best battlefield for [10gen]() to sell you MongoDB. This storage is awesome for replication because many high-risk human operations are deferred to automatic out-of-the-box processes.
We use it at Plemi, we use it at Retentio, _it's really simple to configure replication; moreover, it works._
That mean, I tried the scariest 2012 scenario for our architecture, (__kill mongo__, __sudo reboot__) and it worked.

## One JSON config to rule them all

From one server, with one JSON and one query, you're able to setup a powerfull & scalable replica set.
Consider 3 MongoDB servers. You start them in a standalone fashion, then on one of them you configure what we call a "replica set" (i.e. replication setup).

```javascript
rs.initiate({ name: "robot", ...})
```

If George Clooney was DBA he would say "What else ?"
To be clear, with this poor litle query, every host will be "contacted" and automatically "connected" together. If you have a simpler solution, then you should start thinking to setup your own startup (11gen ?).

## Democracy instead slavery

I won't talk about the past version of MongoDB, so right now MongoDB uses a Primary/secondary setup instead of the classic Master-Slave.
Consider 3 MongoDB servers [configured with a poor JSON]() for your replica set. Once aware of each others, the servers will vote and elect __one__ primary and many secondaries (two in our example).
Every read and write are done on the Primary, insert and update operation are replicated to the secondary.

##

Yet, the PHP driver (and probably other drivers) comes with lot of option.
* They are able to

