# `pr`
`pr` is a command-line tool used to paginate or columnate files for printing. `pr` can be considered shorthand for "print."

## Columnate
The `--columns` and `-a` options can be used with `pr` to:
- split the input file and merge the lines as columns
- merge consecutive lines of input into columns

The `-N` options is equivalent to `--columns=N` and can be considered shorthand for separating the input into `N` columns. The default page width is **72 characters**, which means each column will be **72/N** characters, including the separator, which default to tab and space characters. The `-J` option will prevent the truncations of values that exceed 72/N characters and `-t` disables the pagination feature.

```bash
# split input into three parts
# each column width is 72/3 = 24 characters max
$ seq 9 | pr -3t
1                       4                       7
2                       5                       8
3                       6                       9
```

A custom separator can be specified by using the `-s` option, followed by the desired separator. When no character is provided following the `-s` option it defaults to tab and space characters. Some examples of the `-s` option in use are below:

```bash
# comma separator
$ seq 9 | pr -3ts,
1,4,7
2,5,8
3,6,9

# multicharacter separator
$ seq 9 | pr -3ts' : '
1 : 4 : 7
2 : 5 : 8
3 : 6 : 9
```

Providing the `-a` option will cause consecutive lines to be merged before columnating the results, resulting in a different ordering. See the example below:

```bash
$ seq 8 | pr -4ts:
1:3:5:7
2:4:6:8

$ seq 8 | pr -4ats:
1:2:3:4
5:6:7:8
```

## Page Width
`(N-1)*length(separator) + N` is the minimum page width needed to display a page, where `N` is the number of desired columns. The `-w` option can be provided to change the page width from the default value of `72`. If the page is not wide enough to display the desired number of columns, an error will be displayed:

```bash
$ seq 100 | pr -50ats,
pr: page width too narrow
```

## Concatenating Files
Two or more files can be concatenated column wise via the `-m` option. An example of the `-m` option is below:

```bash
$ pr -mts colors_1.txt colors_2.txt
Blue    Black
Brown   Blue
Orange  Green
Purple  Orange
Red     Pink
Teal    Red
White   White
```

The `-n` option will prepend each line with a line number, up to five-digit numbers. This option can take two parameters, the maximum number of digits to display (1-5) and the separator character to be used. If both are provided the separator should be specified first.

## Miscellaneous
The `-d` option can be provided to double-space the input content, effectively turning each new line character into two new line characters.

The `-v` option can be provided to replace non-printing characters (e.g. carriage return, backspace, etc.) with their octal representations.
