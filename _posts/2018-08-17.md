---
layout: post
title: Passing Arguments To An Interactive Program In a Non-Interactively Way
---

## The problem
Oftentimes happen we have to call an interactive program (ie: a program made to be used from a real user) from a batch script or 
from another non-interactive program.

## The solution
In Linux we have several ways of doing this. Suppose we have the following script:

```
#!/bin/bash
# interactive-script.sh
read val
echo $val
 
read val
echo $val
 
read val
echo $val
```
we can call it in a non-interactively way using one of the following methods:

1. Input Pipe
```
echo "yes
no
maybe" | interactive-script.sh
```

2. Redirect from File
```
interactive-script.sh < answers.txt
```

3. Heredocs
```
interactive-script.sh << ANSWERS
yes
no
maybe
ANSWERS
```

4. Herestring
```
interactive-script.sh <<< $'yes\nno\nmaybe\n'
```
