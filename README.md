## Page Contents
- [Introduction](#introduction)
- [Features](#features)
- [Contributing](#contributing)
- [Synopsis](#synopsis)
- [Version](#version)
- [Downloading and Installing](#downloading-and-installing)
  - [Installing using Homebrew on Mac](#installing-using-homebrew-on-mac)
    - [Install](#install)
    - [Uninstall](#uninstall)
  - [Installing the debian package on Ubuntu or Debian or Raspberry pi](#installing-the-debian-package-on-ubuntu-or-debian-or-raspberry-pi)
    - [Inspect the package content](#inspect-the-package-content)
    - [Install](#install)
    - [Uninstall](#uninstall)
  - [Install the RPM package](#install-the-rpm-package)
    - [Inspect the package content](#inspect-the-package-content)
    - [Install/Upgrade](#install-upgrade)
    - [Uninstall](#uninstall)
  - [Install from archive](#install-from-archive)
    - [Inspect the content](#inspect-the-content)
    - [Install Linux](#install-linux)
    - [Install Windows](#install-windows)
      - [Installing using Scoop on Windows](#installing-using-scoop-on-windows)
        - [Install](#install)
        - [Uninstall](#uninstall)
      - [Installing Manually](#installing-manually)
- [Compiling](#compiling)
- [Examples](#examples)
  - [Show SMTP server information](#show-smtp-server-information)
    - [StartTLS will be used if server supports it](#starttls-will-be-used-if-server-supports-it)
    - [Use SSL. Note the port is different](#use-ssl-note-the-port-is-different)
  - [Send mail with a text message](#send-mail-with-a-text-message)
  - [Send mail with a HTML message](#send-mail-with-a-html-message)
  - [Attach a PDF file](#attach-a-pdf-file)
  - [Attach a PDF file and an image](#attach-a-pdf-file-and-an-image)
  - [Attach a PDF file and embed an image](#attach-a-pdf-file-and-embed-an-image)
  - [Set Carbon Copy and Blind Carbon copy](#set-carbon-copy-and-blind-carbon-copy)
  - [Send mail to a list of users](#send-mail-to-a-list-of-users)
  - [Add Custom Headers](#add-custom-headers)
  - [Write logs to a file](#write-logs-to-a-file)
  - [Specify a different character set](#specify-a-different-character-set)
- [License (is MIT)](#license-is-mit)
- [See Also](#see-also)

# Introduction

`mailsend-go` is a command line tool to send mail via SMTP protocol. This is the
[golang](https://golang.org/) incarnation of my C version of
[mailsend](https://github.com/muquit/mailsend/). However, this version is much
simpler and all the heavy lifting is done by the package
[gomail.v2](https://gopkg.in/gomail.v2)

If you use [mailsend](https://github.com/muquit/mailsend), please consider
using mailsend-go as no new features will be added to 
[mailsend](https://github.com/muquit/mailsend).

If you have any question, request or suggestion, please enter it in the 
[Issues](https://github.com/muquit/mailsend-go/issues) with appropriate label.


# Features

* Add a mail body
* Support Multiple Attachments
* Supports ESMTP Authentication
* Supports StartTLS and SSL
* Send mail to a list of users
* Show SMTP server info
* Fixes [issues of mailsend](https://github.com/muquit/mailsend#known-issues)

# Contributing

Please send a pull request if you add features, fix bugs or update the documentation. 

If you want to update the documentation, **please do not update README.md directly**, 
rather update the Markdown files in _docs/_ directory. README.md 
is generated by [markdown_helper](https://github.com/BurdetteLamar/markdown_helper) ruby gem by
assembling the individual Markdown files in the _docs/_ directory. If you
would like to generate README.md, type `make gen` (you will need required tools of
course)

# Synopsis
```
 Version: @($) mailsend-go v1.0.7

 mailsend-go [options]
  Where the options are:
  -debug                 - Print debug messages
  -sub subject           - Subject
  -t to,to..*            - email address/es of the recipient/s. Required
  -list file             - file with list of email addresses. 
                           Syntax is: Name, email_address
  -fname name            - name of sender
  -f address*            - email address of the sender. Required
  -cc cc,cc..            - carbon copy addresses
  -bcc bcc,bcc..         - blind carbon copy addresses
  -rt rt                 - reply to address
  -smtp host/IP*         - hostname/IP address of the SMTP server. Required
  -port port             - port of SMTP server. Default is 587
  -domain domain         - domain name for SMTP HELO. Default is localhost
  -info                  - Print info about SMTP server
  -ssl                   - SMTP over SSL. Default is StartTLS
  -verifyCert            - Verify Certificate in connection. Default is No
  -ex                    - show examples
  -help                  - show this help
  -q                     - quiet
  -log filePath          - write log messages to this file
  -cs charset            - Character set for text/HTML. Default is utf-8
  -V                     - show version and exit
  auth                   - Auth Command
   -user username*       - username for ESMTP authentication. Required
   -pass password*       - password for EMSPTP authentication. Required
  body                   - body command for attachment for mail body
   -msg msg              - message to show as body 
   -file path            - or path of a text/HTML file
   -mime-type type       - MIME type of the body content. Default is detected
  attach                 - attach command. Repeat for multiple attachments
   -file path*           - path of the attachment. Required
   -name name            - name of the attachment. Default is filename
   -mime-type type       - MIME-Type of the attachment. Default is detected
   -inline               - Set Content-Disposition to "inline". 
                           Default is "attachment"
  header                 - Header Command. Repeat for multiple headers
   -name header          - Header name
   -value value          - Header value

The options with * are required. 

Environment variables:
   SMTP_USER_PASS for auth password (-pass)

```
# Version
The current version of mailsend-go is 1.0.7, released on Feb-16-2020 

Please look at [ChangeLog](ChangeLog.md) for what has changed in the current version.

# Downloading and Installing

Pre-compiled `mailsend-go` binaries are available for the following platforms:

* Windows - 32 and 64 bit (zip, Scoop)
* Linux - 64 bit (tgz, debian and rpm)
* MacOS - 64 bit (tgz, Homebrew)
* Raspberry pi - 32 bit (debian, rpm)

Please download the binaries from the [releases](https://github.com/muquit/mailsend-go/releases)
page.  

Please add an [issue](https://github.com/muquit/mailsend-go/issues) if you would need binaries for any other         platforms.

Before installing, please make sure to verify the checksum.

When the tgz or zip archives are extracted they create a directory `mailsend-go-dir/` with the 
content.

**Example**

```
    $ tar -tvf mailsend-go_x.x.x_linux_64-bit.tar.gz
	-rw-r--r--  0 muquit staff    1081 Jan 26 15:21 mailsend-go-dir/LICENSE.txt
	-rw-r--r--  0 muquit staff   14242 Jan 27 13:47 mailsend-go-dir/README.md
	-rw-r--r--  0 muquit staff   16866 Jan 27 13:47 mailsend-go-dir/docs/mailsend-go.1
	-rwxr-xr-x  0 muquit staff 5052992 Feb  9 19:23 mailsend-go-dir/mailsend-go
```

```
	$ unzip -l mailsend-go_x.x.x_windows_64-bit.zip
	Archive:  mailsend-go_x.x.x_windows_64-bit.zip
	  Length      Date    Time    Name
	---------  ---------- -----   ----
		 1081  01-26-2019 15:21   mailsend-go-dir/LICENSE.txt
		14242  01-27-2019 13:47   mailsend-go-dir/README.md
		16866  01-27-2019 13:47   mailsend-go-dir/docs/mailsend-go.1
	  4933632  02-09-2019 19:23   mailsend-go-dir/mailsend-go.exe
	---------                     -------
	  4965821                     4 files
```

## Installing using Homebrew on Mac

You will need to install [Homebrew](https://brew.sh/) first.

### Install

First install the custom tap.

```
    $ brew tap muquit/mailsend-go https://github.com/muquit/mailsend-go.git
    $ brew install mailsend-go
```

### Uninstall
```
    $ brew uninstall mailsend-go
```


## Installing the debian package on Ubuntu or Debian or Raspberry pi

### Inspect the package content
```
    $ dpkg -c mailsend-go_linux_64-bit.deb
	drwxr-xr-x 0/0               0 2019-02-10 20:17 usr/
	drwxr-xr-x 0/0               0 2019-02-10 20:17 usr/local/
	drwxr-xr-x 0/0               0 2019-02-10 20:17 usr/local/share/
	drwxr-xr-x 0/0               0 2019-02-10 20:17 usr/local/share/docs/
	drwxr-xr-x 0/0               0 2019-02-10 20:17 usr/local/share/docs/mailsend-go/
	-rw-r--r-- 0/0            1081 2019-02-10 20:17 usr/local/share/docs/mailsend-go/LICENSE.txt
	drwxr-xr-x 0/0               0 2019-02-10 20:17 usr/local/bin/
	-rwxr-xr-x 0/0         5052992 2019-02-10 20:17 usr/local/bin/mailsend-go
	drwxr-xr-x 0/0               0 2019-02-10 20:17 usr/local/share/man/
	drwxr-xr-x 0/0               0 2019-02-10 20:17 usr/local/share/man/man1/
	-rw-r--r-- 0/0           20896 2019-02-10 20:17 usr/local/share/man/man1/mailsend-go.1
	-rw-r--r-- 0/0           19236 2019-02-10 20:17 usr/local/share/docs/mailsend-go/README.md
```

### Install

```
    $ sudo dpkg -i mailsend-go_linux_64-bit.deb 
	Selecting previously unselected package mailsend-go.
	(Reading database ... 4039 files and directories currently installed.)
	Preparing to unpack mailsend-go_linux_64-bit.deb ...
	Unpacking mailsend-go (x.x.x) ...
	Setting up mailsend-go (x.x.x) ...
    $ mailsend-go -V
    @(#) mailsend-go vx.x.x
```

### Uninstall

```
    $ sudo dpkg -r mailsend-go
```

## Install the RPM package

### Inspect the package content
```
    $ rpm -qlp mailsend-go_linux_64-bit.rpm
    /usr/local/bin/mailsend-go
    /usr/local/share/docs/mailsend-go/LICENSE.txt
    /usr/local/share/docs/mailsend-go/README.md
    /usr/local/share/man/man1/mailsend-go.1
```
### Install/Upgrade
```
    # rpm -Uvh mailsend-go_linux_64-bit.rpm
    # mailsend-go -V
    @(#) mailsend-go vx.x.x
```
### Uninstall
```
    # rpm -ev mailsend-go
```

## Install from archive

### Inspect the content
```
    $ tar -tvf mailsend-go_x.x.x_linux_64-bit.tar.gz
    -rw-r--r--  0 muquit staff    1081 Jan 26 15:21 mailsend-go-dir/LICENSE.txt
    -rw-r--r--  0 muquit staff   14242 Jan 27 13:47 mailsend-go-dir/README.md
    -rw-r--r--  0 muquit staff   16866 Jan 27 13:47 mailsend-go-dir/docs/mailsend-go.1
    -rwxr-xr-x  0 muquit staff 5052992 Feb  9 19:23 mailsend-go-dir/mailsend-go
```

```
    $ unzip -l mailsend-go_x.x.x_windows_64-bit.zip
    Archive:  mailsend-go_x.x.x_windows_64-bit.zip
      Length      Date    Time    Name
    ---------  ---------- -----   ----
     1081  01-26-2019 15:21   mailsend-go-dir/LICENSE.txt
    14242  01-27-2019 13:47   mailsend-go-dir/README.md
    16866  01-27-2019 13:47   mailsend-go-dir/docs/mailsend-go.1
      4933632  02-09-2019 19:23   mailsend-go-dir/mailsend-go.exe
    ---------                     -------
      4965821                     4 files
```

### Install Linux
```
    $ tar -xf mailsend-go_x.x.x_linux_64-bit.tar.gz
    $ sudo cp mailsend-go-dir/mailsend-go /usr/local/bin
    $ sudo cp mailsend-go-dir/doc/mailsend-go.1 /usr/local/share/man/man1
```

### Install Windows

#### Installing using Scoop on Windows

You will need to install [Scoop](https://scoop.sh/) first.

##### Install

```batch
c:\> scoop install mailsend-go
````

##### Uninstall

```batch
c:\> scoop uninstall mailsend-go
````

#### Installing Manually

After [downloading](#downloading-and-installing) the latest .zip file (e.g., mailsend-go_x.x.x_windows_64-bit.zip), unzip it, and copy `mailsend-go-dir\mailsend-go.exe` somewhere in your PATH or run it from the directory.

# Compiling

Compiling from scratch requires the [Go programming language toolchain](https://golang.org/dl/) and git. Note: *mailsend-go* uses [go modules](https://github.com/golang/go/wiki/Modules) for dependency management.

To download, build and install (or upgrade) mailsend-go, run:

```
    $ go get -u github.com/muquit/mailsend-go
```
If you see the error message `go: cannot find main module; see 'go help
modules'`, make sure GO111MODULE environment variable is not set to on. Unset it by
typing `unset GO111MODULE`


To compile yourself:

* If you are using very old version of go, install dependencies by typing:

```
    $ make tools
    $ make
```

* If you are using go 1.11+, dependencies will be installed via go modules.
If you cloned mailsend-go inside your $GOPATH, you have to set env var:

```
    $ export GO111MODULE=on
```
* Finally compile mailsend-go by typing:

```
    $ make
```

As mailsend-go uses go modules, it can be built outside $GOPATH e.g.
```
    $ cd /tmp
    $ git clone https://github.com/muquit/mailsend-go.git
    $ cd mailsend-go
    $ make
    $ ./mailsend-go -V
    @(#) mailsend-go v1.0.1
```
* List the packages used (if you are outside $GOPATH)
```
    $ go list -m "all"
    github.com/muquit/mailsend-go
    gopkg.in/alexcesaro/quotedprintable.v3 v3.0.0-20150716171945-2caba252f4dc
    gopkg.in/gomail.v2 v2.0.0-20160411212932-81ebce5c23df
```
Type `make help` for more targets:


# Examples

Each example mailsend-go command is a single line. In Unix back slash \ 
can be used to continue in the next line. Also in Unix, use single quotes 
instead of double quotes, otherwise if input has any shell character like 
$ etc, it will get expanded by the shell.

## Show SMTP server information

### StartTLS will be used if server supports it

```
  mailsend-go -info -smtp smtp.gmail.com -port 587
```    

```
[S] 220 smtp.gmail.com ESMTP k185-v6sm17739711qkd.27 - gsmtp
[C] HELO localhost
[C] EHLO localhost
[S] 250-smtp.gmail.com at your service, [x.x.x.x]
[S] 250-SIZE 35882577
[S] 250-8BITMIME
[S] 250-STARTTLS
[S] 250-ENHANCEDSTATUSCODES
[S] 250-PIPELINING
[S] 250-CHUNKING
[S] 250-SMTPUTF8
[C] STARTTLS
[S] 220-2.0.0 Ready to start TLS
[C] EHLO localhost
[S] 250-smtp.gmail.com at your service, [x.x.x.x]
[S] 250-SIZE 35882577
[S] 250-8BITMIME
[S] 250-AUTH LOGIN PLAIN XOAUTH2 PLAIN-CLIENTTOKEN OAUTHBEARER XOAUTH
[S] 250-ENHANCEDSTATUSCODES
[S] 250-PIPELINING
[S] 250-CHUNKING
[S] 250-SMTPUTF8
Certificate of smtp.gmail.com:
 Version: 3 (0x3)
 Serial Number: 149685795415515161014990164765 (0x1e3a9301cfc7206383f9a531d)
 Signature Algorithm: SHA256-RSA
 Subject: CN=Google Internet Authority G3,O=Google Trust Services,C=US
 Issuer: GlobalSign
 Not before: 2017-06-15 00:00:42 +0000 UTC
 Not after: 2021-12-15 00:00:42 +0000 UTC
[C] QUIT
[S] 221-2.0.0 closing connection k185-v6sm17739711qkd.27 - gsmtp
```

### Use SSL. Note the port is different

```
  mailsend-go -info -smtp smtp.gmail.com -port 465 -ssl
```

## Send mail with a text message

Notice "auth" is a command and it takes -user and -pass arguments. "body" is
also a command and here it took -msg as an argument. The command "body" can
not repeat, if specified more than once, the last one will be used.

```
    mailsend-go -sub "Test"  -smtp smtp.gmail.com -port 587 \
     auth \
      -user jsnow@gmail.com -pass "secret" \
     -from "jsnow@gmail.com" -to  "mjane@example.com" \
     body \
       -msg "hello, world!"
```                    
The environment variable "SMTP_USER_PASS" can be used instead of the flag
`-pass`.

## Send mail with a HTML message
```
    mailsend-go -sub "Test"  \
    -smtp smtp.gmail.com -port 587 \
    auth \
     -user jsnow@gmail.com -pass "secret" \
    -from "jsnow@gmail.com"  \
    -to  "mjane@example.com" -from "jsnow@gmail.com" \
    body \
     -msg "<b>hello, world!</b>"
```

## Attach a PDF file
MIME type will be detected. Content-Disposition will be set to "attachment",
Content-Transfer-Encoding will be "Base64". Notice, "attach" is a command it
took -file as an arg. The command "attach" can repeat.
```
    mailsend-go -sub "Test"  \
    -smtp smtp.gmail.com -port 587 \
    auth \
     -user jsnow@gmail.com -pass "secret" \
    -from "jsnow@gmail.com"  \
    -to  "mjane@example.com" -from "jsnow@gmail.com" \
    body \
     -msg "A PDF file is attached" \
    attach \
     -file "/path/file.pdf"

```
## Attach a PDF file and an image
Notice, the "attach" command is repeated here.
```
    mailsend-go -sub "Test"  \
    -smtp smtp.gmail.com -port 587 \
    auth \
     -user jsnow@gmail.com -pass "secret" \
    -from "jsnow@gmail.com"  \
    -to  "mjane@example.com" -from "jsnow@gmail.com" \
    body \
     -msg "A PDF file and a PNG file is attached" \
    attach \
     -file "/path/file.pdf" \
    attach \
     -file "/path/file.png"
```
## Attach a PDF file and embed an image
Content-Disposition for the image will be set to "inline". It's an hint to the
mail reader to display the image on the page. Note: it is just a hint, it is
up to the mail reader to respect it or ignore it.
```
    mailsend-go -sub "Test"  \
    -smtp smtp.gmail.com -port 587 \
    auth \
     -user jsnow@gmail.com -pass "secret" \
    -from "jsnow@gmail.com"  \
    -to  "mjane@example.com" -from "jsnow@gmail.com" \
    body \
     -msg "A PDF file is attached, image should be displayed inline" \
    attach \
     -file "/path/file.pdf" \
    attach \
     -file "/path/file.png" \
     -inline
```
## Set Carbon Copy and Blind Carbon copy
```
    mailsend-go -sub "Testing -cc and -bcc" \
    -smtp smtp.gmail.com -port 587 \
    auth \
     -user example@gmail.com -pass "secret" \
     -to jsoe@example.com \
     -f "example@gmail.com" \
     -cc "user1@example.com,user2@example.com" \
     -bcc "foo@example.com" \
     body -msg "Testing Carbon Copy and Blind Carbon copy"
```
Cc addresses will be visible to the recipients but Bcc address will not be.

## Send mail to a list of users

Create a file with list of users. The syntax is ```Name,email_address``` in a line. Name can be empty but comma must be specified. Example of a list file:

```
    # This is a comment.
    # The syntax is Name,email address in a line. Name can be empty but comma 
    # must be specified
    John Snow,jsnow@example.com
    Mary Jane,mjane@example.com
    ,foobar@example.com
```

Specify the list file with ```-list``` flag. 

```
    mailsend-go -sub "Test sending mail to a list of users" \
    -smtp smtp.gmail.com -port 587 \
    auth \
     -user example@gmail.com -pass "secret" \
        -f "me@example.com" \
        -to "xyz@example.com" \
        body \
        -msg "This is a test of sendmail mail to a list of users" \
        attach \
            -file "cat.jpg" \
         attach \
            -file "flower.jpg" \
            -inline \
         -list "list.txt"
```

## Add Custom Headers

Use the command "header" to add custom headers. The command "header" can be
repeated.

```
    mailsend-go -sub "Testing custom headers" \
    -smtp smtp.gmail.com -port 587 \
    auth \
     -user example@gmail.com -pass "secret" \
     -to jdoe@example.com \
     -f "example@gmail.com" \
     body -msg "Testing adding Custom headers"
     header \
         -name "X-MyHeader-1" -value "Value of X-MyHeader-1" \
     header \
         -name "X-MyHeader-2" -value "Value of X-MyHeader-2"

```

## Write logs to a file

Use the flag `-log path_of_log_file.txt`

```
    mailsend-go -sub "test log" \
     -smtp smtp.example.com -port 587 \
     auth \
      -user example@gmail.com -pass "secret" \
      -to jdoe@example.com \
      -f "example@gmail.com" \
      body -msg "Testing log file" \
      -log "/tmp/mailsend-go.log"
```

## Specify a different character set

The default character set is utf-8

```
    mailsend-go -sub "test character set" \
     -smtp smtp.example.com -port 587 \
     auth \
      -user example@gmail.com -pass "secret" \
      -to jdoe@example.com \
      -from "example@gmail.com" \
      -subject "Testing Big5 Charset" \
      -cs "Big5" \
      body -msg "中文測試"

```

---

(Generated from docs/examples.md)

---

# License (is MIT)

License is MIT

Copyright © 2018-2020 muquit@muquit.com

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the "Software"),
to deal in the Software without restriction, including without limitation
the rights to use, copy, modify, merge, publish, distribute, sublicense,
and/or sell copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included
in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM,
DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE
OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

# See Also

Original [mailsend](https://github.com/muquit/mailsend) (in C)

---
* This file is assembled from docs/*.md with [markdown_helper](https://github.com/BurdetteLamar/markdown_helper)
* The software is released with [goreleaser](https://goreleaser.com/)
