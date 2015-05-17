1. Create a new config file in the uber-build/config. The easiest way to do this is to copy the config file of the previous release. The config file contains a `*_GIT_BRANCH` property, which specifies the branch or tag of the corresponding git repo. During the build this branch or tag is checked out and put together. If the repo has been updated since the last release and the plan is to include these updates in the next release, a new tag needs to be specified.

1. After all tags are specified in the uber-build/config file, they need to be set to all git repos. This can either be done manually or with the help of the `mk-git-tags.sh` file. This file should be called in the following way:

```sh
cd /path/to/uber-build
./mk-git-tags.sh config/release-config-file.conf /path/to/scala-ide/base/dir
```

`mk-git-tags.sh` assumes that all repos under the scala-ide organization are located in a base directory. If this is the case if will load the specified config file and add all the tags to the repositories. After that it will upload them to Github. During this process all tags are signed with a GPG key. If such a key does not exist, either create it or create the tags manually without any signing.

1. In order to build the Scala IDE SDK (the Eclipse product), an Eclipse installation is required. Download the "Eclipse IDE for Java Developers" from their [download page](https://www.eclipse.org/downloads/) and extract it somewhere in your filesystem. It is not important which OS dependent archive is downloaded - Eclipse will figure out by itself how to create executables for each OS based on the extracted Eclipse installation.

1. Before uber-build can be run, it is probably necessary to specify some environment variables. All the variables that may be necessary are part of the file `exports.sh`.

1. Run uber-build.

1. Build changelog for the `scala-ide/docs` repo. Run the following script in the scala-ide/scala-ide repo to generate the changelog:

```
cd /path/to/scala-ide/repo
./GenChangeLog.bash <previous-release> <new-release>
```

where `previous-release` and `new-release` are tags. The output of this script should be included in `scala-ide/docs/src/sphinx/changelog.rst`. Note, that the output of this script is not perfect, you may want to manually improve it.

1. Create release notes for the `scala-ide/scala-ide.github.com` repo. See the README of this repo for more information about how to proceed in creating the release notes.

1. Amazon S3: http://www.hongkiat.com/blog/amazon-s3-the-beginners-guide/
