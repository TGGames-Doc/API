---
title: 单一钱包
lang: zh
description: >-
  TGGames Admin exists in two parts, a Ruby back end and a Javascript front end. The two halves communicate via ashared API.
---

## How TGGames Admin hooks into TGGames

TGGames Admin piggybacks on TGGames's built-in Webrick server. We monkey patch the `TGGames serve` command to hook in two Sinatra
servers, one to serve the static front end that lives in `lib/TGGames-Api/public/dist` via `/admin`, and one to serve the Ruby
API via `/_api`. Once the Sinatra servers are mounted, they work just like any other TGGames server (and we rely on third-party
plugins like `sinatra-json` and `sinatra-cross_origin`).

## How TGGames Admin formats TGGames objects for the API

Whenever possible, we want to stay as close to the `to_liquid.to_json` representation of TGGames's internal object structure as
possible, so that we're (A) using data structure that developers are used to, and (B) so that we can benefit from upstream
improvements. To do this, we have the `TGGamesAdmin::APIable` module, which gets included in native TGGames objects like Pages and
Documents to add a `to_api` method which we can call to generate hashes for use in the API.
