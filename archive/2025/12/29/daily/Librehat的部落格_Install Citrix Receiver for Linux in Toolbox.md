---
title: Install Citrix Receiver for Linux in Toolbox
url: https://www.librehat.com/install-citrix-receiver-for-linux-in-toolbox/
source: Librehat的部落格
date: 2025-12-29
fetch_date: 2025-12-30T03:32:30.583685
---

# Install Citrix Receiver for Linux in Toolbox

[Librehat的部落格](https://www.librehat.com)

* [About Me](https://www.librehat.com/about/)
* [Leave a Message](https://www.librehat.com/leave-message/)
* [Privacy Policy](https://www.librehat.com/privacy-policy/)

# Install Citrix Receiver for Linux in Toolbox

Written by

[librehat](https://www.librehat.com/author/librehat/)

in

[Linux](https://www.librehat.com/category/os/linux/)

In the era of LLM AI chat bots, this was suggested by ChatGPT as a solution for me to upgrade from Fedora 42 to 43. See [this discussion](https://discussion.fedoraproject.org/t/fedora-silverblue-cant-update-to-fedora-43-because-of-conflicting-interests/171563) for the context.

TL;DR. I moved my Citrix application from native to [toolbox](https://docs.fedoraproject.org/en-US/fedora-silverblue/toolbox/) (Fedora’s version of [distrobox](https://distrobox.it/)), and it works well!

Here is how I did it in a step by step guide.

## Install Citrix in a Toolbox Container

In the terminal console (Konsole?).

1. `toolbox create --release 22.04 --distro ubuntu`
2. `toolbox enter ubuntu-toolbox-22.04`

This creates a new container with Ubuntu 22.04 as the base. I chose this over Fedora because Ubuntu in general has better support with commercial partners such as Citrix. Besides, Ubuntu LTS enjoys a longer support than Fedora. Since my whole point here is to have a working Citrix receiver application (or Citrix Workspace Launcher), I need a container that is not going to be EOL anytime soon.

The second command *enters* the container, note that `toolbox` mounts the home directory and it is bidirectional. This works fine in my use case. If you don’t like that, you are better off using `distrobox`.

Now we can install the ICA client (assume that you’ve already downloaded the DEB package from Citrix website yourself).

```
sudo apt update
sudo dpkg -i Downloads/icaclient_<VERSION>_amd64.deb
sudo apt --fix-broken install
sudo dpkg-reconfigure locales
```

The last step here is important, especially if your language and keyboard layout is not English (US).

If the only thing you need is the receiver, run `/opt/Citrix/ICAClient/wfica` and fix any broken/missing dependencies with `apt` (it seems Citrix doesn’t declare the full dependencies in its package metadata). If you need the workspace launcher, then do the same, but for `/opt/Citrix/ICAClient/selfservice`.

![](https://www.librehat.com/wp-content/uploads/2025/12/image-1.png)

You should be able to see the GUI dialog (it should work, *out-of-the-box*).

## Setup Desktop Launcher

Taking KDE Plasma Desktop as an example, you can add desktop launchers by right clicking the Application Launcher (Linux’s version of Start menu) -> Edit Applications…

![](https://www.librehat.com/wp-content/uploads/2025/12/image-2.png)

This launches **KDE Menu Editor**, now click New -> New Item… in the toolbar, and add a launcher for Citrix Receiver manually:

![](https://www.librehat.com/wp-content/uploads/2025/12/image-3-1024x673.png)

|  |  |
| --- | --- |
| **Field** | **Value** |
| Name | Citrix Receiver |
| Program | `toolbox` |
| Command-line arguments | `run --container ubuntu-toolbox-22.04 /opt/Citrix/ICAClient/wfica %u` |

You should adjust these based on your setup if you use `distrobox` other than `toolbox`, and if you use a different Linux distro or version.

Use `selfservice` other than `wfica` if you want a launcher for the workspace launcher instead of (or in addition to) the receiver.

Lastly, let’s associate the `.ica` files with the receiver. Once you have an `.ica` file downloaded, right click and choose to open it with other application.

![](https://www.librehat.com/wp-content/uploads/2025/12/image-4.png)

Click “Show All Installed Applications” if you don’t see Citrix in the list. Tick “Set as default app to open Citrix ICA settings file files”, then click Citrix Receiver.

If you want to use the stock icons from Citrix (as shown in the screenshot above), you can find them inside `toolbox` -> `/opt/Citrix/ICAClient/icons`, though to use them in the host (Fedora), you’ll need to copy them out.

## Post Installation

Now I can remove ICAClient from the host, and do an upgrade from Fedora 42 to Fedora 43 (Kinoite):

```
sudo rpm-ostree remove ICAClient
sudo rpm-ostree rebase fedora:fedora/43/x86_64/kinoite
```

[English](https://www.librehat.com/tag/english/)

←[Soft AP on Linux with WPS](https://www.librehat.com/soft-ap-on-linux-with-wps/)

## Comments

### Leave a Reply [Cancel reply](/install-citrix-receiver-for-linux-in-toolbox/#respond)

Your email address will not be published. Required fields are marked \*

Comment \*

Name \*

Email \*

Website

[ ]  Save my name, email, and website in this browser for the next time I comment.

Δ

This site uses Akismet to reduce spam. [Learn how your comment data is processed.](https://akismet.com/privacy/)

## More posts

* ### [Install Citrix Receiver for Linux in Toolbox](https://www.librehat.com/install-citrix-receiver-for-linux-in-toolbox/)

  [29th December 2025](https://www.librehat.com/install-citrix-receiver-for-linux-in-toolbox/)
* ### [Soft AP on Linux with WPS](https://www.librehat.com/soft-ap-on-linux-with-wps/)

  [17th February 2025](https://www.librehat.com/soft-ap-on-linux-with-wps/)
* ### [My Attempts to Integrate Home Assistant with ADT UK](https://www.librehat.com/my-attempts-to-integrate-home-assistant-with-adt-uk/)

  [3rd November 2024](https://www.librehat.com/my-attempts-to-integrate-home-assistant-with-adt-uk/)
* ### [Citrix Receiver UK Keyboard Layout Issue on Linux and Chrome OS](https://www.librehat.com/citrix-receiver-uk-keyboard-layout-issue-on-linux-and-chrome-os/)

  [10th May 2024](https://www.librehat.com/citrix-receiver-uk-keyboard-layout-issue-on-linux-and-chrome-os/)

## [Librehat的部落格](https://www.librehat.com)

Code can be lovely

Twenty Twenty-Five

Designed with [WordPress](https://en-gb.wordpress.org)