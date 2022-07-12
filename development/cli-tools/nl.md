# `nl`
`nl` is a command-line tool for numbering lines in an input stream. The command accepts options to format the number, the separator, and numbering scheme. By default, `nl` will treat all input as a single body block and will number each non-empty line with a `6` character wide number followed by a tab.

```bash
$ nl <(printf "mat\ncat\nbat\n\ntat\nfat")
     1  mat
     2  cat
     3  bat
        
     4  tat
     5  fat
```

The `-n` option can be provided to customize the numbering format. It accepts three values:

- `rn` - Right justified with whitespace padding *(default)*
- `rz` - Right justified with `0` padding
- `ln` - Left justified with whitspace padding

```bash
$ nl -n rz <(printf "mat\ncat\nbat\n\ntat\nfat")
000001  mat
000002  cat
000003  bat
        
000004  tat
000005  fat%

$ nl -n ln <(printf "mat\ncat\nbat\n\ntat\nfat")
1       mat
2       cat
3       bat
        
4       tat
5       fat
```

The `-w` option can be provided to customize the width of the numbering column. The default is `6` spaces.

```bash
$ nl -n rz -w 3 <(printf "mat\ncat\nbat\n\ntat\nfat")
001     mat
002     cat
003     bat
   
004     tat
005     fat
```

The `-s` option can be provided to customize the string used as the separator between the numbering column and the input.

```bash
$ nl -n rz -w 3 -s ' <-> ' <(printf "mat\ncat\nbat\n\ntat\nfat")
001 <-> mat
002 <-> cat
003 <-> bat
    <-> 
004 <-> tat
005 <-> fat
```

The `-v` option can be used to specify the starting integer for the numbering column. The `-i` option can be used to specify the positive integer used to increment the numbering column.

```bash
$ nl -w 3 -v -10 -i 2 <(printf "mat\ncat\nbat\n\ntat\nfat")
-10     mat
 -8     cat
 -6     bat
   
 -4     tat
 -2     fat
 ```

## Sections
`nl` also supports a rudimentary section recognition using specific two character delimiter. By default, `nl` uses `\:` as the delimiter string to recognize header, body, and footer sections.

Lines preceded by:
- `\:\:\:` - indicate a header
- `\:\:` - indicate a body
- `\:` - indicate a footer

Numbering can be controlled based on section type using the `-h`, `-b`, and `-f` options, respectively, and by default numbering restarts in each new section, with headers and footers not output with line numbers. The three flags accept the following arguments:

- `a` - number all lines, including empty lines
- `t` - number lines except empty ones
- `n` - do not number lines 
- `pBRE` use basic regular expressions (BRE) to filter lines

```bash
$ nl -w 3 <(printf "\:\:\:\nmatt's header\n\:\:\nhis cat\nhis bat\n\:\:\nher tat\nher fat\n\:\ngood foots")
        matt's header
  1     his cat
  2     his bat
  1     her tat
  2     her fat
        good foots
```

The `-p` option can be provided to not restart numbering with each newly encountered section.

```bash
$ nl -w 3 -p <(printf "\:\:\:\nmatt's header\n\:\:\nhis cat\nhis bat\n\:\:\nher tat\nher fat\n\:\ngood foots")
        matt's header
  1     his cat
  2     his bat
  3     her tat
  4     her fat
        good foots
```

The `-d` option can be used to provide another two character string to be used as the delimiter for recognizing header, body, and footer blocks.