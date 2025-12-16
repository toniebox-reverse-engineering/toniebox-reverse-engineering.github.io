---
title: Explanation & Goal
description: Information on how the teddyCloud works.
bookCollapseSection: true
---
# Explanation & Goal

What’s the goal of this whole project? And how does Teddycloud work at all?  
Well, an unmodified Toniebox is communicating with the official Toniecloud (by Boxine) for example to download any audio content (besides other things). So when you put a Tonie on the box, the real content of the audiobook will be downloaded from the Toniecloud (once) and is stored on the Toniebox for offline listening. The communication between the Toniebox and the Toniecloud takes place on a secured **HTTPS** connection, so **certificates** are needed on both sides (server/client). Without going too much into technical details, this is a simplified overview:
![overview of communication with boxine](/img/teddyCloud_overview_communication_boxine.png)
Now think of our self-hosted Teddycloud as a man-in-the-middle between the Toniebox and the Toniecloud. So at the end of this tutorial, the Toniebox won’t communicate with the Toniecloud/Boxine anymore but only with the Teddycloud. The Teddycloud itself needs to pretend to be a real Toniebox in order to be allowed to communicate with the original Toniecloud. And in order to achieve this, we need to extract the client certificates from our Toniebox and store them in Teddycloud so that from now on our Teddycloud looks like a real Toniebox from Boxine’s point of view. Without the client certificates, the original cloud won’t let Teddycloud download any content and not even establish a coummunication.

As a last step, the Toniebox needs to communicate with our new Teddycloud and this also needs to take place in a secured HTTPS way. That’s why we have to use a (self-generated) CA certificate on the Teddycloud Server and this certificate has to be known by the Toniebox as well (because never trust a stranger). So we need to patch the original firmware of the Toniebox and replace the original CA certificate (from Boxine) with our self-generated one of the Teddycloud. Finally this patched firmware has to be written to the flash chip of the Toniebox. In addition to that, we have to change our local DNS for the Toniebox and point a public url to our Teddycloud instance. This is necessary because besides the CA certificate, we don’t change a single thing in the original firmware. So when our Toniebox wants to connect to the Toniecloud (`prod.de.tbs.toys`) after we put a Tonie on it, this call should not lead to the official server (Boxine) but to our local Teddycloud.

![overview of communication with teddycloud and boxine](/img/teddyCloud_overview_man_in_the_middle.png)

The Teddycloud itself on the other hand needs to resolve the official Boxine server, so we have to make sure that our DNS change is not effective for the Teddycloud instance. Of course It’s up to the enduser to decide whether the original Toniecloud should be used in the first place. This feature actually is turned off by default. But you have to activate it if you want to continue using original Tonies and download/listen to their original audio content. Be aware that the Teddycloud already filters a lot of unnecessary traffic between the Toniebox and the Toniecloud, like analytics data (how often you listen to what etc.). And even if you never plan to use the Toniecloud again, you still have to patch the firmware of the Toniebox so that it can establish a secure connection with your Teddycloud.

