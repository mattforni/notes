# `seq`
[`seq`](https://www.gnu.org/software/coreutils/manual/html_node/seq-invocation.html#seq-invocation) is a command-line tool that produces sequences of integers and floating point numbers in bot ascending and descending order. Three numbers are used to generate the sequence: `start`, `step`, and `stop`.

## Arguments

### One Argument
When one argument is provided `seq` interprets the argument as the `stop` argument and the `start` and `step` values default to `1`.

```bash
# start=1, step=1 and stop=3
$ seq 3
1
2
3
```

### Two Arguments
When two arguments are provided `seq` interprets the arguments as the `start` and `stop` arguments and the `step` value continues to default to `1`, if the `stop` value is greater than the `start` value.

```bash
# start=3, step=1 and stop=5
$ seq 3 5
3
4
5
```

Otherwise, the `step` value defaults to `-1`.

```bash
$ seq 3 1
3
2
1
```

### Three Arguments
When three arguments are provided `seq` interprets the arguments as `start`, `step`, and `stop`. Specifying a negative step value will create sequences that descend from the `start` value to the `stop` value.

```bash
$ seq 3 -2 -3
3
1
-1
-3
```

### Errors
`seq` will throw an error if the values provided to the arguments do not make sense. For example, if the `stop` value is larger than the `start` value, but a negative `step` value is provided.

```bash
$ seq 1 -1 3
seq: needs positive increment
```
The same is true when the `start` value is larger than the `stop` value and a positive `step` value is provided.

```bash
$ seq 3 1 1
seq: needs negative decrement
```

## Floating-point Sequences
Since the `start` and `step` arguments both default to `1` at least one of the values must be changed to a floating-point number for `seq` to output floating-point numbers.

```bash
$ seq 0.75 3
0.75
1.75
2.75
```

[E Notation](https://en.wikipedia.org/wiki/Scientific_notation#E_notation) can also be used to pass floating-point numbers to the `seq` command.

```bash
$ seq 1.21e2 1.23e2
121
122
123
```

## Separators
`seq` uses a new line to separate generated numbers by default, but the separator can be customized using the `-s` option. Multiple characters are allowed when encompassed by single quotes (`'...'`) and many systems will allow escaped characters (e.g. `\t`).

```bash
$ seq -s' ->\t' 1.2e2 1.24e2
120 ->	121 ->	122 ->	123 ->	124 ->
```

> Note that the separator will be appended to the last value despite the fact that another number will not be generated.

## Leading Zeros
By default, the output of `seq` will remove leading zeros, even if the values of `start` and/or `stop` include additional leading zeroes. Passing the `-w` option will cause `seq` to output values that are padded to the max length of the `start` and `stop` values.

```bash
$ seq -w 001 100 1001  
0001
0101
0201
0301
0401
0501
0601
0701
0801
0901
1001
```

## `printf` Style Floating-point Formatting
The `-f` option allows for [`printf`](https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html#index-printf) style formatting for floating-point numbers.

```bash
$ seq -f'%.3e' 1.2e2 0.752 1.22e2
1.200e+02
1.208e+02
1.215e+02
```

## Limitations
As [the man page](https://www.gnu.org/software/coreutils/manual/html_node/seq-invocation.html#seq-invocation) for `seq` indicates:

> On most systems, seq can produce whole-number output for values up to at least 2^53. Larger integers are approximated. The details differ depending on your floating-point implementation.

```bash
# example with approximate values
$ seq 100000000000000000000 3 100000000000000000010
100000000000000000000
100000000000000000000
100000000000000000008
100000000000000000008
```

> However, note that when limited to non-negative whole numbers, an increment of 1 and no format-specifying option, seq can print arbitrarily large numbers.

```bash
$ # no approximation for step value of 1
$ seq 100000000000000000000000000000 100000000000000000000000000005
100000000000000000000000000000
100000000000000000000000000001
100000000000000000000000000002
100000000000000000000000000003
100000000000000000000000000004
100000000000000000000000000005
```
