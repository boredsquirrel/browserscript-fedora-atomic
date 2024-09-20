# Install Brave or Vivaldi on Fedora Atomic (Kinoite, Silverblue, ...) easily
<img src="https://fedoraproject.org/assets/images/logos/fedora-blue.png" alt="Fedora Logo" width="300"/> ![Brave-icon](https://brave.com/static-assets/images/brave-logo-sans-text.svg) <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/e4/Vivaldi_web_browser_logo.svg/200px-Vivaldi_web_browser_logo.svg.png" alt="Vivaldi Logo" width="60"/>

A simple interactive script to download the Brave (stable/beta/nightly) or Vivaldi (stable/snapshot) repository and PGP signature, and install it using `rpm-ostree`.

Usage:

```bash
cd ~/.local/bin
curl -sSLO https://github.com/boredsquirrel/browserscript-fedora-atomic/raw/main/browserscript
chmod +x braveinstall
./braveinstall
```

example output:

```
======== Browser Install Script ========

Available Browsers:
1: Brave stable (recommended)
2: Brave beta
3: Brave nightly
4: Vivaldi stable (x86_64) (if unsure, use this)
5: Vivaldi stable (arm64) (Snapdragon, Apple Silicon, many SOCs)
6: Vivaldi snapshot (x86_64)
7: Vivaldi snapshot (arm64)

Which Browser do you want to install? (1-7): 1

Downloading Repo and Key, you need to enter a password.
Brave Repo and Key downloaded.

Securing the repo files against manipulation. Please enter your password one last time.

Installing...
< rpm-ostree install process >
```

## Background
***Brave*** has [three different repositories for Stable, Beta and Nightly.](https://brave.com/linux/#release-channel-installation), but only two different PGP verification key files! (one has two keys inside)

The repositories are downloaded to a temporary directory and then moved to `/etc/yum.repos.d/` and the keys to `/etc/pki/rpm-gpg/`. That way you will always download official versions of Brave. The process avoids connecting to the internet as root, which can be really dangerous.

***Vivaldi*** has 2 repositories, this time divided by architecture (`x86_64/amd64` and `arm64/aarch64`). It uses a strange method of downloading .RPM files, which place those repo files. I extracted the repo files for you, and they are simply written manually.

In the end an `rpm-ostree install` command layers Brave or Vivaldi to your system, but the script also works on traditional Fedora and you can just install it manually:

### Usage on DNF Fedora

```bash
sudo dnf install brave-browser
#or brave-browser-beta or brave-browser-nightly

sudo dnf install vivaldi-stable
# or vivaldi-snapshot
```

### Security & Flatpak
Using Chromium Browsers as native packages on Linux is recommended for security reasons.

The native versions utilize filesystem and syscall Sandboxes to isolate every Website and process from one another and your from your OS.

Flatpak browsers are sandboxed from the system, which blocks their ability to create isolated processes. This means
- Firefox Flatpak simply doesn't use namespace sandboxes (but runs anyways!) and relies purely on seccomp filters. Dont use this! This is a weaker sandbox!
- Chromium Browsers (Chrome, Chromium, Brave, Vivaldi, Edge, Opera) wouldnt normally run at all, so packages use [zypak](https://github.com/refi64/zypak) to make the sandboxing fork server use `flatpak-spawn` instead of their regular sandbox. This is unconventional, and could be an advantage against exploits targeting only the Chromium sandbox. But it is also way less tested and used, and could have weaker seccomp filters and isolation.

So using Flatpak Chromium browsers is controversial but may be fine or even more secure!

Using Flatpak Firefox is simply less secure, as one layer of Sandboxing is entirely removed. Especially if you use your browser more as a ***platform*** than a ***program***, where the protection between browser and OS may be less important than between processes within the browser.

Example: You store passwords and credit cards in your browser, or use Element Web etc. Weaker process-isolation within the browser may make this information 

Especially on Android, Chromium Browsers are way more secure, because Firefox for Android doesn't have a working filesystem sandbox at all. When relying on builtin sync features, Brave can be the best choice at the moment.

### Bubblejail
I recomend to use [Bubblejail](https://github.com/igo95862/bubblejail/) when possible. It allows to sandbox natively installed programs with bubblewrap, just like Flatpak.

The difference is, that it allows custom filters and sandbox configurations, enabling the regular Sandbox for Firefox or Chromium Browsers, while also sandboxing the parent process.

Bubblejail is available for Fedora [via COPR](https://copr.fedorainfracloud.org/coprs/secureblue/bubblejail), you can use [my COPR script](https://github.com/boredsquirrel/COPR-command) to add the repo on Fedora Atomic.

### Privacy
Brave is the only Chromium Browser on the Desktop that is degoogled (like ungoogled Chromium) **and** privacy friendly out of the box.

[I dont recommend to use Braves Crypto Stuff](https://www.spacebar.news/stop-using-brave-browser/), so make sure to disable BAT, Wallet and more via a policy or `brave:flags`
