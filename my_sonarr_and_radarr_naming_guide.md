# My Sonarr and Radarr Naming Guide


---

<!-- TOC depthFrom:1 depthTo:2 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Introduction](#introduction)
- [Sonarr](#sonarr)
- [Radarr](#radarr)

<!-- /TOC -->

---


# Introduction

I like having my media files _loosely_ named with the [scene naming style](https://scenerules.org/). This format _**works**_ with Plex (even though they recommend [this](https://support.plex.tv/hc/en-us/articles/200220687-Naming-Series-Season-Based-TV-Shows)). I just like the ability to quickly see the info on the release (i.e. the episode quality, proper, edition, release group) by just looking at the filename. Another benefit is that Subzero (subtitle) lookups yield more accurate results.

_Update: I have decided to remove `{Episode.CleanTitle}` from the naming format because 1) The episode titles can sometimes get so large that you have trouble storing/uploading it, and 2) The titles can often change on TVDB, making it a nuisance to fix later. However, if you decide to keep it, you may leave it in (it comes after the episode number and before the quality proper tags)._

_Update 2: Added Year to TV Show Titles._

```
Examples:

TV Shows
└── Gotham (2014)
    └── Season 01
        └── Gotham.2014.S01E01.1080p.BluRay.x264-DEMAND.mkv

Movies
└── Guardians of the Galaxy Vol. 2 (2017)
    └── Guardians.of.the.Galaxy.Vol.2.2017.1080p.BluRay.x264-SPARKS.mkv

```
---

# Sonarr

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

\*Haven't figured out what to do with these yet, so I left them as-is.


### Episode Naming

#### Replace Illegal Characters

```
Yes
```

#### Standard Episode Format

```
{Series.CleanTitleYear}.S{season:00}E{episode:00}.{QUALITY.REAL}.{QUALITY.PROPER}.{Quality.Title}.{MediaInfo.VideoCodec}-{RELEASE.GROUP}
```

> Single Episode Example: The.Series.Title.2010.S01E01.PROPER.720p.HDTV.x264-RLSGRP


#### Daily Episode Format

```
{Series.CleanTitleYear}.{Air.Date}.{QUALITY.REAL}.{QUALITY.PROPER}.{Quality.Title}.{MediaInfo.VideoCodec}-{RELEASE.GROUP}
```

> Daily-Episode Example: The.Series.Title.2010.2013.10.30.PROPER.720p.HDTV.x264-RLSGRP

#### Anime Episode Format

```
{Series.CleanTitleYear}.S{season:00}E{episode:00}.{QUALITY.REAL}.{QUALITY.PROPER}.{Quality.Title}.{MediaInfo.VideoCodec}-{RELEASE.GROUP}
```

> Anime Episode Example: The.Series.Title.2010.S01E01.720p.HDTV.x264-RLSGRP

#### Series Folder Format

```
{Series TitleYear}
```

> Series Folder Example: The Series Title (2010)

_Doing `{Series CleanTitleYear}` would add the year without the parenthesis (i.e. `The Series Title 2010`), which is why I had to do `{Series TitleYear}`._

#### Season Folder Format

```
Season {season:00}
```

> Season Folder Example: Season 01

Padding the season numbers (e.g. `01`, `02`, etc) is preferred by the scene. This also helps sort your seasons in order when you have 10+ seasons.

_This style is also preferred by [Plex](https://support.plex.tv/hc/en-us/articles/200220687-Naming-Series-Season-Based-TV-Shows)._

#### Multi-Episode Style

```
Prefixed Range
```

>Multi-Episode Example: The.Series.Title.2010.S01E01-E03.PROPER.720p.HDTV.x264-RLSGRP
>
>Anime Multi-Episode Example: The.Series.Title.2010.S01E01-E03.720p.HDTV.x264-RLSGRP

Even though releases sometimes use the `repeat` style for multi episode TV shows (e.g. S01E01E02), the scene actually prefers the `Prefixed Range` style (e.g. S01E01-E02), which is also the naming style [Plex](https://support.plex.tv/hc/en-us/articles/200220687-Naming-Series-Season-Based-TV-Shows) recommends.







***


# Radarr


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


\*Haven't figured out what to do with these yet, so I left them as-is.



### Movie Naming

#### Replace Illegal Characters
```
Yes
```

#### Colon Replacement Format:
```
Replace with Space Dash
```

#### Standard Movie Format
```
{Movie.CleanTitle}.{Release.Year}.{EDITION.TAGS}.{QUALITY.REAL}.{QUALITY.PROPER}.{Quality.Title}.{MediaInfo.VideoCodec}-{RELEASE.GROUP}
```

>Movie Example: The.Movie.Title.2010.ULTIMATE.EXTENDED.EDITION.PROPER.1080p.BluRay.x264-EVOLVE


#### Movie Folder Format
```
{Movie CleanTitle} ({Release Year})
```

>Movie Folder Example: The Movie Title (2010)
