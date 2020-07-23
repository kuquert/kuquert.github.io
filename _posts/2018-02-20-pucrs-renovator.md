---
layout: post
title: Renovator PUC-RS
categories: blog
intro: O app pra renovar automagimente seus livros da biblioteca pucrs.
---

### What is an OTP aplication??
It refers to the --app option on `phx.new`  command.

list of commands
```shell
mkdir my_api | cd my_api # creates a folder to store the project
mix phx.new . --no-html # creates a new phoenix project
mix phx.server # NOOOOO, error appears.
```
[error image here]
```shell
mix deps.get

mix ecto.create
```

New we need a machine withg the configurations to run our new server.
For that we use docker, with a `docker-compose.yml` file.

Resources:
https://github.com/rbeene/phoenix-with-docker