# Mac Setup with Ansible

This repo provides an automated way to install packages and applications on your Mac. It uses [Ansible](https://www.ansible.com) to orchestrate programmatically installing software libraries and applications using [Homebrew](https://brew.sh), MAS, pip or R. The repo comes with some default install configuration, but you can also customize it to include the applications you care about.


## Getting Started
The Ansible "playbook" we will use relies on a few things. Follow the install/setup steps below.

  1. Install Apple Xcode -- you can install Xcode from the Mac App Store.
  1. Install [Homebrew](https://brew.sh) -- this is a popular Mac package manager. 
  1. Use Homebrew to install Ansible: `brew install ansible`.
  1. Create a code directory formatted like: `~/code/<git_platform>/<platform_account>/<repo_name>`.
  1. Clone this repository to your local drive from the platform_account directory with `git clone https://github.com/benjoaquin/mac-dev-playbook`.
  1. Accept the Xcode license agreement by running: `sudo xcodebuild -license accept`.

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

## Configuring your playbook

This repo comes with the file `default.config.yml` -- this provides some default configuration, including a list of useful packages and applications. 

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file. Here's an example config file which enables installation of Mac AppStore apps, Python (3), and R, and adds some specific Mac AppStore apps.

```
---
# enable (yes) or disable (no) certain software
configure_mac_app_store: yes
configure_python: yes
configure_rstats: yes

# Mac App Store (mas) applications
# NOTE: need to save your password in AppStore for free apps....
mas_installed_apps:
  - { id: 1333542190, name: "1Password" } 
  - { id: 409222199, name: "Cyberduck"}
  - { id: 441258766, name: "Magnet"}
  - { id: 1179623856, name: "Pastebot"}
mas_email: "your-email@internet.com"
mas_password: ""
```

Any variable you include in your config file will *replace* the default value -- e.g. if you wanted to add applications to the `brew` list, you can copy-paste the existing `homebrew_installed_packages` values from `default.config.yml` to `config.yml` and add more entries. 

#### Finding Apps

* For Homebrew packages and applications it is **simplest** to [search all of homebrew on the web](https://formulae.brew.sh). It makes it easy to search both packages and applications and confirm it is what you think it is.
* **Brew packages**: To find brew packages to install, run the command `brew search <package-name>` and look for listings under "Formulae".
* **Brew applications**: brew installs applications using `brew cask` (an add-on). To find brew applications, run the command `brew search <app-name>` and look for listings under "Casks".
* **Mac AppStore**: Mac AppStore apps are installed using the `mas` package. To find application info, run the command `mas search <app-name>`. NOTE: you will need to run the ansible playbook once initially to install the `mas` package (or manually run `brew install mas`).

## Running the playbook

To run the Ansible playbook and install all the configured software, do the following in a command line app like `terminal`. This will prompt you for a sudo password to start.

1. Change into the playbook directory -- for example:  
	`$ cd ~/path/to/repo`
2. Run the playbook:  
	`$ ansible-playbook main-playbook.yml -i inventory -K`
 
Optionally, you can filter which set of processes to run by adding 1 or more "tags" to the playbook command. For example, the call below will just run the installation process for homebrew apps and python. The tags available are `homebrew`, `mas` (Mac AppStore), `rstats`, `python`.

	$ ansible-playbook main-playbook.yml -i inventory -K --tags "homebrew,python"


<br>

## Additional configuration
There are several configurations that are meanigful to me but I don't know how to automate. 

- MacOS
	- Sign into iMessage and enable iCloud and phone forwarding
	- Change default apps for some extension
		- .yml, Sublime
		- .md, MacDown
	- System preferences
		- General
			- Grey accent
			- Graphite highlight
			- Allow handoff
		- Desktop & Screensaver	
			- Desktop color to Tungsten
			- Hotcorners
				- Top right - Application windows
				- Bottom right - Sleep display
				- Bottom left - Desktop
		- Dock
		    - Position left
		    - Auto hide
		- Siri
		    - Indian (Female)
	- Finder preferences
		- Show connected servers 
		- Sidebar to include /, ~/, Pictures
- Other apps
	- Sourcetree
		- For GitHub configure Basic auth over HTTPS with a Personal Access Token
		- Github > Settings > Developer settings > Personal access tokens
			- Name the PAT "Sourcetree on <device model> (<device serial>)"
		- [If you need SSH these docs may be helpful](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/connecting-to-github-with-ssh)

<br>

------

## Acknowledgements

This repo was based off of [this mac-dev-playbook](https://github.com/geerlingguy/mac-dev-playbook) from geerlingguy.

  
