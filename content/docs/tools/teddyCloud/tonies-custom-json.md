---
title: "tonies-custom-json config"
description: "Info about the tonies-custom-json file"
bookCollapseSection: true
---
# tonies.custom.json
TeddyCloud uses the tonies-custom-json file to read the metadata of custom tags in the same manner it's done for the official boxine tonies in the tonies-json file. The structure is the same, but it's not overwritten as the tonies-json file through regularly updates. So you can use the tonies-custom-json to save metadata of your own custom tags.

Initially the tonies-custom-json file looks like the following:

```
[]
```

In the gui, a custom tag looks initially like:

_old gui_

![Old GUI](/img/tonies-custom-json_empty_oldgui.png)


_new /web gui_

![New GUI](/img/tonies-custom-json_empty_newgui.png)

Enriched with metadata for the above shown custom tag the tonies-custom-json file looks like this (more details see below in section [Specification](#specification)):

```
[{"no": "0", "model": "123456", "audio_id": ["369519776"], "hash": ["af9e61a9c1b12138fb060908d595742334b04515"], "title": "Custom Tonie Example Title", "series": "Custom Tonies", "episodes": "This is my custom tonie", "tracks": ["Title 1", "Title 2", "Title 3", "Title 4", "Title 5", "Title 6", "Title 7", "Title 8", "Title 9", "Title 10"], "release": "0", "language": "de-de", "category": "custom", "pic": "https://upload.wikimedia.org/wikipedia/en/6/6b/Hello_Web_Series_%28Wordmark%29_Logo.png"}]
```

This results after restart of TeddyCloud in the following changed appearance in the GUIs:

_old gui_

![Old GUI](/img/tonies-custom-json_filled_oldgui.png)


_new /web gui_

![New GUI Tonie Cards](/img/tonies-custom-json_filled1_newgui.png)


![New GUI with track display](/img/tonies-custom-json_filled2_newgui.png)


## Specification

The tonies-custom-json file uses the JSON Array Structure.  It contains zero, one, or more ordered elements, separated by a comma. The JSON array is surrounded by square brackets [ ].

Each element consists of a JSON object with the following keys:


| Option         | Example value                                                                               | Description |
|----------------|---------------------------------------------------------------------------------------------|-------------|
| no             | `"0"`                                                                                       | Number of custom tag |
| model          | `""`                                                                                        | A model number of the custom tag, can be left empty |
| audio_id       | `["369519776"]`                                                                             | Enter the custom audio ID of the custom tag. Can be found in the old GUI as shown below |
| hash           | `["af9e61a9c1b12138fb060908d595742334b04515"]`                                              | Enter the hash of the custom tag. Can be found in the old GUI as shown below |
| title          | `"Custom Tonie Example Title"`                                                              | Enter the title of the custom tag, it's currently not displayed, use the series and episode tag to give your custom tag a title which is shown in the GUI |
| series         | `"Custom Tonie"`                                                                            | Enter the Series of the custom tag, will be shown in the GUI |
| episodes       | `"This is my custom tonie"`                                                                 | Enter the Episode of the custom tag, will be shown in the GUI |
| tracks         | `["Title 1", "Title 2"]`                                                               | Enter the tracks of the custom tag, will be shown in the new GUI only |
| release        | `"0"`                                                                                       | currently unused |
| language       | `"de-de"`                                                                                   | language code, currently unused |
| category       | `"custom"`                                                                                  | category of the custom tag, currently unused |
| pic            | `"https://upload.wikimedia.org/wikipedia/en/6/6b/Hello_Web_Series_%28Wordmark%29_Logo.png"` | url of the picture which shall be shown as custom tag image in the GUI |


![Getting the Audio ID and Hash value](/img/tonies-custom-json_sources_audioId_Hash_oldgui.png)