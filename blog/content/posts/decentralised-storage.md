---
layout: post
title: Decentralised image storage hosted within Secured LAN
date: 2024-11-05 01:35:00 +0000

---

In this report/tutorial, An Open-source decentralised image storage service will be configured, using a reverse proxy, free tier cloud servers and a ultra-low power storage device (RPi 2B). This setup splits the workload of the compute across the different hardware avaliable and allows for acceptable application performance while FAR below the recommended minimum hardware specfication for the hosting application, Immich.


**On Remote Oracle Free tier server**

Download and extract latest frp version for your cpu architecture from

https://github.com/fatedier/frp

Navigate to the directory you extracted the tar file to,

Open the frp server config file:  
	  
	nano frps.toml

Choose a port for the reverse proxy to be opened on:

For example (default is port 7000):  
![image1](./image11.png)

 **On your Oracle cloud instance dashboard**

To create a firewall exception for this port, click on your subnet name:

![image2](./image13.png)

Next navigate to the default security list setup automatically when the compute instance was created, the list will look like this:

![image3](./image4.png)

Next add a new stateless ingress rule as follows:

![image4](./image2.png)

**Back on the Remote Server**

Run the server using ./frps \-c ./frps.toml

![image5](./image3.png)

To run in the background and keep using the same shell use:  
	  
	nohup ./frps \-c ./frps.toml &

**On local storage server**

Attach Storage to local linux machine

Use lsblk to show storage devices and partitions, note the name of the partition that you would like to share with cloud server

![image6](./image8.png)

Next create a mount point for the partition, if there is not one already

For example:

	sudo mount /dev/sda1 /media/sandisk

Next install frp

Open the frp client config file

	nano frpc.toml

![image7](./image12.png)

Take note of the client port (default port 7000\)

Next, run the client using the command:

	./frpc \-c ./frpc.toml

Again, to run in the background and reclaim the shell, use nohangup:

nohup  ./frpc \-c ./frpc.toml &

![image8](./image9.png)

The remote server will show the following message when the client connects successfully

![image9](./image7.png)

Next install sshfs 

	sudo apt install sshfs

**On remote server**

Also Install sshfs 

	sudo apt install sshfs

Set up a mount point for the remote folder to be mounted to:

For example:

	mkdir /mnt/data2

next connect to the local storage server with the following command:

	sshfs \-oport=\[frp remoteport\] pi@127.0.0.1:\[local disk mount point\] \[remote server mount point\]

For example:

	sshfs \-oport=6000 pi@127.0.0.1:/media/sandisk /mnt/data2

NOTE: don't run as su or mount point will be inaccessible by non su

If you are having trouble using sshfs, you can show debug output using environment variables like so:

	sshfs \-odebug,sshfs\_debug,loglevel=debug \[^^ rest of command\]

Next install docker

Remove all previous installed docker versions (clean start optional)

	for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

To install docker and its plugins use

	sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

If apt is not able to find docker you may have to add docker to your repository, to do so, read the official docker page here:

 

Next install immich

Create and enter a directory for immich to use

	mkdir ./immich-app  
	cd ./immich-app

Next pull the required files docker will use to build the immich instance

	wget \-O docker-compose.yml https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml

	wget \-O .env https://github.com/immich-app/immich/releases/latest/download/example.env

Make sure this file is saved with the name “.env”

Open the .env file with a text editor

![image10](./image6.png)

Make sure to change the upload location to the remote server mount point you created earlier and set your database password to something private.

Next open the docker-compose.yml in a text editor

To minimise memory usage if needed we can prevent machine learning process from running at the expense of some features by commenting out it's declaration like so:

![image11](./image5.png)

Next limit the memory each process can use and allowing limited swap file usage 

Under the “immich-server” process add memory limits like so:  
![image12](./image1.png)

Again under the “database” process add the limit parameters as required  
![image13](./image10.png)

If you are running Immich with 1GB of ram or less (far below minimum recommended) swap file usage is required for stable operation. To create a swap file 

	sudo dd if=/dev/zero of=/swapfile bs=1MB count=1024

Next protect the file by setting the file permissions to read and write access for root only

	sudo chmod 600 /swapfile

Then format the new swap file using the following command:

	sudo mkswap /swapfile

Enable the use of a swap file using command:

	sudo swapon /swapfile

Next download the latest immich version using 

	sudo docker compose pull

And start the server using 

	sudo docker compose up



## References

Tunnel
https://github.com/fatedier/frp

SSHFS
https://phoenixnap.com/kb/sshfs

https://phoenixnap.com/kb/linux-swap-file

https://www.reddit.com/r/immich/comments/1ccxm2c/guide_how_to_setup_immich_from_scratch_in_10/