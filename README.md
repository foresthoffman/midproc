## Reap

"reap" provides functionality for running commands with optional arguments as background processes. This package allows Go programs to create detached processes which can live and exit independently of their parents. This package is intended to be utilized to create an intermediate process runner (*). Such a runner is used to create a 2nd-degree child (grandchild) process, then it is discarded, which results in a detached child process.

(*) See [reaper](https://github.com/foresthoffman/reaper) for an intermediate process runner.

### Installation

Run `go get github.com/foresthoffman/reap`

### Importing

Import this package by including `github.com/foresthoffman/reap` in your import block.

e.g.

```Go
package main

import(
    ...
    "github.com/foresthoffman/reap"
)
```

### Usage

Here's a simple example of the syntax:

```Go
package main

import(
    "fmt"
    "github.com/foresthoffman/reap"
)

func main() {
    pid, err := reap.Exec("sleep", "30") // Creates a detached child process
                                           // that will run the "sleep" command
                                           // with a duration of 30 seconds,
                                           // after which it exits.
    if nil != err {
        panic(err)
    }
    fmt.Println(pid) // Outputs the process ID of the "sleep" command.
}
```

Advanced usage, with dynamic commands and arguments:

```Go
package main

import(
    "flag"
    "fmt"
    "github.com/foresthoffman/reap"
)

func main() {
    cmdFlag := flag.String("cmd", "", "The command that will be run by the middle-man process.")
    argsFlag := flag.String("args", "", "The arguments for the command provided by the -cmd flag. Optional.")
    flag.Parse()

    //... flag input validation

    // Creates a detached child process that will run the command provided
    // in the `-cmd` CLI flag with the provided arguments in the `-args`
    // CLI flag, if any. After which, it will exit.
    pid, err := reap.Exec(*cmdFlag, *argsFlag)
    if nil != err {
        panic(err)
    }

    // Outputs the process ID of the provided command.
    fmt.Println(pid)
}
```

_That's all, enjoy!_
