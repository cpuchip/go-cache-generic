# go-cache-generic

[![Test](https://github.com/cpuchip/go-cache-generic/actions/workflows/go.yml/badge.svg)](https://github.com/cpuchip/go-cache-generic/actions/workflows/go.yml)

It is a fork of [go-cache](https://github.com/patrickmn/go-cache) implemented with Go 1.18 generics.
Despite this enhancement, it remains
an in-memory key:value store/cache similar to memcached that is
suitable for applications running on a single machine. Its major advantage is
that, being essentially a thread-safe `map[string]T` with expiration
times, it doesn't need to serialize or transmit its contents over the network.

Any object can be stored, for a given duration or forever, and the cache can be
safely used by multiple goroutines.

Although go-cache isn't meant to be used as a persistent datastore, the entire
cache can be saved to and loaded from a file (using `c.Items()` to retrieve the
items map to serialize, and `NewFrom()` to create a cache from a deserialized
one) to recover from downtime quickly. (See the docs for `NewFrom()` for caveats.)

## Installation

`go get github.com/cpuchip/go-cache-generic`

## Usage

```go
import (
 "fmt"
 "github.com/cpuchip/go-cache-generic"
 "time"
)

func main() {
 // Create a cache with a default expiration time of 5 minutes, and which
 // purges expired items every 10 minutes
 c := cache.New[any](5*time.Minute, 10*time.Minute)

 // Set the value of the key "foo" to "bar", with the default expiration time
 c.Set("foo", "bar", cache.DefaultExpiration)

 // Set the value of the key "baz" to 42, with no expiration time
 // (the item won't be removed until it is re-set, or removed using
 // c.Delete("baz")
 c.Set("baz", 42, cache.NoExpiration)

 // Get the string associated with the key "foo" from the cache
 foo, found := c.Get("foo")
 if found {
  fmt.Println(foo)
 }
}
```
