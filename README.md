
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

```


## (linked) Lists

Lists are stored in memory as linked lists,

* list length computation is linear
* performance of list concatenation depends of left-hand list length


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
