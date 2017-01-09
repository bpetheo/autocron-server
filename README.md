# autocron-server
Server side watcher and dispatcher for the [yii2-cron extension](https://github.com/bpetheo/yii2-cron).

These two shell scripts should be installed to a path where the cron daemon can run them. Then they should be added to the cron tab.

The watcher part searches the projects directory for yii2 projects with installed and configured yii2-cron components and creates a list which will be processed by the dispatcher. Both projects folder and list path can be configured.

The dispatcher part polls the serviceable projects' yii2-cron components. Every installed component checks if there are any jobs waiting to be run and executes them if necessary.

## Installation
Just place autocron-watch and autocron-dispatch scripts wherever cron can reach it. Make sure that it is executable by the user who runs the cron tab.

## Configation

### autocron-watch parameters
* PATH: make sure that php executable path is included
* PROJECTS_PATH: path to the directory where your repos are placed
* LIST_PATH: path to the file where the result of this script are stored

### autocron-dispatch parameters
* PATH: make sure that php executable path is included
* LIST_PATH: path to the file where the result of autocron-watch are stored

### Cron job for autocron-watch
The watch script can be quite resource intensive if you have got lots of projects and/or there is heavy IO load on your server so make sure it is not called too often. However if there is too much time between checks you might have to wait too much for a newly deployed project to be recognized. If your repo structure is static, you can run this script manually only once and then run it again avery time your repo structure updates.
```
# check for yii2cron projects every 10 minutes
*/10 * * * * cd /home/www/autocron-server/ && ./autocron-watch
```
### Cron job for autocron-dispatch
This script must be run every minute. It executes `yii cron` on every supported project. Then each cron command decides if it has any job to be run.
```
# invoke yii2cron every minute
* * * * * cd /home/www/autocron-server/ && ./autocron-dispatch
```

## Contributing
I've made this project to fit my own needs. You might have different use cases which is not covered, but feel free to extend or modify the code to make it suitable to more people. 