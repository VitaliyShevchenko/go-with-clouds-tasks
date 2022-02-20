## Brainfuck Compiler

### Task Description
Implement Brainfuck compiler. The following phases have to be supported: syntax analysis, optimization, execution in Golang, code generation.

### Input Parameters
Command-line parameter specifying the path to the file that contains the Brainfuck program.

### Syntax Analysis
In the scope of this task, all the commands of Brainfuck language must be supported with the exception of “,” (input command). It must be ignored or treated as an illegal symbol.

### Optimization
The chains of the same commands must be replaced by a single command that performs aggregated action, e.g. “+++++” chain must be replaced by a single command that performs “increment by 5” operation.

### Execution in Golang
Compiled and optimized Brainfuck program must be executed in Golang. The output must be the same as for the original Brainfuck code as by specification.

### Unit Tests and Documentation
A good level of documentation and unit test coverage are required.