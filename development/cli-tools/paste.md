# `paste`
`paste` is a command-line tool used to combine two or more inputs into a single output ordered in columns.

## Concatenating files
By default, `paste` inserts a tab character between input sources.

```bash
$ paste <(seq 5) <(seq 6 10)
1       6
2       7
3       8
4       9
5       10
```

The delimiter can be specified using the `-d` option with shell metacharacters wrapped in single quotes.

```bash
$ paste -d '|' <(seq 5) <(seq 6 10)
1|6
2|7
3|8
4|9
5|10
```

Inputs can also be interleaved by specifying `\n` as the delimiter.

```bash
$ paste -d '\n' <(seq 5) <(seq 6 10)
1
6
2
7
3
8
4
9
5
10
```

### Multicharacter delimiters
The `-d` option also accepts a list of characters, using each character as a delimiter, in order, for each line of output. If there are more delimiters than need the remaining delimiters will be ignored. If fewer delimiters than are needed are specified they will be recycled, started over at the beginning.

```bash
# Multicharacter delimiter
$ paste -d ',-' <(seq 3) <(seq 4 6) <(seq 7 9)
1,4-7
2,5-8
3,6-9

# More delimiters are specified than needed
$ paste -d ',- \/' <(seq 3) <(seq 4 6) <(seq 7 9) <(seq 10 12) <(seq 13 15)
1,4-7 10/13
2,5-8 11/14
3,6-9 12/15

# Fewer delimiters are specified than needed
$ paste -d ',-' <(seq 3) <(seq 4 6) <(seq 7 9) <(seq 10 12) <(seq 13 15)
1,4-7,10-13
2,5-8,11-14
3,6-9,12-15
```

## Specifying columns 
Single inputs can be formatted into multiple columns by using the `-` character. Each instance of `-` will consume a line of input and append it as a column in the output.

```bash
seq 5 | paste - - - 
1       2       3
4       5
```

## Serializing
The `-s` option serializes all lines of an input into a single line. If multiple inputs are provided each input will be represented as an individual line in the output.

```bash
$ paste -s -d - <(printf 'mat\ncat\nhat\ntat')
mat-cat-hat-tat

# Each input gets its own line of output
$ paste -sd, <(seq 3) <(seq 5 9)
1,2,3
5,6,7,8,9
```