---
title: "Custom Tags"
description: "Information on how to set up and use custom tags in teddyCloud."
bookCollapseSection: true
---
# Custom Tags
To use custom tags within teddyCloud, you need to obtain specific tags and follow these steps.

## 1. Buy specific tags
The Toniebox only accepts specific tags that meet the following criteria:

- Type: SLIX-L
- Privacy-Mode
- UID length: 16 characters
- UID starts with: E0 04 03

**NO other NFC tags will be accepted by the Toniebox.**

## (OPTIONAL) 2. Read and save the tag ID
Once you place a new tag on the Toniebox, it will automatically activate privacy mode on this tag, making it unrecognizable by other NFC readers. Privacy mode must be deactivated beforehand.  

If you want to save the tag ID for later use, please use a common NFC tag reader (e.g., via an iOS or Android app).

## 3. Place the new NFC tag on the Toniebox
If the teddyCloud setup and Toniebox flashing are already complete, you can proceed with this step.  

- Place the new custom NFC tag on your Toniebox for the first time. (The Toniebox will respond with an error code, but that's fine for now since no custom play has been assigned yet.)  
- Check the teddyCloud user interface: You should now see a new custom tag:

  ![GUI](/img/tonies-custom-json_empty.png)

## 4. Assign content
Now you can assign content to the new custom tag:  

- Click on the "Edit" button.  
- Choose a source from your library or clone a Tonie you already own.  
- Click on "Save".  
- Press and hold one of the Toniebox's ears for 3 seconds to perform a freshness check. After that, your new custom tag should work.  

## 5. Change name and picture of custom tag
If you want to change the name and the picture of your custom tag, please continue:
- Create a new "Model" described here [tonies.custom.json config](../tecdoc/tonies-custom-json.md) and save it to your `tonies.custom.json` file.
- Go to teddyCloud-GUI, klick on "settings" and "Reload Tonies.json" to refresh the created model.

![GUI](/img/gui-tonies-reload-config.png)

- Edit your custom tag again and insert the unique ID you gave your new model beforehands and klick on "Save".

![GUI](/img/gui-tonies-edit-model.png)

The name, description and picture should have changed now.
