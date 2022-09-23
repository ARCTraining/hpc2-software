# CMake

This chapter will look at using [CMake](https://cmake.org/) to build software.

```{note}
The steps outlined below are performed on the [ARC4](https://arcdocs.leeds.ac.uk/systems/arc4.html) HPC system where CMake exists as a [loadable module](https://arcdocs.leeds.ac.uk/software/infrastructure/cmake.html).
```

## Introduction

CMake is a collection of tools for building, testing and packaging software. 
It is open source and cross platform and supports a variety of different build tools including Make, Microsoft Visual Studio, Apple's Xcode and others.

CMake works by generating build files from a series of CMake configuration files which the relevant build tool can be used to build the software from.
This in many cases means CMake is used to create a series of Makefiles which allow us to use GNU make to build our software.

## Installation

If you don't have CMake installed you can download the latest version for your operating system from the [CMake website](https://cmake.org/download/).

## Building software with CMake

In the following section we'll explore a small example of building the command line tool [cURL](https://curl.se/) using CMake. 
cURL is a tool for transferring data using various network protocols and is commonly used for downloading data from the internet.

```{note}
cURL is already installed on ARC4, but we'll look at installing our own personal version for this tutorial.
```

### Cloning the cURL source code

To get started we need to download the cURL source code. cURL is available via GitHub so we can clone the source code at the most recent tagged release with the following line.

```bash
$ git clone --single-branch --branch curl-7_85_0 https://github.com/curl/curl.git
```

This will download the cURL source code with the history of the associated release. You will likely see a warning message about being in a `detached head state` which you can ignore for this tutorial.

### Configuring with CMake

Now we've downloaded the cURL source code we need to configure it before building using CMake.
You can do this directly in the command line by running `cmake .` and specifying additional options or you can use `ccmake` to do configuration interactives through a GUI-like interface.

#### Using cmake directly

Running CMake directly runs all the configuration steps and provides some output to the screen. 
We can modify CMake default behaviour by passing additional arguments to `cmake` at this stage.

```bash
$ cd curl

$ cmake .
```

This command at a basic level, will detect that there are GNU Makefiles present and, using the CMakeLists.txt file, configure a series of Makefiles for building the software.
Running this line on ARC4 will provide output similar to this:

```
-- curl version=[7.85.0-DEV]
-- Could NOT find LibPSL (missing: LIBPSL_LIBRARY LIBPSL_INCLUDE_DIR)
-- Could NOT find LibSSH2 (missing: LIBSSH2_LIBRARY LIBSSH2_INCLUDE_DIR)
-- Enabled features: SSL IPv6 unixsockets libz AsynchDNS Largefile alt-svc HSTS NTLM HTTPS-proxy
-- Enabled protocols: DICT FILE FTP FTPS GOPHER GOPHERS HTTP HTTPS IMAP IMAPS LDAP MQTT POP3 POP3S RTSP SMB SMBS SMTP SMTPS TELNET TFTP
-- Enabled SSL backends: OpenSSL
-- Configuring done
-- Generating done
-- Build files have been written to: /home/home01/arcuser/curl
```

As we're doing this on ARC4 where a version of cURL is already installed we should probably make some tweaks. 
We can do this directly by passing specific arguments to `cmake`, in this example we want to specify a specific local build directory and set the name of the associated library `libcurl` that gets built to avoid conflicts with the system-installed `libcurl`.

```bash
$ mkdir build

$ cmake . -DCMAKE_INSTALL_PREFIX=$(pwd)/build -DLIBCURL_OUTPUT_NAME=libcurl2
```

It is normal to specify CMake options by their name with a `-D` prepended to the option name.
By default CMake will install software in standard location on our machine, this often means locations such as `/usr/local` which requires `sudo` rights to write to.
Therefore, it's often better for software we want to build just for ourselves to specify the `-DCMAKE_INSTALL_PREFIX` argument to be a local directory our user has access to.

```{note}
This example has been picked for it's simplicity and doesn't require any additional libraries that aren't already installed and available on ARC4.
Most software has additional dependencies that you will either need to load as a module or install separately before being able to have CMake configure successfully.
Always be sure to check the documentation to understand what the exact requirements are for the software you're trying to install.
```

#### Using ccmake

You can also do the CMake configuration steps using `ccmake` which provides a GUI-like interface for adjusting CMake configuration options.

You can open the ccmake GUI by running the following line within the `curl/` directory:

```bash
$ ccmake .
```
```output
 BUILD_CURL_EXE                   ON
 BUILD_SHARED_LIBS                ON
 BUILD_TESTING                    ON
 CMAKE_BUILD_TYPE
 CMAKE_INSTALL_PREFIX             /home/home01/arcuser/curl/build
 CMAKE_LBER_LIB                   lber
 CMAKE_LDAP_INCLUDE_DIR
 CMAKE_LDAP_LIB                   ldap
 CURL_BROTLI                      OFF
 CURL_CA_BUNDLE                   /etc/pki/tls/certs/ca-bundle.crt
 CURL_CA_BUNDLE_SET               ON
 CURL_CA_FALLBACK                 OFF
 CURL_CA_PATH                     /etc/ssl/certs
 CURL_CA_PATH_SET                 ON
 CURL_DISABLE_OPENSSL_AUTO_LOAD   OFF
 CURL_ENABLE_SSL                  ON
 CURL_LTO                         OFF

BUILD_CURL_EXE: Set to ON to build curl executable.
Keys: [enter] Edit an entry [d] Delete an entry             CMake Version 3.22.3
      [l] Show log output   [c] Configure
      [h] Help              [q] Quit without generating
      [t] Toggle advanced mode (currently off)

```

This provides the following interface where we can navigate through options using the arrow keys and adjust entries as required.
Using `ccmake` is often a useful way to view all the available configuration settings for a project and adjust them as required.
You can also use `c` to run the configuration step to check everything configures successfully before confirming you want to proceed with that configuration by pressing `g` for generate.

#### Building CMake configured software

Now that CMake has configured everything we can proceed to building the software using GNU Make (here we pass the option `-j 4` to use multiple threads).

```bash
$ make -j 4
```

Once the make step completes successfully we can install the cURL executable to the previously specified install location with the command:

```bash
$ make install
```

If you've set everything up as I have on ARC4 and run this in your /home directory your new cURL executable will be at `~/curl/build/bin`. 
However, because we already have a system-installed cURL version we need to do an extra step before testing out our newly built version of cURL.
Crucially, we need to make the `libcurl2` library that was also built with cURL available for our new cURL executable to use.
If we try and run our new cURL without doing this step we get the following error:

```bash
$ ~/curl/build/bin/curl --version
```
```output
/home/home01/arcuser/curl/build/bin/curl: error while loading shared libraries: libcurl2.so: cannot open shared object file: No such file or directory
```

This is because `libcurl2` is not available to use via the associated environment variable. 
To solve this we need to update the `LD_LIBRARY_PATH` variable to include the directory containing `libcurl2.so`.
We do this by appending a directory path to the colon-separated `LD_LIBRARY_PATH` variable.

```bash
$ export "LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$HOME/curl/build/lib64"

$ ~/curl/build/bin/curl --version
```
```output
curl 7.85.0-DEV (Linux) libcurl/7.85.0-DEV OpenSSL/1.0.2k-fips zlib/1.2.7
Release-Date: [unreleased]
Protocols: dict file ftp ftps gopher gophers http https imap imaps ldap mqtt pop3 pop3s rtsp smb smbs smtp smtps telnet tftp
Features: alt-svc AsynchDNS HSTS HTTPS-proxy IPv6 Largefile libz NTLM SSL UnixSockets
```

So after adding the directory location containing our `libcurl2.so` file our newly created cURL executable is able to find it's associated library and run correctly.

```{note}
Editing variables like this during a shell session do not persist between shell sessions.
In order to keep these changes between different shell sessions you will need to add the above `export` line the `.bashrc` login script that exists in your home directory.
```

## Summary

```{important}
- Introduces CMake tool for configuring build systems
- Linked to CMake download page for installation steps
- Showcased an example of build `curl` using CMake
    - Included how to configure specific settings using `-D` option
    - Highlighted using `-DCMAKE_INSTALL_PREFIX` for appropriate installation of software locally
    - Showed steps to build using GNU Make after configuring with CMake
    - Highlighted modifying `LD_LIBRARY_PATH` for locally installed software and libraries
```