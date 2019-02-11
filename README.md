# An (under development) C# library for accessing the Call of Duty API
*Not licensed by or associated with Activision or Call of Duty.*

## Features
* Full Black Ops 4 Support - blackout, mp, zombies
* Get profile information - prestige, rank, xp, and more...
* Get recent matches and stats - map, mode, win/loss, kills, deaths, SPM, K/D Ratio, and more...
* Get leaderboard information - weekly, monthly, alltime, for all game modes

[![NuGet](https://buildstats.info/nuget/codbo4-csharp)](https://www.nuget.org/packages/codbo4-csharp/)

## Install
You can install via the NuGet package manager

```powershell
Install-Package codbo4-csharp
```
Or clone the repository and install dependencies
```sh
git clone https://github.com/mostlyash/codbo4-csharp.git
```

## Usage
You can use this API for free, forever and without any limits.

### Validate User
__Note:__ You don't need to validate user before making a request, although they must be stored on the http://bo4tracker.com/ database.

#### Parameters
* username *string* - Gamertag of the user
* platform - PS4, Xbox One, Steam, Battle.net

#### Code Example
```csharp
using CODBO4;

var user = await Task.Run(() => API.ValidateUser("RandomUsername", Platform.PS4));

if (user.success)
{
    Console.WriteLine("User Id: " + user.uid);
    //...
}
else
{
    //stats not found
}
```

#### Data Example
```json
{
    "success": true,
    "username": "RandomUsername",
    "id": 123456
}
```

### Username by User Id
#### Parameters
* userid *long array* - Containing user ids

#### Code Example
```csharp
var users = await Task.Run(() => API.GetUserById(327154, 396158));

foreach (var user in users)
{
    Console.WriteLine("Username: " + user.username);
}
//...
```

### Get Profile Stats
__Tip:__ While the *userId* parameter is not required, providing it will give you a much faster response. 

#### Parameters
* username *string* - Name of the user
* userId *long* - Id of the user (optional)
* platform - PS4, Xbox One, Steam or Battle.net
* mode - Multiplayer or Blackout

#### Code Example
```csharp
var profile = await Task.Run(() => API.GetProfile("tapxtherace", 326423, Platform.PS4, Mode.Multiplayer));

Console.WriteLine("Username: " + profile.user.username);
Console.WriteLine("Prestige: " + profile.user.stats.prestige);
Console.WriteLine("Level: " + profile.user.stats.level);
//...
```

#### Data Example
```json
{
    "identifier": "abcdefg-hijklmn-opqrstu-xyz1234",
    "user": {
        "id": 326423,
        "username": "tapxtherace",
        "platform": "psn",
        "title": "bo4"
    },
    "stats": {
        "blackoutExtra": {
            "top5placementteam": 1,
            "top10placementteam": 2,
            "top15placementteam": 2,
            "top5placementsolo": 1,
            "top25placementsolo": 3,
            "vehicleescapes": 0,
            "vehiclescavengerair": 0,
            "vehiclescavengerland": 0,
            "vehiclescavengerwater": 0,
            "vehiclesdestroyed": 1,
            "vehicleusedall": 0,
            "vehiclelockexits": 0,
            "vehiclessestroyedoccupied": 1,
            "vehicledamageoccupied": 877,
            "killsvehicledriver": 0,
            "vehicledamage": 877,
            "winswithoutkills": 0,
            "mostkillsinagame": 0,
            "distancetraveledwingsuit": 253554,
            "distancetraveledwingsuitmiles": 4,
            "distancetraveledvehicleland": 347339,
            "distancetraveledvehiclelandmiles": 5,
            "distancetraveledvehicleair": 0,
            "distancetraveledvehicleairmiles": 0,
            "distancetraveledvehiclewater": 30723,
            "distancetraveledvehiclewatermiles": 0,
            "itemspickedup": 461,
            "itemsdropped": 0,
            "killsrevenge": 0
        },
        "multiplayerExtra": [],
        "level": 39,
        "maxlevel": 0,
        "prestige": 5,
        "prestigeid": 0,
        "maxprestige": 0,
        "kills": 17002,
        "killsconfirmed": 70,
        "deaths": 5072,
        "gamesplayed": 422,
        "wins": 295,
        "losses": 127,
        "melee": 12,
        "hits": 45816,
        "misses": 159629,
        "rankxp": 727623,
        "careerscore": 2841495,
        "totalheals": 323270,
        "ekia": 23989,
        "longestkillstreak": 44,
        "curwinstreak": 0,
        "totalshots": 316929,
        "teamkills": 64,
        "suicides": 24,
        "offends": 1289,
        "killsdenied": 101,
        "captures": 741,
        "defends": 1273,
        "timeplayed": 201801,
        "weapondata": "true"
    },
    "matches": [
        {
            "identifier": "abcdefg-hijklmn-opqrstu-xyz1234",
            "kills": 36,
            "deaths": 3,
            "ekia": 43,
            "gamesplayed": 1,
            "wins": 0,
            "losses": 1,
            "totalshots": 405,
            "captures": 0,
            "defends": 0,
            "careerscore": 4375,
            "timeplayed": 263,
            "rankxp": 0,
            "time": 1539869661,
            "format": "2018-10-18 16:34:21"
        }
    ],
    "weapondata": [
        {
            "identifier": "ability_smart_cover",
            "name": "Smart Cover (Ability)",
            "kills": 14,
            "backstabber_kill": 0,
            "deaths": 4,
            "timesused": 0,
            "used": 0,
            "deathsduringuse": 0,
            "hits": 0,
            "ekia": 20,
            "destroyed": 0,
            "headshots": 0,
            "shots": 49,
            "assists": 8,
            "damagedone": 2843
        }
    ]
}
```

### Get User Matches
#### Parameters
* username *string* - Name of the user
* platform - PS4, Xbox One, Steam or Battle.net
* mode - Multiplayer or Blackout

#### Code Example
```csharp
var matches = await Task.Run(() => API.GetUserMatches("username", Platform.PS4, Mode.Multiplayer));

foreach (var match in matches.entries)
{
    Console.WriteLine("Map Name: " + match.mapName);

    Console.WriteLine("Kills: " + match.stats.kills);
    Console.WriteLine("Deaths: " + match.stats.deaths);
}
//...
```

#### Data Example
```json
{
    "success": true,
    "uid": 123456,
    "username": "username",
    "platform": "psn",
    "game": "bo4",
    "type": "mp",
    "matchesCount": 20,
    "entries": [
        {
            "identifier": "8870392559064980469",
            "gameMode": "bounty",
            "gameModeName": "Heist",
            "map": "mp_icebreaker",
            "mapName": "Ice breaker",
            "mapImage": "https://callofdutytracker-public-files.theapinetwork.com/maps/mp_icebreaker.jpg",
            "matchStart": 1541764351,
            "matchEnd": 1541764971,
            "matchWon": 1,
            "CTS": 780,
            "teams": {
                "winningTeam": 2,
                "playerTeam": 2,
                "playerPosition": 5,
                "teamScore": {
                    "team1": 1,
                    "team2": 4
                }
            },
            "stats": {
                "kills": 4,
                "ekia": 3,
                "assists": 0,
                "deaths": 2,
                "highestKillStreak": 3, /*ONLY-MULTIPLAYER*/
                "headshots": 0,
                "shotsFired": 96,
                "shotsLanded": 21,
                "shotsMissed": 75
            },
            "formatForSite": "2018-11-09 13:02:51",
            "privateMatch": false
        },
    ]
}
```

### Get Recent Matches
#### Parameters
* rows *integer* - Number of rows to fetch

**Currently only allows for requesting up to the first 100 rows of players.**

#### Code Example
```csharp
var matches = await Task.Run(() => API.GetRecentMatches(50));

foreach (var match in matches.entries)
{
    Console.WriteLine("Map Id: " + match.matchInfo.matchMapId);

    foreach (var player in match.playerEntries)
    {
        Console.WriteLine("Player Id: " + player.uid);
        Console.WriteLine("Kills: " + player.kills);
    }
}
//...
```

#### Data Example
```json
{
    "success": true,
    "rows": 1,
    "game": "bo4",
    "platform": "psn",
    "entries": [
        {
            "mid": "10443371133280017458",
            "utcStart": 1541003893,
            "utcEnd": 1541004262,
            "matchInfo": {
                "matchDuration": 369,
                "matchType": "mp",
                "matchMapId": "mp_hacienda",
                "matchMode": "tdm"
            },
            "teams": {
                "teamScore": {
                    "team1": 65,
                    "team2": 75
                },
                "winningTeam": 2
            },
            "playerEntries": [
                {
                    "uid": 327154,
                    "prestige": 2,
                    "rank": 46,
                    "team": 1,
                    "position": 2,
                    "kills": 20,
                    "deaths": 12,
                    "ekia": 23,
                    "highestKillStreak": 3,
                    "assists": 3,
                    "headshots": 0,
                    "shotsFired": 428,
                    "shotsLanded": 106,
                    "shotsMissed": 322
                },
                {
                    "uid": 396158,
                    "prestige": 1,
                    "rank": 50,
                    "team": 2,
                    "position": 5,
                    "kills": 12,
                    "deaths": 8,
                    "ekia": 16,
                    "highestKillStreak": 5,
                    "assists": 4,
                    "headshots": 0,
                    "shotsFired": 271,
                    "shotsLanded": 82,
                    "shotsMissed": 189
                }
            ]
        }
    ]
}
```

### Get Match
#### Parameters
* matchId *long* - Id of the match to fetch

#### Code Example
```csharp
var matches = await Task.Run(() => API.GetMatch(10443371133280017458));

foreach (var match in matches.entry)
{
    Console.WriteLine("Map Id: " + match.matchInfo.matchMapId);

    foreach (var player in match.playerEntries)
    {
        Console.WriteLine("Player Id: " + player.uid);
        Console.WriteLine("Kills: " + player.kills);
    }
}
//...
```

#### Data Example
```json
{
    "success": true,
    "game": "bo4",
    "platform": "psn",
    "entry": [
        {
            "mid": "10443371133280017458",
            "utcStart": 1541003893,
            "utcEnd": 1541004262,
            "matchInfo": {
                "matchDuration": 369,
                "matchType": "mp",
                "matchMapId": "mp_hacienda",
                "matchMode": "tdm"
            },
            "teams": {
                "teamScore": {
                    "team1": 65,
                    "team2": 75
                },
                "winningTeam": 2
            },
            "playerEntries": [
                {
                    "uid": 327154,
                    "prestige": 2,
                    "rank": 46,
                    "team": 1,
                    "position": 2,
                    "kills": 20,
                    "deaths": 12,
                    "ekia": 23,
                    "highestKillStreak": 3,
                    "assists": 3,
                    "headshots": 0,
                    "shotsFired": 428,
                    "shotsLanded": 106,
                    "shotsMissed": 322
                },
                {
                    "uid": 396158,
                    "prestige": 1,
                    "rank": 50,
                    "team": 2,
                    "position": 5,
                    "kills": 12,
                    "deaths": 8,
                    "ekia": 16,
                    "highestKillStreak": 5,
                    "assists": 4,
                    "headshots": 0,
                    "shotsFired": 271,
                    "shotsLanded": 82,
                    "shotsMissed": 189
                }
            ]
        }
    ]
}
```

#### Data Example
```json
[
    {
        "uid": 327154,
        "username": "Jedi_340_VI",
        "platform": "psn",
        "game": "bo4"
    },
    {
        "uid": 396158,
        "username": "MOST-Nebula",
        "platform": "psn",
        "game": "bo4"
    }
]
```

### Get Leaderboard Data
#### Parameters
* platform - PS4, Xbox One, Steam or Battle.net
* scope - Kills, Deaths, Ekia, Wins, Losses, Games Played, Time Played
* rows *integer* - Number of rows to fetch

**Currently only allows for requesting up to the first 100 rows of players.**

#### Code Example
```csharp
var data = await Task.Run(() => API.GetLeaderboard(Platform.PS4, Scope.Kills, 50));

foreach (var user in data.entries)
{
    Console.WriteLine(user.username);
    Console.WriteLine(user.kills);
}
//...
```

#### Data Example
```json
{
    "rows": 100,
    "platform": "all",
    "scope": "kills",
    "entries": [
        {
            "username": "DooMTheRace",
            "platform": "psn",
            "level": {
                "id": 27,
                "image": "https://callofdutytracker-public-files.theapinetwork.com/assets/bo4/ranking/rank_27.png"
            },
            "prestige": {
                "id": 4,
                "image": "https://callofdutytracker-public-files.theapinetwork.com/assets/bo4/prestiges/prestige_4.png"
            },
            "kills": 22490,
            "deaths": 7094,
            "ekia": 24948,
            "wins": 486,
            "losses": 129,
            "gamesplayed": 615,
            "timeplayed": 278477
        },
        {
           ...
        }
    ]
}
```

# License
This project is licensed under the General Public License v3.
