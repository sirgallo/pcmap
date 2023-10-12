# PCMap

## Persistent-Concurrent Map


## Overview

The `pcmap` repository is an implementation of a concurrent hash array mapped trie that persists to disk using memory mapped files. Most chamts are implemented as in-memory only, which means that while the data structure itself is immutable, the data is volatile and the data structure will not persist on system restarts. This implementation looks to create a persistent version of a chamt so that the data structure is serialized and stored to disk, eliminating the need to rebuild the entire structure if a system restarts.

`pcmap` is inspired by `Boltdb`, which was my introduction into how databases work at a more fundamental level. `Boltdb` utilizes a memory-mapped file as its mechanism to persist the data to disk while also maintaining both high read and write speeds. The difference lies in the data structure used, however. `Boltdb` utilizes a `B+tree` as its underlying datastructure, which excels at sequential reads. However, hash array mapped tries excel at random reads and writes, with amortized time complexity of `O(1)` for both single read and write operations. 

This project is an exploration of memory mapped files and taking a different approach to storing and retrieving data within a database.


## Usage

```go
package main

import "os"
import "path/filepath"

import "github.com/sirgallo/pcmap"


func main() {
  // initialize the pcmap
  filepath := os.UserHomeDir() + "yourfilename"
  opts := pcmap.PCMapOpts{ Filepath: filepath }

  // open the pcmap
  pcMap := pcmap.Open(opts)

  key := []byte("hello")
  value := []byte("world")

  // put a value in the pcmap
  _, putErr = pcMap.Put(key, value)
  if putErr != nil { panic(putErr.Error()) }

  // get a value in the pcmap
  fetched, getErr = pcMap.Get(key)
  if getErr != nil { panic(getErr.Error()) }

  // delete a value in the pcmap
  _, delErr = pcMap.Delete(key)
  if delErr != nil { panic(delErr.Error()) }
}
```


## Tests

`pcmap`
```bash
go test -v ./tests
```

`murmur`
```bash
go test -v ./common/murmur/tests
```

`mmap`
```bash
go test -v ./common/mmap/tests
```


## godoc

For in depth definitions of types and functions, `godoc` can generate documentation from the formatted function comments. If `godoc` is not installed, it can be installed with the following:
```bash
go install golang.org/x/tools/cmd/godoc
```

To run the `godoc` server and view definitions for the package:
```bash
godoc -http=:6060
```

Then, in your browser, navigate to:
```
http://localhost:6060/pkg/github.com/sirgallo/pcmap/
```


## Sources

[CMap](./docs/CMap.md)

[PCMap](./docs/PCMap.md)

[Murmur](./docs/Murmur.md)

[Tests](./docs/Tests.md)