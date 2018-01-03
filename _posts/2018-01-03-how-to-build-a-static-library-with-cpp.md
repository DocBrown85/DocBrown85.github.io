---
layout: post
title: How To Build a Static Library With C++
---

## Building a Static Library

Organizing code in function libraries is a good practice that can help to avoid code duplication by increasing code reuse.

Suppose to have a header file `my_stuff.hpp` with all declarations to be exposed as the library API, for example:

```
int my_suff_foo(int, int);

```
then we have the source code for `my_stuff_foo` function in `my_stuff.cpp`:

```
int my_stuff_foo(int a, int b) {
    int result;
    
    result = a + b;
    
    return result;
}
```

To build the library we need the following Makefile, which:

1. Compiles `my_stuff.c` into `my_stuff.o`

```
$(CXX) $(CXXFLAGS) -c my_stuff.c
```

2. Invokes the archiver `ar` to produce a static library (named libmystuff.a) out of the object file my_stuff.o.

```
ar rcs libmystuff.a my_stuff.o
```
Note that the library must start with the three letters **lib** and have the suffix **.a**. 

Here's the complete source of the Makefile:

```
CXX=g++
CXXFLAGS=-g -std=c++11 -Wall -pedantic
INCLUDES=-I.
CXXFLAGS+=$(INCLUDES)

all: my_stuff_lib

my_stuff.o:
	$(CXX) $(CXXFLAGS) -c my_stuff.c

my_stuff_lib: my_stuff.o
	ar rcs libmystuff.a my_stuff.o

clean:
	rm -f *.o
	rm -f *.a
```
	
## Linking The Library
