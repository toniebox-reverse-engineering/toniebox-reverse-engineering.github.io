---
title: "Edit JSON files with vscode"
description: "edit json files locally"
bookCollapseSection: true
---

# Edit JSON files with vscode

You can use a json schema which can provide you the ability for autocompletion and validation in your favorite editor. This article describes it for vscode.

Schemas:

* [content-json-schema.json](/teddyCloud/tecdoc/content-json-schema.json)
* [tonies-custom-json-schema.json](/teddyCloud/tecdoc/tonies-custom-json-schema.json)
* [tonieboxes-custom-json-schema.json](/teddyCloud/tecdoc/tonieboxes-custom-json-schema.json)
* [plalist-tap-json-schema.json](/teddyCloud/tecdoc/plalist-tap-json-schema.json)

## content.json

Add the Schemaurl to the json file.

Example:

```json
{
 "$schema": "{{< siteurl >}}/teddyCloud/tecdoc/content-json-schema.json",
 "live": false,
 "nocloud": false,
 "source": "",
 "skip_seconds": 0,
 "cache": false,
 "cloud_ruid": "",
 "cloud_auth": "",
 "cloud_override": false,
 "tonie_model": "",
 "_version": 5
}

```

## tonies.custom.json / tonieboxes.custom.json

To configure this for vscode you have to edit `.vscode/settings.json`.

Add the following parts to the `settings.json`:

```json
{
    "json.schemaDownload.enable": true,
    "json.schemas": [
        {
            "fileMatch": ["tonies.custom.json"],
            "url": "{{< siteurl >}}/teddyCloud/tecdoc/tonies-custom-json-schema.json"            
        },
        {
            "fileMatch": ["tonieboxes.custom.json"],
            "url": "{{< siteurl >}}/teddyCloud/tecdoc/tonieboxes-custom-json-schema.json"
        }
    ]
}
```

## Plalist (TAP) Files

```json
{
  "$schema": "{{< siteurl >}}/teddyCloud/tecdoc/plalist-tap-json-schema.json",
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
