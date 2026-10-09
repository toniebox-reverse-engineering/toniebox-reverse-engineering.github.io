---
title: Synology NAS setup
description: Create a teddy cloud docker image and run it on Synology NAS
weight: 30
bookCollapseSection: true
---
# Synology NAS setup

Example for Synology NAS DS 923+
I don’t recommend you to host a docker image on Synology. ;-) On your Synology NAS there are lots of Services running and lots of the ports like 80 and 443 are already used. This means whenever you want to host a docker image, you have to make sure there are no port conflicts. You have the possibility to run (you must run) your docker with network macvlan, but you will never be able to follow a step by step description and you must always modify the docker compose file, before you can create the image on your NAS. This following description is an example for this problem.

I would recommend installing docker and teddyCloud on a separate device.  
But if the step it’s to big to setup a new device and you want to run only one docker image, teddyCloud (are you sure it’s only this 1 image? ;-)), then follow the following steps.

I installed Container Manager to host my docker images on my NAS. I think with a different application it will be similar.

As mentioned in my introduction port 443 and port 80 is already used by Synology. The Toniebox uses port 443 to communicate with the cloud. We have to use macvlan to be connected directly to the physical network and to act as a standalone container in order to provide port 443 for the modded Toniebox.

1.  Create the following folders. This will be the location for all data of teddycloud.
	- /volume1/docker/teddycloud/certs (certificates of the server)
	- /volume1/docker/teddycloud/config (config file for the server and the boxes)
	- /volume1/docker/teddycloud/content (microSD representation for the boxes)
	- /volume1/docker/teddycloud/library (library to manage content)
	- /volume1/docker/teddycloud/custom_img (location to store custom images for custom tonies json)
	- /volume1/docker/teddycloud/firmware (firmware backups)
	- /volume1/docker/teddycloud/cache (img cache for content images)
2. Now we have to check if there is already a defined macvlan networking driver.
   Open the container App and check on Tab ‘Networks’, if you see an item with type ‘macvlan’.   
   ![network tab on container app](/img/setup_synology_container_manager_mcvlan.PNG)
   If yes, then proceed with option b (this was my option and is tested), where we have to integrate the existing driver to our teddycloud compose file. 
   If no, then proceed with option a. The macvlan networking driver must be created. As you see in the printscreen, I choose a too specific name related to an other project. I recommand to set a more general name like 'macvlan_home_network'. Because this is the driver you will use for the moste docker images hosted by Synology.
3. Create a new project and copy the content of option a or option b.
   ![container app, create new project](/img/setup_synology_container_manager_new_proj.PNG)
   
   **Option a (macvlan driver does not exist)**
   Replace the driver's name with your own invented name and set the ip adress. Check on your dhcp server (typically the router) for an unused address. To create the macvlan driver you have to specify the parent (env0). This can be checked by opening a terminal window, connecting to Synology via *ssh* and run the command *ifconfig* . You get a list of network interfaces. Search the interface with the ip of your NAS. 
   ![ssh connection to synology](/img/setup_synology_ssh_connection.PNG)
   ![ssh connection to synology](/img/setup_synology_ssh_ifconfig.PNG)
   
```
   services:
  teddycloud:
    container_name: teddycloud
    hostname: teddycloud
    image: ghcr.io/toniebox-reverse-engineering/teddycloud:latest
    ports:
      - 80:80 #optional (for the webinterface)
      - 8443:8443 #optional (for the webinterface)
      - 443:443 #Port is needed for the connection for the box, must not be changed! 
    volumes:
      - /volume1/docker/teddycloud/certs:/teddycloud/certs #certificates of the server
      - /volume1/docker/teddycloud/config:/teddycloud/config #config file for the server and the boxes
      - /volume1/docker/teddycloud/content:/teddycloud/data/content #microSD representation for the boxes
      - /volume1/docker/teddycloud/library:/teddycloud/data/library #library to manage content
      - /volume1/docker/teddycloud/custom_img:/teddycloud/data/www/custom_img #location to store custom images for custom tonies json
      - /volume1/docker/teddycloud/firmware:/teddycloud/data/firmware #firmware backups
      - /volume1/docker/teddycloud/cache:/teddycloud/data/cache #img cache for content images
    restart: unless-stopped
    networks:
      npm_network:
       ipv4_address: 192.168.22.237 #Add your Ip
networks:
  npm_network:
      name: npm_network
      driver: macvlan
      driver_opts:
        parent: eth0 #Check the parent with ifconfig
      ipam:
        config:
          - subnet: 192.168.22.0/24 #Change Ip
            ip_range: 192.168.22.0/24 #Change Ip
            gateway: 192.168.22.1 #Change Ip => your router
```
   Save the compose file and create the image. If you don't see any errors and the container was started, you're almost done.
   
   **Option b (macvlan driver exists)** 
   Replace the name of macvlan with your name and set the ipadress.
```
services:
  teddycloud:
    container_name: teddycloud
    hostname: teddycloud
    image: ghcr.io/toniebox-reverse-engineering/teddycloud:latest
    ports:
      - 80:80 #optional (for the webinterface)
      - 8443:8443 #optional (for the webinterface)
      - 443:443 #Port is needed for the connection for the box, must not be changed! 
    volumes:
      - /volume1/docker/teddycloud/certs:/teddycloud/certs #certificates of the server
      - /volume1/docker/teddycloud/config:/teddycloud/config #config file for the server and the boxes
      - /volume1/docker/teddycloud/content:/teddycloud/data/content #microSD representation for the boxes
      - /volume1/docker/teddycloud/library:/teddycloud/data/library #library to manage content
      - /volume1/docker/teddycloud/custom_img:/teddycloud/data/www/custom_img #location to store custom images for custom tonies json
      - /volume1/docker/teddycloud/firmware:/teddycloud/data/firmware #firmware backups
      - /volume1/docker/teddycloud/cache:/teddycloud/data/cache #img cache for content images
    restart: unless-stopped
    networks:
      npm_network: #change name
       ipv4_address: 192.168.22.237 #Add your Ip
networks:
  npm_network: #change name
    external: true
```
   Save the compose file and create the image. If you don't see any errors and the container was started, you're almost done.
4. Keep in mind when the container is started the first time, it takes some minutes until all server certificates are generated. You can't access the teddycloud immediatly. Switch to the log tab and check the output. 
    ![container app, create new project](/img/setup_synology_container_manager_log.PNG)
   
   When the certificates are created, you can try to access the teddycloud typing the ip adress in your browser. If you see the start screen you succeeded.
4. At this point I recommand you to recreate the server certificates following the step 7 and 8 in this [description](https://forum.revvox.de/t/teddycloud-cc3235-newbie-howto/899). Only this step (7, 8). If you do this, then you are also ready to onboard a Toniebox 3235. 
5. Restart teddycloud and now you can proceed and onboard your Toniebox. Go to the menu Toniebox and select the menu item Add Toniebox.

In case you see an error, when the image is created and you can't read the whole message, then change the position of the message box by drag&drop. The message box is rerendered and now you have a vertical scrollbar.
Copy the error message and open chatgpt. Explain you want to create a docker image with synology, add your compse file and explain you're getting the error.
Maby you can solve the problem with chatgpt.
