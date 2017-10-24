# MessagePack encoding for Golang

[![Build Status](https://travis-ci.org/mgechev/msgpack.svg?branch=v2)](https://travis-ci.org/mgechev/msgpack)
[![GoDoc](https://godoc.org/github.com/mgechev/msgpack?status.svg)](https://godoc.org/github.com/mgechev/msgpack)

The only difference from the original implementation by [@vmihailenco](https://github.com/vmihailenco) is the date serialization.

Since JavaScript cannot accurately represent uint64 we don't need to use 8 byte `time.Time` serialization.

Supports:
- Primitives, arrays, maps, structs, time.Time, strfmt.DateTime and interface{}.
- Appengine *datastore.Key and datastore.Cursor.
- [CustomEncoder](https://godoc.org/github.com/mgechev/msgpack#example-CustomEncoder)/CustomDecoder interfaces for custom encoding.
- [Extensions](https://godoc.org/github.com/mgechev/msgpack#example-RegisterExt) to encode type information.
- Renaming fields via `msgpack:"my_field_name"`.
- Omitting individual empty fields via `msgpack:",omitempty"` tag or all [empty fields in a struct](https://godoc.org/github.com/mgechev/msgpack#example-Marshal--OmitEmpty).
- [Map keys sorting](https://godoc.org/github.com/mgechev/msgpack#Encoder.SortMapKeys).
- Encoding/decoding all [structs as arrays](https://godoc.org/github.com/mgechev/msgpack#Encoder.StructAsArray) or [individual structs](https://godoc.org/github.com/mgechev/msgpack#example-Marshal--AsArray).
- Simple but very fast and efficient [queries](https://godoc.org/github.com/mgechev/msgpack#example-Decoder-Query).

API docs: https://godoc.org/github.com/mgechev/msgpack.
Examples: https://godoc.org/github.com/mgechev/msgpack#pkg-examples.

## Installation

Install:

```shell
go get -u github.com/mgechev/msgpack
```

## Quickstart

```go
func ExampleMarshal() {
	type Item struct {
		Foo string
	}

	b, err := msgpack.Marshal(&Item{Foo: "bar"})
	if err != nil {
		panic(err)
	}

	var item Item
	err = msgpack.Unmarshal(b, &item)
	if err != nil {
		panic(err)
	}
	fmt.Println(item.Foo)
	// Output: bar
}
```

## Benchmark

```
BenchmarkStructVmihailencoMsgpack-4   	  200000	     12814 ns/op	    2128 B/op	      26 allocs/op
BenchmarkStructUgorjiGoMsgpack-4      	  100000	     17678 ns/op	    3616 B/op	      70 allocs/op
BenchmarkStructUgorjiGoCodec-4        	  100000	     19053 ns/op	    7346 B/op	      23 allocs/op
BenchmarkStructJSON-4                 	   20000	     69438 ns/op	    7864 B/op	      26 allocs/op
BenchmarkStructGOB-4                  	   10000	    104331 ns/op	   14664 B/op	     278 allocs/op
```

## Howto

Please go through [examples](https://godoc.org/github.com/mgechev/msgpack#pkg-examples) to get an idea how to use this package.

## See also

- [Golang PostgreSQL ORM](https://github.com/go-pg/pg)
- [Golang message task queue](https://github.com/go-msgqueue/msgqueue)
