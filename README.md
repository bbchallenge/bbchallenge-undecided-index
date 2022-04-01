# BBchallenge undecided index

The goal of [https://bbchallenge.org/](https://bbchallenge.org/) is to decided the behavior of all 5-state Turing machine (from all-0 tape), see [Story](https://bbchallenge.org/story).

We do so by writing [deciders](https://bbchallenge.org/method#deciders), which are programs that decide machines.

The machine that have not yet be decided by any decider are compiled in the index `bb5_undecided_index` stored in this repository. See [undecided machines index file](https://bbchallenge.org/method#undecided-machines-index-file).

This file is a binary file containing a sorted succession of big-endian uint32 (4-byte). Each of these integers is the index of an undecided machine in the [seed database](https://bbchallenge.org/method#seed-database). See https://github.com/bbchallenge/bbchallenge-seed. 

## Why on github?

By definition, this index file is meant to evolve thanks to new deciders being coded and hopefully reach a size of 0 when all the machines are decided.

Note also that sometimes we discover bugs in already applied deciders hence we may want to restore the index to a precedent version.

This is why using git is handy as we can keep track of each index's version and backtrack if needed.

## Reading the index

Here are some routines to help you reading the index:

*Python*
```Python
def get_indices_from_index_file(index_file_path):
  index_file_size = os.path.getsize(index_file_path)

  machines_indices = []
  with open(index_file_path, "rb") as f:
    for i in range(index_file_size//4):
      chunk = f.read(4)
      machines_indices.append(int.from_bytes(chunk, byteorder="big"))

  return machines_indices
 ```
 
 *Go*
 ```go
 func GetIndicesFromIndexFile(indexFilePath string) (
  machinesIndices []uint32, err error) {

  var rawIndex []byte
  rawIndex, err = ioutil.ReadFile(indexFilePath)

  if err != nil {
    return machinesIndices, err
  }

  for i := 0; i < len(rawIndex)/4; i += 4 {
    machinesIndices = append(
    machinesIndices, binary.BigEndian.Uint32(rawIndex[i:i+4]))
  }

  return machinesIndices, err
}
```
