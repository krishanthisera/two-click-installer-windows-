# two-click-installer-windows
If you have to face a situation which you don’t have windows server but you have to install a windows application to multiple machine and update them, let’s assume update with an exe. 

Here is my situation,
I've an install file named  PhonikIP.MSI and I need to install it to my local pcs and update the exe with nerwer version on the Apache server which installed in to the pcs.
I've a Appache AKA HTTPD server install to my local centos server and i've coppied above file to appache server's root folder.
This is my app,ication (‘.bat’) to install and update the application by running it as ADMINISTRATOR. I've use an additional textfile to do the configuration.
The configuration file is saved to local drive “C:” as configInputs.txt (C:\configInputs.txt) followed by following structure.

            Note that this program has configured to update a file call “configcallcenterpass.txt” and to update application's configuration file which use in the programe. Make sure you change this source as you desire.

            <Primary Server IP>;<Secondary server IP>;<Password to update>;<Install File>;<Application Directory>;<File to be copy from the server>
            Ex,
            192.168.1.10;192.168.1.1;passw0rd;PhonikIP.msi;C:\Program Files (x86)\iPhonik\PhonikIP\;Project.exe

Plase reffer the .Batch script below.

            REM Setup the configInputs.txt
            REM copy configInputs.txt C:\configInputs.txt
            REM read config text file

            FOR /F "tokens=1,2,3 delims=;" %%G IN (C:\configInputs.txt) DO set pri_svr_ip=%%G 
            FOR /F "tokens=1,2,3 delims=;" %%G IN (C:\configInputs.txt) DO set sec_svr_ip=%%H
            FOR /F "tokens=1,2,3 delims=;" %%G IN (C:\configInputs.txt) DO set pwd=%%I  
            REM pri_svr=192.168.1.150
            
            echo Primary server IP  : %pri_svr_ip% 
            echo Secondary server IP: %sec_svr_ip%
            
            echo ===========================================================================================
            
            echo Checking the Primary server connectivity..... 
            ping %pri_svr_ip% 

            echo ===========================================================================================

            echo Checking the Secondary server connectivity.....
            ping %sec_svr_ip%

            echo ===========================================================================================
            
            echo .
            echo ..
            echo ...
            echo ....
            echo .....
              
            echo You're about to modify the following files
            echo *********************************************************************
            echo *********************************************************************
            echo 1. C:\Program Files (x86)\iPhonik\PhonikIP\Project.exe
            echo 2. C:\Program Files (x86)\iPhonik\PhonikIP\configcallcenter.txt
            echo 2. C:\Program Files (x86)\iPhonik\PhonikIP\configcallcenterpass.txt
            echo *********************************************************************
            echo *********************************************************************
            timeout 10

            echo ===========================================================================================

            REM Terminate the Running process
            echo Trying to terminate Project.exe (If exisits).... 
            taskkill /f /t /im Project.exe

            REM Uninstalling existing Application
            msiexec.exe /qn /x PhonikIP.MSI


            REM Downloading the software
            echo .
            echo ..
            echo ...

            echo Installing PhonikIP 
            echo This may take few seconds 
            FOR /F "tokens=1,2,3 delims=;" %%G IN (C:\configInputs.txt) DO msiexec.exe /qb /i http://%%G/PhonikIP.msi
            timeout 6 /nobreak
 

            REM Change the WD
            pushd C:\Program Files (x86)\iPhonik\PhonikIP\ 


            REM [Optional] In case you need the grant the permission Granting access privilages
            REM CACLS Project.exe /e /p Everyone:f
            REM CACLS configcallcenter.txt /e /p Everyone:f
            REM CACLS configcallcenterpass.txt /e /p Everyone:f
            
            REM Delete the Files in order to replace them
            del Project.exe /f
            
            REM Download the Update from the server
            FOR /F "tokens=1,2,3 delims=;" %%G IN (C:\configInputs.txt) DO bitsadmin /transfer t http://%%G/Project.exe  "%cd%\Project.exe"
            
            del C:\configcallcenter.txt /f
            del configcallcenterpass.txt /f
            
            
            REM *******Sever adresses are here
            FOR /F "tokens=1,2 delims=;" %%G IN (C:\configInputs.txt) DO echo %%G;%%H>configcallcenter.txt
            
            REM ******** configcallcenterpassword 
            FOR /F "tokens=1,2,3 delims=;" %%G IN (C:\configInputs.txt) DO echo %%I>configcallcenterpass.txt
                       
                       
                        
                        
            REM Deleting Temporary Files
            del C:\configInputs.txt /f 
               
                     
                     
               
                        
            REM Configured By Krishan@Iphonik.com     
