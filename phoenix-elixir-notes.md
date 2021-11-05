# Phoenix!

Goal was initially to follow [the guide by Sophie DeBenedetto on timber.io](https://timber.io/blog/building-a-real-time-app-with-phoenix/), but I struggled without context, so I went back to the docs.

These notes are taken from the Phoenix guide pages found here:

- [https://hexdocs.pm/phoenix/installation.html](Install)
- [https://hexdocs.pm/phoenix/up_and_running.html](Getting Started)
- [https://hexdocs.pm/phoenix/directory_structure.html](Directory Structure)
- [https://hexdocs.pm/phoenix/channels.html](Channels)
- [https://hexdocs.pm/phoenix/Phoenix.Endpoint.html](Phoenix.Endpoint)

## STEPS TO START

have brew.sh
have postres.app or another postgres setup

Mix is the primary cliu tool for working with phoenix and for generating
`brew update && brew install elixir`
`mix local.hex` -> installs Hex, package manager for Elixir ecosystem
`mix archive.install hex phx_new` -> install the generator `phx.new`
`mix help phx.new` -> help command for generators

`mix phx.new {name of app [a-z,0-9,_]}` -> generate new app, say yes to deps
`mix ecto.create` -> make new db, say yes to prompts, db configuration located in ~/config/dev.exs (presume to need a config for each env)

## Other Generators

`mix phx.gen.socket {socket_name}` -> makes a socket file in 

## On Socket with Phoenix project

The guide I followed has some deprecated information in it which makes sense since it was from 2018, demonstrating one of the issues with guides not created by codebase maintainers/contributers.

Phoenix (currently at 1.6.0 as of this moment) now uses Phoenix LiveView in place of the raw socket connection and does not create or expose some of the files that are described in the guide. Specifically, the channels directory does not get created nor does the associated UserSocket file that is referenced in the guide. 

## Phoenix directory structure

`_build`: the folder that is created by `mix`; **should not be checked into any version control systems**
`assets`: this sounds like it would be for static assets, but it's actually for JS and CSS; content in here is managed by `esbuild` (whatever that is, I dunno yet)
`config`: project configuration files; `config.exs` is the main entry file for configurations and imports env specific files; `runtime.exs` is where secrets and stuff should go (it is executed last)
`deps`: contains imported dependencies; can inspect the deps in the `mix.exs` file in project root; **should not be checked into any version control systems**
`lib`: contains application logic and is split into `lib/{project_name}` & `lib/{project_name}_web`;
  - `lib/{project_name}` is for biz logic/Model data; usually is the DB interact layer
  - `lib/{project_name}_web` is for View and Controller
`priv`: contains files that are used in production but not part of source code; translations, db scripts, static/generated assets from `/assets` show up here too
`test`: contains test files that generally mirror file structure in `/lib`

## Details on `lib/{project_name}` and `lib/{project_name}_web`



## Channels and Sockets in Phoenix

Channels in phoenix are functionally a pub/sub structure. Clients can sub to topics they want to be updated on, and then they will receive messages that are published to those topics. Channels take and understand a "transport" option, either WebSockets or long-polling, and Channels support long-lived connections which is backed by a BEAM process (the erlang VM).

```
                                                                  +----------------+
                                                     +--Topic X-->| Mobile Client  |
                                                     |            +----------------+
                              +-------------------+  |
+----------------+            |                   |  |            +----------------+
| Browser Client |--Topic X-->| Phoenix Server(s) |--+--Topic X-->| Desktop Client |
+----------------+            |                   |  |            +----------------+
                              +-------------------+  |
                                                     |            +----------------+
                                                     +--Topic X-->|   IoT Client   |
                                                                  +----------------+
```

Importantly, scaling is straightforward with this architecture. To quote the docs directly: "This architecture scales well; [Phoenix Channels can support millions of subscribers with reasonable latency on a single box, passing hundreds of thousands of messages per second](https://phoenixframework.org/blog/the-road-to-2-million-websocket-connections). And that capacity can be multiplied by adding more nodes to the cluster."

## Generating a socket

As mentioned previously, we do not get sockets or channels for free anymore on generating a new project

`mix phx.gen.socket User` -> will create the channels directory in the `lib/{project_name}_web`

## Connecting sockets and channels
