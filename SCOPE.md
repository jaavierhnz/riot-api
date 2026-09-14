# Scope v1

## Goal
A read-only Lol API that returns stats of an user using the official Riot API.

## Endpoints
GET /health
	Check if the service is online.

GET /summoner/{name}
	Tier, rank, LP, account level.

GET /summoner/{name}/recent
	Last 10 matches info: champion, role, win/loss, KDA, duration.

GET /summoner/{name}/stats
	Winrate by role and most played champions (last 20 matches).

## Out of scope v1
- LP gain/loss (for future)
- Player comparison
- Web frontend
- Authentication

## Constraints
- Riot development API key expires every 24 hours
- Rate limited: 20 request p/s, 100 per 2 min.
