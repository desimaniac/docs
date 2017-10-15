I have always liked having my media named with the [Scene](https://scenerules.org/) naming rules. I like the convenience of knowing the episode title, quality, proper, edition, and release group by just looking at the filename. This format is Plex compatible and I am posting this here for anyone who may find this helpful.


## Sonarr


### Quality Definitions

These are important, or else the naming format will not work. 

| Quality      | Title        |
| ------------ | ------------ |
| Unknown*     | Unknown      |
| SDTV*        | SDTV         |
| WEBDL-480p   | 480p.WEB     |
| DVD          | DVDRip       |
| HDTV-720p    | 720p.HDTV    |
| HDTV-1080p   | 1080p.HDTV   |
| Raw-HD*      | Raw-HD       |
| WEBDL-720p   | 720p.WEB     |
| Bluray-720p  | 720p.BluRay  |
| WEBDL-1080p  | 1080p.WEB    |
| Bluray-1080p | 1080p.BluRay |
| HDTV-2160p   | 2160p.HDTV   |
| WEBDL-2160p  | 2160p.WEB    |
| Bluray-2160p | 2160p.BluRay | 

\*Haven't figured out what to do with these yet so I left them as-is.


### Episode Naming

#### Standard Episode Format
```
{Series.CleanTitle}.S{season:00}E{episode:00}.{Episode.CleanTitle}.{QUALITY.REAL}.{QUALITY.PROPER}.{Quality.Title}.{MediaInfo.VideoCodec}-{RELEASE.GROUP}
```

Single Episode Example: The.Series.Title.2010.S01E01.Episode.Title.1.PROPER.720p.HDTV.x264-RLSGRP

#### Daily Episode Format
```
{Series.CleanTitle}.{Air.Date}.{Episode.CleanTitle}.{QUALITY.REAL}.{QUALITY.PROPER}.{Quality.Title}.{MediaInfo.VideoCodec}-{RELEASE.GROUP}
```

Daily-Episode Example: The.Series.Title.2010.2013.10.30.Episode.Title.1.PROPER.720p.HDTV.x264-RLSGRP

#### Anime Episode Format
```
{Series.CleanTitle}.S{season:00}E{episode:00}.{Episode.CleanTitle}.{QUALITY.REAL}.{QUALITY.PROPER}.{Quality.Title}.{MediaInfo.VideoCodec}-{RELEASE.GROUP}
```

Anime Episode Example: The.Series.Title.2010.S01E01.Episode.Title.1.V2.720p.HDTV.x264-RLSGRP


#### Series Folder Format
```
{Series CleanTitle}
```

Series Folder Example: The Series Title 2010


#### Season Folder Format
```
Season {season:00}
```

Season Folder Example: Season 01

Padding (i.e. Season `01`, `02`, etc) is helpful for shows where you have more than 10 seasons.

#### Multi-Episode Style
```
Prefixed Range
```

Multi-Episode Example: The.Series.Title.2010.S01E01-E03.Episode.Title.PROPER.720p.HDTV.x264-RLSGRP

Anime Multi-Episode Example: The.Series.Title.2010.S01E01-E03.Episode.Title.V2.720p.HDTV.x264-RLSGRP

Not exactly the preferred Scene way (i.e. `Repeat` style), but `Prefixed Range` works better if you have many more episodes in one file (it is also the naming style that is recommended by [Plex](https://support.plex.tv/hc/en-us/articles/200220687-Naming-Series-Season-Based-TV-Shows)).

An example would be a TV special you downloaded that had all the webisodes in one file; it would be better to have it named as `S00E01-E10`, than `S01E01E02E03E04E05E06E07E08E09E10`.







***


## Radarr


### Quality Definitions

These are important, or else the naming format will not work. 


| Quality      | Title              |
| ------------ | ------------------ |
| Unknown*     | Unknown            |
| WORKPRINT*   | WORKPRINT          |
| CAM*         | CAM                |
| TELESYNC*    | TELESYNC           |
| TELECINE*    | TELECINE           |
| REGIONAL*    | REGIONAL           |
| DVDSCR*      | DVDSCR             |
| SDTV*        | SDTV               |
| DVD          | DVDRip             |
| DVD-R        | DVDR               |
| WEBDL-480p   | 480p.WEB           |
| Bluray-480p  | 480p.BluRay        |
| Bluray-576p  | 576p.BluRay        |
| HDTV-720p    | 720p.HDTV          |
| WEBDL-720p   | 720p.WEB           |
| Bluray-720p  | 720p.BluRay        |
| HDTV-1080p   | 1080p.HDTV         |
| WEBDL-1080p  | 1080p.WEB          |
| Bluray-1080p | 1080p.BluRay       |
| Remux-1080p  | 1080p.BluRay.REMUX |
| HDTV-2160p   | 2160p.HDTV         |
| WEBDL-2160p  | 2160p.WEB          |
| Bluray-2160p | 2160p.BluRay       |
| Remux-2160p  | 2160p.BluRay.REMUX |
| BR-DISK*     | BR-DISK            |
| Raw-HD*      | Raw-HD             |


\*Haven't figured out what to do with these yet. 



### Movie Naming

#### Replace Illegal Characters `Yes`


#### Standard Movie Format
```
{Movie.CleanTitle}.{Release.Year}.{EDITION.TAGS}.{QUALITY.REAL}.{QUALITY.PROPER}.{Quality.Title}.{MediaInfo.VideoCodec}-{RELEASE.GROUP}
```

#### Movie Folder Format
```
{Movie CleanTitle} ({Release Year})
```
