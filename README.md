<p align="center">
    <a href="https://github.com/missemily2022/GDriveUtils">
        <img width="200" src="https://cdn.dribbble.com/users/1501052/screenshots/5468049/searching_tickets.gif" alt="GDriveUtils">
    </a>
</p>


<p align="center">

## GDriveUtils

> A simple Telegram Bot for searching data on Google Drive. Able to clone data from Drive or GDToT links. Supports MongoDB for storing authorized users record.

</p>


### [Features]

Here's the list of features supported by the bot


• Searching on multiple TeamDrives with Recursive Search
• Cloning data from Google Drive or GDToT links
• Counting data from Google Drive or GDToT links
• Deleting data from Google Drive
• Setting data permission to 'Anyone with the link' directly from the bot
• MongoDB for storing authorized users
• Deploying with Docker
• Deploying to Heroku with GitHub Workflow
• Generating Index link of data
• Service Account (SA) for bypassing Google's limits
• Executing bash commands from the bot

### Getting started
> This guide will cover everything for deploying the bot properly. Focus and patience are needed are as always.
- Run the below script to clone the repo
```
git clone https://github.com/missemily2022/GDriveUtils
cd GDriveUtils
```

### Configuring the bot
 There are three mandatory files that are needed for the bot to run
1. [credentials.json]
2. [drive_list]
3. [config.env]

