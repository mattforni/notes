# `join`
`join` is a command-line tool for combining two input files based on a common field. It has various options that allow for great flexibility in how the input is joined together. `join` works best when the input is *already sorted*.

## Default `join`
By default, `join` combines input files based on the first field of input files, referred to as the **key**. Only lines with common keys will be included in the output, with the key output as the first field.

`join` will ignore all whitespace preceding the first character in an input line, and will consider any whitespace thereafter as a field separator, unless otherwise specified.

```bash
$ join <(printf "brad\t34\nclare\t32\neric\t28\nmatt\t33") <(printf "brad\tpasadena\nclare\thobart\neric\tsanta-monica\nmatt\thobart")
brad 34 pasadena
clare 32 hobart
eric 28 santa-monica
matt 33 hobart
```

By default, if a key is present multiple times in the same input files, all possible combinations will be present in the output.

```bash
$ join <(printf "brad\t33\nbrad\t34\nclare\t32\neric\t28\nmatt\t33") <(printf "brad\tpasadena\nclare\thobart\neric\tsanta-monica\nmatt\thobart\nmatt\tthousand-oaks")
brad 33 pasadena
brad 34 pasadena
clare 32 hobart
eric 28 santa-monica
matt 33 hobart
matt 33 thousand-oaks
```

## Unmatched Lines
By default, the output only includes lines with keys in both input files. The `-a` option can be provided to also include unmatched lines from the input files. The values `1` and `2` can be provided to indicate that unmatched lines from the first and second input files should be included, respectively.

```bash
# Only include matched lines
$ join <(printf "alex\t34\nclare\t32\neric\t28\nmatt\t33") <(printf "brad\tpasadena\nclare\thobart\neric\tsanta-monica\nmatt\thobart\nzach\tminneapolis")        
clare 32 hobart
eric 28 santa-monica
matt 33 hobart

# Include unmatched lines from the first file
$ join -a1 <(printf "alex\t34\nclare\t32\neric\t28\nmatt\t33") <(printf "brad\tpasadena\nclare\thobart\neric\tsanta-monica\nmatt\thobart\nzach\tminneapolis")
alex 34
clare 32 hobart
eric 28 santa-monica
matt 33 hobart

# Include unmatched lines from the second file
$ join -a2 <(printf "alex\t34\nclare\t32\neric\t28\nmatt\t33") <(printf "brad\tpasadena\nclare\thobart\neric\tsanta-monica\nmatt\thobart\nzach\tminneapolis")
brad pasadena
clare 32 hobart
eric 28 santa-monica
matt 33 hobart
zach minneapolis

# Include unmatched lines from both files
$ join -a1 -a2 <(printf "alex\t34\nclare\t32\neric\t28\nmatt\t33") <(printf "brad\tpasadena\nclare\thobart\neric\tsanta-monica\nmatt\thobart\nzach\tminneapolis")
alex 34
brad pasadena
clare 32 hobart
eric 28 santa-monica
matt 33 hobart
zach minneapolis
```



The `-v` option can be provided to **only** include unmatched lines in the output. Again, the values `1` and `2` can be provided to indicate that unmatched lines from the first and second files should be included, respectively.



```bash
# Only include unmatched files from the first file
$ join -v1 <(printf "alex\t34\nclare\t32\neric\t28\nmatt\t33") <(printf "brad\tpasadena\nclare\thobart\neric\tsanta-monica\nmatt\thobart\nzach\tminneapolis")
alex 34

# Only include unmatched files from the second file
$ join -v2 <(printf "alex\t34\nclare\t32\neric\t28\nmatt\t33") <(printf "brad\tpasadena\nclare\thobart\neric\tsanta-monica\nmatt\thobart\nzach\tminneapolis")
brad pasadena
zach minneapolis
```

## Files with Headers
The `--header` option can be provided to indicate that the first line in both input files should be ignored from the algorithm and then appended in the output.

