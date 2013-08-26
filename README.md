Bedrock Linux 1.0alpha4 Flopsie Installer
=========================================

This is the installer for Bedrock Linux 1.0alpha4 Flopsie.  It has three main
purposes:

- Install *some* components of a new Bedrock Linux system.  Some components,
  like the bootloader, are out of the scope of this installer at the time of
  writing.

- Update core components of an existing Bedrock Linux system.  For example, if
  a security hole is found in Busybox, this script should be able to update
  busybox.

- Generate Bedrock Linux components or dependencies for other reasons, such as
  development work.  For example, if you are working on a FUSE filesystem which
  you would like to be statically compiled, this script can be used to generate
  the dependencies for you to make it easy to do.

Usage
=====

- Run the installer with the `-g` flag to have it kick out a commented
  configuration file.
- Fill out the configuration file and download the source code of the
  components you would like compiled.
- Run the installer with the `-c` flag.
- If any issues arise, it should point you to a verbose log.  Take a look there
  at what the problem could be; for example, you could be missing a development
  library which is required.  Resolve the issue and then re-run the installer
  with the `-c` flag.
- You may have to manually clear out the `OUTPUT_DIR` between runs, as the
  installer will not do this for you.  This is in case you mount pre-existing,
  populated filesystems (such as `/home`) in `OUTPUT_DIR`.

Design
======

The over-all design of the installer breaks down like this:

- There are a list of components which the user may want created.
	- e.g.: filesystem structure, busybox, brc, etc.
- There is a list of source code locations (as either directories or tarballs)
	- e.g.: bedrocklinux-userland, musl-libc, busybox, etc
- There are two directories: the output directory, where the final compiled
  components will go, and the scratch directory, where in-progress work goes.
- The installer will copy everything required into the scratch directory (it
  should not alter the original copies of the source code).
- The installer will compile the components in the scratch directory if applicable.
- The installer will copy the resulting components into the output directory.

Current limitations
===================

- The installer can only compile a fraction of the entirety of the components
  at the moment.
- In the long run, every library dependency for anything compiled by the
  installer should be compiled by the installer to ensure everything is as
  portable as is possible; it should not be dependent on system libraries.
  However, at the moment, if something is easy to compile statically with
  glibc or other libraries likely or easily installed onto the system, then
  those are used.
- The installer does not (yet) automate things such as adding users, setting
  the root password, or setting the hostname.
