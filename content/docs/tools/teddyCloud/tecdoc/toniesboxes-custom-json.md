---
title: "tonieboxes-custom-json"
description: "Info about the toniesboxes.custom.json file"
bookCollapseSection: true
---
# tonieboxes.custom.json
teddyCloud uses the tonieboxes-custom-json file to read the metadata of customized tonieboxes in the same manner it's done for the official boxine tonieboxes in the tonieboxes-json file. The structure is the same, but it's not overwritten as the tonieboxes-json file through possible updates. So you can use the tonieboxes-custom-json to save metadata of your own customized Tonieboxes.

Initially the tonieboxes-custom-json file looks like the following:

```
[]
```

After adding the meta information of your customized Toniebox the tonieboxes-custom-json file looks like this (more details see below in section [Specification](#specification)):

```json
[{
    "id": "11-0001",
    "name": "Customized Toniebox Teddycloud Limited Edition",
    "img_src": "http://teddycloud.local/customToniebox.png",
    "crop": [
        0,
        0,
        1
    ]
}]
```

This results after restart of teddyCloud in the following changed appearance in the GUI:

Edit Box model modal:

![Toniebox Modal](/img/tonieboxes-custom-json-file-editModel.png)

Toniebox Card:

![Toniebox Cards](/img/tonieboxes-custom-json-file-customizeTonieboxCard.png)


## Specification

The tonieboxes-custom-json file uses the JSON Array Structure.  It contains zero, one, or more ordered elements, separated by a comma. The JSON array is surrounded by square brackets [ ].

Each element consists of a JSON object with the following keys:


| Option  | Example value                                      | Description                                                                                                                                                                                                   |
|---------|----------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id      | `"11-0001"`                                        | Model number (must be not one of the existing model numbers!                                                                                                                                                  |
| name    | `"Customized..."` | Name of the customized Toniebox                                                                                                                                                                               |
| img_src | `"http://.../customToniebox.png"`     | url of the picture which shall be shown as customized Toniebox image in the GUI                                                                                                                               |
| crop    | `[0,0,1]`                                          | Array of 3 elements: [x, y, scale] with x and y as Image shift in x / y direction, scale as factor for scaling the image. Try the correct values till the image has the right size and it's placed correctly. |
