# `head` and `tail`
`cat` and `less` are great tools for viewing the entire contents of a file, all at once or in pages respectively, but when only need to take a peak at a file or extract explicit lines the `head` and `tail` commands are particularly effective.

## Leading and Trailing Lines
`head` and `tail` will show the first and last ten (10) lines of a file respectively. If the file has fewer than ten 
(10) lines, the entire contents of the file will be printed. The `-nN` option can be provided to adjust the number 
of leading or trailing lines to display. The number of lines to show should be provided in the place of `N`. An 
example is below:

```bash
# first three lines
# space between -n and N is optional
$ head -n3 sample.txt
 1) Hello World
 2)
 3) Hi there

# last two lines
$ tail -n2 sample.txt
14) He he he
15) Adios amigo
```

## Excluding Lines
`head` can be instructed to select all leading lines, expect the `N` number of lines that would be returned from `tail -nN` by calling `head -n -N`. An example is below:

```bash
# except the last 11 lines
# space between -n and -N is optional
$ head -n -11 sample.txt
 1) Hello World
 2)
 3) Hi there
 4) How are you
```

## Starting from the Nth Line
`tail` can be instructed to select all trailing lines, except the `N-1` number of lines that would be returned from 
`head -nN` by calling `tail -n +N`. And example is below:

```bash
# all lines starting from the 11th line
# space between -n and +N is optional
$ tail -n +11 sample.txt
11) mango
12)
13) Much ado about nothing
14) He he he
15) Adios amigo
```

## Multiple Inputs
Both `head` and `tail` can also process multiple files. The logic will be applied independently and the output will be prefixed with a filename and suffixed with a blank line by default. The `-q` option will remove the filename and blank line.

## Byte Selection
The `-c` option works much like the `-n` option, but on the byte granularity. It is likely not a good decision to use this option with multibyte characters.

## Range Selection
A range of lines can be selected by using `head` and `tail` in conjunction. These two commands can be strung together easily using the pipe (`|`) command.

Source: [Command line text processing with GNU Coreutils](https://learnbyexample.github.io/cli_text_processing_coreutils/head-tail.html)