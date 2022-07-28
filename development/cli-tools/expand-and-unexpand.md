# `expand` and `unexpand`
`expand` and `unexpand` are command-line tools used to convert tabs to spaces and vice versa, respectively. Both have various options to customize how the conversion is computed. By default, both commands use a tab stop of `8`.

## `expand`
The `expand` command is used to convert tabs to spaces and by default it inserts spaces to align text at multiples of `8` columns.

```bash
printf 'mat\tmate\tmaterial\na\tab\tabc' | cat -t          
mat^Imate^Imaterial
a^Iab^Iabc

# 'mat' is 3 bytes so \t expands 5 spaces
# 'mate' is 4 bytes so \t expands to 4 spaces
# 'material' is 8 bytes so \t does not expand
$ printf 'mat\tmate\tmaterial\na\tab\tabc' | expand
mat     mate    material
a       ab      abc
```

**Note** `expand` also considers backspace characters to determine the number of spaces needed.

The `-i` option can be provided to only convert leading tab characters to spaces instead of all tab characters. In this case the first occurrence of a non-whitespace character will terminate the conversion.

```bash
printf 'mat\tmate\tmaterial' | gexpand -i | cat -t 
mat^Imate^Imaterial%

$ printf '\tmat\tmate\tmaterial' | gexpand -i | cat -t
        mat^Imate^

$ printf '\t \tmat\tmate\tmaterial' | gexpand -i | cat -t 
                mat^Imate^Imaterial
```

The `-t` option can be provided to customize the tab stop width. This option has various features and syntax for specifying tab stop width. The most basic usage is to specify an alternate tab stop width `N`.

```bash
$ printf 'mat\tmate\tmaterial' | expand -t 4 | cat -t
mat mate    material
```

Multiple width can be specified separated by a comma each of which referring to absolute width from the start of the line. Any tab characters following the last tab stop width will be expanded to a single space. If the tab stop widths are not strictly increasing an error will be emitted.

```bash
$ printf 'mat\tmate\tmaterial' | expand -t 10,18
mat       mate    material

$ printf 'mat\tmate\tmaterial\tadditional' | expand -t 8,12 | cat -t
mat     mate material additional

$ printf 'mat\tmate\tmaterial' | expand -t 4,8,8
expand: bad tab stop spec
```

Prefixing the last width with a `/` character will cause `expand` to continue to expand tabs using multiples of the last width. Prefixing the last width with a `+` character will cause `expand` to continue to expand tabs using multiples of the last width offset by the second to last width.

```bash
# Expand to 4, then multiples of 7 (i.e. 7, 14, 21, etc.)
printf 'mat\tcat\ttat\tbat\tfat\tsat\that\tlat' | gexpand -t 4,/7
mat cat       tat    bat    fat    sat    hat    lat

# Expand to 4, then 4 + multiples of 7 (i.e. 11, 18, 25, etc.)
printf 'mat\tcat\ttat\tbat\tfat\tsat\that\tlat' | gexpand -t 4,+7
mat cat    tat    bat    fat    sat    hat    lat
```

## `unexpand`
The `unexpand` command is used to convert spaces to tabs and by default it *only* converts *leading* spaces of `8` spaces or more to tabs with the first non-whitespace character terminating conversion. Only two more characters will be considered for conversion.

```bash
print '        mat        cat        bat' | unexpand | cat -t
^Imat        cat        bat
```

The `-a` option can be provided to unexpand *all* occurrences of `spaces at tab boundaries. Keep in mind tab boundaries are determined by the tab stop width and not the number of whitespace characters between words

```bash
print '        mat        cat        bat' | unexpand -a | cat -t
^Imat^I   cat^I      bat

print '        mat     cat    bat' | unexpand -a | cat -t
^Imat^Icat    bat
```

The `-t` option can be provided in the same way and with the same syntax as it is in `expand`.