### Getting the credentials.json file
- Visit the [Google Cloud Platform](https://console.developers.google.com/apis/credentials)
- Go to the *OAuth consent screen* tab, fill the form and save it
- Then go to the *Credentials* tab and click on *Create Credentials* -> *OAuth client ID*
- Choose *Desktop* from the list and click on the *Create* button
- Now click on the *Download JSON* button to download the credential file
- Move that file to the root of the repo and rename it to **credentials.json**
- Then run the below script
```
pip3 install google-api-python-client google-auth-httplib2 google-auth-oauthlib
```

### Getting the drive_list file
- Run the below script and follow the screen to get the **drive_list** file
```
python3 dlist.py
```

### Getting the Service Account files
**Warning:** Abuse of this feature is not the aim and not recommended to make a lot of projects, just one project and 100 SAs will allow a plenty of use. It's also possible that over abuse might get the projects banned by Google.
> **NOTE:** If you have created SAs in the past from this script, you can also just re-download the keys by running the below script
```
python3 gen_sa.py --download-keys PROJECTID
```
<br>

> There are two methods available for creating Service Accounts
- [Creating SAs in existing project (Recommended)]
- [Creating SAs in new project]
##### Creating SAs in existing project (Recommended)
- List projects ids
```
python3 gen_sa.py --list-projects
```
- Enable services
```
python3 gen_sa.py --enable-services PROJECTID
```
- Create Sevice Accounts
```
python3 gen_sa.py --create-sas PROJECTID
```
- Download Sevice Accounts
```
python3 gen_sa.py --download-keys PROJECTID
```
> **Note:** Remember to replace `PROJECTID` with your project id.

##### Creating SAs in new project
- Run the below script to generate Service Accounts and download automatically
```
python3 gen_sa_accounts.py --quick-setup 1 --new-only
```

### Getting the GDToT cookies
- Login / Register to [GDToT](https://new.gdtot.eu)
- Copy the below script and paste it on the address bar
```
javascript:(function () {
  const input = document.createElement('input');
  input.value = JSON.stringify({url : window.location.href, cookie : document.cookie});
  document.body.appendChild(input);
  input.focus();
  input.select();
  var result = document.execCommand('copy');
  document.body.removeChild(input);
  if(result)
    alert('Copied cookies to clipboard');
  else
    prompt('Copy the below cookies\n', input.value);
})();
```
- Hit enter and the browser will prompt an alert
- Now check the clipboard, there will be a data like this, that's the GDToT cookies
```
{"url":"https://new.gdtot.eu/","cookie":"crypt=NGxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxWdSVT0%3D; PHPSESSID=k2xxxxxxxxxxxxxxxxxxxxj63o"}
```
- Then focus on the cookies, there will be the value of `PHPSESSID` and `CRYPT`

### Setting up the config.env file
> Rename config_sample.env to config.env

***Required config***
- `BOT_TOKEN`: Get this token by creating a bot in [@BotFather](https://t.me/BotFather)
- `OWNER_ID`: Fill the user ID of bot owner
- `DRIVE_TOKEN`: Run the below script to generate this token
```
python3 dtoken.py
```
- `DRIVE_FOLDER_ID`: Fill the folder ID of drive where the data will be cloned to

***Optional config***
- `AUTHORIZED_CHATS`: Fill the user_id and/or chat_id you want to authorize, separate them with space.<br>**Example:** *1234567890* *-1122334455* *921229569*
- `DATABASE_URL`: Create a cluster on [MongoDB](https://www.mongodb.com) to get this value
- `IS_TEAM_DRIVE`: Set to **True** if the *DRIVE_FOLDER_ID* is from a Shared Drive
- `USE_SERVICE_ACCOUNTS`: Set to **True** if the data will be cloned using Service Accounts. Refer to [Getting the Service Account files] for this to work.
- `PHPSESSID`: Refer to [Getting the GDToT cookies] to get this value
- `CRYPT`: Refer to [Getting the GDToT cookies] to get this value
- `DRIVE_INDEX_URL`: Refer to maple's [GDIndex](https://github.com/maple3142/GDIndex/) or Bhadoo's [Google Drive Index](https://gitlab.com/ParveenBhadooOfficial/Google-Drive-Index)<br>**Note:** The Index URL should not have any trailing '**/**'
- `ACCOUNTS_ZIP_URL`: Archive the **accounts** folder to a zip file. Then fill the direct download link of that file.
- `DRIVE_LIST_URL`: Upload the **drive_list** file on [GitHub Gist](https://gist.github.com). Now open the raw link of that gist and remove Commit ID from the link. Then fill the var with that link.<br>**Note:** This var is required for Deploying with [Workflow] to Heroku.


### Deploying the bot
> There are two guides available for deploying the bot
- [Deploying to Heroku](Free)
   ![Heroku](https://i.imgur.com/boSd3We.png)

> There are two methods available for deploying this bot to Heroku
- Deploying with [Workflow]
- Deploying with [CLI]

### Deploying with Workflow
- Fork the [repo](https://github.com/missemily2022/GDriveUtils)
- On the forked repo, go to *Settings* -> *Secrets* and click on the *New repository secret* button
- Now enter the vars one by one with value
1. `HEROKU_API_KEY`: Get the API Key from Heroku [Account Settings](https://dashboard.heroku.com/account)
2. `HEROKU_EMAIL`: Email address of your Heroku Account
3. `HEROKU_APP_NAME`: Name of your Heroku App. It must be unique on Heroku.
4. `CONFIG_ENV_URL`: Upload the **config.env** file on [GitHub Gist](https://gist.github.com). Now open the raw link of that gist and remove Commit ID from the link. Then fill the var with that link.
- Then go to the *Actions* tab on your repo
- Select *Deploy to Heroku* from the *All workflow* list
- Click on *Run workflow* -> *Run workflow*
- After that turn on the app dyno 
> **Note**: Don't change variables from Heroku. If you want to change any var, do it in config.env file on your gist, after that restart the app dyno.

### Deploying with CLI
- Install [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)
- Login to your heroku account
```
heroku login -i
```
- Create a new app
```
heroku apps:create APPNAME
```
- Connect the app to the repo
```
heroku git:remote -a APPNAME
```
- Change stack of the app to container
```
heroku stack:set container -a APPNAME
```
- Add the config files
```
git add -f .
```
- Commit new changes
```
git commit -m "Added config"
```
- Push the repo to heroku
```
git push heroku main --force
```
- Now restart the app dyno
```
heroku ps:scale worker=0 -a APPNAME
```
```
heroku ps:scale worker=1 -a APPNAME
```
> **Note**: Remember to replace `APPNAME` with your app's name.
- [Deploying to VPS] (Paid)
   - Install Docker Compose
```
sudo apt install docker-compose
```

- Start the bot
```
docker-compose up
```

### Searching data efficiently
> As there's a telegraph limit exists for contents, you may not get the data you were looking for. Using flags can reduce the problem most of the time. There are two flags available.
- `-f` is for searching files only
```
/search -f <query>
```
- `-d` is for searching folders only
```
/search -d <query>
```

### [Commands]

List of commands for the bot

### Commands list
```
start - Start the bot
search - Search data on drives
clone - Copy data to Drive
count - Count data of Drive
perm - Set data permission to Anyone
del - Delete data from Drive
authorize - Grant authorization of an user
unauthorize - Revoke authorization of an user
users - View authorized chats
shell - Execute bash commands
log - Get the log file
help - Get help
```

### Setting up the commands
- Open [@BotFather](https://t.me/BotFather)
- Send `/mybots` command
- Choose your bot from the list
- Click on *Edit Bot* -> *Edit Commands*
- Then send the [commands list]

### [Changelog]

**Here's the list of changes made to the bot**

- Cloned the GDriveUtils-bot repo created by Sreeraj V R, applied patches from Kaizoku
- Added option to load config.env and drive_list file externally from a direct link
- Removed TELEGRAPH_TOKEN env variable, now it will be generated automatically when the bot starts
- Removed authentication with Google Drive using token.pickle file, added DRIVE_TOKEN env variable, it will be used to authenticate with Google Drive
- Added option to load Authorized Chats from env variable, removed option for authorized_chats.txt and removed Authorization module
- Added support for deploying the bot to Heroku with GitHub Workflow
- Re-wrote the whole wiki from scratch and made it beginner friendly
- Reduced time to build the bot
- Added Shell module to execute bash commands on the bot
- Added clone module to clone data from drive
- Added count to get the size of a directory or file
- Added delete module to delete data on the drive from bot
- Added help module to get help about the bot
- Added GDToT support for cloning files from GDToT link
- Brought back authorized_chats.txt support and Authorization module
- Added MongoDB support for storing authorized users
- Added permission module for setting data permission to 'Anyone with the link' from the bot
- Added option to use token.json (DRIVE_TOKEN) file for authorization when the bot gives error while using SA

### [FAQ - Frequently Asked Questions]

### What's this bot about?
This is a telegram bot writen in python for searching data on Google Drive. Supports multiple Shared Drives (TDs). Able to clone data from drive.

### Can I deploy this bot on [Railway](https://railway.app)?
No, they will flag your account.

### Why there's no "Deploy to Heroku" button?
Deploy with [Workflow] or [CLI]. But if you need that button, follow the below steps
- Fork the [repo](https://github.com/missemily2022/GDriveUtils)
- Upload this [app.json](https://gist.github.com/missemily2022/76c65d741ba96ed7f700d764edff7c8d) file on the root directory of your forked repo
- Then add the below code in Readme.md
```
[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/<username>/GDriveUtils)
```
> **Note**: Remember to replace `<username>` with your GitHub username

### Does this bot support recursive search?
Yes, that means it can search on sub-directories too.

### Will this bot work with Folder ID?
No

### Does this bot support cloning files?
Yes, that means it can clone data from drive or gdtot links.

### How to remove commit id from a gist raw link?
For example here's a gist raw link from which we will remove the commit id
```
https://gist.githubusercontent.com/missemily2022/7o1eed3691f87f7dcah11af1ld4bdeb1/raw/92b7a460031d5v171a546ccad274f3e7f44fka46/config.env
```
Now focus on the link, there's an alphanumeric code exists after **raw** and before **config.env**. That's the commit id. Now remove that code along with a slash (**/**). So the final link will be like this
```
https://gist.githubusercontent.com/missemily2022/7o1eed3691f87f7dcah11af1ld4bdeb1/raw/config.env
```

### How to add SAs into Shared Drive
- Mount the accounts folder
```
cd accounts
```
> Grab emails form all SAs into emails.txt file

- For Windows using PowerShell
```
$emails = Get-ChildItem .\**.json |Get-Content -Raw |ConvertFrom-Json |Select -ExpandProperty client_email >>emails.txt
```
- For Linux / MacOs
```
grep -oPh '"client_email": "\K[^"]+' *.json > emails.txt
```
- Unmount the acounts folder
```
cd ..
```
- Now add emails from emails.txt to a Google Group, then add that Google Group to your Shared Drive and promote to `Manager`. After that delete email.txt file from the accounts folder.

### Getting this error - 'python3' is not recognized as an internal or external command, operable program or batch file.
Use `py` instead of `python3`

### How to fix when the bot gets stuck while searching?
Check the log file first. If it doesn't help then go [here](https://github.com/missemily2022/GDriveUtils/tree/master/bot/helper/drive_utils/gdriveTools.py#L30) and change the `telegraph_limit` value to a lower value. Default is 120.

### [Credits]

List of contributors of the bot
- [Sreeraj V R](https://github.com/SVR666) - Created the bot
- [Kaizoku](https://github.com/animekaizoku) - Applied various patches to improve performance and stability
- [Snape](https://github.com/snape541) - Fixed some bugs
- [Anas](https://github.com/anasty17) - Added count, delete and shell modules
- [Yusuf](https://github.com/oxosec) - Added GDToT support
