Reference: https://godoc.org/github.com/coreos/etcd/wal    

```
import "github.com/coreos/etcd/wal"
```

Package wal provides an implementation of a write ahead log that is used by etcd.    

A WAL is created at a particular directory and is made up of a number of segmented WAL files. Inside of each file the raft state and entries are appended to it with the Save method:
```
metadata := []byte{}
w, err := wal.Create(zap.NewExample(), "/var/lib/etcd", metadata)
...
err := w.Save(s, ents)
```
After saving a raft snapshot to disk, SaveSnapshot method should be called to record it. So WAL can match with the saved snapshot when restarting.
```
err := w.SaveSnapshot(walpb.Snapshot{Index: 10, Term: 2})
```
When a user has finished using a WAL it must be closed:
```
w.Close()
```

Each WAL file is a stream of WAL records. A WAL record is a length field and a wal record protobuf. The record protobuf contains a CRC, a type, and a data payload.     

## Variables   
```
var (
    // SegmentSizeBytes is the preallocated size of each wal segment file.
    // The actual size might be larger than this. In general, the default
    // value should be used, but this is defined as an exported variable
    // so that tests can set a different segment size.
    SegmentSizeBytes int64 = 64 * 1000 * 1000 // 64MB

    ErrMetadataConflict = errors.New("wal: conflicting metadata found")
    ErrFileNotFound     = errors.New("wal: file not found")
    ErrCRCMismatch      = errors.New("wal: crc mismatch")
    ErrSnapshotMismatch = errors.New("wal: snapshot mismatch")
    ErrSnapshotNotFound = errors.New("wal: snapshot not found")
)
```

## func Exist   
func Exist(dir string) bool
Exist returns true if there are any files in a given directory.   


## func Repair   
```
func Repair(lg *zap.Logger, dirpath string) bool
```
Repair tries to repair ErrUnexpectedEOF in the last wal file by truncating.   

## func Verify   
```
func Verify(lg *zap.Logger, walDir string, snap walpb.Snapshot) error
```
Verify reads through the given WAL and verifies that it is not corrupted. It creates a new decoder to read through the records of the given WAL.    






























