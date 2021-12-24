# `cat` and `tac`
`cat` is a command-line tool with many uses that derives its name from con**cat**enate.

`tac` operates much like `cat`, but displays input lines in reverse order (`cat` -> `tac`).

## Creating Text Files
One of the most common uses of `cat` is writing contents to a file by typing them directly in the terminal. If `cat` 
is invoked without a file argument or a stream from `stdin` the terminal will prompt the user for input before 
proceeding. In the example below the output of `cat` will be written to `greeting.txt`.

```bash
# press Enter key and Ctrl+d after typing all the required characters
$ cat > greeting.txt
Hi there
Have a nice day
```
[**Here Documents**](https://www.gnu.org/software/bash/manual/bash.html#Here-Documents) are another popular way to 
create text files directly from the terminal. This type of redirection reads from the shell until a specific word is 
read. All of the lines up to that point are treated as `stdin`. Here is an example:

```bash
$ cat << 'EOF' > cars.txt
> Audi A4
> Subaru Crosstrek
> Mazda Miata
> EOF

$ cat cars.txt
Audi A4
Subaru Crosstrek
Mazda Miata
```
 
## Concatenate Files
Of course, the main utility of the `cat` command is concatenating files. One or more files can be passed to `cat` as 
arguments and `cat` will concatenate the contents of those files and print them to `stdout`. The output can be saved 
by using redirection (`>`) in the shell. Here is an example:

```bash
$ cat greeting.txt fruits.txt nums.txt > op.txt

$ cat op.txt
Hi there
Have a nice day
banana
papaya
mango
3.14
42
1000
```

`cat` can also be instructed to use `stdin` by passing the `-` as a file argument. Where this is no file argument 
the `-` is implied and does explicitly need to be passed as an argument.

```bash
# only stdin (- is optional in this case)
$ echo 'apple banana cherry' | cat
apple banana cherry

# both stdin and file arguments
$ echo 'apple banana cherry' | cat greeting.txt -
Hi there
Have a nice day
apple banana cherry
```

## Options
`cat` has many options that allow for future manipulation of text.

The `-s` option will "squeeze" consecutive empty 
lines into a **single** empty line.

The `-n` option will prefix each line with a tab and a line number. This 
includes blank lines, but the `-b` option will only number non-blank lines.

The `-v` option is particularly useful 
in that shows all special characters, revealing not only backspaces and carriage returns, but also the `NUL` 
character. Each of these characters will be displayed using [caret notation](https://en.wikipedia.org/wiki/ASCII#Control_code_chart).

The `-T` option reveals tab characters and the `-E` option places a `$` at the end of each line, which is very 
useful for visualizing trailing whitespace.

Then there are options that combine the above options:
- The `-e` option is equivalent to `-vE`
- The `-t` option is equivalent to `-vT`
- The `-A` option is equivalent to `-vET`

## Useless Use of `cat`
Keep in mind that `cat` is not always necessary when piping content to other commands. Many commands accept file 
parameters, and even for those that do not, `cat` can often be supplanted by using shell redirection (`<`). For example:

```bash
# useless use of cat
$ cat greeting.txt | tr 'a-z' 'A-Z'
HI THERE
HAVE A NICE DAY

# use shell redirection instead
$ <greeting.txt tr 'a-z' 'A-Z'
HI THERE
HAVE A NICE DAY
```

There may not be immediately perceptible impacts, but over extremely large inputs, using `cat` unnecessarily can 
cause performance degredation.

## `tac`
As mentioned above `tac` displays input lines in reverse order. When using `tac` to concatenate multiple files, each 
file will be reversed individually and then concatenated. Reversing input lines is often desirable when the text 
needs to be further processed since many other text processing utilities (e.g. `grep` and `sed`) readily support 
matching the **first** 
occurrence of a pattern but do not have any means of matching the **last**.

By default `tac` uses the newline character (`\n`) to separate input into lines, however this can be configured 
using the `-s` option. Here is an example:

```bash
$ printf 'apple banana cherry' | tac -s ' ' | cat -e
cherrybanana apple $

$ printf 'apple banana cherry ' | tac -s ' ' | cat -e
cherry banana apple $ 
```

The separator can be treated as a regular expression if the `-r` option is also provided. Here is an example:

