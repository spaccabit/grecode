# How to compile grecode
# Introduction #

There is a make file, and no external dependencies.


# Details #
## Linux ##
  * Make sure you have the gnu c++ compiler installed
  * Make sure you have "make" installed
  * Call `make clean; make`
  * Hopefully this will create the grecode executable.
  * copy or symlink it into a directory that is in your path, probably as superuser.

## Windows ##

  * A windows executeable can be created by installing mingw compiler suite, with g++ and make. Then call make in the same directory where the makefile is.