# `split`
`split` is a command-line tool used to split inputs into output files based on lines, bytes, or file size. By default, `split` separates input into a separate file every 1000 lines and outputs files with the prefix `x` and a two character alphabet suffix starting with `aa`. 

```bash
# Split every 1000 lines by default
$ seq 3000 | split
$ head -n 1 xaa xab xac
==> xaa <==
1

==> xab <==
1001

==> xac <==
2001
```

The `-l` option can be provided to change the number of lines to be included in each file.

```bash
# Split ever 5 lines
$ seq 15 | split -l 5
$ head -n 5 xaa xab xac

head -n 2 *
==> xaa <==
1
2

==> xab <==
6
7

==> xac <==
11
12
```

The `-b` option can be provided to split the input by a number of bytes. This option also accepts human-readable suffixes (e.g. `K` for `1024` bytes, `KB` for `1000` bytes, `M` for `1024 * 1024` bytes).

```bash
$ printf "mat\tcat\tbat\ttat\tfat\tsat" | split -b 10
$ head *     
==> xaa <==
mat     cat     ba
==> xab <==
t       tat     fat
==> xac <==
sat
```

The `-C` option behaves much like `-b` accept that it tries to break on line boundaries where possible. However, a line will still be broken into multiple parts if it exceeds the given limit.

```bash
$ printf "mat\tcat\tbat\ntat\tfat\tsat" | split -C 10
$ head *
==> xaa <==
mat     cat     ba
==> xab <==
t

==> xac <==
tat     fat     sa
==> xad <==
t
```

The `-t` option can be provided to specify a custom single byte character to be used as the line separator.

## The `-n` option
The `-n` option has many uses.

When the argument is strictly numeric (i.e. `N`) `split` will separate the input into `N` files of roughly the same size.

```bash
$ printf "mat\tcat\tbat\ttat\tfat\tsat" | split -n 3  
$ head *
==> xaa <==
mat     cat
==> xab <==
        bat     ta
==> xac <==
t       fat     sat
```

**Note** that `stdin` data cannot be used with this incarnation of the `-n` option since division is based on the total, known input size.

When the argument specifies two numeric values separated by a `/` (i.e. `K/N`) `split` will output the `K`th chunk of the results of running the command *without* creating any output files.

```bash
$ split -n 2/3 example.txt 
        bat     ta
```

The argument can be preceded by an `l/` to instruct `split` to avoid splitting lines when chunking the input into output files. 

The argument can be preceded by a `r/` to instruct `split` to interleave lines between output files instead of outputting to a single file before outputting to the next file.

```
$ seq 9 | gsplit -n r/3
$ head *
==> xaa <==
1
4
7

==> xab <==
2
5
8

==> xac <==
3
6
9
```

## Custom Filenames
By default, `x` is used at the prefix for all files created by `split`. Providing an argument after the input source allows for specifying a custom filename.

The `-a` option can be provided to change the length of the suffix and the `-d` option can be provided to use numeric instead of alphabetic suffixes.

```bash
split -n 3 example.txt split_
forni@waldenpond ex % head *
==> example.txt <==
mat     cat     bat     tat     fat     sat
==> split_aa <==
mat     cat
==> split_ab <==
bat     ta
==> split_ac <==
t       fat     sat

split -n 3 -d -a 4 example.txt split_
forni@waldenpond ex % head *
==> example.txt <==
mat     cat     bat     tat     fat     sat
==> split_0000 <==
mat     cat
==> split_0001 <==
        bat     ta
==> split_0002 <==
t       fat     sat
```

## Preprocessing
The `--filter` option can be provided to apply additional commands to the intermediate results of the `split` command before the data is output. In the filter clauses the output filename can be referenced with `$FILE`.

```bash
$ cat example.txt 
%=%=
mat
cat
bat
%=%=
tat
fat
sat

# Split and filter out the first line of each output file
$ gsplit -n l/2 --filter 'tail -n +2 > $FILE' example.txt

$ head x*
==> xaa <==
mat
cat
bat

==> xab <==
tat
fat
sat
```