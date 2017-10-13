I have always liked having my media named with the [Scene](https://scenerules.org/) naming rules. I like the convenience of knowing the episode title, quality, proper, edition, and release group by just looking at the filename. This format is Plex friendly. I am posting this here for anyone who may find this helpful.


### Sonarr

#### Standard Episode Format
```
{Series.CleanTitle}.S{season:00}E{episode:00}.{Episode.CleanTitle}.{QUALITY.REAL}.{QUALITY.PROPER}.{Quality.Title}.{MediaInfo.VideoCodec}-{RELEASE.GROUP}
```

#### Daily Episode Format
```
{Series.CleanTitle}.{Air.Date}.{Episode.CleanTitle}.{QUALITY.REAL}.{QUALITY.PROPER}.{Quality.Title}.{MediaInfo.VideoCodec}-{RELEASE.GROUP}
```

#### Anime Episode Format
```
{Series.CleanTitle}.S{season:00}E{episode:00}.{Episode.CleanTitle}.{QUALITY.REAL}.{QUALITY.PROPER}.{Quality.Title}.{MediaInfo.VideoCodec}-{RELEASE.GROUP}
```

#### Series Folder Format
```
{Series.CleanTitle}.S{season:00}E{episode:00}.{Episode.CleanTitle}.{QUALITY.REAL}.{QUALITY.PROPER}.{Quality.Title}.{MediaInfo.VideoCodec}-{RELEASE.GROUP}
```

#### Season Folder Format
```
Season {season:00}
```

Padding (i.e. Season `01`, `02`, etc) is helpful for shows where you have more than 10 seasons.

#### Multi-Episode Style
```
Prefixed Range
```
Not exactly the preferred Scene way (i.e. `Repeat`), but `Prefixed Range` works better if you have many more episodes in one file (it is also the naming style Plex recommends).

An example would be a TV special you downloaded that had all the webisodes in one file; it would be better to have it named as `S00E01-E10`, than `S01E01E02E03E04E05E06E07E08E09E10`.


***


### Radarr

#### Replace Illegal Characters `Yes`


#### Standard Movie Format
```
{Movie.CleanTitle}.{Release.Year}.{EDITION.TAGS}.{QUALITY.REAL}.{QUALITY.PROPER}.{Quality.Title}.{MediaInfo.VideoCodec}-{RELEASE.GROUP}
```

#### Movie Folder Format
```
{Movie CleanTitle} ({Release Year})
```
