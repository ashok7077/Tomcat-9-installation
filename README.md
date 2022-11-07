# Tomcat-9-installation
login your instance.
I am using RedHat linux.

To check your OS Information run this command:
```
sudo cat /etc/os-release
```

![image](https://user-images.githubusercontent.com/97009002/200231862-7c14164f-b051-4030-878c-91d447cff896.png)

## We need to update the general pakages using following command:
```
sudo yum update -y
```
## Befor install Tomcat we need to install dependences for tomcat.
First we need to install java using following:
```
dnf install -y java-11-openjdk-devel
```
Check the java version
```
java -version
```
![image](https://user-images.githubusercontent.com/97009002/200235400-21d959bc-ecec-49fd-9f2f-015b126fe394.png)

Next download the [tomcat](https://tomcat.apache.org/) pakage.
Click on first link.
![image](https://user-images.githubusercontent.com/97009002/200233773-3dc9d637-0d78-497d-a9bd-f8434b76c790.png)

* select the tomcat9 like following screenshot.

![image](https://user-images.githubusercontent.com/97009002/200234466-a7dd8e5e-67ce-42bc-9819-0452ee703031.png)

* copy the tar.gz link.

![image](https://user-images.githubusercontent.com/97009002/200234631-892fe95f-c0b6-4796-bc78-3bd283c837b8.png)

go to /usr/local/ path using following command:
```
cd /usr/local/
```
Clone the tomcat pakage using wget command:
```
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.68/bin/apache-tomcat-9.0.68.tar.gz
```
unzip the tar.gz file using following command:
```
tar -xvf apache-tomcat-9.0.68.tar.gz
```
Rename the tomcat directory using following command:
```
mv apache-tomcat-9.0.68 tomcat-9
```
The directory name has been changed.
![image](https://user-images.githubusercontent.com/97009002/200237018-03237558-6358-4b60-be79-f6a2202fd57a.png)

The service will run with permissions of a system user called `Tomcat` which you need to create it using useradd command:
```
useradd -r tomcat
```
Once the tomcat user is created, give it permissions and ownership rights to the Tomcat installation directory and all of its contents using the following chown command:
```
chown -R tomcat:tomcat /usr/local/tomcat-9
```
![image](https://user-images.githubusercontent.com/97009002/200238107-66c1d4f2-ebfa-4cd5-be02-cc9eee3212dd.png)

Next, create a tomcat.service unit file under `/etc/systemd/system/` directory using vi text editor:
```
vi /etc/systemd/system/tomcat.service
```
Insert the following configuration in the tomcat.service file and save it.
```
[Unit]
Description=Apache Tomcat Server
After=syslog.target network.target

[Service]
Type=forking
User=tomcat
Group=tomcat

Environment=CATALINA_PID=/usr/local/tomcat-9/temp/tomcat.pid
Environment=CATALINA_HOME=/usr/local/tomcat-9
Environment=CATALINA_BASE=/usr/local/tomcat-9

ExecStart=/usr/local/tomcat-9/bin/catalina.sh start
ExecStop=/usr/local/tomcat-9/bin/catalina.sh stop

RestartSec=10
Restart=always
[Install]
WantedBy=multi-user.target
```
Reload the systemd configuration to apply the recent changes using the following command:
```
systemctl daemon-reload
```
Start the tomcat service, enable it to auto-start at system boot and check the status using the following commands:
```
systemctl start tomcat.service
systemctl enable tomcat.service
systemctl status tomcat.service
```
![image](https://user-images.githubusercontent.com/97009002/200239691-4ec0eb3d-d6a8-46c2-ad5c-f69a44debd54.png)

### Enable HTTP Authentication for Tomcat Manager and Host Manager

```
vi /usr/local/tomcat-9/conf/tomcat-users.xml
```
![image](https://user-images.githubusercontent.com/97009002/200240309-463bcc0b-4053-46d3-b94e-c1b57e2396cc.png)

Remove `<!--`..............
...`-->` to uncommant the users and set password for user like following screenshot:

![image](https://user-images.githubusercontent.com/97009002/200240447-2e3169f4-edb2-4cd9-89b1-f072ead48f50.png)

### Enable Remote Access to Tomcat Manager and Host Manager

Manager:

Go to the following /usr/local/tomcat-9/webapps/manager/META-INF/context.xml file.
```
vi /usr/local/tomcat-9/webapps/manager/META-INF/context.xml
```
Add `<!--` ..............`-->` to comment the manager access.

![image](https://user-images.githubusercontent.com/97009002/200244397-18fb1afd-1d82-4773-9801-1321241448ea.png)


Hostmanager:


Go to the following /usr/local/tomcat-9/webapps/host-manager/META-INF/context.xml file.

```
vi /usr/local/tomcat-9/webapps/host-manager/META-INF/context.xml
```

Add `<!--` ..............`-->` to comment the Hostmanager access.

![image](https://user-images.githubusercontent.com/97009002/200244609-ea471efb-cb79-4e58-85d4-d3aa2030694d.png)

Now restart the tomcat server using following command:
```
systemctl restart tomcat.service
```
### Access Apache Tomcat Web Interface

`http://SERVER_IP:8080`

![image](https://user-images.githubusercontent.com/97009002/200245217-1cd49d0b-56db-4dab-bc59-72b548efcb30.png)

Check the Manager Apps

`http://SERVER_IP:8080/manager`

Give the username and password what ever you inserted in /usr/local/tomcat-9/webapps/manager/META-INF/context.xml
# Tomcat has been successfully installed in RedHat linux
