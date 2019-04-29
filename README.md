# Original file is located here: [dword4/nhlapi](https://github.com/dword4/nhlapi)

# NHL API Documentation

All of this has been compiled and tested by hand in Jan of 2018, prior to this most of the information was spread across the internet in various posts and not available in a cohesive
single place.

[OpenAPI 3.0 specification file for the NHL API](https://github.com/erunion/sport-api-specifications/tree/master/nhl) thanks to @[erunion](https://github.com/erunion)

[Teams](#teams)

[Divisions](#divisions)

[Conferences](#conferences)

[People](#people)

[Game-IDs](#game-ids)

[Schedule](#schedule)

[Standings](#standings)

[Standings Types](#standings-types)

[Stats Types](#stats-types)

[Team Stats](#team-stats)

[Draft](#draft)

[Prospects](#prospects)

[Awards](#awards)

---

### <a name="teams"></a>Teams

`GET https://statsapi.web.nhl.com/api/v1/teams` Returns a list of data about
all teams including their id, venue details, division, conference and franchise information.

`GET https://statsapi.web.nhl.com/api/v1/teams/ID` Returns the same information as above just
for a single team instead of the entire league.
#### Modifiers
`?expand=team.roster` Shows roster of active players for the specified team 
`?expand=person.names` Same as above, but gives less info.
`?expand=team.schedule.next` Returns details of the upcoming game for a team
`?expand=team.schedule.previous` Same as above but for the last game played
`?expand=team.stats` Returns the teams stats for the season
`?expand=team.roster&season=20142015` Adding the season identifier shows the roster for that season
`?teamId=4,5,29` Can string team id together to get multiple teams
`?stats=statsSingleSeasonPlayoffs` Speciy which stats to get. Not fully sure all of the values

`GET https://statsapi.web.nhl.com/api/v1/teams/ID/roster` Returns entire roster for a team
including id value, name, jersey number and position details.

---
### <a name="divisions"></a>Divisions
`GET https://statsapi.web.nhl.com/api/v1/divisions`  Returns full list of divisions
and associated data like which conference they belong to, id values and API links.
Does not show inactive divisions

`GET https://statsapi.web.nhl.com/api/v1/divisions/ID` Same as above but only for a
single division. This can show old inactive divisions such as 13 Patrick.

---
### <a name="conferences"></a>Conferences
`GET https://statsapi.web.nhl.com/api/v1/conferences` Returns conference details
for all current NHL conferences.

`GET https://statsapi.web.nhl.com/api/v1/conferences/ID` Same as above but for
specific conference, also can look up id 7 for World Cup of Hockey.

---
### <a name="people"></a>People
`GET https://statsapi.web.nhl.com/api/v1/people/ID` Gets details for a player, must
specify the id value in order to return data.

`GET https://statsapi.web.nhl.com/api/v1/people/ID/stats` Complex endpoint with
lots of append options to change what kind of stats you wish to obtain

#### Modifiers
`?stats=statsSingleSeason&season=19801981`  Obtains single season statistics
for a player
`?stats=homeAndAway&season=20162017` Provides a split between home and away games.
`?stats=winLoss&season=20162017` Very similar to the previous modifier except it provides the W/L/OT split instead of Home and Away
`?stats=byMonth&season=20162017` Monthly split of stats
`?stats=byDayOfWeek&season=20162017` Split done by day of the week
`?stats=vsDivision&season=20162017` Division stats split
`?stats=vsConference&season=20162017` Conference stats split
`?stats=vsTeam&season=20162017` Conference stats split
`?stats=gameLog&season=20162017` Provides a game log showing stats for each game of a season
`?stats=regularSeasonStatRankings&season=20162017` Returns where someone stands vs
the rest of the league for a specific regularSeasonStatRankings
`?stats=goalsByGameSituation&season=20162017` Shows number on when goals for a
player happened like how many in the shootout, how many in each period, etc.
`?stats=onPaceRegularSeason&season=20172018` This only works with the current
in-progress season and shows **projected** totals based on current onPaceRegularSeason

---
### <a name="game"></a>Game
`GET https://statsapi.web.nhl.com/api/v1/game/ID/feed/live` Returns all data about
a specified game id including play data with on-ice coordinates and post-game
details like first, second and third stars and any details about shootouts.  The
data returned is simply too large at often over 30k lines and is best explored
with a JSON viewer.

`GET https://statsapi.web.nhl.com/api/v1/game/ID/boxscore` Returns far less detail
than `feed/live` and is much more suitable for post-game details including goals,
shots, PIMs, blocked, takeaways, giveaways and hits.

`GET http://statsapi.web.nhl.com/api/v1/game/ID/content` Complex endpoint returning
multiple types of media relating to the game including videos of shots, goals and saves.

`GET https://statsapi.web.nhl.com/api/v1/game/ID/feed/live/diffPatch?startTimecode=yyyymmdd_hhmmss`
Returns updates (like new play events, updated stats for boxscore, etc.) for the specified game ID
since the given startTimecode. If the startTimecode param is missing, returns an empty array.

#### <a name="game-ids">Game IDs
The first 4 digits identify the season of the game (ie. 2017 for the 2017-2018 season). The next 2 digits give the type of game, where 01 = preseason, 02 = regular season, 03 = playoffs, 04 = all-star. The final 4 digits identify the specific game number. For regular season and preseason games, this ranges from 0001 to the number of games played. (1271 for seasons with 31 teams (2017 and onwards) and 1230 for seasons with 30 teams). For playoff games, the 2nd digit of the specific number gives the round of the playoffs, the 3rd digit specifies the matchup, and the 4th digit specifies the game (out of 7).

---
### <a name="schedule">Schedule

`GET https://statsapi.web.nhl.com/api/v1/schedule` Returns a list of data about the schedule for a specified date range. If no date range is specified, returns results from the current day.

#### Modifiers
`?expand=schedule.broadcasts` Shows the broadcasts of the game
`?expand=schedule.linescore` Linescore for completed games
`?expand=schedule.ticket` Provides the different places to buy tickets for the upcoming games
`?teamId=30` Limit results to a specific team. Team ids can be found through the teams endpoint
`?date=2018-01-09` Single defined date for the search
`?startDate=2018-01-09` Start date for the search
`?endDate=2018-01-12` End date for the search

`GET https://statsapi.web.nhl.com/api/v1/schedule?teamId=30` Returns Minnesota Wild games for the current day.

`GET https://statsapi.web.nhl.com/api/v1/schedule?teamId=30&startDate=2018-01-02&endDate=2018-01-02` Returns Minnesota Wild games for January 2, 2018 with attached linescores and broadcasts.

---
### <a name="standings">Standings

`GET https://statsapi.web.nhl.com/api/v1/standings` Returns ordered standings data
for each team broken up by divisions


#### Modifiers
`?season=20032004` Standings for a specified season
`?date=2018-01-09` Standings on a specified date
`?expand=standings.record` Detailed information for each team including home and away records, record in shootouts, last ten games, and split head-to-head records against divisions and conferences

---

### <a name="standings-types">Standings Types

`GET https://statsapi.web.nhl.com/api/v1/standingsTypes` Returns all the standings types
to be used in order do get a specific standings

---

### <a name="stats-types">Stats Types

`GET https://statsapi.web.nhl.com/api/v1/statTypes` Returns all the stats types
to be used in order do get a specific kind of player stats

---

### <a name="team-stats">Team Stats

`GET https://statsapi.web.nhl.com/api/v1/teams/5/stats` Returns current season stats and the current season rankings for a specific team

---

### <a name="draft">Draft

`GET https://statsapi.web.nhl.com/api/v1/draft` Get round-by-round data for current year's NHL Entry Draft.

`GET https://statsapi.web.nhl.com/api/v1/draft/YEAR` Takes a YYYY format year and returns draft data

### <a name="prospects">Prospects

`GET https://statsapi.web.nhl.com/api/v1/draft/prospects` Get all NHL Entry Draft prospects.

`GET https://statsapi.web.nhl.com/api/v1/draft/prospects/ID` Get an NHL Entry Draft prospect.


### <a name="awards">Awards

`GET https://statsapi.web.nhl.com/api/v1/awards` Get all NHL Awards.

`GET https://statsapi.web.nhl.com/api/v1/awards/ID` Get an NHL Award.
