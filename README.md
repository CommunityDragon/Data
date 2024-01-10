# Data

Additional data provided by CDragon.


## Repository setup

A special merge driver is used for hash files.
Add the following to `.git/config` to enable it (replace `<root>` by local repository path).
```
[merge "hashfile"]
  name = "Merge hash list files"
  driver = <root>/tools/git-merge-hashes %A %B
```

Moreover, `tools/hashes-prepare` should be run before comitting hashes.


## Hashes

Client and game files use various hashes.
Those hashes are "reversed" to get the associated human-readable string.


### Format

Hashes are stored as plain text files, one line per hash: `hash string`.
Hash is formatted as lowercase hexadecimal. Its size depends on the hashing algorithm (usually, 32 or 64 bits).
Lines are sorted by string for convenience.

Each collection of hashes is stored in its own file: `hashes.{name}.txt`.
However, file that are too large are split; this is required because of Github file size limit.
Files are then named `hashes.{name}.txt.0`, `hashes.{name}.txt.1`, ...

The shell script `tools/hashes-merge` merges split files.
It can be used from a Git hook to automatically merge hashes when updating the repository.

The shell script `tools/hashes-prepare` merge, sort and splits hash files.
It is intended to be used by maintainers to prepare files for commit.

The shell script `tools/git-merge-hashes` is a Git merge driver to automatically merge hash files.


### WAD paths

Paths of files in WAD archives.
64-bit [XXH64](https://xxhash.com/) with seed 0, applied on lowercased string value.
Paths use mostly letters, digits and characters from `._-/`. Rare occurrences also use spaces and `@`.

- `hashes.game.txt` -- hashes from game archives (`.wad.client` files)
- `hashes.lcu.txt` -- hashes from client archives (`.wad` files)

### Bin hashes

Paths of files in bin files.
32-bit [FNV-1a](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function#FNV-1a_hash) applied on lowercased string value.

- `hashes.binentries.txt` -- bin entry path
- `hashes.bintypes.txt` -- type name for a structure
- `hashes.binfields.txt` -- field name from a structure
- `hashes.binhashes.txt` -- hashed identifier value

### Translation files

Key for a *stringtrable* translation file. 
Truncated 64-bit [XXH64](https://xxhash.com/) with seed 0, applied on lowercased string value.
Hash value is truncated to 39 bits (lower bits are kept). Older versions truncated to only 10 bits.

- `hashes.rst.txt` -- stringtable keys (hashes are truncated to 40 bits)

