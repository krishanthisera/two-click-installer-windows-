# two-click-installer-windows
If you have to face a situation which you don’t have windows server but you have to install a windows application to multiple machine and update them, let’s assume update with an exe. 

This ‘.bat’ will install and update your desired application by running it as ADMINISTRATOR. 
The configuration file should be saved to local drive “C:” as configInputs.txt (C:\configInputs.txt) followed by following structure.

            Note that this program is configured to update the password file call “configcallcenterpass.txt” and to update application configuration file call “configcallcenter.txt”. Make sure you change this source as you desire.

            <Primary Server IP>;<Secondary server IP>;<Password to update>;<Install File>;<Application Directory>;<File to be copy from the server>
            Ex,
            192.168.1.10;192.168.1.1;passw0rd;PhonikIP.msi;C:\Program Files (x86)\iPhonik\PhonikIP\;Project.exe
