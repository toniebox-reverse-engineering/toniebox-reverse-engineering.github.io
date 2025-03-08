---
title: "tonies.custom.json config"
description: "Info about the tonies.custom.json file"
bookCollapseSection: true
---
# tonies.custom.json
teddyCloud uses the `tonies.custom.json` file from `config` folder to read the metadata of custom tags in the same manner it's done for the official boxine tonies in the `tonies.json` file. The structure is the same, but it's not overwritten as the `tonies.json` file through regularly updates. So you can use the `tonies.custom.json` to save metadata of your own custom tags.

You can generate the needed JSON data via GUI (Edit Tag / Create new model) and save it in the `tonies.custom.json` file, or you can generate it by hand, described below.

Initially the `tonies.custom.json` file looks like the following:

```
[]
```

In the GUI, a custom tag looks initially like:

![GUI](/img/tonies-custom-json_empty.png)

Enriched with metadata for the above shown custom tag the `tonies.custom.json` file looks like this (more details see below in section [Specification](#specification)):

```json
[{"no": "0", "model": "123456", "audio_id": ["369519776"], "hash": ["af9e61a9c1b12138fb060908d595742334b04515"], "title": "Custom Tonie Example Title", "series": "Custom Tonies", "episodes": "This is my custom tonie", "tracks": ["Title 1", "Title 2", "Title 3", "Title 4", "Title 5", "Title 6", "Title 7", "Title 8", "Title 9", "Title 10"], "release": "0", "language": "de-de", "category": "custom", "pic": "https://upload.wikimedia.org/wikipedia/en/6/6b/Hello_Web_Series_%28Wordmark%29_Logo.png"}]
```

**Note:** After you changed the `tonies.custom.json` file you need to reload it via TeddyCloud GUI ( Settings / Reload Tonies.json ) or restart the server.

![GUI](/img/gui-tonies-reload-config.png)

This results after reloading of the `tonies.custom.json` file in the following changed appearance in the GUI:


![Tonie Cards](/img/tonies-custom-json_filled1.png)


![with track display](/img/tonies-custom-json_filled2.png)


## Specification

The `tonies.custom.json` file uses the JSON Array Structure.  It contains zero, one, or more ordered elements, separated by a comma. The JSON array is surrounded by square brackets [ ].

Each element consists of a JSON object with the following keys:


| Option         | Example value                                                                               | Description |
|----------------|---------------------------------------------------------------------------------------------|-------------|
| no             | `"0"`                                                                                       | Number of custom tag |
| model          | `"123456"`                                                                                        | A model number of the custom tag |
| audio_id       | `["369519776"]`                                                                             | Enter the custom audio ID of the custom tag. Can be found in GUI as shown below |
| hash           | `["af9e6..b04515"]`                                              | Enter the hash of the custom tag. Can be found in the GUI as shown below |
| title          | `"Custom Tonie Example Title"`                                                              | Enter the title of the custom tag, it's currently not displayed, use the series and episode tag to give your custom tag a title which is shown in the GUI |
| series         | `"Custom Tonie"`                                                                            | Enter the Series of the custom tag, will be shown in the GUI |
| episodes       | `"This is my custom tonie"`                                                                 | Enter the Episode of the custom tag, will be shown in the GUI |
| tracks         | `["Title 1", "Title 2"]`                                                               | Enter the tracks of the custom tag, will be shown in the GUI |
| release        | `"0"`                                                                                       | currently unused |
| language       | `"de-de"`                                                                                   | language code, will be shown in the gui if it's another language than the dominant one |
| category       | `"custom"`                                                                                  | category of the custom tag, currently unused |
| pic            | `"https://.../Logo.png"` | url of the picture which shall be shown as custom tag image in the GUI. If you stored a custom picture in custom_img folder in TeddyCloud, the link looks like this: "https://your-ip:your-port/custom_img/Logo.png" |

### How to get the Audio ID and the Hash value of a TAF

Navigate to the taf file in the library (or content if library is not enabled) and double click on the row. A modal will be shown which contains the information:

![Getting the Audio ID and Hash value](/img/tonies-custom-json_getaudioid_hash.png)
