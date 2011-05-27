**offline-gists** is a simple Ruby Rakefile that makes it easy to keep
offline copies (i.e. backups) of [gists](https://gist.github.com/).

## Why?

1. **Paranoia** -- what happens if github loses my critical gists?
2. **Offline access** -- not all airplanes have wifi

## Requirements

* Ruby 1.9.2
* Rubygems
  * `bundler` gem
  * `git` gem
  * `psych` gem

## Getting started

Google will help you get Ruby and Rubygems installed if you don't have
them already. (You can check with these commands: `ruby -v` and
`gem -v`.) Then:

0. Run `bundle`
1. Clone this repository (`git clone git://github.com/masnick/offline-gists.git`)
2. Rename `gists/manifest_example.yaml` to `gists/manifest.yaml` and add
   your gists.
3. From the root folder of the repository, run `rake`. You should see
   your gists cloned into the `gists/` subfolder.

## manifest.yaml

The format is simple YAML:

    ---
    'local name for gist': 478d9e3cd926028c91f6
    'local name for another gist': 901651

You can have as many gists as you want in here.

The name for each gist is just used for naming the folder of the local
repository. Spaces are converted into dashes and all non-alphanumeric
characters are removed.

## Multiple manifest files

You can have as many folders as you want at the same level as `gists/`.
Each of these can have a `manifest.yaml` file. The rake task defaults to
the `gists/` folder, but you can run `rake run[foo]` to use the manifest file
in the `foo/` folder.

## Sharing your manifest

This is easy! Just make your `gists/` or other folder a git repository.

    cd gists
    git init

Then add the following to `gists/.gitignore`:

    *
    !.gitignore
    !manifest.yaml

Then make your initial commit:

    git add -A
    git ci -am "initial commit"

Now you can push/pull this repository as you would normally. **Note:**
if you share this repository publicly and it includes private gists,
these gists will be accessible to anyone who looks at the manifest file!


## Copying

All rights reserved. Feel free to use for non-commercial or personal projects.
For other uses, please contact me via [my website](http://max.masnick.me).
