# Storage & Retrieval
- In this chapter we are going to learn how diff. DB storage engines does the storage & retreival part
- We'll look at 2 main ones, named as:
    - Log-structured storage engines
    - Page-oriented storage engine such as B-Tree
- A simple storage to store a key value in a file system storage
```BASH
#!/bin/bash
db_set() {
    echo '$1,$2' >> database
}
db_get() {
    grep "/^$1,/" database | sed -e "s/^$1,//" | tail -n 1
}
```
- In the above, set is going to append always to the end of file, so there would be duplicate occurances if you are setting the key value multiple times

## SSTables(Sorted String) and LSM(Log Structured Merged)-Trees
- Store data in key value structure, sorted by key
- For constructing & maintaining SSTable, this is what is done, 
    - For incoming write, add it to a sorted tree DS like AVL or Red-Black tree, known as **memTable**
    - If it gets bigger, write to disk as an SSTable file, with incoming writes still writing to memTable
    - For read request, check in memTable, then in most recent on-disk segment, then going to next-older segment so on & so forth
    - Time to time, run a compaction process in the background, which can combine these segment files & discard overwritten or duplicates.
- LSM tree concept used by Lucene an indexing engine, which is used by ES and Solr
- For performance optimization, of a case where, a key doesn't exist in DB, storage engine often use additional **bloom filters**

## B-Trees
- Most popular, widely used by most relational DB as well as non-relational ones
- Uses the same key-value structure as SS-Tables, but that's where the similarity ends
- Rather than using variable size segments like LSM tree, B-Tree uses a fixed size block structure, about 4KB each, which matches with how disks are arranged
- No. of child od B-Tree are called branching factor
- B-Tree with n keys have depth O(logn)
- So most real world DB, can have a depth of 3 to 4 level deep, with a branching factor of 500, in reality that can store equivalent to 256TB data, which is huge

### Making B-Tree reliable
- The main diff. to LSM-tree, is in B-Tree it over-writes a page on disk with new data, unlike LSM where it appends always
- But this also require an additional DS **write-ahead log**, as if the DB has crashed while doing an insert operation, which involves separating a block to two. 
- Thus to ensure that no such child-block remained orphan incase of a crash, post recovery it reads from write-ahead log & construct the B-Tree again.

## Comparing B-Trees with LSM-Trees
- The common notion is B-Tree are good for faster reads where are LSM-Tree are good for faster write.
- 
