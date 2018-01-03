---
layout: post
title: How to Call Make Command in Project Subdirectories
---

Oftentimes, when dealing with complex projects, there are several components we need to build and link to produce the final
product.

When dealing with such projects, it is common to organize components source code into subdirectories of the main project,
for example:

```
project_main_dir
      |
      +------------ subdir-1
      |
      +------------ subdir-2
      |
      +------------ ...
      |
      +------------ subdir-n
```

When building code in such situations we need a way to call subdirectories Makefiles from the project root directory, namely
`project_main_dir`.

We can do that with the following Makefile, which:

1. Declares a list of all subdirectories that needs to be built.
2. Loops over the list to call the `make` command from each subdirectory with the `make -C <subdir>` option, which basically
tells the `make` command to change to the specified directory before executing.

Here's the source code for the `project_main_dir` Makefile:

```
SUBDIRS = subdir-1 subdir-2 subdir-n

all:
	for dir in $(SUBDIRS); do	\
		$(MAKE) -C $$dir;	\
	done

clean:
	for dir in $(SUBDIRS); do	\
		$(MAKE) -C $$dir clean;	\
	done
```
