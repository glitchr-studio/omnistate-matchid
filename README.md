# omnistate/matchid

France's deaths since 1970 - INSEE's file of deceased persons - searched through
[matchID](https://deces.matchid.io), for [glitchr/omnistate](https://github.com/glitchr-studio/omnistate).

```php
$registry = new MatchId($httpClient);                       // a token as 2nd argument raises the quota
$records = $registry->search(new CivilQuery(familyName: 'Chirac', givenName: 'Jacques', born: Period::year(1932)));
$records[0]->date;                      // 2019-09-26
$records[0]->place;                     // Paris 6e Arrondissement (INSEE 75106)
$records[0]->principal()->birthDate;    // 1932-11-29
$records[0]->number;                    // the act's number: 129
$registry->find($records[0]->identifier);
```

No framework needed: the package requires `glitchr/omnistate` and `symfony/http-client`.
`$httpClient` is the HTTP client to call with - the application's, `HttpClient::create()` in
plain PHP, a `MockHttpClient` in a test.

| | |
|---|---|
| Countries | FR (people who died in France, or French abroad, whose death INSEE recorded) |
| Kinds | deaths only |
| Years | deaths since 1970 |
| Picture of the act | no: the file is an index; the act is at the town hall of the place of death |
| Access | free, no key |

Documentation: [docs/](docs/index.md). License: MIT since 2026-10-09; earlier versions remain published under LGPL-3.0-or-later.
