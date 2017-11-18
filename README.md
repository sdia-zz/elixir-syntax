
# Table of contents

1. Introduction
2. Basic types
3. Basic operators
4. Pattern matching
5. case, cond and if
6. Binaries, strings and char lists
7. Keywords and maps
8. Modules and Functions
9. Recursion
10. Enumerables and streams
11. Processes
12. IO and the file system
13. alias, require and import
14. Module attributes
15. Structs
16. Protocols
17. Comprehension
18. Sigils
19. try, catch and rescue
20. Typespecs and behaviours
21. Erlang libraries
22. Where to go next




# Basic types

## Basics
``` elixir
1            # integer
0x1F         # integer
1.0          # float
true
:atom
"elixir"
[1, 2, 3]    # list
{1, 2, 3}    # tuple

10 / 2       # returns a float
div(10, 2)   # int
rem 10, 3

# function arity matters
> h myfunction/1
> h myfunction/2
```


## Booleans
```elixir
# bool
> true
true

> true == false
false

> is_boolean true
true

> is_boolean 1
false
```


## Atoms

```elixir
# an atom is its own value
> :hello
:hello

> is_atom :hello
true

# aliases start in upper case are also atoms
> is_atom Hello
true

> :hello == :world
false

> true == :true
true

> is_boolean :false
true

```


## Strings

``` elixir
> "hellö"
"hellö"

# interpolation
> "hellö #{:world}"
"hellö world"


# escape
> IO.puts "hello\nworld"

# strings are binaries
> is_binary("hellö")
true

> byte_size "hellö"
6

> String.length "hellö"
5

> String.upcase "hellö"
"HELLÖ"
```


## Anonymous functions

```elixir
> add = fn a, b -> a + b end

> add. 1 2
3

> is_function add
true

> is_function add 2
true

# the & shorthand
> add = &(&1 + &2)

# clause and guards
> f = fn
>   x, y when x > 0  -> x + y
>   x, y -> x * y
> end

```


## (linked) Lists

```elixir
> [1, 2, true, :hello]
[1, 2, true, :hello]


# concatenation
> [1,2,3] ++ [4,5,6]
[1,2,3,4,5,6]

#hd and tl
> list = [1,2,3]
> hd list
1

> tl list
[2,3]


# single quotes VS double-quotes
'hello' == "hello"
false

> [104, 101, 108, 108, 111]
'hello'

> i 'hello'
... data type List ...
```


Lists are stored in memory as linked lists,

* list length computation is linear
* performance of list concatenation depends of left-hand list length


```elixir
> list = [1,2,3]
> [0] ++ list      # traverse only 1 element before append
> list ++ 4        # traverse 4 elements...
```


## Tuples

```elixir
> {1,2, :hello, false}
{1,2, :hello, false}

> tuple_size {:ok, "hello"}
2

> mytuple = {:ok, "hello"}
{:ok, "hello"}

> elem(tuple, 1)
"hello"

> tuple_size tuple
2


# immutability
> put_elem tuple 1 "world"
{:ok, "world"}

> tuple
{:ok, 'hello'}
```

Tuples are stored contiguously in memory:

* getting tuple size is fast
* accessing element by index is fast
* update or adding new element requires new tuple, it's an expensive operation


Elixir guidance

* there is `elem\2` for tuples but not for lists
* `size` means constant time
* `length` means linear time


# Basic operators

```elixir
+, -, *, /, div/2, rem/2   # arithmetics

++, --  # list concatenation, dissociation

<>      # string concatenation

and, or, not  # only booleans

||, &&, !     # boolean and other (all values except false and nil evaluate to true)

== vs ===, the latter is more strict when comparing integers and float
```


# Pattern matching

`=` is actually called _the match operator_

```elixir
> x = 1
1

> x
1

> 1 = x
1

> 2 = x
... match error ...

> {a,b,c} = {:hello, "world", 42}
{:hello, "world", 42}

> a
:hello

> b
"world"

> {:ok, result} = {:ok, 13}
> result
13

> [a,b,c] = [1,2,3]
> [head|tail] = [1,2,3]

> list = [1,2,3]
> [0|list]
[0,1,2,3]

# pin operator
> x = 1
> x = 2
> x
2
> ^x = 1
... match error...
```


# case, cond and if

## case

```elixir
> case {1,2,3} do
>      {1,2,3} -> "won't match"
>      {1,x,3} -> "match and bind x to 2"
>            _ -> "match any value"
> end    
"match and bind x to 2"


> case 10 do
>      ^x -> "match if x is 10, otherwise no bound"
>       _ -> "will match"
> end
"will match"


# guard
> case {1,2,3} do
>      {1,x,3} when x>0 -> "will match"
>                     _ -> "would match if guard was not satisfied"
> end
"will match"


# error in guard wont bubble
> case 1 do
>      x when raise_error() -> "won't match"
>      x -> "Got #{x}"
> end     


# CaseClauseError
> case :ok do
>      :error -> "won't match"
> end     
... CaseClauseError ...
```


## cond

`cond` check different conditions and find the first one that evaluates to `true`.

```elixir

> cond do
>     2 + 2 == 5 -> "this will not be true"
>     2 * 2 == 3 -> "nor this"
>     1 + 1 == 2 -> "this will"
> end    
"this will"


# CondClauseError
> cond do
>     false -> "won't be true"
> end    
... CondClauseError ...


# catch all clause
> cond do
>     2 + 2 == 5 -> "this will not be true"
>     2 * 2 == 3 -> "nor this"
>     true -> "this is always true (equivalent to else)"
> end    
"this is always true (equivalent to else)"


# any value besides nil and false are true
> cond do
>     hd [1,2,3] -> "1 is considered true"
> end    
"1 is considered true"
```


## if and unless

```elixir
> if true do
>     "this works"
> end    
"this works"


> unless true do
>     "this will never happen"
> end    


> if nil do
>     "this won't happen"
> else
>     "this will"
> end    
"this will"
```

## do / end blocks

`if/2` and `unless\2` are macros...

```elixir
# passing arguments using keywords list
> if true, do: 1 + 2
3

> if false, do: :this, else: :that
:that

> if true, do: (
>     a = 1 + 2
>     a + 10
> )
13
```


# Binaries, strings and char lists

## UTF-8 and Unicode

```elixir
> string = "hełło"
"hełło"
> byte_size(string)
7
> String.length string
5

# get character code point
> ?a
97        # 1 byte
> ?ł
322       # 2 bytes


# split by codepoints
> String.codepoints "hełło"
["h", "e", "ł", "ł", "o"]
```

## Binaries (bitstrings)

a binary is defined using `<<>>`
```elixir
> <<0, 1, 2, 3>>
<<0, 1, 2, 3>>

> byte_size <<0, 1, 2, 3>>
4

> String.valid?(<<239,191,19>>)
false

# string concatenation is actually a binary operation
> <<0,1>> <> <<2,3>>
<<0,1,2,3>>

# common trick...
> "hełło" <> <<0>>
<<104, 101, 197, 130, 197, 130, 111, 0>>


# each number given to a binary is meant to represent a byte
> <<255>>
<<255>>

> <<256>>
<<0>>      # truncated to byte

> <<256 :: size(16)>> # use 16 bits (2 bytes) to store the number
<<1,0>>

> <<256 :: utf8 >>   # the number is a code code point
"Ā"

> <<256 :: utf8, 0>>
<<196, 128, 0>>
```

stop page 51, to be continued
