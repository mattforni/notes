# `basename` and `dirname`
`basename` and `dirname` are command-line tools used to extract the last (usually filename) and directory components of a filepath respectively.

## `basename`
By default, the `basename` command removes the leading directory component of a path and any trailing slashes.

```bash
# Removes the leading directory
$ basename /Users/forni/Notes/development/cli-tools/basename-and-dirname.md
basename-and-dirname.md

# Removes the leading directory and the trailing slash
$ basename /Users/forni/Notes/development/cli-tools/
cli-tools
```

The `-s` option can be provided to remove any suffix from the output, though it is most often used to remove the file extension. If removing the provided  suffix would result in an empty output, the removal is not performed.`

```bash
$ basename -s '.md' /Users/forni/Notes/development/cli-tools/basename-and-dirname.md
basename-and-dirname

$ basename -s '-and-dirname.md' /Users/forni/Notes/development/cli-tools/basename-and-dirname.md
basename

# No suffix removal is performed since the output would be empty
$ basename -s 'basename-and-dirname.md' /Users/forni/Notes/development/cli-tools/basename-and-dirname.md
basename-and-dirname.md
```

## `dirname`
By default, the `dirname` command removes the last path component (after first removing any trailing slashes). `dirname` accepts multiple arguments, removing the last path component from each and outputting the result.

```bash
$ dirname /Users/forni/Notes/development/cli-tools/basename-and-dirname.md
/Users/forni/Notes/development/cli-tools

$ dirname /Users/forni/Notes/development/cli-tools/
/Users/forni/Notes/development

$ dirname /Users/forni/Notes/development/cli-tools/basename-and-dirname.md /Users/forni/Notes/development/cli-tools/
/Users/forni/Notes/development/cli-tools
/Users/forni/Notes/development
```

The `basename` and `dirname` command can be combined using command substitution to extract other components of the path.

```bash
# Extract the second to last component of the path
$ basename $(dirname /Users/forni/Notes/development/cli-tools/basename-and-dirname.md)
cli-tools
```
