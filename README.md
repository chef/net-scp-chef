# Net::SCP

* Issues: https://github.com/chef/net-scp-chef/issues
* Codes: https://github.com/chef/net-scp-chef/

Net::SCP is a pure-Ruby implementation of the SCP protocol. This operates over SSH (and requires the Net::SSH library), and allows files and directory trees to be copied to and from a remote server.

## About this Fork

This is a fork of https://github.com/net-ssh/net-scp/ that has been mildly cleanup up, but more importantly allows for the usage of net-ssh 5.x. We commit to investing in the continued maintenance of this gem for its existing use case.

## Features

* Transfer files or entire directory trees to or from a remote host via SCP
* Can preserve file attributes across transfers
* Can download files in-memory, or direct-to-disk
* Support for SCP URI's, and OpenURI

## Synopsis

In a nutshell:

  require 'net/scp'

  # upload a file to a remote server
  Net::SCP.upload!("remote.host.com", "username",
    "/local/path", "/remote/path",
    :ssh => { :password => "password" })

  # download a file from a remote server
  Net::SCP.download!("remote.host.com", "username",
    "/remote/path", "/local/path",
    :ssh => { :password => "password" })

  # download a file to an in-memory buffer
  data = Net::SCP::download!("remote.host.com", "username", "/remote/path")

  # use a persistent connection to transfer files
  Net::SCP.start("remote.host.com", "username", :password => "password") do |scp|
    # upload a file to a remote server
    scp.upload! "/local/path", "/remote/path"

    # upload from an in-memory buffer
    scp.upload! StringIO.new("some data to upload"), "/remote/path"

    # run multiple downloads in parallel
    d1 = scp.download("/remote/path", "/local/path")
    d2 = scp.download("/remote/path2", "/local/path2")
    [d1, d2].each { |d| d.wait }
  end

  # You can also use open-uri to grab data via scp:
  require 'uri/open-scp'
  data = open("scp://user@host/path/to/file.txt").read

For more information, see Net::SCP.

## Requirements

* Net::SSH

## Install

* gem install net-scp-chef (might need sudo privileges)

## License

(The MIT License)

Copyright (c) 2008 Jamis Buck <jamis@37signals.com>
Copyright (c) 2019 Chef Software

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
