+++
categories = ["setup", "python", "git", "windows"]
date = "2015-12-28T12:08:21-03:00"
description = "How to setup Python 3 and Git on Windows 10 (the hacker-way) using Chocolatey"
keywords = ["setup", "python", "git", "windows"]
title = "Setup Python 3 & Git on Windows 10"
+++

Personally, I don't like installing software using binary installers from the official site, because in the long run you have to deal with incompatibilities and a spreaded development ecosystem.

On systems as OSX or Linux you have well kwonk package-managers like [`brew`](brew.sh) and `apt-get`.

In the case of Windows there is [**Chocolatey**](https://chocolatey.org/).

## Chocolatey

### Installing

We open the `powershell.exe` in **Administrator Mode**.

> You can find it at: `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`

To install, we have to lower the security levels with:

```sh
Set-ExecutionPolicy Unrestricted
```

Let's install it with:

```sh
iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))
```

### Usage

You can install any program listed on [chocolatey.org/packages](https://chocolatey.org/packages) with `choco install`.

Example:

```sh
choco install firefox
```

You can add a `-y` argument to install inmediatly:

```sh
choco install atom -y
```


## Git

To install git it's simply as:

```sh
choco install git.install
```

To start using it, we have to restart the PowerShell or refresh or `$PATH` enviorement variable with:

```sh
$env:Path = [System.Environment]::GetEnvironmentVariable("Path", "User")
```

Now `git` command should work.

```sh
git help
```

### Optional Git Setup

I recomend:

```sh
echo "Identify yourself"
git config --global user.name "NAME LASTNAME"
git config --global user.email email@foo.com

echo "Don't ask again our user and password"
git config --global credential.helper wincred

echo "Colors!"
git config --global color.ui auto
```

## Python 3

```sh
choco install python -y
```

### pip

Pip comes with this installation, you can see it at `C:\tools\python\Scripts`, but it is not added to the `$PATH`.

To do so:

`[Environment]::SetEnvironmentVariable("Path", "$env:Path;C:\tools\python\Scripts\", "User")`

Now refresh the  `$PATH`:

```sh
$env:Path = [System.Environment]::GetEnvironmentVariable("Path", "User")
```

Let's install some common libraries:

```sh
pip install -i https://pypi.binstar.org/carlkl/simple numpy
pip install --upgrade matplotlib
```
