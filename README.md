Based on [Zabbix official media](https://git.zabbix.com/projects/ZBX/repos/zabbix/browse/templates/media/telegram)

Added support for hardcoded emoji in Subject.

Construct like #sev[0-5]# replaced with hardcoded emoji. Use with #sev{TRIGGER.NSEVERITY}#
1. Not classified (0) - 😶 (0x1F636)
2. Information (1) - 🤔 (0x1F914)
3. Warning (2) - 😫 (0x1F62B)
4. Average (3) - 😭 (0x1F62D)
5. High (4) - 😨 (0x1F628)
6. Disaster (5) - 😱 (0x1F631)

Construct like #sta(ok)|(problem)# replaced with hardcoded emoji. Use with #sta{TRIGGER.STATUS#}
1. OK - ✅ (0x2705)
2. Problem - ⚠️ (0xx26A0)

# Telegram webhook 

This guide describes how to send notifications from Zabbix 5.0 to the Telegram messenger using Telegram Bot API and Zabbix webhook feature.

## Supported features:
* Personal and group notifications
* Markdown/HTML support

## Not implemented:
* Graphs sending - waiting for [ZBXNEXT-5611](https://support.zabbix.com/browse/ZBXNEXT-5611)
* Emoji support

## Telegram setup

1\. Register a new Telegram Bot: send "/newbot" to @BotFather and follow the instructions. The token provided by @BotFather in the final step will be needed for configuring Zabbix webhook.

2\. If you want to send personal notifications, you need to obtain chat ID of the user the bot should send messages to.

Send "/getid" to "@myidbot" in Telegram messenger.


Ask the user to send "/start" to the bot, created in step 1. If you skip this step, Telegram bot won't be able to send messages to the user.


3\.If you want to send group notifications, you need to obtain group ID of the group the bot should send messages to. To do so:

Add "@myidbot" and "@your_bot_name_here" to your group.
Send "/getgroupid@myidbot" message in the group.
In the group chat send "/start@your_bot_name_here". If you skip this step, Telegram bot won't be able to send messages to the group.

## Zabbix setup

1\. In the "Administration > Media types" section, import the media_telegram.xml.
2\. Configure the added media type: 
Copy and paste your Telegram bot token into the "telegramToken" field.

In the `ParseMode` parameter set required option according to the Telegram's documentation. 
Read the Telegram Bot API documentation to learn how to format action notification messages: [Markdown](https://core.telegram.org/bots/api#markdown-style) / [HTML](https://core.telegram.org/bots/api#html-style) / [MarkdownV2](https://core.telegram.org/bots/api#markdownv2-style).
Note: in this case, your Telegram-related actions should be separated from other notification actions (for example, SMS), otherwise you may get plain text alert with raw Markdown/HTML tags.

Test the media type using chat ID or group ID you've got.

If you have forgotten to send '/start' to the bot from Telegram, you will get the following error:

3\.To receive notifications in Telegram, you need to create a Zabbix user and add Media with the Telegram type.
In the "Send to" field enter Telegram user chat ID or group ID obtained during Telegram setup.

Make sure the user has access to all hosts for which you would like to receive Telegram notifications.

Great, you can now start receivng Zabbix notifications in Telegram!
