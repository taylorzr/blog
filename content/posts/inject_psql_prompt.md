---
title: "Inject psql prompt"
date: 2019-03-06T12:55:24-06:00
draft: false
tags: [shell, postgres, direnv]
---

I've been working with databases quite a bit recently. Usually these databases are temporary forks
used for testing, and they only have an ip address. It gets hard to keep track of which ip is which,
and I'm always worried running the wrong command on an actual production database.

So I came up with this shell function `db`, used like `db $ZACH_DB_URL`. When run this connects psql
to that database, and give me a prompt like `ZACH@127.0.0.1 > _`. This makes it obvious what database
I'm connected to. This is how it works.

Simple psql prompt:
```sql
-- ~/.psqlrc
-- `prompt` gets passed in via the shell function `db`
\set PROMPT1 '%:prompt:@%M > '
```

Shell function:
```sh
# ~/.zshrc
function db() {
  local url_var prompt url
  url_var=$(env | grep _DB_URL | grep "$1" | cut -d '=' -f 1)
  prompt=$(echo $url_var | cut -d '=' -f 1 | sed 's/_DB_URL//g')
  url=$(print -rl -- ${(P)url_var})
  psql -v "prompt=$prompt" "$url"
}
```

I used direnv for setting envs in specific project directories. Most of the connection information
is the same, so I just set host, and loop over each host exporting env vars. The env vars must end
in `_DB_URL` for the shell function to pick them up.

```sh
# ~/code/<some-project/.envrc

fork_prefix='postgres://<user>:<password>@'
fork_suffix=':5432/<database>'

declare -A databases
databases[ZACH_1]='127.0.0.1'
databases[ZACH_2]='127.0.0.2'
databases[ZACH_3]='127.0.0.3'

for key in "${!databases[@]}"; do
  export "${key}_DB_URL=${fork_prefix}${databases[$key]}${fork_suffix}"
done

# Can also just set full urls
export ZACH_DB_URL='postgres://localhost'
```

Usage:
```sh
$ db $ZACH_DB_URL
psql (9.4.11, server 9.4.11)
Type "help" for help.

ZACH@localhost > select 'Hello World!' as greeting;
   greeting
--------------
 Hello World!
(1 row)

ZACH@localhost >
```
