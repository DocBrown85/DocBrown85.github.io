---
layout: post
title: How To Build a Static Library With C++
---

## Building a Static Library

Organizing code in function libraries is a good practice that can help to avoid code duplication by increasing code reuse.

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
