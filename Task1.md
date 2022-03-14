## Pre requirements
1. Tools needed for development: **go1.17**, **iTerm**, **git**, **IntelliJ IDEA**.
2. IntelliJ IDEA should have next plugins installed: **golang**, **golint**, **markdown**, **git**, **github**, **key promoter x**.

## Brainfuck Interpreter

### Project Description
Implement Brainfuck interpreter.  
Specification of the language can be found on Wiki: https://en.wikipedia.org/wiki/Brainfuck.
Implementation should be readable, maintainable and easily extendable 
so that it is easy to add a new command to the language by just a small number of modifications.
For the first phase of development we can skip the `,` input operator.

### Input Parameters
Command-line parameter specifying the path to the file that contains the Brainfuck program.

### Testing
Execute the simple Hello world program in Brainfuck and verify the correct output.

    ++++++++[>++++[>++>+++>+++>+<<<<-]>+>+>->>+[<]<-]>>.>---.+++++++..+++.>>.<-.<.+++.------.--------.>>+.>++.

### Unit Tests and Documentation
A good level of documentation and unit test coverage are required.

### Additional tasks
* Add a new commands, where the chains of the same commands can be replaced by a single command that performs aggregated action, 
e.g. “+++++” chain must be replaced by a single command that performs “increment by 5” operation.
* Allow empty spaces and line breaks in the input for the brainfuck interpreter.

### Useful Links
https://en.wikipedia.org/wiki/Brainfuck
https://www.dcode.fr/brainfuck-language

### Golang articles and docs
* [Go modules and packages](https://levelup.gitconnected.com/using-modules-and-packages-in-go-36a418960556)
* [Using Go modules](https://go.dev/blog/using-go-modules)
* [Golang tips: why pointers to slices are useful and how ignoring them can lead to tricky bugs](https://medium.com/swlh/golang-tips-why-pointers-to-slices-are-useful-and-how-ignoring-them-can-lead-to-tricky-bugs-cac90f72e77b)

### Tasks
* Create personal brainfuck repo on GitHub
* Read about [GitFlow strategy](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy)
* Implement basic Brainfuck interpreter in a feature branch, open PR
* Read Golang articles and docs mentioned above

### Supporting info materials
* [A tour of Go](https://go.dev/tour/) - quick way to get familiar with GoLang syntax
* [Go By Example site](https://gobyexample.com/) - a way to quickly recall the Go syntax
* "Go in Action" - a book to deep dive in Go
* [Go language official spec](https://go.dev/doc/)
* [Git official docs](https://git-scm.com/doc)