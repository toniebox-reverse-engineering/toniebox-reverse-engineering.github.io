---
title: "Playlists (TAP)"
description: "playlist feature / tap files"
bookCollapseSection: true
---
# Playlist Feature (TAP files)

TeddyCloud supports basic playlist handling. A playlist file (TAP) can be linked to multiple sources, as long as it is supported by ffmpeg. 

TAP files can be placed into library folder. The Tonies (from content folder) can then get assigned to any TAP file, just like it would be a normal TAF.

TAP files usually looks like the following:

```json
{
  "type": "tap",
  "audio_id": 0,
  "filepath": "lib://by/tapID/radio.taf",
  "name": "Radio",
  "files": [
    {
      "filepath": "lib://by/audioID/1234567890.taf",
      "name": "Hörspiel Tonie"
    },
    {
      "filepath": "lib://mp3/album/title.mp3",
      "name": "A Song"
    },
    {
      "filepath": "http://nas.intranet/musiclibrary/album/song.mp3",
      "name": "Network Audio"
    }
  ]
}
```

## Specification
| Option         | Type   | Default | Description |
|----------------|--------|---------|-------------|
| type           | str   | `tap` | Sets the type of playlist |
| audio_id        | uint32   | `0` | Sets the audio id if the file is cached. Set it to 0 to force recreating the cached file |
| filepath         | str    | `""`    | Sets a path of TAF, which will be produced by ffmpeg |
| name   | str | `""`     | Sets the name of the playlist |
| files          | array   | `[]` | Define a list of audio files for this playlist |
| files.filepath | str | `""` | Sets the path to audio resource (must be compatible to ffmpeg) |
| files.name | str | `""` | Sets the name of the audio resource |
