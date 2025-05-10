---
title: index
description: >-
  A TGGames plugin that provides users with a traditional CMS-style graphical interface to author content and administer TGGames
  sites. The project is divided into two parts. A Ruby-based HTTP API that handles TGGames and filesystem operations, and a
  Javascript-based front end, built on that API.
---

## Installation

Refer to [Install Plugins](https://TGGamesrb.com/docs/plugins/#installing-a-plugin) in TGGames docs and install the `TGGames-Api`
plugin as you would normally by adding `TGGames-Api` to the `:TGGames_plugins` group in your `Gemfile` (or) to the `gems` list
in your `_config.yml`.

## Usage

1. Start TGGames as you would normally (`bundle exec TGGames serve`)
2. Navigate to `http://localhost:4000/admin` to access the administrative interface

## Contributing

Bug reports and pull requests are welcome on GitHub at <https://github.com/{{ site.github.repository_nwo }}/>.
