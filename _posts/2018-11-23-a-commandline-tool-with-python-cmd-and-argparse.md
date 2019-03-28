---
layout: post
title: A Commandline Tool With Python Cmd And Argparse
---

## The problem
Develop a commandline tool with several commands and input parameters for each command.

## The solution
With Python we can use Cmd module to quickly setup a commandline utility and Argparse module to hande input parameters with related 
help messages and errors.

```python

#!/usr/bin/env python3


import argparse
import ast
from cmd import Cmd
import os
try:
    import readline
except ImportError:
    readline = None
import sys


class NameValuePairType(object):


    def __init__(self):
        pass


    def __call__(self, value):
        pair = value.split("=")
        if not (2 == len(pair)):
            raise argparse.ArgumentTypeError("invalid name=value pair")
        try:
            name = pair[0]
            value = ast.literal_eval(pair[1])
        except Exception as e:
            raise argparse.ArgumentTypeError("invalid value: " + pair[1])

        return [name, value]


class CommandLineTool(Cmd):

    _historyFile = os.path.expanduser('~/.cmdtool_history')
    _historySize = 10000

    intro = 'Welcome to the CommandLineTool shell. Type help or ? to list commands.\n'
    prompt = '(CLI) '


    def __init__(self, completekey='tab', stdin=None, stdout=None):
        Cmd.__init__(self, completekey=completekey, stdin=stdin, stdout=stdout)
        self._defaults = self.CommandLineToolDefaults()


    def preloop(self):
        self._loadHistory()


    def postloop(self):
        self._storeHistory()


    def _loadHistory(self):
        if readline and os.path.exists(self._historyFile):
            readline.read_history_file(self._historyFile)


    def _storeHistory(self):
        if readline:
            readline.set_history_length(self._historySize)
            readline.write_history_file(self._historyFile)


    ############################################################################
    # Exported Commands                                                        #
    ############################################################################


    def do_foo(self, args):
        """
        A useful command: foo
        """
        parser = argparse.ArgumentParser(prog="foo")
        parser.add_argument('--parameter-a',
            dest='parameterA',
            default=self._defaults.PARAMETER_A,
            help="Parameter A Help"
        )
        parser.add_argument('--parameter-b',
            type=int,
            dest='parameterB',
            default=self._defaults.PARAMETER_B,
            help="Parameter B Help"
        )
        parser.add_argument('--parameter-pair',
            required=True,
            dest='parametersPairs',
            type=NameValuePairType(),
            action='append',
            help='the parameters list, in the form of name=value pairs, if value is a string it needs to be enclosed in double quotes and watchout for spaces'
        )
        try:
            arguments = self._getCommandArguments(parser, args)

            print(arguments.parameterA)
            print(arguments.parameterB)
            print(arguments.parametersPairs)

            return 0
        except Exception as e:
            # cannot avoid doing this if we do not want to halt the interpreter
            # when in loop() mode
            return None


    def do_quit(self, args):
        """
        Quits the program.
        """
        print("bye!")
        raise SystemExit


    ############################################################################
    # Inner class to hold default values and constants                         #
    ############################################################################
    class CommandLineToolDefaults(object):
        PARAMETER_A = "aDefaultValue"
        PARAMETER_B = 42


    ############################################################################
    # Internal commodity Routines                                              #
    ############################################################################
    def _getCommandArguments(self, parser, args):
        if args:
            argsList = args.split(" ")
        else:
            argsList = None
        try:
            arguments = parser.parse_args(argsList)
            return arguments
        except SystemExit:
            pass
        raise Exception


def main():
    prompt = CommandLineTool()
    if len(sys.argv) > 1:
        rc = prompt.onecmd(' '.join(sys.argv[1:]))
        if rc == 0:
            sys.exit(0)
        else:
            sys.exit(1)
    else:
        try:
            prompt.cmdloop()
        finally:
            prompt._storeHistory()


if __name__ == '__main__':
    main()

```
