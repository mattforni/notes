# `sort`
`sort` is a command-line tool with a robust set of uses, though the basic usage is to sort various types of input.

## Lexicographic Sorts
At its core `sort` is a tool that sorts input into lexicographical or numerical order. By default, `sort` orders the input into ascending order.

```bash
$ printf "cat\ngnat\nmat\nbat\ntat" | sort
bat
cat
gnat
mat
tat
```

The `-f` option can be provided to ignore casing.

```bash
$ printf "cat\nGnat\nmat\nBat\ntat" | sort -f
Bat
cat
Gnat
mat
tat
```

The `-d` option can be provided to *only* consider alphabetical, numerical, and spacing characters in sorting.

```bash
$ printf "(cat)\ngnat\n(mat)\n[bat]\ntat" | sort -d
[bat]
(cat)
gnat
(mat)
tat
```

The `-i` option can be provided to further expand the set of characters considered in sorting to only printable characters.

The `-r` option can be provided to reverse the output order. Note that this does not change how the comparisons are performed, it merely outputs the results in reversed order.

```bash
$ printf "cat\ngnat\nmat\nbat\ntat" | sort -r    
tat
mat
gnat
cat
bat
```

## Numerical Sorts
Providing the `-n` option to `sort` will handle most numerical sorts.

```bash
# lexicographic sorts do not work for numbers
$ printf "2\n20\n12\n123" | sort
12
123
2
20

# -n works for sorting these though
$ printf "2\n20\n12\n123" | sort -n
2
12
20
123
```

By default, the `-n` option automatically handles negative and floating-point numbers. Note that parsing the decimal and thousands separators will depend on the `locale` settings.

```bash
$ printf "3.14\n-12.34\n214\n-1234\n45" | sort -n
-1234
-12.34
3.14
45
214
```

The `-g` option can be provided to properly sort numbers that are preceeded with a `+` or are in [E scientific notation](https://en.wikipedia.org/wiki/Scientific_notation#E_notation).

```bash
$ printf "+3.14\n-1.234e-2\n2.14e+2\n-1234\n+45" | sort -g
-1234
-1.234e-2
+3.14
+45
2.14e+2
```

Unless otherwise instructed `sort` will break ties by using the entire line of input, even when the `-n` option is provided and the remainder of the line is alphabetic.

```bash
$ printf "1 cat\n2 gnat\n2 mat\n1 bat\n-1 tat" | sort -n
-1 tat
1 bat
1 cat
2 gnat
2 mat
```

For commands that have human-readable output (e.g. `du` which has the `-h` and `-si` options that display numbers with [SI suffixes](https://en.wikipedia.org/wiki/International_System_of_Units) like `k`, `K`, `M`, `G`, etc.) the `-h` option can be provided.

```bash
$ printf "12M\tmed.txt\n3.12K\tsmall.txt\n4.34G\tbig.txt" | sort -h
3.12K	small.txt
12M	med.txt
4.34G	big.txt
```

The `-V` option can be provided for input with a mix of alphabetical and numerical characters.

```bash
$ printf "output2\n1.2.4\noutput1.3\n0.12" | sort -V
0.12
1.2.4
output1.3
output2
```

The `-R` option can be used to display the output in random order.

```bash
$ printf "cat\ngnat\nmat\nbat\ntat" | sort -R           
mat
cat
bat
gnat
tat

$ printf "cat\ngnat\nmat\nbat\ntat" | sort -R
cat
gnat
bat
tat
mat
```

The `-u` option can be used to keep only the first copy of duplicate input lines.

```bash
$ printf "cat\ngnat\nmat\ngnat\ntat" | sort -u
cat
gnat
mat
tat
```

The `-f` option can be included with the `-u` option to ignore casing when determining uniqueness.

```bash
$ printf "cat\ngnat\nmat\nGnat\ntat" | sort -u
Gnat
cat
gnat
mat
tat

$ printf "cat\ngnat\nmat\nGnat\ntat" | sort -fu
cat
gnat
mat
tat
```

## Column Sorting
The `-k` option can be provided to sort on specific columns instead of the entire line of input. The `-k` option accepts input in the form `start,end` (e.g. `-k2,4`).

```bash
# Sorted numerically by the second column
$ printf "cat\t7.1\ngnat\t1.23\nmat\t0\nGnat\t3.14\ntat\t7" | sort -k2,2n
mat	0
gnat	1.23
Gnat	3.14
tat	7
cat	7.1

3 Sorted lexicographically by the first column
$ printf "cat\t7.1\ngnat\t1.23\nmat\t0\nGnat\t3.14\ntat\t7" | sort -k1,1   
Gnat	3.14
cat	7.1
gnat	1.23
mat	0
tat	7
```

Note that the `-k` can be provided several times to create a chain of tie breaks for ordering.

```bash
# Sorted by the first column lexicographically and then the second column in reverse numerical order
$ printf "cat\t7.1\ngnat\t1.23\nmat\t0\ngnat\t0.14\ntat\t7" | sort -k1,1 -k2,2nr
cat	7.1
gnat	1.23
gnat	0.14
mat	0
tat	7
```

The `-k` optional also accepts character positions within a column for advanced sorting where the starting character is separated from the column by a `.` (e.g. `-kstart.char,end`).

```bash
# Sorted by the first column starting with the second char lexicographically and then the second column in reverse numerical order
$ printf "cat\t7.1\ngnat\t1.23\nmat\t0\ngnat\t0.14\ntat\t7" | sort -k1.2,1 -k2,2nr
cat	7.1
tat	7
mat	0
gnat	1.23
gnat	0.14
```

## Other Options
The `--debug` option can be provided to help identify output that does not match expectations.

The `-c` and `-C` options can be provided to inspect input and point out the first out or order element. `-c` will print out the exception whereas the `-C` option will only affect the exit code.

```bash
$ printf "cat\ngnat\nmat\nGnat\ntat" | sort -c                        
sort: -:4: disorder: Gnat
```

The `-o` option can be provided to specify an output file for the results.
