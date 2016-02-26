# Downloading the Source

**BEFORE YOU READ THIS GUIDE PLEASE MAKE SURE YOU CAN ACCESS GOOGLE！！！！！**

The Android-x86 source tree is located in a Git repository hosted by Android-x86. The Git repository includes metadata for the Android source, including those related to changes to the source and the date they were made. This document describes how to download the source tree for a specific Android-x86 code-line.

## Installing Repo

Repo is a tool that makes it easier to work with Git in the context of Android-x86. For more information about Repo, see the [Google Developing section](https://source.android.com/source/developing.html).

To install Repo:

1. Make sure you have a bin/ directory in your home directory and that it is included in your path:

 ```
$ mkdir ~/bin
$ PATH=~/bin:$PATH
 ```

2. Download the Repo tool and ensure that it is executable:

 ```
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
$ chmod a+x ~/bin/repo
 ```

## Initializing a Repo client

After installing Repo, set up your client to access the Android-x86 source repository:

1. Create an empty directory to hold your working files. Give it any name you like:

 ```
 $ mkdir WORKING_DIRECTORY
 $ cd WORKING_DIRECTORY
 ```

2. Run repo init to bring down the latest version of Repo with all its most recent bug fixes. You must specify a URL for the manifest, which specifies where the various repositories included in the Android-x86 source will be placed within your working directory.
 ```
$ repo init -u git://gitscm.sf.net/gitroot/android-x86/manifest -b $branch
 ```

 You can use "lollipop-x86" or "marshmallow-x86" to instead "$branch" to get the source you want. There is also some other choices, you can read [the branches in Android-x86 tree](http://www.android-x86.org/getsourcecode) to get more information.

## Downloading the Android-x86 Source Tree

To pull down the Android-x86 source tree to your working directory from the repositories as specified in the default manifest, run
 ```
repo sync
 ```
The Android-x86 source files will be located in your working directory under their project names. The initial sync operation will take an hour or more to complete. If your network broken, re-run repo sync will resume the broken download.
