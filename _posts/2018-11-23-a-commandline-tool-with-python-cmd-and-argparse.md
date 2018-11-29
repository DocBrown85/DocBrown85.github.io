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

from cmd import Cmd
import argparse


class NameValuePairValidator(argparse._AppendAction):
    def __call__(self, parser, namespace, values, option_string=None):
        if not (2 == len(values)):
            raise argparse.ArgumentError(self,
                "%s takes 2 values in the form of name:value pair, %d given" % (option_string, len(values))
            )
        super(ParameterNameValuePairValidator, self).__call__(parser, namespace, values, option_string)


class CommandLineTool(Cmd):


    intro = 'Welcome to the CommandLineTool shell. Type help or ? to list commands.\n'
    prompt = '(CLI) '


    def __init__(self, completekey='tab', stdin=None, stdout=None):
        Cmd.__init__(self, completekey=completekey, stdin=stdin, stdout=stdout)
        self._defaults = self.CommandLineToolDefaults()


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
            type=lambda x: x.split(':', 2),
            action=NameValuePairValidator,
            help='the parameters list, in the form of name:value pairs'
        )
        try:
            arguments = self._getCommandArguments(parser, args)
        except SystemExit as exc:
            return

        print(arguments.parameterA)
        print(arguments.parameterB)
        print(arguments.parametersPairs)


    def do_quit(self, args):
        """
        Quits the program.
        """
        print("bye")
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
        arguments = parser.parse_args(argsList)
        return arguments


def main():
    prompt = CommandLineTool()
    prompt.cmdloop()


if __name__ == '__main__':
    main()
```
