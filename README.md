# daemon
怎样让python程序以守护进程运行

Forking a Daemon Process on Unix

Forking a daemon on Unix requires a certain specific sequence of system calls, which is explained in W. Richard Steven’s seminal book, Advanced Programming in the Unix Environment (Addison-Wesley). We need to fork twice, terminating each parent process and letting only the grandchild of the original process run the daemon’s code. This allows us to decouple the daemon process from the calling terminal, so that the daemon process can keep running (typically as a server process without further user interaction, like a web server, for example) even after the calling terminal is closed. The only visible effect of this is that when you run this script as a main script, you get your shell prompt back immediately.

For all of the details about how and why this works in Unix and Unix-like systems, see Stevens’s book. Stevens gives his examples in the C programming language, but since Python’s standard library exposes a full POSIX interface, this can also be done in Python. Typical C code for a daemon fork translates almost literally to Python; the only difference you have to care about—a minor detail—is that Python’s os.fork does not return -1 on errors but throws an OSError exception. Therefore, rather than testing for a less-than-zero return code from fork, as we would in C, we run the fork in the try clause of a try/except statement, so that we can catch the exception, should it happen, and print appropriate diagnostics to standard error.
