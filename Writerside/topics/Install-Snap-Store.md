# Install Snap Store

## Install Snap daemon
On Debian, snap can be installed directly from the command line:

```bash
sudo apt update
sudo apt install snapd
```

If the sudo command isn’t installed (usually because a root password was provided at install time), you can install snap by first switching to the root account:

```bash
su root
apt update
apt install snapd
```
Either log out and back in again, or restart your system, to ensure snap’s paths are updated correctly.

After this, install the snapd snap to get the latest snapd.

```bash
sudo snap install snapd
```

:information_source: Note: some snaps require new snapd features and will show an error such as snap "lxd" assumes unsupported features" during install. You can solve this issue by making sure the core snap is installed (`snap install core`) and it’s the latest version (`snap refresh core`).

To test your system, install the hello-world snap and make sure it runs correctly:

```bash
sudo snap install hello-world
hello-world 6.3 from Canonical✓ installed
hello-world
Hello World!
```

Snap is now installed and ready to go! If you’re using a desktop, a great next step is to install the Snap Store app.

## Install the Snap Store app

Snaps can be easily installed and managed from the command line.

But if you use a desktop environment, snaps can be more readily discovered, installed and managed from the Snap Store desktop app.



The Snap Store app is installed with the following command:

```bash
sudo snap install snap-store
```

Snap Store can now be launched from your desktop’s default launcher.

![image_107.png](image_107.png)