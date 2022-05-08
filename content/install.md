# Installation

ApacheExpress should install fine on pretty much any Unix system that can run
Swift and Apache 2.4. Including exotic setups like Raspberry Pi systems.

We also provide a macOS Homebrew tap which makes it really easy to install
ApacheExpress and its dependencies on macOS. We highly recommend that over a
custom install.

On the Linux side we test w/ Ubuntu Trusty and Xenial, though it should work
pretty much anywhere.

## Install on macOS using Homebrew

Got no Homebrew? [Get it!](https://brew.sh)

Install Apache2:

    brew install httpd

If PostgreSQL access is needed, APRUtil needs to be installed manually,
it is not available as part of Homebrew anymore.

Then add the mod_swift tap and install ApacheExpress:

    brew tap modswift/mod_swift
    brew install apacheexpress

*(yes, the account is just modswift w/o underscore due to GitHub limitations)*

## Install on Linux (or macOS w/o Homebrew)


On macOS: We strongly advise that you rather use Homebrew, more importantly
          the Apache provided by Homebrew.

Ubuntu packages required (assuming you have Swift 3 installed already), this includes
the PostgreSQL and SQLite3 database adaptors, add additional ones as desired:

    sudo apt-get update
    sudo apt-get install \
       curl pkg-config libapr1-dev libaprutil1-dev \
       libxml2 apache2 apache2-dev \
       libnghttp2-dev \
       libaprutil1-dbd-sqlite3 \
       libaprutil1-dbd-pgsql

Install mod_swift:

    curl -L -o mod_swift.tgz \
         https://github.com/modswift/mod_swift/archive/0.9.2.tar.gz
    tar zxf mod_swift.tgz && cd mod_swift-0.9.2
    make
    sudo make install

Install ApacheExpress:

> WORK IN PROGRESS, STAY TUNED

    curl -L -o apacheexpress.tgz \
         https://github.com/ApacheExpress/ApacheExpress/archive/TODO.tar.gz
    tar zxf apacheexpress.tgz && cd apacheexpress-TODO
    make debug=yes
    make debug=no
    sudo make debug=yes install
    sudo make debug=no  install

Those put things into `/usr/local`. If you want to have it in `/usr`, do:

    sudo make prefix=/usr install

## Check whether the installation is OK

You can call `swift apache validate` to make sure the Apache installation is OK:

```
$ swift apache validate
swift-driver version: 1.45.2 The Swift Apache build environment looks sound.

  srcroot:   /Users/helge/tmp/mods_helloworld
  module:    mods_helloworld
  config:    debug
  product:   /Users/helge/tmp/mods_helloworld/.build/mods_helloworld.so
  apxs:      /opt/homebrew/bin/apxs
  moddir:    /opt/homebrew/lib/httpd/modules
  relmoddir: /
  mod_swift: /opt/homebrew/opt/mod_swift
  swift:     5.6.0
  cert:      self-signed-mod_swift-localhost-server.crt
  http/2:    yes
```

## Troubleshooting

If something isn't working in a Homebrew setup, check whether:

    brew doctor

outputs anything unusual.

If you need any help, feel free to ask on the
[Mailing List](https://groups.google.com/d/forum/mod_swift)
or our
[Slack channel](http://slack.noze.io).
