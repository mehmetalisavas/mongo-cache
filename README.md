[![Build Status](https://travis-ci.org/mehmetalisavas/mongo-cache.svg?branch=master)](https://travis-ci.org/mehmetalisavas/mongo-cache)

## Cache using mongoDB for Go

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

Granted Config options:
- func MustEnsureIndexExpireAt() Option
- func StartGC() Option
- func SetTTL(duration time.Duration) Option
- func SetGCInterval(duration time.Duration) Option
- func SetCollectionName(collName string) Option

```


## Examples
```go
var (
	// session is the default session with default options
	session = initMongo()
)

mgoCache := NewMongoCacheWithTTL(session)
defer mgoCache.StopGC()

key := "cacheType"
value := "mongoDB"

if err := mgoCache.Set(key, value); err != nil {
    return err
}
data, err := mgoCache.Get(key) // data : "mongoDB"
if err != nil {
    return err
}

```



## License
©2016 Mehmet Ali Savas under the [MIT license](http://www.opensource.org/licenses/mit-license.php):

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
