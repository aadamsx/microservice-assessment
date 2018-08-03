# Microservice Assessment

In the following assessment, we will break down the process build a microservice using WebAPI and ASP.NET Core.

## Expectations

- You'll be building a leaderboard, showing the top 10 players and their total score.
- You should use an in-memory data store to handle the matches and scores.
- Expected Classes that you'll author: `Match`, `Leaderboard`. Leaderboard be composed of a list of matches and calculate the leaderboard.
- Use ASP.NET Core

The API Calls we'll run against your API are the following, with definitions below.

```
POST /api/leaderboard/reset
POST /api/match
PUT /api/match/:id
GET /api/leaderboard
```

## POST `/api/leaderboard/reset`

- Will reset the scoreboard
- Effectively called at the start of a new day

### headers

```
N/A
```

### body

```
None
```

### Result

Status: 201

```
{
  leaderboard: {
    createdAt: 1533298622
  }
}
```

## POST `/api/match`

- Creates a match

### headers

```
{
  ContentType: "application/json",
  Accept: "application/json"
}
```

### body

The potentialPoints is calculated by adding the two levels of the opponents together, and multiplying that by 10.

So a level 2 and a level 4 is a 60 point match. Two level 20's is a 400 point match.

example inputs:

- `player`: "loreweaver"
- `level`: 11
- `opponentName`: "darksbane"
- `opponentLevel`: 16

```
{
  match: {
    player: "$playerName",
    level: $playerLevel,
    opponentName: "$opponentName",
    opponentLevel: $opponentLevel
  }
}
```

### result

Status: 201

```
{
  match: {
    id: 56,
    status: "in-progess",
    potentialPoints: 270,
    player: "loreweaver",
    level: 11,
    opponentName: "darksbane",
    opponentLevel: 16
  }
}
```

## PUT `/api/match/:id`

Updates the match with a winner. This should then update the leaderboard.

### method

PUT

### headers

```
{
  ContentType: "application/json",
  Accept: "application/json"
}
```

### body

Include the attribute for the winner, whether `player` or `opponent`

example inputs:

- `winner`: "player"

```
{
  match: {
    status: "complete",
    winner: "player"
  }
}
```

### result

Status: 200

```
{
  match: {
    id: 56,
    status: "complete",
    winner: "player",
    potentialPoints: 270,
    player: "loreweaver",
    level: 11,
    opponentName: "darksbane",
    opponentLevel: 16
  }
}
```

## GET /api/leaderboard

Will return a leaderboard with a sum for all completed matches

### headers

```
{
  Accept: "application/json"
}
```

### result

Assuming there were three matches using the match data above, and the player won 2, the opponent won 1:

```
{
  leaderboard: [
    {
      player: "loreweaver",
      rank: 1,
      score: 3520
    },
    {
      player: "darksbane",
      rank: 2,
      score: 1760
    },
  ]
}
```

# Submitting the API

When complete please send a link to GitHub, GitLab, or BitBucket public repo.
