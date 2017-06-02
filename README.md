# README

Allows for using git as a deployment and auto update tool. Assuming your package is self contained within the git repository, it should be a fairly straight forward addition to your repository. It will automatically utilize the security and validity of `https` (use `ssh` at your own risk - see below) built into git!

You can view a working example with my [autoUpdateTest](https://github.com/mredig/autoUpdateTest) (which doubles as my testing grounds for debugging).


### Usage

1. Copy `git_auto_update` into your repository
	* you can rename it to something more suitable to your repository (but don't change it once deployed! cron won't know what to do!)
1. Edit your copy's first variables and enter your repo location and cron timer for updates
	* use `https` for security over `ssh` (if client gets compromised, ssh could allow access for hackers to server and even worse, could then compromise the main repo)
	* conforms to cron's rules, but adds the ability to randomize values:
		* `r0-59 * * * *` will run every hour of every day at a random minute value
		* `r0-29 r0-23 * * *` will run once every day at a random hour at a random minute on the first half of the hour
	* this way you can spread out load on your update server using rng
1. Using this file, you can deploy installs as well - just copy `git_auto_update` to the client and run it from the user and location you want the software running from
	* do *NOT* deploy from within the repo as it will install the repo again within itself
1. In *your* repository, you can include a `Scripts` folder with pre and postflight scripts named `preflight` and `postflight`
	1. `preflight` script will be run every time before an update is checked or performed
	1. `postflight` script will be run every time after an update is checked or performed
		* upon updating, it will run the new `postflight` script
1. To update `git_auto_update`, save the customized variables at the top of the script
1. If you need to uninstall, run `git_auto_update --uninstall`. It will disable the cron entry triggering updates and invoke the uninstall script, if you desire to perform any further actions beyond deactivating the autoupdate.



### Customized Deployment

If you want to deploy via a method such as `perl -e "$(curl -fsSL https://raw.githubusercontent.com/mredig/autoUpdateTest/master/autoUpdater)"`, you will need to hardcode the filename of the script into itself.

1. Change this line (in the user variable part of the script):
	* `$thisScript = &getExecutableName;`
1. to
	* `$thisScript = "theFilename";`

So, if you name the script `git_auto_update` it would read `$thisScript = "git_auto_update";`

#### Notes:
* This script assumes your repo conforms to the typical git naming scheme of `[protocol][server][path][reponame].git`
* don't forget that pre and postflight scripts must be executable! (that cause me a bit of a headache during my testing!)


#### TODO

* might wanna reset to HEAD? before updating?
* update logs
	* provide a variable to set a path to save a log to
