+++
categories = ["c", "setup", "osx"]
date = "2015-12-29T22:44:57-03:00"
description = ""
keywords = ["c", "setup", "osx"]
title = "C Development Environment with Atom on OSX"
+++

There is two kind of people: You love `vim` or you hate `vim`.

Therefore I use [Atom](https://atom.io/) 💗.

## Install Atom

Use [Homebrew](http://brew.sh/)!

```sh
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Then, install [Homebrew Cask](http://caskroom.io/)

```sh
brew tap caskroom/cask
```

Finally, install Atom:

```sh
brew cask install atom
```

## Plugins

### [autocomplete-clang](https://atom.io/packages/autocomplete-clang)

We will install `autocomplete-clang`, but first we need to install `clang`:

```sh
brew install llvm
```

See if `clang` was installed:

```sh
clang -v
# Apple LLVM version 7.0.2 (clang-700.1.81)
# Target: x86_64-apple-darwin15.2.0
# Thread model: posix
```

Then, install the package:

```sh
apm install autocomplete-clang
```

### [atom-beautify](https://atom.io/packages/atom-beautify)

To format our C code (and C++, Objective-C, etc) we need [uncrustify](http://sourceforge.net/projects/uncrustify).

```sh
brew install uncrustify
```

Then:

```
apm install uncrustify
```

That's it! 🎉

{{% figure src="/images/c-development.png"%}}

> My theme is: [Atom Material](https://atom.io/themes/atom-material-ui) + [DuoTone dark](https://atom.io/themes/duotone-dark-syntax).
