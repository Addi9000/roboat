<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:E3342F,50:7f0000,100:1a0000&height=260&section=header&text=⛵%20roboat&fontSize=80&fontColor=ffffff&animation=fadeIn&fontAlignY=42&desc=python%20%C2%B7%20roblox%20%C2%B7%20built%20different&descAlignY=63&descAlign=50&descColor=ffcccc&descSize=18" width="100%"/>

<br/>

<img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=700&size=18&pause=1000&color=E3342F&center=true&vCenter=true&width=600&lines=⛵+sail+through+the+roblox+api;no+cookie.+no+hassle.+just+oauth.;async+%2B+sync.+your+choice.;datastores%2C+bans%2C+messaging+via+open+cloud;rap+tracking+%26+market+analysis+built+in;real-time+events+that+actually+work;a+terminal+that+slaps" alt="Typing SVG" />

<br/><br/>

[![Python](https://img.shields.io/badge/python-3.8%2B-E3342F?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![MIT](https://img.shields.io/badge/license-MIT-991B1B?style=for-the-badge)](LICENSE)
[![Version](https://img.shields.io/badge/version-2.1.0-E3342F?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Addi9000/roboat/releases)
[![Site](https://img.shields.io/badge/roboat.pro-visit-7f0000?style=for-the-badge&logo=googlechrome&logoColor=white)](https://roboat.pro)
[![Stars](https://img.shields.io/github/stars/Addi9000/roboat?style=for-the-badge&color=E3342F&logo=github&logoColor=white&label=stars)](https://github.com/Addi9000/roboat/stargazers)

<br/>

> **roboat** is a Python library for the Roblox API that doesn't suck.
> OAuth auth, typed responses, async support, a built-in database, Open Cloud integration,
> marketplace tools, and a terminal you'll actually want to use.

<br/>

</div>

---

## install

```bash
pip install roboat

# need async? add aiohttp
pip install "roboat[async]"

# want everything
pip install "roboat[all]"
```

---

## the basics

```python
from roboat import RoboatClient

client = RoboatClient()

# look up a user
user = client.users.get_user(156)
print(user)  # Builderman (@builderman) ✓ [ID: 156]

# get game stats
game = client.games.get_game(2753915549)
print(f"{game.name} — {game.visits:,} visits, {game.playing:,} playing right now")

# search the catalog
for item in client.catalog.search(keyword="fedora", sort_type="Sales"):
    print(f"{item.name} — {item.price}R$")
```

No setup required. No cookies to extract. Just works.

---

## auth

roboat uses **Roblox OAuth 2.0**. You open a browser, you log in, done.

```python
from roboat import OAuthManager, RoboatClient

manager = OAuthManager(timeout=120)
token = manager.authenticate()  # opens browser, waits up to 120 seconds

client = RoboatClient(oauth_token=token)
```

Or from the terminal — just type `auth` and your browser opens automatically.
You get a **live countdown** while it waits.

```
» auth
  ℹ  Opening Roblox authorization page...
  Waiting... 117s remaining
  ✔  Authentication successful!
```

---

## the terminal

```bash
roboat
```

```
  ____       _     _            _    ____ ___
 |  _ \ ___ | |__ | | _____  __/ \  |  _ \_ _|
 | |_) / _ \| '_ \| |/ _ \ \/ / _ \ | |_) | |
 |  _ < (_) | |_) | | (_) >  < ___ \|  __/| |
 |_| \_\___/|_.__/|_|\___/_/\_/_/  \_|_|  |___|

  roboat v2.1.0  ·  roboat.pro

» start 156
» game 2753915549
» rap 156
» friends 156
» inventory 156
» catalog fedora
```

Every command that needs data requires `start <userid>` first. If you forget, it tells you.

| command | what it does |
|---|---|
| `start <id>` | begin session as a user |
| `auth` | oauth login via browser |
| `game <id>` | game stats + votes |
| `friends <id>` | friends list |
| `inventory <id>` | limiteds + RAP |
| `rap <id>` | total RAP calculation |
| `catalog <kw>` | search avatar shop |
| `presence <id>` | online status |
| `trades` | your trade list |
| `servers <placeid>` | active servers |
| `universe <id>` | dev universe info |
| `newdb / loaddb` | local database |
| `watch <id>` | visit milestone alerts |

---

## clients

### sync

```python
from roboat import RoboatClient, ClientBuilder

# builder pattern if you want more control
client = (
    ClientBuilder()
    .set_oauth_token("TOKEN")
    .set_timeout(15)
    .set_cache_ttl(60)     # cache responses for 60s
    .set_rate_limit(10)    # max 10 req/s built in
    .build()
)
```

### async

```python
import asyncio
from roboat import AsyncRoboatClient

async def main():
    async with AsyncRoboatClient() as client:

        # fire multiple requests at the same time
        game, votes, icon = await asyncio.gather(
            client.games.get_game(2753915549),
            client.games.get_votes([2753915549]),
            client.thumbnails.get_game_icons([2753915549]),
        )

        # fetch 500 users — auto splits into batches of 100
        users = await client.users.get_users_by_ids(list(range(1, 501)))

asyncio.run(main())
```

### open cloud

```python
from roboat import RoboatCloudClient

cloud = RoboatCloudClient(api_key="roblox-KEY-xxxxx")
```

---

## what's covered

```
users        friends      games        catalog
groups       badges       economy      presence
avatar       trades       messages     inventory
thumbnails   develop      opencloud    analytics
marketplace  social       notifications publish
moderation   events       database     session
```

That's 24 modules. Every major Roblox API endpoint is in here somewhere.

---

## groups

```python
group = client.groups.get_group(7)
roles = client.groups.get_roles(7)

# stuff that needs auth
client.groups.set_member_role(7, user_id=1234, role_id=role.id)
client.groups.kick_member(7, user_id=1234)
client.groups.post_shout(7, "hey everyone")
client.groups.accept_all_join_requests(7)   # yes, all of them
client.groups.pay_out(7, user_id=1234, amount=500)
```

---

## marketplace & economy

```python
from roboat.marketplace import MarketplaceAPI

market = MarketplaceAPI(client)

# limited item data — RAP, trend, supply
data = market.get_limited_data(1365767)
print(f"{data.name}  {data.recent_average_price:,}R$  {data.price_trend}")

# is it worth buying to flip?
profit = market.estimate_resale_profit(1365767, purchase_price=12000)
print(f"net: {profit.estimated_profit:,}R$  roi: {profit.roi_percent}%")

# snapshot RAP over time and see what changed
tracker = market.create_rap_tracker([1365767, 1028606])
tracker.snapshot()
# ... later ...
tracker.snapshot()
print(tracker.summary())

# find limiteds selling below RAP
deals = market.find_underpriced_limiteds([1365767, 1028606, 19027209])
```

---

## open cloud — developer tools

```python
API_KEY  = "roblox-KEY-xxxxx"
UNIVERSE = 123456789

# datastores
client.develop.set_datastore_entry(UNIVERSE, "PlayerData", "p_1", {"coins": 500}, API_KEY)
client.develop.get_datastore_entry(UNIVERSE, "PlayerData", "p_1", API_KEY)
client.develop.increment_datastore_entry(UNIVERSE, "Stats", "deaths", 1, API_KEY)

# leaderboards (ordered datastores)
client.develop.list_ordered_datastore(UNIVERSE, "Leaderboard", API_KEY, max_page_size=10)
client.develop.set_ordered_datastore_entry(UNIVERSE, "Leaderboard", "p_1", 9500, API_KEY)

# message every live server instantly
client.develop.announce(UNIVERSE, API_KEY, "double xp starts now!")
client.develop.broadcast_shutdown(UNIVERSE, API_KEY)

# bans
client.develop.ban_user(UNIVERSE, 1234, API_KEY,
    duration_seconds=86400,
    display_reason="you got banned lol",
    private_reason="exploiting detected")
client.develop.unban_user(UNIVERSE, 1234, API_KEY)
```

---

## events

```python
from roboat import EventPoller

poller = EventPoller(client, interval=30)

@poller.on_friend_online
def on_online(user):
    print(f"{user.display_name} just came online")

@poller.on_new_friend
def on_friend(user):
    print(f"new friend: {user.display_name}")

# alert when a game hits a visit milestone
poller.track_game(2753915549, milestone_step=1_000_000)

@poller.on("visit_milestone")
def on_milestone(game, count):
    print(f"{game.name} hit {count:,} visits")

poller.start()  # runs in the background
```

---

## database

There's a built-in SQLite layer for persisting data between runs.

```python
from roboat import SessionDatabase

db = SessionDatabase.load_or_create("myproject")

# save api objects directly
db.save_user(client.users.get_user(156))
db.save_game(client.games.get_game(2753915549))

# key-value store for anything
db.set("tracked_ids", [156, 261, 1234])
db.set("last_run", "2024-06-01")

print(db.stats())
# {'users': 10, 'games': 5, 'session_keys': 3, 'log_entries': 42}
```

Useful for inventory diff tracking, caching expensive lookups, or storing config across runs.

---

## pagination

```python
from roboat.utils import Paginator

# lazily iterate all followers — auto fetches every page
for follower in Paginator(
    lambda c: client.friends.get_followers(156, limit=100, cursor=c)
):
    print(follower)

# or just grab the first 500
top = Paginator(
    lambda c: client.friends.get_followers(156, limit=100, cursor=c),
    max_items=500,
).collect()
```

---

## errors

```python
from roboat import UserNotFoundError, RateLimitedError, RoboatAPIError
from roboat.utils import retry

# auto retry on rate limit with backoff
@retry(max_attempts=3, backoff=2.0)
def get(uid):
    return client.users.get_user(uid)

try:
    user = get(99999999999)
except UserNotFoundError:
    print("doesn't exist")
except RateLimitedError:
    print("slow down")
except RoboatAPIError as e:
    print(e)
```

---

## tools

```bash
# bulk fetch users → csv or json
python tools/bulk_lookup.py --ids 1 156 261 --format csv
python tools/bulk_lookup.py --usernames Roblox builderman --format json

# snapshot a user's RAP and diff on next run
python tools/rap_snapshot.py --user 156
python tools/rap_snapshot.py --user 156 --diff

# live game stats with milestone alerts
python tools/game_monitor.py --universe 2753915549 --interval 60

# check your environment
python scripts/check_env.py
```

---

## integrations

| | file | what |
|---|---|---|
| 🤖 | `integrations/discord_bot.py` | slash commands: `/user` `/game` `/status` |
| 🌐 | `integrations/flask_api.py` | REST wrapper for all major endpoints |

---

## social graph

```python
from roboat.social import SocialGraph

sg = SocialGraph(client)

mutuals   = sg.mutual_friends(156, 261)
snap      = sg.presence_snapshot([156, 261, 1234])
online    = sg.who_is_online([156, 261, 1234])
nodes     = sg.most_followed_in_group([156, 261, 1234])
suggests  = sg.follow_suggestions(156, limit=10)
```

---

## license

```
MIT License — Copyright (c) 2024 roboat contributors

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the "Software"),
to deal in the Software without restriction, including without limitation
the rights to use, copy, modify, merge, publish, distribute, sublicense,
and/or sell copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included
in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.
```

---

<div align="center">

<br/>

[![roboat.pro](https://img.shields.io/badge/🌐%20roboat.pro-E3342F?style=for-the-badge)](https://roboat.pro)
&nbsp;
[![github](https://img.shields.io/badge/github-Addi9000%2Froboat-1a0000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Addi9000/roboat)
&nbsp;
[![issues](https://img.shields.io/badge/issues-report%20a%20bug-991B1B?style=for-the-badge)](https://github.com/Addi9000/roboat/issues)

<br/>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:1a0000,50:7f0000,100:E3342F&height=140&section=footer&text=⛵&fontSize=40&fontColor=ffffff&animation=fadeIn&fontAlignY=65" width="100%"/>

</div>
