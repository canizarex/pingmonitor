# Pingmonitor

Pingmonitor is a bash script that can ping several targets at regular
intervals and send notifications using a Telegram bot when a host is down/up.

Its small size and few dependencies makes it a good fit for home routers.

## Requirements

This script needs bash 4 and curl to work.

## Installation

TL;DR: get the token and chat id, paste them in the config file, move the
config file to ```/etc``` and create a new service.

### Get the files

First clone the repo:

```shell
git clone https://github.com/canizarex/pingmonitor.git
```

or download a copy if git is not installed in your device:

```shell
wget https://github.com/canizarex/pingmonitor/archive/master.zip
```

### Create a new bot in Telegram

Start a new conversation with @botfather and send the comand ```/newbot```. Then
follow the instructions until you get your token.

As per the official documentation: _"The token is a string along the lines of 110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw that is required to authorize the bot and send requests to the Bot API. Keep your token secure and store it safely, it can be used by anyone to control your bot."_

See detailed instructions here: <https://core.telegram.org/bots>

### Get the chat id

Now that your bot is created, start a conversation with it and get the
chat_id of that conversation using curl:

```shell
curl https://api.telegram.org/bot<your_token>/getUpdates
```

Once you have the response, look for a string like the one below:

```shell
"chat":{"id":123456789,
```

### Prepare the config file

The config file just needs the token and the chat id, so copy them into the
pingmonitor.conf file. It should look like this:

```config
token="110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw"
chat_id=123456789
```

Now copy the config file to ```/etc``` since the script expects it to be there.

### Create a new service

The script is now ready to run.

However, you may want to run it as a service. If that's the case, just keep in mind that the targets
to be monitored are passsed to the script as arguments, so you should have something like this in your service file:

```shell
ExecStart=/usr/local/bin/pingmonitor example1.com example2.com example3.com
```

or

```shell
procd_set_param command /usr/bin/pingmonitor example1.com example2.com example3.com
```

Take a look to the [templates](/service-templates) folder to see some service file templates.
