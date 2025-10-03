# Local Drupal on macOS

_A designer’s guide for setting up a local Drupal instance._

## ❓ Why do this?

If your goal is to prepare for the [Site Builder exam](#certification) by taking the [Acquia course](#course), or to have an experience that’s closer to your project’s environment, where you can install and configure specific modules, then a local sandbox will give you that control. You will need to run commands using the Terminal app and this guide will show you how. One of the nice things about running Drupal locally is that it’s super easy to start over if something goes awry.

A local sandbox is certainly not necessary if you just want to experiment with a Drupal site. For this you could use a service like [Pantheon](https://pantheon.io/) to create a free Drupal site, or try the [Drupal CMS launcher](https://new.drupal.org/drupal-cms/launcher) to install Drupal like a regular Mac app. These are both great options for a designer to get a Drupal instance running so that you can kick the tires and build some content.

## 📖 Course

Acquia’s free [Drupal Site Building course](https://community.acquiaacademy.com/learn/courses/669/drupal-site-building) provides a helpful overview of Drupal’s structure and features.

## 🎓 Certification

The [Acquia Certified Site Builder Drupal 10/11 exam](https://www.acquia.com/support/training-certification/acquia-certification/drupal-certification-track) lasts for 75 minutes and costs $155.

## ⌨️ Using the Terminal app

If you aren’t familiar with the macOS terminal, here’s a quick introduction to Terminal.app on your Mac. You’ll need to use the command line to install Drupal locally, install additional modules, and keep your software up-to-date. This video is an introduction that shows none of these things. It simply compares your Home folder in the Finder with the same view of the filesystem using Terminal, and demonstrates a few basic commands, to help you get a feel for using Terminal.

[![Side-by-side comparison of a macOS Finder window showing the user's Home directory on the left as a series of icons with text labels and the same data in a Terminal window on the right displayed using the ls command. You can see the 1 to 1 correspondence between the folders in the left window and the directory names in the right window.](comparing-home-directory-in-finder-vs-terminal.png)](#)

Designers getting started with the terminal can also check out Apple’s [Terminal User Guide](https://support.apple.com/guide/terminal/welcome/mac).

## 🐳 Install Docker

Download and install from the [Docker homepage](https://www.docker.com/). Click the **Download Docker Desktop** button and choose the appropriate version for your platform.

**Optional:** The Learning Center prompted me to try some tutorials when I first installed and I ran through a few of them. I have previously used Docker and still found them helpful.

## 🍺 Install Homebrew

[Homebrew](https://brew.sh/) is a package management system for macOS that allows you to easily install software using the command line in Terminal. We will use it to install DDEV.

**Package managers** allow you to install software—usually other tools that you will use from the command line—on your system. Most Linux distributions include a package management tool, but macOS does not (and is based on BSD rather than Linux). Homebrew was created to make installing software on the macOS command line more similar to working on a Linux system. This is why the authors dub their project “the missing package manager for macOS.”

We are going to install Homebrew exactly as instructed on their website. The first step is to open Terminal from Applications → Utilities. Then use the following commands. 

**Suggestions:** A couple of things to keep in mind…

-   Be sure to read the comments but type only the commands. The pound sign # indicates comments. The commands are the text that is not prefixed by # on each line. 
-   Some commands take time to run. Give each command time to execute. Wait until you see the command prompt with a blinking cursor before typing more commands.

Here are the cmmands:

	# install Homebrew from GitHub
	
	/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
	
	# determine your username 
	
	whoami
	
	# The next commands make it easier to run homebrew from
	# the terminal. Read the output of the previous command 
	# and look for “Next steps". If the instructions you 
	# see differ from below, use what you see be sure to 
	# replace your-username with your own username
	
	echo >> /Users/your-username/.zprofile
	
	echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/your-username/.zprofile
	
	eval "$(/opt/homebrew/bin/brew shellenv)"
	
	# update homebrew
	
	brew update
	
	brew upgrade

**Software update note:** You should make a reminder to run **brew update** and **brew upgrade** regularly to keep Homebrew and the packages it installs updated. This is especially important for the security of your Mac. Once a week is great but once a month should be fine too.

## ⚙️ Install DDEV

The next step is  to install DDEV, a tool for creating local web development environments. DDEV takes care of what would otherwise be a lot of busywork to prepare your machine with the necessary software and configuration to host Drupal. You can read more about the following commands on the [DDEV install page](https://ddev.readthedocs.io/en/stable/users/install/ddev-installation/) and [Starting a project](https://ddev.readthedocs.io/en/stable/users/project/) documentation.

Commands:

	# use Homebrew to install DDEV
	
	brew install ddev/ddev/ddev
	
	# One-time initialization of mkcert
	
	mkcert -install

**Git note:** If you have previously installed git, your system may pop up a dialog that says “git-credential-osxkeychain wants to use your confidential information stored in ‘github.com’ in your keychain”. It is safe to enter your Mac password and **Always Allow** this.

“**CA” note:** After the **mkcert -install** step you will receive a notice that a new “local CA” was created and added to the “system trust store.” In this context, CA refers to _Certificate Authority_. DDEV installs a local CA so that you can access local sites over HTTPS in your browser. You can read more about this in [Configuring Browsers for DDEV Projects](https://ddev.readthedocs.io/en/stable/users/install/configuring-browsers/).

## 💧 Install Drupal

This command uses Composer via DDEV to install Drupal. We’ll use the name **waterfall** as the project name because this is used in the [course](#course), but it could be anything as long as you use hyphens between words if using more than one (e.g. **calvin-hobbes**). Choose wisely as this will become the first part of your site’s hostname (URL). These instructions are based on the  [DDEV installation guide for Drupal](https://ddev.readthedocs.io/en/stable/users/quickstart/#drupal).

Commands:

	# list the files in your home directory
	
	cd
	
	ls -al
	
	# decide where you want your local site files to live 
	# e.g. create a folder called Projects and change into 
	# it you only need to create this directory once 
	
	mkdir Projects && cd Projects
	
	# make a directory for this project and change into it
	# each project will need its own directory like this
	
	mkdir waterfall && cd waterfall
	
	# print working directory to see where you are
	
	pwd
	
	# configure a Drupal 11 project here and set the 
	# document root to web
	
	ddev config --project-type=drupal11 --docroot=web
	
	# initialize ddev in the waterfall directory
	
	ddev start
	
	# use ddev and composer to create a Drupal 11 project 
	# with the above config
	
	ddev composer create-project "drupal/recommended-project:^11"
	
	# install drush, admin_toolbar, other useful modules
	
	ddev composer require drush/drush
	
	ddev composer require drupal/admin_toolbar
	
	# these are the ones I use but you may not need them
	
	ddev composer require drupal/metatag
	
	ddev composer require drupal/pathauto
	
	ddev composer require drupal/redirect
	
	ddev composer require drupal/simple_sitemap

Behold the result!

[https://waterfall.ddev.site](https://waterfall.ddev.site/)

Open this site in your browser. This is your local copy of Drupal. Note that for this URL to work after a computer restart you need to have Docker running and you need to navigate into your project folder as above and run **ddev start**.

## 🏁 Run Drupal installer

View [https://waterfall.ddev.site](https://waterfall.ddev.site/) in your browser and complete the installation process.

**Note:** Be aware of [Securing file permissions and ownership](https://www.drupal.org/docs/administering-a-drupal-site/security-in-drupal/securing-file-permissions-and-ownership) on securing `settings.php` file for public-facing sites after completing installation. We don’t need to do this on our local copies, but it is an important step for any site that is exposed to the Internet.

## 🆕 Update Drupal

These are the basic steps to update Drupal based on the instructions at [Updating Drupal core via Composer](https://www.drupal.org/docs/updating-drupal/updating-drupal-core-via-composer). However, it is wise to back up your Drupal site first in case something goes wrong with the Drupal update. See [Back up Drupal](#back-up-drupal) for how to do this.

Commands:

	# in Terminal, navigate to your project folder
	
	cd ~/Projects/waterfall
	
	# check for Drupal updates 
	
	ddev composer outdated "drupal/*"
	
	# update drush 
	
	ddev composer update drush/drush
	
	# choose: if you want to update Drupal only specify `core-`
	
	ddev composer update "drupal/core-*" --with-all-dependencies
	
	# or: if you want to update modules as well omit `core-`
	
	ddev composer update "drupal/*" --with-all-dependencies
	
	# update database and rebuild cache
	
	ddev drush updatedb
	
	ddev drush cache:rebuild

At some point you might want to know which non-core modules you have installed. While you can check for this information in the admin UI, there’s also a command you can run that provides a simple list:

Commands:

	# list all enabled non-core modules
	
	ddev drush pm-list --type=Module --no-core --status=enabled
	
	# or: list all enabled modules including core
	
	ddev drush pm-list --type=Module --status=enabled

Knowing which non-core modules you are running can be helpful if you want to keep a record of the `composer require <package name>` modules you would want to install in a new project.

## 🗄️ Back up Drupal

It’s a good idea to back up Drupal before [updating](#update-drupal). These instructions are based on [Back up & Restore / migrate your composer-managed site using the command line](https://www.drupal.org/docs/develop/using-composer/back-up-restore-migrate-your-composer-managed-site-using-the-command-line).

### Make a database snapshot

Firstly, let’s make a snapshot of your site’s database.

The most straightforward method is to use a DDEV command called **snapshot**. While there are some limitations to **snapshot**, it is easy and will be suitable in most cases.

Commands:
	
	# in Terminal, navigate to your project folder
	
	cd ~/Projects/waterfall
	
	# Create a snapshot 
	# (will start container if not already running)
	
	ddev snapshot
	
	# That's it! Your snapshot will have the date and 
	# time appended. You can view all your snapshots 
	# using the --list option.
	
	ddev snapshot --list

Another method  is to use the Drush **sql-dump** command. While **snapshot** is dependent on your database version, **sql-dump** is not. This will rarely matter, but might be a good idea occasionally if you want to keep a more robust copy of your local site’s database that could be imported into a future version of mysql/mariadb, or to recreate your site elsewhere. This makes it more portable.

Commands

	# in Terminal, navigate to your project folder, e.g.
	
	cd ~/Projects/waterfall
	
	# create a folder where drush can put your backup file
	
	mkdir backups
	
	# export your database using a meaningful backup filename before .sql
	
	ddev drush sql-dump > backups/waterfall.sql

Finally, to be extra-sure that your backup is safe, navigate to this file in the Finder and copy it outside of the site folder, such as in Projects → **backups** folder for safe keeping. This protects it in the event your **waterfall** site directory is overwritten or damaged.

![Finder window titled Projects showing waterfall.sql in Projects - waterfall - backups. An annotation on the filename says Copy.](project-waterfall-backups-waterfall-sql.png)

![Finder window titled Projects showing waterfall.sql in Projects - backups. An annotation on the filename says Paste.](project-backups-waterfall-sql-copy.png)

**Make a copy of site files**

**Note:** This step is only necessary if your local sites aren’t in version control. If they are, you should be able to restore the site files and code from your repo.

To back up your local copy of Drupal (both the files and database) you need to copy some files and run some commands. You can use the Finder to make a copy of your site files. Navigate into the project folder, and find **web/sites/default**:

![macOS Finder window titled Projects in list view showing open path to waterfall - web - sites - default - files.](project-web-sites-default-files.png)

Make a copy of the **files** folder. For example, you could create a folder in your Projects directory called **backups** and copy/paste **files** there.

![Finder window in Projects showing a copy of files in backups](project-backups-files.png)

## 🪄 Restore Drupal

If something does go wrong with the [Drupal update](#update-drupal), and you need to restore your local Drupal site, then as long as you [Backed up Drupal](#back-up-drupal) all is not lost!

### Rebuild the project

**Tip:** If your project is still in place and you just want to change the database, skip to [Restore the database](#restore-the-database). This section is only if you have to completely rebuild from a backup.

If you are starting over completely, your first step is to rebuild the Drupal site as you originally did. Rename your original folder to something else (e.g. from “waterfall” to “waterfall-old”). 

Then revisit the [Install Drupal](#install-drupal) instructions and repeat those steps to recreate your waterfall project. Note that if you installed additional modules since first setting up your project, add those additional modules to your setup steps at this point.

For example:

	# I added these modules after initial setup 
	# so I would include them now
	
	ddev composer require drupal/simple\_sitemap
	
	ddev composer require drupal/metatag

### Copy files

Your next step is to copy your files back to their location within the site folder. 

Firstly, using the Finder, navigate to your backup versions. If you used the instructions above, they will be in  (your projects) → backups. Copy the “files” folder. Then go to (your projects) → waterfall → web → sites. Paste the files folder you just copied, overwriting any existing files folder.

Secondly, return to (your projects) → backups. Copy the SQL file (e.g.  waterfall.sql). Navigate to (your projects) → waterfall → web and make a new folder called “backups”. Open this folder and paste the SQL file here.

Now that your project is rebuilt and your files are copied back, the last step is to restore the Drupal database.

### Restore the database

To restore your database from a DDEV snapshot, run these commands:

	# navigate to your project folder
	
	cd ~/Projects/waterfall
	
	# invoke the snapshot restore command
	
	ddev snapshot restore
	
	# if you see more than one, use arrow keys to select 
	# the verison then press Enter

Easy, right? It’s probably a good idea to create snapshots before you make big configuration changes or after you create a significant amount of new content.

**Tip:** You can also use this method to test something out in Drupal and quickly roll back your changes. Just be sure to **snapshot** before you make your changes and do so again to save your new work. To roll back, use **snapshot restore**. If you want to roll forward again, it’s the same process. The tool provides an interactive mode and you use up/down arrow keys to select the version you want. A moment or two later, your local Drupal database is updated to reflect that version.

To restore your database from a sql file created using sql-dump, run these commands:

	# navigate to your project folder
	
	cd ~/Projects/waterfall
	
	# restore your database using the file you copied 
	# in the Copy files step
	
	ddev drush sqlc < backups/waterfall.sql
	
	# clear the site cache
	
	ddev drush cache:rebuild

That’s it! You should now be able to visit your site at [https://waterfall.ddev.site](https://waterfall.ddev.site/).

## 📚 Further reading

-   Designers getting started with the terminal can check out Apple’s [Terminal User Guide](https://support.apple.com/guide/terminal/welcome/mac). 
-   Drupal provides a guide to [getting started with DDEV](https://new.drupal.org/docs/drupal-cms/get-started/install-drupal-cms/install-drupal-cms-locally-with-ddev) which might suit advanced users.
-   Take a look at [Getting started with VS Code](https://code.visualstudio.com/docs/introvideos/basics), a popular free code editor from Microsoft.

## ✨ Credits

Special thanks to [Kelly Smith](https://github.com/kellysmith1008), [Jessica Straatmann](https://github.com/jastraat), and [Daniel Mundra](https://github.com/dmundra) for their assistance.