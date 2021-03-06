# PyFTPd Mirror
This repository exists as a mirror of `pyftpd 0.8.4.5`, which was known to have a vulnerability. 

This branch has been slightly modified, a mirror for the original source code can be found [here](https://github.com/brendonky18/pyftpd-0.8.4.5_mirror/blob/mirror/auth_db_config.py). The original source can be found [here](http://kassiopeia.juls.savba.sk/~garabik/software/pyftpd/).

The next section contains the original `README`. 

# README
Documentation is not quite finished yet.

pyftpd is a is a multiplatform (currently tested only on linux, hurd, 
solaris, hp/ux, NetBSD and windows95/98/NT/2000) ftp daemon written in
python. It can work under normal user account and does not require any
special rights to start. Thanks to the modular design, there are
possible many different authentification schemes, most notably anonymous
user authentification, authentification from internal database of users
(these are virtual users, independent from the operating system users),
and authentification against PAM[1].

pyftpd uses its own permission modules, and can pinpoint user
permissions down to a simple file and simple ftp operation.

pyftpd uses python threads; if your python interpreter was compiled
without thread support, it will use forking instead, with limited
functionality (limiting number of simultaneously logged users and some
other options won't work). If neither threads nor fork is available,
pyftpd will serve just one connection at a time.

You can start pyftpd from inetd, it should automatically recognize it,
with similar restrictions to forking.


The main pyftpd configuration is in file config.py. Configuration for
additional modules is in files modulename_config.py. You can change
configuration either by editing appropriate file, or by running
graphical configuration tool (Tkinter required). Configuration tool for
config.py file is called conf_configure.py, configuration tools for
other modules are called conf_modulename.py. When you launch
conf_configure.py, in the window you are configuring only the list
of authentification modules (Edit button is a bit misleading - it does
not edit, but rather replace the module with another one). Global
options are accessible via the Option button.

You should include at least one auth_* module and exactly one fs_* module.
And it is probably a good idea to include optional *list module,
because the built-in LIST command gives just basic output format,
which most GUI ftp clients cannot grok.

To configure individual modules, you have to run appropriate
conf_*.py scripts.

On virtual and real paths: Path to a file or directory, as seen on the
disk, is a real path. 
However, path as visible to ftp clients can be mangled in some ways[2].
The mangled visible path is called virtual path, and this is the path
you enter as Initial dir or in cwd_module or in perm_acl_module.

On performance: pyftpd, when possible, uses multiple threads, which
means it manages the memory (which is the decisive factor when serving
many clients) in quite an efficient way. As an example, on my Debian
Linux with python 1.5.2, idle server takes 1564kB RAM, of which 588kB is
shareable with eventual other python interpreter. Each connected client
takes additional 44kB memory, and some more kB when up/downloading a
file (this is configurable via sbufsize and rbufsize options). Compare
with proftpd 1.2.0, which (in standalone mode) takes 532kB (not counting
libc and other system libraries) and each connected client takes
additional 312kB memory. Or with wu-ftpd, which takes 492kB memory, and
each connection takes additional 344kB. This is due to multithreaded
model of pyftpd, compared to forking of other daemons.

On the other hand, there is a little nice single-threaded ftp server,
using non blocking sockets and poll, called betaftpd
(http://members.xoom.com/sneeze/betaftpd-lynx.html), which takes 220kB
memory, and each connections takes additional 12kB. This shows up the
efficiency of C over python :-).



[1] It is currently not quite functioning. 

[2] Since version 0.8, you can use virtual filesystem, having nothing
    to do with real filesystem.
