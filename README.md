# Install Brave on Fedora Atomic (Kinoite, Silverblue, ...) automatically
![Brave-icon](https://brave.com/static-assets/images/brave-logo-sans-text.svg) 

A simple interactive script to download the Brave (stable/beta/nightly) repository and PGP signature, an install it using `rpm-ostree`.

Usage:
```
curl -sSLO https://github.com/trytomakeyouprivate/braveinstall-fedora-atomic/raw/main/braveinstall
chmod +x braveinstall
./braveinstall
```

example output:

```
user@PC:/var/home/user$ curl -sSLO https://github.com/trytomakeyouprivate/braveinstall-fedora-atomic/raw/main/braveinstall
chmod +x braveinstall
./braveinstall
Which version of Brave do you want to install? (stable/beta/nightly): stable
[sudo] Password for user: 
Repo and Key downloaded.
installing...
```
### Details
Brave has [three different repositories for Stable, Beta and Nightly.](https://brave.com/linux/#release-channel-installation), but only two different PGP verification key files! (one has two keys inside)

The repositories are downloaded to `/etc/yum.repos.d/` and the keys to `/etc/pki/rpm-gpg/`. That way you will always download official versions of Brave.

In the end an `rpm-ostree install` command layers Brave to your system, but the script also works on traditional Fedora and you can just install it manually:

```
sudo dnf install brave-browser
#or brave-browser-beta or brave-browser-nightly
```

Using Chromium Browsers on Linux is recommended for security reasons, as the native versions utilize Sandboxes to isolate every Website from another and your OS. They are more secure than Firefox, and Flatpak browsers have these sandboxes removed, as the entire browser is sandboxed using `bubblewrap`. This weakens the security of their builtin ones, and Chromium is now the same version as on ChromeOS. Brave meanwhile is the only Chromium Browser that is degoogled (like ungoogled Chromium) **and** privacy friendly (fingerprint protection etc.) out of the box.

[I dont recommend to use Braves Crypto Stuff](https://www.spacebar.news/stop-using-brave-browser/), so make sure to disable BAT, Wallet and more via a policy or `brave:flags`
