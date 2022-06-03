# `shuf`
[`shuf`](https://www.gnu.org/software/coreutils/manual/html_node/shuf-invocation.html) is a command-line tool that randomizes the provided input lines. The tool can limit the number of lines returned, repeat lines in the output, and even generate random positive integers.

## Randomizing input
By default, `shuf` will randomize the order of all input lines provided to it.

```bash
$ printf "hat\ncat\nmat\npat\ntat" | shuf       
tat
cat
pat
mat
hat
```

The number of lines included in the output can be limited using the `-n` option.

```bash
$ printf "hat\ncat\nmat\npat\ntat" | shuf -n 3
hat
mat
cat
```

Lines of input ran be repeated in the randomized output by providing the `-r` option.

```bash
printf "hat\ncat\nmat\npat\ntat" | shuf -n 3 -r
mat
mat
tat
```

The `-r` option is almost always used with the `-n` option since when the latter is omitted `shuf` outputs random lines indefinitely.

```bash
printf "hat\ncat\nmat\npat\ntat" | shuf -n 3 -r
mat
mat
tat
cat
tat
pat
...
```

## Input from an argument
`shuf` can also accept multiple input lines in the form of an argument via the `-e` option.

```bash
$ shuf -e hat cat mat pat tat
tat
mat
hat
pat
cat
```

The `-e` option also allows for `shuf` to autocomplete unquoted glob patterns providing a simple solution to selecting random files that match the provided glob.

```bash
$ shuf -e **/*.md -n 4
development/cli-tools/pr.md
development/react/markdown/prop-types.md
development/cli-tools/head-and-tail.md
development/react/markdown/styles.md
```

## Generate random numbers
`shuf` can also generate random positive numbers up to `2^64 -1` via the `-i` option.

```bash
shuf -i 10-25 -n 5
25
24
15
22
16
```

## Output file
Lastly, `shuf` can store the output into a file using the `-o` option.

```bash
$ shuf -i 10-25 -n 5 -o numbers.txt
$ cat numbers.txt
19
12
22
16
21

```