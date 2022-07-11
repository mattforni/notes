# `comm`
`comm` is a command-line tool for identifying common and unique lines between two input files. By default, `comm` outputs a table with three columns in which:
- the first contains lines unique to the first file
- the second contains lines unique to the second file
- the third contains lines common to both files

```bash
$ comm <(printf "cat\nfat\ntat") <(printf "bat\ncat\nfat\nsat")
	bat
		cat
		fat
	sat
tat
```

Note that both files should be sorted using the same lexicographical ordering for `comm` to work so pre-processing is often necessary for the input files.

```bash
$ comm <(printf "tat\nfat\ncat") <(printf "sat\nfat\ncat\nbat")               
        sat
        fat
        cat
        bat
tat
fat
cat
```

## Suppressing Columns
Each of the columns can be suppressed from the output by providing the following options:

- `-1` suppresses lines unique to the first file
- `-2` suppresses lines unique to the second file
- `-3` suppresses lines common to both files

These options can also be combined to suppress more than one column.

```bash
# Suppress the lines common to both files
$ comm -3 <(printf "cat\nfat\ntat") <(printf "bat\ncat\nfat\nsat")
        bat
        sat
tat

# Suppress the lines unique to both files
$ comm -12 <(printf "cat\nfat\ntat") <(printf "bat\ncat\nfat\nsat")
cat
fat

# Suppress the lines unqiue to the first and common to both
$ comm -13 <(printf "cat\nfat\ntat") <(printf "bat\ncat\nfat\nsat")
bat
sat
```

The `--total` option can be provided to include a count of the number of values in each column in the output table.

```bash
$ gcomm --total <(printf "cat\nfat\ntat") <(printf "bat\ncat\nfat\nsat") 
        bat
                cat
                fat
        sat
tat
1       2       2       total
```

## Duplicate Lines
The number of duplicate lines in the third column will be the minimum of the occurrences in the two files, with any additional repeated lines appearing as unique in the respective file.

```bash
$ comm <(printf "cat\ncat\ncat\nfat\ntat") <(printf "bat\ncat\ncat\nfat\nsat") 
        bat
                cat
                cat
cat
                fat
        sat
tat
```
