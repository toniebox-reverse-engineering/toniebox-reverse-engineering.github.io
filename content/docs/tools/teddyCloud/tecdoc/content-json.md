---
title: "content-json config"
description: "Info about the content json file"
bookCollapseSection: true
---
# content.json
Toniecloud uses the content-json file to save some data and control the behaviour of a tag. Its name equal to the TAFs name but with a json file ending like `500304E0.json`. This file sits alongside the actual audio content.

Usually it looks like the following:

```
{
	"live":	false,
	"nocloud":	false,
	"source":	"",
	"skip_seconds":	0,
	"cache":	false,
	"cloud_ruid":	"",
	"cloud_auth":	"",
	"cloud_override":	false,
	"tonie_model":	"",
	"_version":	5
}
```

## Specification
| Option         | Type   | Default | Description |
|----------------|--------|---------|-------------|
| live           | bool   | `false` | Always start the content from the beginning and redownload its content |
| nocloud        | bool   | `false` | Do not sync the TAF with the boxine cloud |
| source         | str    | `""`    | Use this TAF as content or convert this file into a TAF, when the box requests content (everything ffmpeg can decode, files or urls for webradio) |
| skip_seconds   | uint32 | `0`     | Skips the audio by given seconds (source) |
| cache          | bool   | `false` | Do not delete the TAF converted via `source` after sending it |
| cloud_ruid     | str    | `""`    | rUID of the Tonie. Enable `Dump rUID/auth` in teddyCloud to fill in the rUID when placing a Tonie. This can also be used to reroute a tag to a different one or download a Tonie via the webinterface by clicking on the download icon beside the json. (cloud_auth needed)|
| cloud_auth     | str    | `""`    | Content password of the Tonie. Enable `Dump rUID/auth` in teddyCloud.  |
| cloud_override | bool   | `false` | Reroute a tag to a different one (cloud_ruid/cloud_auth) |
| tonie_model    | str    | `""`    | Identify the Tonie |
| _version       | uint32 | `5`     | Version of the file |

## Examples

* Custom audiobooks that you added to Teddycloud (no official Tonies) typically will have `nocloud` enabled
* You can download content right from an URL by setting the `source`. In combination with `nocloud` you can easily set up custom content. The custom content will be downloaded to your Toniebox when triggering a content update (long press one of the volume-ears).
