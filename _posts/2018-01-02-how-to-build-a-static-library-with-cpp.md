---
layout: post
title: How To Build a Static Library With C++
---

## Building a Static Library

Organizing code in function libraries is a good practice that can help to avoid code duplication by increasing code reuse.

Suppose to have a header file `my_stuff.h` with all declarations to be exposed as the library API, for example:

```
int my_suff_foo(int, int);

```
then we have the source code for `my_stuff_foo` function in `my_stuff.cc`:

```
int my_stuff_foo(int a, int b) {
    int result;
    
    result = a + b;
    
    return result;
}
```

To build the library we need the following Makefile, which:

1. Compiles `my_stuff.cc` into `my_stuff.o`

```
$(CXX) $(CXXFLAGS) -c my_stuff.cc
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
INCLUDES=-I./includes
CXXFLAGS+=$(INCLUDES)

all: my_stuff_lib

my_stuff.o:
	$(CXX) $(CXXFLAGS) -c my_stuff.cc

my_stuff_lib: my_stuff.o
	ar rcs libmystuff.a my_stuff.o

clean:
	rm -f *.o
	rm -f *.a
```
	
## Linking The Library

We now have our `libmystuff.a` file, which is basically a collection of all **object** files compiled into a single file.

To use the functions we packed into `libmystuff.a` in our next project we have to **link** it. To link our library we need to
tell the compiler its name and where it is located.

To do this we use two Makefile variables:

1. `LDLIBS`, which contains all libraries names we need to link.
2. `LIBS`, which contains all non standard search paths to look for to find the libraries we need.

Note we also need to tell the compiler where it's located the header file which contains all `libmystuff.a` APIs.

Here's the source of the Makefile:

```
CXX=g++
CXXFLAGS=-g -std=c++11 -Wall -pedantic
INCLUDES=-I. -I<path/to/my_stuff.h>
CXXFLAGS+=$(INCLUDES)
LDLIBS=-lmystuff
LIBS=-L<path/to/libmystuff.a>

all: 

my_cool_project: my_cool_project.o
	$(CXX)							\
	$(CXXFLAGS)						\
	my_cool_project.cc					\
	-o							\
	my_cool_project						\
	$(LDLIBS)						\
	$(LIBS)

clean:
	rm -f *.o
	rm -f my_cool_project
```
