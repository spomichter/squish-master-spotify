# Spotify Data Management and Analysis App for 🍯🌭

Continually download your spotify listening history. Gets around the 50 song history limit of the spotify API.

* Download the songs you listen to for > 30 seconds (this is your normal spotify listening history)
* ~~Generate detailed events (sub second accuracy) for spotify including play, skip, seek, pause, repeat, shuffle and spotify connect~~ (discontinued)

Also contains scripts for useful things:

* Import last three months of listening history from spotify data export (scripts/gdpr.py)
* Move songs from one playlist to another if not saved within 30 days of being added (scripts/sounds_good.py)

Coming soon:

* Big Data Analysis to tell you everything you already knew about your music taste  

## Installation 

The downloader needs to run on a server so it can execute an hourly cron job. You could run it on your computer but remember that, due to spotify limitations, if you listen to more than 50 songs with your computer turned off then some songs will be lost. I personally use Digital Ocean (the cheapest tier). The downloader uses very little resources 

1. Go to the [Spotify Developer Dashboard](https://developer.spotify.com/dashboard) and create a new application
2. In the dashboard, click on the application you just created and in the green "Edit Settings" add "http://localhost:8080" as a redirect url. 
3. Download [get_token.py] to **your computer**. 
4. Run ```$ python3 get_token.py```. Note you need to install the packages ```bottle``` and ```requests```
5. Follow the instructions, a browser should open with all the environment variables you need.

Next ssh into your server 

1. Paste the contents of step 5 (all of the variables) into your ~/.bashrc (or ~/.bash_profile) file. 
2. Reload bash via ```$ bash```
3. ```$ git clone https://github.com/spomichter/squish-master-spotify/```
4. ```pip3 install -r requirements.txt```
5. ```python3 main.py```

Next set up AWS CLI + credentials
1. Set up AWS account
2. Install AWS CLI (https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
3. ```aws configure```
4. Follow command line setup
5. Update env variable BUCKET-NAME in s3_upload.py

that should work. You can inspect main.sqlite using ```sqlite3 main.sqlite``` and run ```select * from play;``` to see the downloaded files.

Finally you need to set up a cron job to run the script every hour. Most linux distros should have a ```/etc/cron.hourly/``` folder you can put scripts into that run every hour. ***DO NOT put main.py directly in this folder***

Instead:

1. Create the following file:

```bash
$ touch /etc/cron.hourly/spotify
$ chmod 777  /etc/cron.hourly/spotify
$ nano /etc/cron.hourly/spotify
```

2. Paste the following into that file, changing the path as needed


```bash
#!/bin/bash

python3 ~/squish-master-spotify/main.py
```

Hopefully that should now work.
