
# JAVA_HOME Shell Wrapper

This is a series of wrapper script for Java, meant to be run by shell. (Currently Bourne Shell only)

## Disclaimer

I am by no means a Java developer.

## Description

I have to use different Java versions to perform different task, so I'd keep all of the versions installed.  
But thing is some OS's (especially some Linux distributions) tend to put all executables of one specific version of Java in somewhere `PATH` environment variable typically contain, typically `/usr/bin`.  
This place is somewhat unable to be removed because it is shared by some system essentials, unless I make a directory containing the needed executables by myself.

To easily resolve this problem one can just prepend the `bin` directory of the Java installation to be used afterwards to the `PATH` environment variable.  
But thing is that by doing this one may introduce incompatible executable.  
e.g. We originally have Java 8 in `PATH` and prepend the Java 17 `bin` directory, the resulting `PATH` will contain Java 8 `unpack200` as well as Java 17 `javac`, since Java 17 does not come with `unpack200`.  

In macOS and Windows world, one can change the Java version by setting environment variable `JAVA_HOME` to the directory contains any Java installation.  
On macOS it is straightforward since the system includes a series of wrappers which read the value of this environment variable and find the executable there.  
This project (or, wrapper scripts) is inspired by the wrappers.  
On Windows however, Java should be usually installed by the installer package which does not touch `PATH` by default.  
Although there is no wrapper performing the task macOS does, the executables run on Windows also typically respect the `JAVA_HOME` environment variable and use corresponding Java installation.   
On Linux and other Unix-like system one may install Java from the package source. In this case the behavior is controlled by the system which distributed them.  
If the executable is placed in `/usr/bin` no one can change the behavior without root permission. In this case `JAVA_HOME` also do not work.

I have searched for the whole GitHub and found no project for doing this task.   
Since I think this task should be simple I'd implement it by myself.

## Designation

Wraps the `java` executable series with the scripts in the `bin` directory in this repository.

When any of the wrapper script is run, following is done:
1. Search for `java` (instead of the executable name currently run) in the directory `JAVA_HOME` environment variable is currently pointing to if set.
1. If `JAVA_HOME` is not set or there is no `java` executable exist there, search for `java` in directories in `PATH` environment variable, and find the first one.

If either of the above found a `java` executable in any path:  
Search for the executable currently run in the aforementioned path and `exec` into it if found, with the arguments passed to wrapper.

If an executable is not find the wrapper script exits with code 127.

## Usage

__Prepend__ (Important! not append) the absolute path of `bin` directory from this repository to `PATH`.

## License

MIT

## TODOs

1. Windows support (run in MSYS2, run Windows Java executable)
1. Tests (Important for 1.0.0 release. A series of fake Java executable is needed. Maybe it should be in binary form CMake may be involved)
1. Other shells (`cmd` is 1st priority but I don't think it's easy for writing logic for it. Important for 2.0.0 release)
