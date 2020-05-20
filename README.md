[![Build Status](https://travis-ci.org/mehmetalisavas/mongo-cache.svg?branch=master)](https://travis-ci.org/mehmetalisavas/mongo-cache)

## Cache using MongoDB for Go

## Install

```bash
go get github.com/mehmetalisavas/mongo-cache
```

## Description about package
In this package, while creating constructor, self-referential function is used to avoid passing nil value to the function as parameter.
It might be confusing at first, but all required functions are granted for usage.
After creating constructor, you can set necessary field with config options (you can see at examples or take a look tests)

## Usage

```go
session = initMongo() // you need to create a session for mongoDB

// Init MongoCache with defaults
NewMongoCacheWithTTL(session)

// Init MongoCache with TTL option
duration := time.Minute * 3
cacheTTL := NewMongoCacheWithTTL(session, SetTTL(duration))

// Also you can pass multiple config options while initing MongoCache
NewMongoCacheWithTTL(session, SetTTL(duration/2), SetGCInterval(duration), StartGC())

Granted Config options are listed below:
- func MustEnsureIndexExpireAt() Option
- func StartGC() Option // starts the garbage collector
- func SetTTL(duration time.Duration) Option // sets the ttl for cache
- func SetGCInterval(duration time.Duration) Option 
- func SetCollectionName(collName string) Option // sets the specific collection name for mongo session

```


## Examples
```go

// session is the default session with default options
// initMongo function is just used for testing.
// (you should initialize your own mongo db)
var session = initMongo()


mgoCache := NewMongoCacheWithTTL(session)
defer mgoCache.StopGC()

key := "cacheType"
value := "mongoDB"

if err := mgoCache.Set(key, value); err != nil {
    return err
}
data, err := mgoCache.Get(key) // "key" : "value"
if err != nil {
    return err
}

```
