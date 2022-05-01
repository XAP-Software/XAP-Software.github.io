---
layout: post
title: Error - Cannot load such file - Ruby Gem bundler
categories: [Ruby, Gem]
author_github: kh4rit
author: Vasiliy Kharitonov
---

You received an error upon command `bundle install` similar to the one below:

``` bash
/.../ruby/bin/bundle:25:in `load': cannot load such file -- /.../ruby/gems/3.1.0/gems/bundler-2.3.11/exe/bundle (LoadError)
	from /.../ruby/bin/bundle:25:in `<main>'
```

This post might explain you how to fix it.

## Don't install gems with sudo

This is actually a prerequisite, but I will still mention it. You don't want
your gems to be installed by a super user. First, it causes problems, and
second, it is not safe. I am trying to use most of my programs without sudo and
it usually works out.

There is a number of ways to install Ruby without sudo, e.g. you can use
Homebrew on macOS:

``` console
$ brew install ruby
```

Note that you must first install [Homebrew](https://brew.sh).

## Fill out the missing version

The error happens when you install a bundle that is linked to a bundler version
that was removed (e.g. updated).

The easiest way to fix this is to install the specific version of bundler that is missing:

``` console
$ gem install bundler -v 2.3.11
```

Then you can run `bundle install` and it should run without the error:

``` console
$ bundle install
Bundle complete! 2 Gemfile dependencies, 102 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
```

Afterwards you can even remove the old version as the bundle will now use the
latest one you have:

``` console
$ gem uninstall bundler -v 2.3.11
```
