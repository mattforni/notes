# `fold` and `fmt`
## `fold`
`fold` is a command-line tool use to wrap lines over a certain number of bytes. By default, it wraps lines at `80` bytes, but the number of bytes to include in a line can be provided via the `-w` option.

```bash
$ printf "Hey there, Matt\!\nHow are you today?" | fold -w 10
Hey there,
 Matt!
How are yo
u today?
```

The `-s` option instructs `fold` to attempt to split lines on whitespace characters instead of mid-word. It will still split words if there are no preceding whitespace characters.

```bash
$ printf "Hey there, Matt\!\nHow are you today?" | fold -s -w 5 
Hey 
there
, 
Matt!
How 
are 
you 
today
?

$ printf "Hey there, Matt\!\nHow are you today?" | fold -s -w 4
Hey 
ther
e, 
Matt
!
How 
are 
you 
toda
y?
```

## `fmt`
`fmt` is a command-line tool that behaves much like `fold`, but makes slightly more intelligent decisions on where to split lines based on sentences, paragraphs, and other details. By default, `fmt` utilizes `93%` of `75` columns.

The width is specified via the `-w` option and the percentage of columns is specified via the `-g` option.

`fmt` will also never split words, even if they exceed the maximum width and will combine lines if the content does not exceed the maximum width, though it does respect paragraph breaks.

```bash
$ printf "Hey there, Matt\!\nHow are you today?\nDoing well I see." | fmt -w 15
Hey there,
Matt!  How are
you today?
Doing well I
see.

$ printf "Hey there, Matt\!\nHow are you today?\nDoing well I see." | fmt
Hey there, Matt!  How are you today?  Doing well I see.

$ printf "Hey there, Matt\!\nHow are you today?\n\nDoing well I see." | fmt      
Hey there, Matt!  How are you today?

Doing well I see.
```

The `-u` options instructs `fmt` to collapse multiple spaces to single spaces except spaces following the end of a sentence, which are collapsed to two spaces.

```bash
printf "Hey       there, Matt\!    How are  you today?" | fmt -u
Hey there, Matt!  How are you today?
```