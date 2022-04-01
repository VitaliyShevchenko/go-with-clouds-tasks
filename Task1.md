# Brainfuck Interpreter project

### Pre requirements

1. Tools needed for development: **go1.17**, **iTerm**, **git**, **IntelliJ IDEA**.
2. IntelliJ IDEA should have next plugins installed:
   **golang**, **golint**, **markdown**, **git**, **github**, **key promoter x**.

### Project Description

Implement Brainfuck interpreter.  
Specification of the language can be found on Wiki: https://en.wikipedia.org/wiki/Brainfuck.  
Implementation should be readable, maintainable and easily extendable so that it is easy to add a new command to the language by
just a small number of modifications. Avoid procedural programming and come up with the design which has abstractions for
commands, memory, etc.  
For the first phase of development we can skip the `,` input operator.

### Input Parameters

Command-line parameter specifying the path to the file that contains the Brainfuck program.

### Testing

Execute the simple Hello world program in Brainfuck and verify the correct output:

    ++++++++[>++++[>++>+++>+++>+<<<<-]>+>+>->>+[<]<-]>>.>---.+++++++..+++.>>.<-.<.+++.------.--------.>>+.>++.

You can use [online brainfuck interpreter](https://www.dcode.fr/brainfuck-language) to verify your results against when executing
different brainfuck programs.

### Advanced features

* Come up with a new command and implement it. The simplest example is to add `5` operator which adds 5 to the current cell.
* Allow empty spaces and line breaks in the input for the brainfuck interpreter.

### Golang articles which are useful during implementation

* [Go modules and packages](https://levelup.gitconnected.com/using-modules-and-packages-in-go-36a418960556)
* [Using Go modules](https://go.dev/blog/using-go-modules)
* [Golang tips: why pointers to slices are useful and how ignoring them can lead to tricky bugs](https://medium.com/swlh/golang-tips-why-pointers-to-slices-are-useful-and-how-ignoring-them-can-lead-to-tricky-bugs-cac90f72e77b)

### Unit Tests

Go through the following articles to get a basic understanding on how to write unit tests in Go:

* [Go Test Your Code: An introduction to testing in Go](https://medium.com/rate-engineering/go-test-your-code-an-introduction-to-effective-testing-in-go-6e4f66f2c259)
  - explains how to build a simple test
* [An Introduction to Testing in Go](https://tutorialedge.net/golang/intro-testing-in-go/) - shows the basic unit tests and table
  driven tests
* [Advanced Go Testing Tutorial](https://tutorialedge.net/golang/advanced-go-testing-tutorial/) - shows how to separate unit and
  integration tests
* (Optional) Get familiar with BDD testing lib: [Ginkgo](https://onsi.github.io/ginkgo/)
  and [Gomega](https://onsi.github.io/gomega/).  
  It will be useful later on and it is used in our enterprise project.

### Tasks

* Create personal brainfuck repo on GitHub
* Read about [GitFlow strategy](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy)
* Implement Brainfuck interpreter according to **Project Description** in a feature branch, open PR
* Read Golang articles and docs mentioned above
* Implement **Advanced features**
* Add unit tests using standard Go `testing` library (use simple tests and table driven tests)
  * `go test -cover` should be showing more than 80 percent coverage.

### Best practices

* Read [Go standards for project layout](https://github.com/golang-standards/project-layout).
* Follow the simple and practical layout for this
  project [here](https://eli.thegreenplace.net/2019/simple-go-project-layout-with-modules/).
* Use [.gitignore](https://git-scm.com/docs/gitignore) to avoid committing IDE, git files and other local development environment.
* Use standard [naming conventions](https://talks.golang.org/2014/names.slide#1) in Go.
* Learn [how to name packages](https://go.dev/blog/package-names).
* Learn how to write [Go docs](https://go.dev/blog/godoc) and add docs where necessary.
* Whenever developing a new Go module, always accompany it with a [README.md](https://www.makeareadme.com/) file with a proper
  description.  
  Follow this [guide](https://www.freecodecamp.org/news/how-to-write-a-good-readme-file/) to write a readme for the project.
* Learn [Go review comments conventions](https://gist.github.com/adamveld12/c0d9f0d5f0e1fba1e551#go-code-review-comments).
* Learn [Effective Go](https://go.dev/doc/effective_go) conventions.
* Consult [DevIQ](https://deviq.com/) site for Clean Code principles and design patterns.

### Supporting info materials

* [A tour of Go](https://go.dev/tour/) - quick way to get familiar with GoLang syntax
* [Go By Example site](https://gobyexample.com/) - a way to quickly recall the Go syntax
* "Go in Action" - a book to deep dive in Go
* [Go language official spec](https://go.dev/doc/)
* [Git official docs](https://git-scm.com/doc)