```bash
$ join --header <(printf "Name,Age\nalex\t34\nclare\t32\neric\t28\nmatt\t33") <(printf "Name,Location\nbrad\tpasadena\nclare\thobart\neric\tsanta-monica\nmatt\thobart\nzach\tminneapolis")
Name,Age
clare 32 hobart
eric 28 santa-monica
matt 33 hobart
```

## Key Field
By default, the first field of both input files is used as the key. The `-1` and `-2` options can be provided to indicate that a different field should be used for the key for the first and second input files, respectively. The `-j` option can be provided if both files use the same field as the key.

```bash
$ join -1 2 <(printf "34\talex\n32\tclare\n28\teric\n33\tmatt") <(printf "brad\tpasadena\nclare\thobart\neric\tsanta-monica\nmatt\thobart\nzach\tminneapolis")
clare 32 hobart
eric 28 santa-monica
matt 33 hobart
```

## Customizing Output
The `-o` option can be provided to customize which fields are included in the output. The `-o` option takes a comma-separated list of file and field pairs, specified as `file.field`.

```bash
$ join <(printf "alex\t34\nclare\t32\neric\t28\nmatt\t33") <(printf "brad\tpasadena\tca\tusa\nclare\thobart\tas\taus\neric\tsanta-monica\tca\tusa\nmatt\thobart\ttas\taus\nzach\tminneapolis\tmn\tusa")
clare 32 hobart as aus
eric 28 santa-monica ca usa
matt 33 hobart tas aus

# Include field 1 and 2 from the first file and 3 and 4 from the second in the output
$ join -o 1.1,1.2,2.3,2.4 <(printf "alex\t34\nclare\t32\neric\t28\nmatt\t33") <(printf "brad\tpasadena\tca\tusa\nclare\thobart\tas\taus\neric\tsanta-monica\tca\tusa\nmatt\thobart\ttas\taus\nzach\tminneapolis\tmn\tusa")
clare 32 tas aus
eric 28 ca usa
matt 33 tas aus
```

The `-o` option can also take the value `auto` to automatically determine how many fields to include in the output. When the `-o auto` option is provided, the `-e` option can be also be provided to indicate the string to be used in the absence of matching fields.

```bash
$ join -a1 -a2 -o auto -e '-' <(printf "alex\t34\nclare\t32\neric\t28\nmatt\t33") <(printf "brad\tpasadena\tca\tusa\nclare\thobart\ttas\taus\neric\tsanta-monica\tca\tusa\nmatt\thobart\ttas\taus\nzach\tminneapolis\tmn\tusa")
alex 34 - - -
brad - pasadena ca usa
clare 32 hobart tas aus
eric 28 santa-monica ca usa
matt 33 hobart tas aus
zach - minneapolis mn usa
```
## Set Operations
```bash
# Union of the two sets
$ join -a1 -a2 <(printf "alex\nchas\nclare\neric\nfoody\nmatt") <(printf "anna\nbrad\nclaire\nclare\neric\nmatt\nross\nzach")
alex
anna
brad
chas
claire
clare
eric
foody
matt
ross
zach

# Symmetric difference (elements in set 1 and set 2 but not the union)
$ join -v1 -v2 <(printf "alex\nchas\nclare\neric\nfoody\nmatt") <(printf "anna\nbrad\nclaire\nclare\neric\nmatt\nross\nzach")
alex
anna
brad
chas
claire
foody
ross
zach

# Intersection
forni@waldenpond Notes % join <(printf "alex\nchas\nclare\neric\nfoody\nmatt") <(printf "anna\nbrad\nclaire\nclare\neric\nmatt\nross\nzach") 
clare
eric
matt

# Difference of set 1
$ join -v2 <(printf "alex\nchas\nclare\neric\nfoody\nmatt") <(printf "anna\nbrad\nclaire\nclare\neric\nmatt\nross\nzach")
anna
brad
claire
ross
zach

# Difference of set 2
% join -v1 <(printf "alex\nchas\nclare\neric\nfoody\nmatt") <(printf "anna\nbrad\nclaire\nclare\neric\nmatt\nross\nzach")
alex
chas
foody
```
