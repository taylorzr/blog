---
title: "![Sand]whiches"
date: 2019-02-14T09:31:52-06:00
draft: false
tags: [shell, ruby]
---

`which` is a useful tool to find the path of an executable, eg:

```sh
$ which ruby
/Users/ztaylo43/.rubies/ruby-2.4.4/bin/ruby
```

`which` also has an `-a` flag which shows all executables, eg:
```sh
$ which -a ruby
/Users/ztaylo43/.rubies/ruby-2.4.4/bin/ruby
/usr/bin/ruby
```

And ruby provides the same for gems, eg:
```sh
$ gem which rspec
/Users/ztaylo43/.gem/ruby/2.4.4/gems/rspec-3.8.0/lib/rspec.rb
```

This can be super helpful for those times when you're debugging, and desperation leads you to follow
the stacktrace into gemland.
