
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




# Basic operators

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

# bool
> true
true

> true == false
false

> is_boolean true
true

> is_boolean 1
false


# an atom is its own value
> :hello
:hello

> :hello == :world
false

> true == :true
true

> is_boolean :false
true

```
