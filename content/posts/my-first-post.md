---
title: "Head and Tail"
date: 2018-01-30T10:27:01-06:00
draft: true
tags: [ "til", "ruby" ]
---

You can grab the head and tail of an array like:

```ruby
some_array = [1, 2, 3]
head, *tail = *some_array
p head # => 1
p tail # => [2, 3]

some_array = [1]
head, *tail = *some_array
p head # => 1
p tail # => []

some_array = []
head, *tail = *some_array
p head # => nil
p tail # => []
```

