# splunk-integration-with-docker-running-apache-nginx-and-log-analysis

<h2> step 1 : download splunk Enterprise for the host system </h2>
<h4>this is server where all the logs from different devices will be managed</h4>


### method 1 using the wget command :

```bash
wget -O splunk-10.4.3-4174a2deda5d-windows-x64.msi "https://download.splunk.com/products/splunk/releases/10.4.3/windows/splunk-10.4.3-4174a2deda5d-windows-x64.msi"

```
### method 2 : using the website :
open the [splunk enterprise website](https://www.splunk.com/en_us/download/splunk-enterprise.html) and download the .msi file

<img width="1477" height="227" alt="image" src="https://github.com/user-attachments/assets/846b17ee-78f4-48bc-99a9-dd24655d263d" />

& install it on your system

if there something running on the default port i.e, 8000, you can change it in the `$SPLUNK_HOME/etc/system/local/web.conf` file

if it still doesn't work 
navigate to the splunk dir and start it manually
```powershell
cd "C:\Program Files\Splunk\bin" && .\splunk start
```
<img width="1816" height="531" alt="image" src="https://github.com/user-attachments/assets/d4e3af8e-0cc3-4124-abe8-534b0c6688f6" />

now that the server is up and running, we'll install the splunk forwarder on the linux vm before downloading the docker image

## STEP 2: install the forwarder on the linux vm <br/>
by using the get command:
```bash
wget -O splunkforwarder-10.4.4-f0f12fcdcaa1-linux-amd64.deb "https://download.splunk.com/products/universalforwarder/releases/10.4.4/linux/splunkforwarder-10.4.4-f0f12fcdcaa1-linux-amd64.deb"
```
navigate to where the file is downloaded and then -i install the package using dpkg:
```bash
sudo dpkg -i ./splunkforwarder*.deb
```
<img width="759" height="229" alt="image" src="https://github.com/user-attachments/assets/1f785c04-1431-40c4-beb6-a6c69f9c96b1" />

make an admin account using the command:
```bash
sudo /opt/splunkforwarder/bin/splunk start --accept-license~  
```
after reading through the manual, it will prompt to create a admin account:

<img width="1093" height="573" alt="image" src="https://github.com/user-attachments/assets/fc20bb18-b67f-4d45-8768-de31d296ca9f" />

```bash
┌──(kali㉿localserver)-[/opt/splunkforwarder]
└─$ sudo /opt/splunkforwarder/bin/splunk enable boot-start
Important: splunk will start under systemd as user: splunkfwd
Systemd unit file installed by user at /etc/systemd/system/SplunkForwarder.service.
Configured as systemd managed service.
                                               
```

set up the port for the splunk indexer (in this case my host windows machine):
```bash
sudo /opt/splunkforwarder/bin/splunk add forward-server <WINDOWS_IP>:9997
```
<sub>in my case the windows ip is going to be the ip of the vmware nat i.e 192.168.239.2, as im running running a vm<sub/>


