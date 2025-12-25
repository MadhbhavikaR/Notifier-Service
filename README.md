# Notifier-Service
A generic Notifier Service to send various automated notifications to various channels

Monitoring tool for DietPi systems that performs periodic checks for available system updates, upgrades, and live patches.t supports configurable notifications via messaging platforms, including Telegram, Discord, Slack, Gotify, Ntfy, Pushbullet, Matrix, Mattermost, Pushover, Rocket.Chat, Pumble, Flock, Zulip, Apprise, Webntfy and Custom

### Overview

This Python script monitors the availability of system updates, upgrades, and live patches on a DietPi system. It periodically checks specific files and sends notifications through various messaging services when updates are available.

### Features

- **Update Monitoring:** Checks for available updates, upgrades, and live patches.
- **Real-time notifications with support for multiple accounts** via:
  - Telegram
  - Discord
  - Slack
  - Gotify
  - Ntfy
  - Pushbullet
  - Pushover
  - Rocket.chat
  - Matrix
  - Mattermost
  - Pumble
  - Flock
  - Zulip
  - Apprise
  - Webntfy
  - Custom webhook
- **Dynamic Configuration:** Load settings from a JSON configuration file.
- **Polling Period:** Adjustable interval for checking updates.

### Requirements
- Python 3.x
- Docker installed and running
- Dependencies: `requests`, `schedule`


### Edit config.json:
You can use any name and any number of records for each messaging platform configuration, and you can also mix platforms as needed. The number of message platform configurations is unlimited.

[Configuration examples for Telegram, Discord, Matrix, Apprise, Pumble, Mattermost, Discord, Ntfy, Gotify, Zulip, Flock, Slack, Rocket.Chat, Pushover, Pushbullet, Webntfy](docs/json_message_config.md)
```
    "CUSTOM_NAME": {
        "ENABLED": false,
        "WEBHOOK_URL": [
            "first url",
            "second url",
            "...."
        ],
        "HEADER": [
            {first JSON structure},
            {second JSON structure},
            {....}
        ],
        "PAYLOAD": [
            {first JSON structure},
            {second JSON structure},
            {....}
        ],
        "FORMAT_MESSAGE": [
            "markdown",
            "html",
            "...."
        ]
    },
```
| Item | Required | Description |
|------------|------------|------------|
| ENABLED | true/false | Enable or disable Custom notifications |
| WEBHOOK_URL | url | The URL of your Custom webhook |
| HEADER | JSON structure | HTTP headers for each webhook request. This varies per service and may include fields like {"Content-Type": "application/json"}. |
| PAYLOAD | JSON structure | The JSON payload structure for each service, which usually includes message content and format. Like as  {"body": "message", "type": "info", "format": "markdown"}|
| FORMAT_MESSAGE | markdown,<br>html,<br>text,<br>simplified | Specifies the message format used by each service, such as markdown, html, or other text formatting.|

- **markdown** - a text-based format with lightweight syntax for basic styling (Pumble, Mattermost, Discord, Ntfy, Gotify),
- **simplified** - simplified standard Markdown (Telegram, Zulip, Flock, Slack, RocketChat).
- **html** - a web-based format using tags for advanced text styling,
- **text** - raw text without any styling or formatting.


```
 "STARTUP_MESSAGE": true,
 "DEFAULT_DOT_STYLE": true,
 "MIN_REPEAT": 1
```

| Item   | Required   | Description   |
|------------|------------|------------|
| STARTUP_MESSAGE | true/false | On/Off startup message. |
| DEFAULT_DOT_STYLE | true/false | Round/Square dots. |
| MIN_REPEAT | 1 | Set the poll period in minutes. Minimum is 1 minute. | 

---
### Running as a Linux Service
You can set this script to run as a Linux service for continuous monitoring.

### Install required Python packages:

```
pip install -r requirements.txt
```

Create a systemd service file:

```
nano /etc/systemd/system/update_check_notifier.service
```

Add the following content:

```
[Unit]
Description=check update aviable
After=multi-user.target
[Service]
Type=simple
Restart=always
ExecStart=/usr/bin/python3 /opt/update_check/update_check.py
[Install]
WantedBy=multi-user.target
```

```
systemctl daemon-reload
```

```
systemctl enable update_check_notifier.service
```

```
systemctl start update_check_notifier.service
```
```sh
virtualenv -p /usr/bin/python3 ../dietpi-notifier-env
sudo nano /etc/systemd/system/update_check_notifier.service
```