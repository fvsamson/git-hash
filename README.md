# git-hash
**Understanding Git's hash calculations and implementing them in shell code**

### Sources
These provided all necessary information without resorting to reading Git's source code, but at least most the shell implementations were not completely correct; but their flaws only start to show, when reading files with embedded newlines or other formatting characters.

WWW search: https://www.startpage.com/do/settings?query=git%20hash%20calculation
- https://stackoverflow.com/a/68806436
- https://gist.github.com/masak/2415865<br />
  Mostly for `git hash-object -t tree`, convoluted but good conversation
- https://stackoverflow.com/questions/14790681/what-is-the-internal-format-of-a-git-tree-object
- https://stackoverflow.com/questions/7225313/how-does-git-compute-file-hashes
- What do Git's file filters (i.e. automatic conversions of file contents) do?<br />
  https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration#_formatting_and_whitespace
- https://stackoverflow.com/questions/6011172/how-to-compute-the-git-hash-object-of-a-directory

Documentation for `git hash-object`, which is more comprehensible than is man-page:<br />
https://git-scm.com/docs/git-hash-object

Git internal workings
- https://git-scm.com/book/en/v2/Git-Internals-Git-Objects
- https://stackoverflow.com/questions/8198105/how-does-git-store-files
- https://nfarina.com/post/9868516270/git-is-simpler
- https://maryrosecook.com/blog/post/git-from-the-inside-out

Git's transition from SHA-1 to SHA-256
- https://lwn.net/Articles/898522/
- https://git-scm.com/docs/hash-function-transition

### Hashing a file

A file corresponds to Git's type `blob`.

`git hash-object -t blob --no-filters --literally "$filename"`
`git hash-object -t blob "$filename"`
`git hash-object "$filename"`

`printf 'blob %s\0' "$(wc -c < "$filename")" | cat - "$filename" | sha1sum`
`printf 'blob %s\0' "$(find -L "${filename%/*}" -maxdepth 1 -name "${filename##*/}" -printf %s)" | cat - "$filename" | sha1sum`
`printf 'blob %s\0' "$(find -L "$(dirname "$filename")" -maxdepth 1 -name "$(basename "$filename")" -printf %s)" | cat - "$filename" | sha1sum`
`stat --printf='blob %s\0' "$filename" | cat - "$filename" | sha1sum`

### Recreating an object entry

ToDo

### Recreating a `tree` object's hash

Comprises one or multiple object entries.

ToDo
