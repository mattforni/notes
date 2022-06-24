# `uniq`
`uniq` is a command-line tool used to identify similar lines that are *adjacent* to each other. It includes options that allow for filtering, counting, grouping and displaying unique and duplicate lines.

## Usage
By default, `uniq` takes input and only displays the first line of duplicate lines in the output.

```bash
$ printf "mat\ncat\ncat\nbat\ntat" | uniq
mat
cat
bat
tat
```

If duplicate lines are not next to each other they will not be considered duplicates, and they may need to be sorted first. Sometimes using `sort -u` can achieve the intended results without having to pipe results to `uniq.

```bash
$ printf "mat\ncat\nbat\ncat\ntat" | uniq
mat
cat
bat
cat
tat

$ printf "mat\ncat\nbat\ncat\ntat" | sort -u
bat
cat
mat
tat

$ printf "mat\ncat\nbat\ncat\ntat" | sort | uniq
bat
cat
mat
tat
```

Note that first sorting the input **does not** preserve order. There are other command line tools that can do that.

The `-d` option can be provided to *only* output lines that have duplicate entries, and the `-D` option can be provided to output all duplicate lines.

```bash
$ printf "mat\nmat\ncat\ncat\ntat" | uniq -d    
mat
cat
$ printf "mat\nmat\ncat\ncat\ntat" | uniq -D 
mat
mat
cat
cat
```

The `-u` option can be provided to *only* output lines that are unique.

```bash
$ printf "mat\ncat\ncat\nbat\ntat" | uniq -u    
mat
bat
tat
```

The `--group` option can be provided to visually group similar lines, separating each group with a newline. This option accepts four values - `separate` (default), `prepend`, `append`, and `both` which indicates whether a newline is added before and/or after output.

```bash
$ printf "mat\ncat\ncat\nbat\ntat" | guniq --group=both

mat

cat
cat

bat

tat

```

The `-c` option can be provided to prepend each line with the number of times it has been repeated.

```bash
$ printf "mat\ncat\ncat\nbat\ntat" | uniq -c           
   1 mat
   2 cat
   1 bat
   1 tat
```

Output from usage of `sort` with the `-c` option can then be piped to `sort` for a sorted list of duplicates.

```bash
$ printf "mat\ncat\ncat\nbat\ntat" | uniq -c | sort -nr
   2 cat
   1 tat
   1 mat
   1 bat
```

The `-i` option can be provided to ignore casing when determining whether lines are duplicates, though only the first occurrence will be output.

```bash
$ printf "mat\nMat\ncat\ntAT\ntat" | uniq -i           
mat
cat
tAT
```

### Partial Match
`uniq` also supports three ways to partially match input lines.

The `-f` option can be provided to skip the first `N` fields (as defined by one or more space/tab characters) when determining duplicates.

```bash
# No duplicate lines
printf "1 mat\n1 cat\n2 cat\n4 bat\n-1 tat" | uniq              
1 mat
1 cat
2 cat
4 bat
-1 tat

# cat is a duplicate line
$ printf "1 mat\n1 cat\n2 cat\n4 bat\n-1 tat" | uniq -f1
1 mat
1 cat
4 bat
-1 tat
```

The `-s` option can be provided to skip the first `N` characters, while the `-w` option can be provided to restrict the match to the first `N` characters. These two options provide inverse functionality.

```bash
# Skip the first character in determining duplicates
$ printf "1mat\n1cat\n2cat\n4bat\n4tat" | uniq -s1
1mat
1cat
4bat
4tat

# Only use the first character in determining duplicates
$ printf "1mat\n1cat\n2cat\n4bat\n4tat" | guniq -w1
1mat
2cat
4bat
```

Note that when these options are used in conjunction, the priority is:
1. `-f`
2. `-s`
3. `-w`
