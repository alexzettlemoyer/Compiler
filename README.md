# CSC412 Project Development
Alex Zettlemoyer, Willie Harris

To run the profiling tool, LLVM must be installed and built.

* To install LLVM clone from https://github.com/llvm-mirror/llvm
* The cloned LLVM should be in our project directory, adjacent to the README.md file, titled "llvm"

* To build the cloned LLVM, from the project directory run

    - `mkdir build`
    - `cd build`
    - `cmake -DCMAKE_BUILD_TYPE=Debug ../llvm`
    - `cmake --build ./`
    - LLVM must be built with Debug

* This program requires the installation of clang, llvm, gcc, and valgrind

    - `sudo apt install clang`
    - `sudo apt install llvm`
    - `sudo apt install gcc`
    - `sudo apt install valgrind`

***** the LLVM build must be in a directory titled `build` in the project directory.
This directory should be in the topmost level of the directory, adjacent to Part1/, Part2/, tests/, output/, etc..

_______
TESTING FILES:

* 2 contrived C programs
    - automaton.c
        - to run the profiler on this program use `./start.sh tests/automaton.c`
    - binrep.c
        - to run the profiler on this program use `./start.sh tests/binrep.c`

* real-world substitute to SPEC CPU: driver.c
    - driver.c is part of a C program implemented in CSC230
    - the program acts as a custom command prompt interface
    - this interface manages a custom implemented C map, which the user can manipulate through the command prompt
    - the map only works for <integer, integer> key-value pairs
    - the source file has 527 lines of non-comment, non-blank source code lines
        - use `cat tests/driver.c | sed '/^\s*$/d' | wc -l` to see the number
    - usage:
        - `set key value`:    store a new key-value pair in the map     (ex: set 4 6)
        - `get key`:          get the value of a key                    (ex: get 4)
        - `remove key`:       remove a key-value pair from the map      (ex: remove 4)
        - `size`:             get the current size of the map           (ex: size)
        - `display`:          displays the current map                  (ex: display)
        - `quit`:             quit the command prompt                   (ex: quit)
    - to run the profiler on this program use `./start.sh tests/driver.c`


_______
PART 1:

Run the script branch_tracer.sh for the full profiling tool
    `usage: ./branch_tracer.sh <path_to_input_C_file>`

for example, to run the profiling tool on example.c in the test directory:
    `./branch_tracer.sh tests/example.c`

This will
1. generate the LLVM IR for the input C file
2. compile the BranchTracer.cpp LLVM custom LLVM transform pass
3. transform the generated LLVM IR using the branch tracer
- this will output the static branch dictionary of all branches in the program, and their start and target lines
- if you want this output to print in a separate file, create a directory `output`
4. run the transformed file
- this will output the value of function pointers when they are invoked
- and the executed branches (from the branch dictionary)
- and the program output, which may require user input
5. copmile the original C file using gcc
6. run valgrind callgrind
- this will output the number of executed instructions (from valgrind's binary profiling tool)
- and the program output, which may require user input


_______
PART 2:

Run the script inputFeatureDetector.sh for the analysis tool
    `usage: ./inputFeatureDetector.sh <path_to_input_C_file>`

for example, to run the profiling tool on example.c in the test directory:
    `./inputFeatureDetector.sh tests/example.c`

This will
1. generate the LLVM IR for the input C file
2. compile the InputFeatureDetector.cpp LLVM custom LLVM transform pass
3. transform the generated LLVM IR using the branch tracer
- this will statically analyze the input file for key points


_______
PART 3:
Run the script start.sh for the tool to run cohesively
    `usage: ./start.sh <path_to_input_C_file>`

for example, to run the profiling and analysis tool on example.c in the test directory:
    `./start.sh tests/example.c`
