# `cut`
`cut` is a command-line tool for selecting specific fields and characters from input.

## Selecting fields
By default, `cut` splits input on the tab character. The `-f` option can be used to specify which fields to select in each line.

```bash
$ printf "mat\tcat\that\ntore\tbore\tfore" | cut -f 2
cat
bore
```

The `-f` option also accepts comma-separated values, in which duplicate field numbers will be ignored, and ranges using the `-` character. Range values can emit either the start or end value, but not both.

```bash
# comma-separated values for fields
$ printf "mat\tcat\that\ntore\tbore\tfore" | cut -f 1,3
mat     hat
tore    fore

# range value with both start and end
$ printf "mat\tcat\that\ntore\tbore\tfore" | cut -f 1-2
mat     cat
tore    bore

# range value with end omitted
$ printf "mat\tcat\that\ntore\tbore\tfore" | cut -f 2- 
cat     hat
bore    fore

# range value with start omitted
$ printf "mat\tcat\that\ntore\tbore\tfore" | cut -f -2
mat     cat
tore    bore

# range value with start and end omitted
$ printf "mat\tcat\that\ntore\tbore\tfore" | cut -f -
cut: [-bcf] list: values may not include zero
```

If a line does not have the specified field, a blank line will be output.

```bash
$ printf "mat\tcat\that\ntore\tbore\tfore" | cut -f 4


```

## Delimiters
The `-d` option can be used to change the input delimiter, however, only a single byte character is allowed.

```bash
$ printf "mat,cat,hat\ntore,bore,fore" | cut -d , -f 2
cat
bore

$ printf "mat,cat,hat\ntore,bore,fore" | cut -d ',,' -f 2
cut: bad delimiter
```

By default, the input delimiter is also used at the output delimiter, but that can be changed via the `--output-delimiter` option, which can be set to any string.

## Complements
The `--complement` option allows for selecting the inverse of the specified selection.

```bash
$ printf "mat,cat,hat\ntore,bore,fore" | cut -d , -f 1,3 --complement
cat
bore
```

## Suppression
By default, input lines that do not contain the input delimiter will be included in the output. The `-s` option suppresses the emission of these lines in the output.

```bash
# first line does not have any tabs
$ printf "mat,cat,hat\ntore\tbore\tfore" | cut -f 1,3 -s
tore    fore
```

# Character selection
The `-b` or `-c` option can be used to select specific bytes of characters from input.

```bash
$ printf "mat\tcat\that\ntore\tbore\tfore" | cut -c 3,6,9,12
tah
rbeo

$ printf "mat\tcat\that\ntore\tbore\tfore" | gcut -c 3,6,9,12 --complement
ma      ct      at
toe     or      fre
```