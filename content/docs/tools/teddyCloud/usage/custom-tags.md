---
title: "Custom Tags"
description: "Info how to setup and use custom tags in TeddyCloud."
bookCollapseSection: true
---
# Custom Tags
To use custom tags within TeddyCloud you need to get specific tags and perform the following steps within TeddyCloud.

## 1. Buy specific tags
TonieBox only allows specific tags with the following criterias:
- Type: SLIX-L
- Privacy-Mode
- UID length: 16 characters
- UID starts with: E0 04 03

**NO other NFC tags will be accepted by TonieBox.**

## 2. Read and save TAG-ID
Once you place a new tag on TonieBox, i will automatically activate privacy mode on this tag and it can not be recogniced by another NFC reader any more. Privacy mode needs to be deactivated beforehands again.

If you want to save the TAG-ID for later usage, please use a common NFC-Tag reader (for example via iOS or android app).

## 3. Place new NFC tag on TonieBox
If teddyCloud setup and TonieBox flashing is already done, you can continue with this step. 

- Place the new custom NFC Tag for the first time on your TonieBox, it will give you an error code, but that is fine because we dont have assigned a custom play to it yet.
- Check TonieBox User Interface: Now you should see a new custom tag:

  ![GUI](/img/tonies-custom-json_empty.png)

## 4. Assign content
Now you can assign content to the new custom tag. 
- klick on "Edit" button
- Choose a source out of your library, or clone a Tonie which you already own.
- Klick on "save"
- Press one ear of your TonieBox for 5 seconds, to do the refresh. Afterwards your new custom tag should work now.

## 5. Change name and picture of custom tag
If you want to change the name and the picture of your custom tag, please continue here.
- Create a new "Model" described here [tonies.custom.json config](../tecdoc/tonies-custom-json.md) and save it to your `tonies.custom.json` file.
- Go to Teddycloud-GUI, and klick on "settings" and "Reload Tonies.json" to refresh the created model.

![GUI](/img/gui-tonies-reload-config.png)

- Edit your custom tag again and insert the unique ID you gave your new model beforehands and klick on "Save".

![GUI](/img/gui-tonies-edit-model.png)


