# MongoDB



### Getting started (using Docker):

Example of `docker-compose.yaml` file which creates two containers: 1) mongoDB 2) mongo express which is a GUI that can be used to view mongoDB.

```yaml
version: "3.8"
services:
  mongodb:
    image: mongo
    container_name: mongodb
    ports:
      - 27017:27017
    volumes:
      - data:/data
    environment:
      - MONGO_INITDB_ROOT_USERNAME=rootuser
      - MONGO_INITDB_ROOT_PASSWORD=rootpass
  mongo-express:
    image: mongo-express
    container_name: mongo-express
    restart: always
    ports:
      - 8081:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=rootuser
      - ME_CONFIG_MONGODB_ADMINPASSWORD=rootpass
      - ME_CONFIG_MONGODB_SERVER=mongodb

volumes:
  data: {}

networks:
  default:
    name: mongodb_network
```

Commands to get containers up and down / start and stop:

```shell
docker-compose -f docker-compose.yaml up
docker-compose -f docker-compose.yaml up -d

docker-compose down

docker-compose start
docker-compose stop
```

To view containers started/running and their ports, etc:

```shell
docker ps
docker-compose ps
```



### Mongo Shell

Allows you to interact with mongoDB via the shell.

First to connect to the container containing mongoDB:

1) Use `docker ps` or docker desktop app to find container ID
2) Get into the docker container using:

```shell
docker exec -it {containerID} bash
```

3. Access mongoDB using:

```shell
mongo mongodb://localhost:27017 -u username -p password
```



### MongoDB Commands

```shell
show dbs; // Show all databases
```

#### Create/Switch Database

`use` command to create or switch to db if exists

```shell
use {databaseName}
```

#### Delete Database

```shell
db.dropDatabase();
```

#### Other Database Commands

```shell
db.help(); // See all available database methods

db.getname(); // Get name of current database
```

#### Collections (Tables)

Mongo stores documents (rows) in collections (tables).

```shell
db.createCollection("{nameOfCollection}");
db.createCollection(name, {size: ..., capped: ..., max: ...});

show collections; // Show all collections
db.{collectionName}.stats(); // Show stats of collection
db.{collectionName}.count(); // Show number of documents inside collection
db.{collectionName}.drop(); // Delete collection
```

#### Documents (Rows)

MongoDB stores data records as BSON documents. BSON is a binary representation of JSON documents.

If you add a document and the collection does not exist, it automatically creates a collection.

```shell
person = {name: "Berkan Marasli"} // Save Javascript object
db.people.insert(person); // Automically creates collection "people"

db.{collectionName}.find(); // Get all documents inside collection
db.{collectionName}.find().pretty();
```

#### Insert Documents

```shell
db.{collectionName}.insert({document}); // Allows you to insert one document

db.{collectionName}.insertMany({arrayOfDocuments}); // Insert array of documents
// Example
students = [{name: "Berkan Marasli"}, {name: "Ernst Young"}]
db.students.insertMany(students)
```

#### Using `Find()`

![Screenshot 2022-05-15 at 17.42.52](/Users/Berkan.Marasli/Downloads/Screenshot 2022-05-15 at 17.42.52.png)

Query criteria: use to narrow down documents

Projection: use to select specific information from narrowed down documents. 1 means you want it selected. To exclude fields, use 0.

```shell
db.{collectionName}.find().pretty();
db.{collectionName}.find().pretty().count;
db.{collectionName}.find({name: "Berkan Marasli"}, {name: 1}).pretty();
```

#### Update Documents

Use `update()` keyword with `$set: {}`.

```shell
db.{collectionName}.update({_id: ObjectId("98a7dahsd9ad8a7sd")}, {$set: {name: "B Marasli"}});

db.{collectionName}.updateMany();

db.{collectionName}.update({_id: ObjectId("98a7dahsd9ad8a7sd")}, {$unset: {name: 1}}); // Remove a property from a document

db.{collectionName}.update({_id: ObjectId("98a7dahsd9ad8a7sd")}, {$inc: {totalSpent: 99}}); // increment by amount of 99

db.{collectionName}.update({_id: ObjectId("98a7dahsd9ad8a7sd")}, {$pull: {favouriteSubjects: "Maths"}}); // Remove Maths from favoruiteSubjects (array of favourite subjects)

db.{collectionName}.update({_id: ObjectId("98a7dahsd9ad8a7sd")}, {$push: {favouriteSubjects: "Maths"}}); // Add Maths to favoruiteSubjects (array of favourite subjects)
```

#### Delete Documents

Use `delete()` keyword.

Use `deleteOne()` keyword.

```shell
db.{collectionName}.deleteOne({_id: ObjectId("98a7dahsd9ad8a7sd")})

db.{collectionName}.deleteMany({gender: "Male"})
```



### Spring Boot Application - `application.properties` to connect to MongoDB

```java
// Based on above docker-compose.yaml file
spring.data.mongodb.authentication-database=admin
spring.data.mongodb.username=rootuser
spring.data.mongodb.password=rootpass
spring.data.mongodb.database={nameOfDatabaseToConnectTo}
spring.data.mongodb.port=27017
spring.data.mongodb.host=localhost
```

### Spring Boot Application - MongoRepository

Pre-reqs:

- @Document - identify java class that represents a Document.
- @Id - identify property that represents Document ID.

```java
public interface {RepositoryName} extends MongoRepository<{documentName}, {IdType}> {
  
}
```















