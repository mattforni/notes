# `csplit`
`csplit` is a command-line tool used to split inputs into output files based on line numbers and regular expressions. By default, `csplit` outputs files with the prefix `xx` and a two character number suffix starting with `00`. 

## Splitting on Line Numbers
`csplit` supports a simple utility to split input into output files based on line numbers much like [`split`](./split.md). To do this, simply provide a numeric argument to `csplit` after the input argument.

```bash
# Split input on the 4th line
$ seq 10 | csplit - 4
6
15
$ head *
==> xx00 <==
1
2
3

==> xx01 <==
4
5
6
7
8
9
10
```

**Note** that by default `csplit` outputs the number of bytes written to each file. The `-q` option can be provided to suppress this output.

## Splitting on Regexp
In order to split input on a regexp, simply provide a regexp as the second argument to `csplit`. Regexp arguments can either be wrapped in `//` or `%%`, which creates different outputs. When wrapped in `//` the first output file will include all lines prior to the match. When wrapped in `%%` all input prior to the match will be discarded.

```bash
# Include the input prior to the regexp match
$ printf 'mat\ncat\ntat\nfat\nsat\nhat' | csplit -q - '/tat/'
$ head *
==> xx00 <==
mat
cat

==> xx01 <==
tat
fat
sat
hat

# Discard the input prior to the regexp match
$ printf 'mat\ncat\ntat\nfat\nsat\nhat' | csplit -q - '%tat%'
$ head *
tat
fat
sat
hat
```

**Note** that an error will be emitted if the regexp is not found in the input.

```bash
$ printf 'mat\ncat\ntat\nfat\nsat\nhat' | csplit -q - '%pat%'
csplit: ‘%pat%’: match not found
```

When providing a regexp argument and *offset* may also be provided, which affects where the matching line and the surrounding lines should be placed in the output files.

When the offset is positive the split will occur after the matching line. When the offset is negative the split will occur before the matching line. If the offset exceeds the number of lines in the input an error will be emitted.

```bash
# When the offset is positive the split occurs after the match
$ printf 'mat\ncat\ntat\nfat\nsat\nhat' | csplit -q - '/tat/2'
$ head *
==> xx00 <==
mat
cat
tat
fat

==> xx01 <==
sat
hat

# When the offset is negative the split occurs before the match
$ printf 'mat\ncat\ntat\nfat\nsat\nhat' | csplit -q - '/tat/-2'
$ head *
==> xx00 <==

==> xx01 <==
mat
cat
tat
fat
sat
hat

# An error is emitted if the offset is out of range
$ printf 'mat\ncat\ntat\nfat\nsat\nhat' | csplit -q - '/tat/-3' 
csplit: ‘/tat/-3’: line number out of range
```

## Repeated Splits
So far each of the examples has only split the input once, which is the default for `csplit`. However, `csplit` can be instructed to preform a split multiple times by providing a final arguments of the form `{N}` where `N` is the number of times to try to perform the split based on the split criteria. Note that `csplit` will emit an error if the requested number of repeats exceeds the number of matches.

```bash
# Split the input on even number lines 3 times 
$ printf 'mat\ncat\ntat\nfat\nsat\nhat' | csplit -q - 2 '{2}'
$ head *
==> xx00 <==
mat

==> xx01 <==
cat
tat

==> xx02 <==
fat
sat

==> xx03 <==
hat

# An error is emitted if the number of repeats exceeds the matches
$ printf 'mat\ncat\ntat\nfat\nsat\nhat' | csplit -q - 2 '{4}'       
csplit: ‘2’: line number out of range on repetition 3
```

When using a regexp as the split criteria the split can be repeated indefinitely by specifying the repeat argument as `{*}`.

```bash
$ printf 'mat\ncat\ntat\nfat\nsat\nhat' | csplit -q - '/.*at/' '{*}'
$ head *
==> xx00 <==

==> xx01 <==
mat

==> xx02 <==
cat

==> xx03 <==
tat

==> xx04 <==
fat

==> xx05 <==
sat

==> xx06 <==
hat
```

By default, `csplit` will delete all output files created when an error is encountered. The `-k` option can be provided to instruct `csplit` to keep the created files, which can be very helpful in debugging.

## Suppression
By default, matches lines are included in the output of `csplit`. The `--suppress-matched` option can be provided to exclude matches from all output.

```bash
$ printf 'mat\ncat\ntat\nfat\nsat\nhat' | csplit -q --suppress-matched - '/.*at/' '{*}'
$ head *
==> xx00 <==

==> xx01 <==

==> xx02 <==

==> xx03 <==

==> xx04 <==

==> xx05 <==

==> xx06 <==
```

This, and other option combinations, can create situations in which empty files are created. The `-z` option can be provided to prevent empty output files from being created.

```bash
$ printf 'mat\ncat\ntat\nfat\nsat\nhat' | csplit -q -z --suppress-matched - '/.*at/' '{*}'
$ lla
total 0
drwxr-xr-x   2 forni  staff   64 Jul 27 05:26 .
drwxr-xr-x  13 forni  staff  416 Jul 27 04:56 ..
```

## Customizing
The `-f` option can be provided to customize the prefix of created filenames.

```bash
$ printf 'mat\ncat\ntat\nfat\nsat\nhat' | csplit -q -z -f 'matches_' - 2 '{1}'
$ head *
==> matches_00 <==
mat

==> matches_01 <==
cat
tat

==> matches_02 <==
fat
sat
hat
```

The `-n` option can be provided to customize the length of the numeric suffix. The `-b` option can be provided to customize the suffix using a `printf`-style formatted string.