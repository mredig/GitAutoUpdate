# README

### Usage

1. Copy `git_auto_update` into your repository
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
		* upon updating, it SHOULD run the new `postflight` script if any changes were made (untested)

#### Notes:
* This script assumes your repo conforms to the typical git naming scheme of `[protocol][server][path][reponame].git`
* updating is not yet implemented, so for now this is an overpowered `git clone` command
* don't forget that pre and postflight scripts must be executable!
