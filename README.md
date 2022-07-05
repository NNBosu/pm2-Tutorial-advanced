# What is pm2?
* pm2 is a Process Manager tool, made for Windows, Linux and Mac

* [Website](https://pm2.keymetrics.io/) | [Docs](https://pm2.keymetrics.io/docs/usage/quick-start/)

* The Usage is everywhere the same

# Installation
* First install [nodejs v12.7 or higher (Suggest is current: 16.13)
* Then type: ```npm i -g pm2```

# Usage - node.js Processes
* It is really simple and based on a schema: ```pm2 [command] [ID/NAMESPACE/all]```

# Starting a process
* go into the folder of the process you want to start
* type: ```pm2 start index.js``` (replace index.js with the file you want to start),
* This also works: ```pm2 start .```
* ```pm2 start index.js --name My_Custom_Name_Process``` (replace My_Custom_Name_Process with your wished process Name, NO SPACES ALLOWED)
* the #4 option is very recommended, because it gives your process a name which is shown in ```pm2 list```
* You can also start already stopped processes: ```pm2 start ID``` (replace the ID with the PROCESS Id which you get from ```pm2 list```, for example: ```pm2 start 1```

# Showing all Processes
* type: ```pm2 list```
* This will show you all Processes, with their Name, ID and current state online/offline
* Sorting them:
* ```pm2 list --sort name:desc``` Or in general:
* ```pm2 list --sort [name|id|pid|memory|cpu|status|uptime][:asc|desc]```

# Showing a specific Process:
* This Shows details about a specific Process
* ```pm2 show ID``` (replace the ID with the PROCESS Id which you get from ```pm2 list```, for example: ```pm2 show 1```

# Stopping a Process
* ```pm2 stop ID``` (replace the ID with the PROCESS Id which you get from ```pm2 list```, for example: ```pm2 stop 1```
* ```pm2 stop all``` (this stops all processes)
* You could also use the process name for this Command, but is not suggested..

# Deleting a Process
* ```pm2 delete ID``` (replace the ID with the PROCESS Id which you get from ```pm2 list```, for example: ```pm2 delete 1```
* ```pm2 delete all``` (this deletes all processes)
* You could also use the process name for this Command, but is not suggested..

# Restarting a Process
* This restarts a process, if it's already started / stopped
* ```pm2 restart ID``` (replace the ID with the PROCESS Id which you get from ```pm2 list```, for example: ```pm2 restart 1```
* ```pm2 restart all``` (this restarts all processes)

# Watching all Processes
* You can either use the [Website Dashboard](https://app.pm2.io/#/register), but this only monitors / shows up to 5 processes (for free)
* So use: ```pm2 dash``` / ```pm2 monit```, there you can navigate through windows, but i would suggest you to "just" stay in the process window and use your arrow * * keys up and down, to show the logs of each single process

# Showing Logs
* type: ```pm2 log```, this will show all pm2 logs of all processes including the process name of each process + a few older log lines
* To few the logs of a specific process type:
* ```pm2 log ID``` (replace the ID with the PROCESS Id which you get from ```pm2 list```, for example: ```pm2 log1```

# Restarting the System, but keeping the processes?
* Very simple, before the restart type: ```pm2 save```
* After the restart type: ```pm2 resurrect``` to get the proccesses + their current states back
* ATTENTION It is suggested to stop the processes before saving, especially if you have many, so that your vps / pc doesn't get overloaded
* ```pm2 stop all``` --- ```pm2 save``` --- ```pm2 resurrect```
* You can type: ```pm2 startup``` If you don't want to use ```pm2 resurrect```

# Starting Java / Python Processes
* Java: ```pm2 start java -- -jar JavaFile.jar```, With name: ```pm2 start java --name Process_Name -- -jar JavaFile.jar```
* Python: ```pm2 start python -- File.py```, With name: ```pm2 start python --name Process_Name -- File.py```

# Reset the ID Counters:
* ```pm2 reset all``` This resets the id, because if you deleted a process, the id don't change

# Other Useful Options:
* ```pm2 start index.js --name Process_Name``` gives the process a specific name
* ```pm2 start index.js --max-memory-restart 2G``` change the maximum allowed memory, before the process restarts (default 100M / 1G)
* ```pm2 start index.js --cron-restart="0 0 * * *"``` Restart the process every day at 0:0 , using [cronjob](--cron-restart="0 0 * * *") rules...
* ```pm2 start index.js --watch``` This auto restarts a process, on file changes
* ```pm2 start index.js --restart-delay=3000``` Set a delay between auto restart with the Restart Delay strategy
* ```pm2 start index.js --no-autorestart``` This is useful in case we wish to run 1-time scripts and don’t want the process manager to restart our script in 
* case it’s completed running.
* ```pm2 start index.js --exp-backoff-restart-delay=100``` A new restart mode has been implemented on PM2 Runtime, making your application restarts in a smarter way. Instead of restarting your application like crazy when exceptions happens (e.g. database is down), the exponential backoff restart will increase incrementaly the time between restarts, reducing the pressure on your DB or your external provider… Pretty easy to use

# Using a Configuration File
When managing multiple applications with PM2, use a JS configuration file to organize them

* pm2 init simpleThis will generate a sample ecosystem.config.js
```js
module.exports = {
  apps : [{
    name   : "app1",
    script : "./app.js"
  }]
}
```
Now start the processes via: ``pm2 start ecosystem.config.js`` This will apply all options inside of the file! Example completed file: [All options](https://pm2.keymetrics.io/docs/usage/application-declaration/)

```js
module.exports = {
  apps : [{
    name: `Discord_Bot_Bosu_Test_Bot`, //the Namespace
    script: 'index.js', //the path where to start
    //max_restarts: 5, //to limit the maximum restarts... not suggested
    cron_restart: "0 3 * * *", //to restart at 3:00
    max_memory_restart: "1G", //restart at 1GB of Ram
    watch: true, //restart on file changes
  }]
};
```

# IMPORTANT INFOS:
pm2 is making many logs, you should regularly clear them if u wanna save storage! To clear them type:
```
pm2 flush
```
here is an automated script-module which clears those automatically every X time (cronjob timing system)
```
pm2 install pm2-logrotate
pm2 set pm2-logrotate:max_size 10M
pm2 set pm2-logrotate:compress true
pm2 set pm2-logrotate:rotateInterval '0 */1 * * *'
```

# Hover over the Boxes then you can copy paste it in the console, everything will be done automatically!
> ***Note that if you do it without SUDO you might not have permissions to execute a specific Command***
> *Make sure to be in the User: "root" by typing `su root` / `su` - / `sudo su`*
