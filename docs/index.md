# omnistate/matchid

## Installation

```sh
composer require glitchr/omnistate omnistate/matchid
```

In a Symfony application, `Omnistate\Bridge\Symfony\OmnistateBundle` registers it when it is installed:

```yaml
# config/packages/omnistate.yaml
omnistate:
    matchid:
        token: '%env(MATCHID_TOKEN)%'   # optional: an account at deces.matchid.io
```

## What is asked

`GET https://deces.matchid.io/deces/api/v1/search` with, from the `CivilQuery`:

| Query | Parameter | |
|---|---|---|
| `familyName`, `givenName` | `lastName`, `firstName` | one of them at least; matchID tolerates spelling variants |
| `born` | `birthDate` | `1932`, `1930-1935` or `29/11/1932` |
| `died` (else `dated`) | `deathDate` | the same three forms |
| `place` | `deathCity` | |
| `sex` | `sex` | `M`, `F` |
| `limit`, `page` | `size`, `page` | |

`find($id)` asks `/id/{id}`.

`supports()` is false for another country than FR, for kinds without DEATH, for a death looked for
before 1970, and without any name.

## What comes back

One `CivilRecord` of kind `DEATH` per person: the death's date and place (name, INSEE code, postcode,
département, coordinates), the act's number, and one `CivilPerson` - names, sex, date and place of
birth, age. `archive` cites INSEE's file and the line; `url` is the record's page at matchID. `raw`
keeps matchID's answer, with its Wikidata link when the person has one.

## Errors

- HTTP 429 or 5xx, or no answer: `UnavailableException` (`retryAfter` from the header, 60 s on a 429).
- HTTP 400 (a date it cannot read): `OmnistateException` with matchID's message.
- Nobody found: an empty list; an unknown identifier: `null`.

## Verified

Against the real service on 2026-10-04 (`search` by name and year, by exact date, by range; `/id`;
an unknown identifier; a refused date). The fixtures in `Tests/Fixtures/` are its answers. The token
(`Authorization: Bearer`) was not tried: no account at hand.
