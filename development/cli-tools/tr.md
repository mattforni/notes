# `tr`
`tr` is a command-line tool for mapping one set of characters to another set of characters. `tr` supports ranges, repeats, character sets, etc. making it an incredibly powerful text 
processing tool.

## Translation
The most straight forward use of `tr` is to translate text by mapping one set of characters to another. Below is an 
example:

```bash
$ echo 'leet speak' | tr 'lets' '1375'
1337 5p3ak
```

**Note** It is a best practice to enclose arguments in single quotes (`'`) to avoid any issues with shell 
metacharacters.

A hyphen (`-`) can be used between two characters to create a range (ascending order only) between those characters. 
Some examples are below:

```bash
# uppercase to lowercase
$ echo 'HELLO WORLD' | tr 'A-Z' 'a-z'
hello world

# swap case
$ echo 'Hello World' | tr 'a-zA-Z' 'A-Za-z'
hELLO wORLD

# rot13
$ echo 'Hello World' | tr 'a-zA-Z' 'n-za-mN-ZA-M'
Uryyb Jbeyq
```

**Note** The second argument to the rot13 substitution has multiple ranges since ranges can only be ascending order. 
In this case `a-z` will map to `n-z` and then `a-m` and the same for the capital letter ranges.

`tr` only works on stdin data, so use shell input redirection for file input.

```bash
$ tr 'a-z' 'A-Z' <greeting.txt
HI THERE
HAVE A NICE DAY
```

## Sets of Differing Lengths
If the second set is longer, the remained of the second set is simply ignored. However, if the first set is longer 
the last character of the second set is used for *all* missing mappings.

```bash
# only abc gets converted to uppercase
$ echo 'apple banana cherry' | tr 'abc' 'A-Z'
Apple BAnAnA Cherry

# c-z will be converted to C
$ echo 'apple banana cherry' | tr 'a-z' 'ABC'
ACCCC BACACA CCCCCC
```

The `-t` option can be provided to truncate the first set to match the length of the second set.

```bash
# d-z won't be converted
$ echo 'apple banana cherry' | tr -t 'a-z' 'ABC'
Apple BAnAnA Cherry
```

Arguments can also include portions of the form `[c*n]` which instructs `tr` to repeat the character `c` a total of 
`n` times. If the `n` is omitted the character `c` is repeated as many times as needed to make the sets equal in 
length.

```bash
# a-e will be translated to A
# f-z will be uppercased
$ echo 'apple banana cherry' | tr 'a-z' '[A*5]F-Z'
APPLA AANANA AHARRY

# a-c and x-z will be uppercased
# rest of the characters will be translated to -
$ echo 'apple banana cherry' | tr 'a-z' 'ABC[-*]XYZ'
A---- BA-A-A C----Y
```

## Escape Sequences and Character Sets
Characters like `newline` and `tab` can be represented using escape sequences or their octal representations (e.g. 
`\NNN`). Certain commonly used ranges like alphabets, digits, punctuations, etc. have *named* character sets, though 
only `[:lower:]` and `[:upper:]` can be used by default. The rest require providing the `-d` or `-s` options. The [tr 
manual page](https://www.gnu.org/software/coreutils/manual/html_node/Character-sets.html) has a full list of named 
ranges.

```bash
# same as: tr 'a-z' 'A-Z' <greeting.txt
$ tr '[:lower:]' '[:upper:]' <greeting.txt
HI THERE
HAVE A NICE DAY
```

## Deleting Characters
The `-d` option can be provided to specify a set of characters to be deleted.

## Complement
The `-c` option can be provided to invert the first set of characters. This option is often used in conjunction with 
the `-d` option to delete any characters not included in the specified set.

```bash
$ s='"Hi", there! How *are* you? All fine here.'
# retain alphabets, whitespaces, period, exclamation and question mark
$ echo "$s" | tr -cd '[:alpha:][:space:].!?'
Hi there! How are you? All fine here.
```

When using `-c` for translation, only a single character can be provided for the second set. This means that all the 
characters not provided by the first set will be mapped to the character specified in the second set.

## Squeeze
The `-s` option can be provided to collapse all repeated characters to a single copy of that character.

```bash
# squeeze lowercase alphabets
$ echo 'hhoowwww aaaaaareeeeee yyouuuu!!' | tr -s 'a-z'
how are you!!

# translate and squeeze
$ echo 'hhoowwww aaaaaareeeeee yyouuuu!!' | tr -s 'a-z' 'A-Z'
HOW ARE YOU!!

# delete and squeeze
$ echo 'hhoowwww aaaaaareeeeee yyouuuu!!' | tr -sd '!' 'a-z'
how are you

# squeeze other than lowercase alphabets
$ echo 'how    are     you!!!!!' | tr -cs 'a-z'
how are you!
```

Source: [Command line text processing with GNU Coreutils](https://learnbyexample.github.io/cli_text_processing_coreutils/tr.html)