# Install Wekan Snap

Wekan allows anyone to easily work with and modify it. It also allows you to host it on your own server, which ensures that you have complete control over your data and can manipulate it as you wish.

## Install Wekan snap

You should update your debian server packages:

```bash
sudo apt update
```

Now it’s time to install Wekan:

```bash
sudo snap install wekan
```

Now, You need to configure the root of the web URL for Wekan:

```bash
sudo snap set wekan root-url="http://your_server_ip"
```

Then, you can use the following command to set the Wekan http port:

```bash
sudo snap set wekan port='80'
```

**Note:** Set a port number to one that isn’t in use. You can use whatever port you want—we use port 3001—but don’t use a common one, like 22, 25, 443, 80, etc.

Now you should restart the MongoDB service on snap:

```bash
sudo systemctl restart snap.wekan.mongodb
```

Then you need to also restart the Wekan service on snap:

```bash
sudo systemctl restart snap.wekan.wekan
```

You can run the following command to check the status:

```bash
sudo ss -tunelp | grep 80
```

In the next step, you should enable the Wekan service to start on system boots:

```bash
sudo snap enable wekan
```

Now, you need to install MongoDB tools by the following command:

```bash
sudo apt install mongo-tools
```
Next, you have to restart Wekan:

```bash
sudo systemctl restart snap.wekan.wekan
```

Finally, you should reload snap:

```bash
sudo snap refresh
```

## How to Access Wekan Web Interface
Note: that to access Wekan Web Interface, you should have an administrator account. To do this first, open your web browser and enter the following URL:

```bash
http://ServerIP/sign-up
```

Then you should create an account on the registration page.

![image_108.png](image_108.png)

After creating an account, you can access the Wekan Web Interface.

![image_109.png](image_109.png)

At this point, you will see your Wekan dashboard.

![image_110.png](image_110.png)

Bibliography:
+ [https://blog.eldernode.com/configure-wekan-server-on-debian/](https://blog.eldernode.com/configure-wekan-server-on-debian/)
+ [https://orcacore.com/install-wekan-server-ubuntu-22-04/](https://orcacore.com/install-wekan-server-ubuntu-22-04/)