---
layout: post
title: Reading/Writing Serial Port from Bash
---

## The problem
Write and read from a serial port from Linux Bash.

## The solution

1. Initialize serial port
```
stty -F /dev/ttyUSB0 -speed 9600 cs8 -cstopb
```

2. Prepare to Read
```
cat -v < /dev/ttyUSB0 &
```

3. Write (send a command)
```
echo -en '\xAA\x04\xC2' > /dev/ttyUSB0
```
