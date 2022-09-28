# Backlight-Brightness

I found this tutorial on https://bbs.archlinux.org/viewtopic.php?id=134972. Then I decided to put this on my github so I wouldn't need to search on Google every time I reinstall my machine. Credit for this tutorial is due to cmorgenstern.


Installation
------------

You need acpi service for this to be works:

    $ sudo pacman -S acpi acpid



Run acpi service
------------
Afterwards enter the following command to run the service on systemd:

     $ sudo systemctl enable acpid
     $ sudo sytemctl start acpid.service
     $ systemctl status acpid



Listen to acpi
------------
Then after enabling the service, run this command for acpi:

     $ acpi_listen
     

Then press the shortcut key for your brightness. If you see something like this, then you are good to go. 

    video/brightnessup BRTUP 00000086 00000000 K
    video/brightnessdown BRTDN 00000087 00000000 K
    

Make Scripts for brightness level
------------
Create two scripts in your /etc/acpi/actions folder and two corresponding files in the /etc/acpi/events folder to control the backlight:
bl_down.sh

     #!/bin/sh
     bl_device=/sys/class/backlight/<your_manufacture>/brightness
     echo $(($(cat $bl_device)-1)) | sudo tee $bl_device


bl_up.sh

     #!/bin/sh
     bl_device=/sys/class/backlight/<your_manufacture>/brightness
     echo $(($(cat $bl_device)+1)) | sudo tee $bl_device
     

Put these scripts into the /etc/acpi/actions directory and dont forget to make them excuteable by running this commands:


     $ chmod +x bl_down.sh
     $ chmod +x bl_up.sh



Create files inside acpi 
------------
Create the corresponding files in /etc/acpi/events.

bl_down

     event=video/brightnessdown BRTDN 00000087 00000000 K
     action=/etc/acpi/actions/bl_down.sh

bl_up

     event=video/brightnessup BRTUP 00000086 00000000 K
     action=/etc/acpi/actions/bl_up.sh


For the event value, paste in the output you got from above using:

     $ acpi_listen
     and depressing the corresponding brightness keys.


     
     
     
     
     
     
     
     
     
     
     
