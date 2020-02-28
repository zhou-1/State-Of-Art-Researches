```
import "github.com/coreos/etcd/wal"
```

Package wal provides an implementation of a write ahead log that is used by etcd.    

A WAL is created at a particular directory and is made up of a number of segmented WAL files. Inside of each file the raft state and entries are appended to it with the Save method:
