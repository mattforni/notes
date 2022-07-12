# `wc`
`wc` is a command-line tool for counting the number of lines, words, and characters in input streams. By default, the `wc` command counts the number of lines, words, and bytes, in that order.

```bash
$ printf "mat\ncat\nfat\nbat\ntat" | wc
       4       5      19
```

The `-l`, `-w`, and `-c` options can be provided to limit the output to line, word, and byte count respectively.

```bash
Only print out the line count
$ printf "mat\ncat\nfat\nbat\ntat" | wc -l
       4

Only print out the line and word count
$ printf "mat\ncat\nfat\nbat\ntat" | wc -wl
       4       5
```

## Multiple Files
The `wc` command can accept multiple files as input in which case it outputs the counts for each file independently as well as a summary of all included files at the end of the output.

```bash
$ wc *
      19     184    1696 README.md
     127     758    4562 cat-and-tac.md
      80     314    2161 comm.md
      91     402    2536 cut.md
      53     353    2135 dig.md
      68     354    1987 fold-and-fmt.md
      60     417    2339 head-and-tail.md
     175     841    6873 join.md
     110     516    3211 nl.md
      85     362    1982 paste.md
      75     481    2863 pr.md
     144     605    4130 seq.md
      92     293    1859 shuf.md
     222     811    5205 sort.md
     134     748    4488 tr.md
     149     563    3404 uniq.md
      21     134     790 wc.md
    1705    8136   52221 total
```

## Other Options
The `-L` option can be provided to output the longest line in the input stream. If multiple files are provided to `wc` when the `-L` option is present, the longest line is output in the `total` field.

```bash
$ wc -L *
  112 README.md
  134 cat-and-tac.md
  185 comm.md
  205 cut.md
  208 dig.md
  230 fold-and-fmt.md
  243 head-and-tail.md
  303 join.md
  303 nl.md
  307 paste.md
  413 pr.md
  287 seq.md
  272 shuf.md
  257 sort.md
  182 tr.md
  266 uniq.md
  191 wc.md
  413 total
```