# simple-telegram-bot-api
Very simple (unofficial) wrapper for the Telegram Bot API (https://core.telegram.org/bots/api)

There are two ways to use this library:
- Importing tgbotapi.py, you get a basic telegram bot
- Importing tgbot.py, you get support for commands and more things

## Tgbotapi
This is the way you work with it:

    import tgbotapi
    bot = tgbotapi.tgBot("bot1234567:asdfghjklasdfghjklasdfghjkl")

    u = bot.getUpdates(offset=1111, limit=10, timeout=20)

    if u:
        for x in u:
            chat = x["message"]["chat"]
            bot.sendMessage(chat_id=chat, text="Hello", reply_markup={"keyboard": [["Hello"], ["Hi"]], one_time_keyboard=True})




Every method of a tgBot object has the same keyword arguments, and return the same things described in [the official API documentation](https://core.telegram.org/bots/api), as a python dict, list or combination of them.

In order to send a file, you have to use the special keyword argument "files". The key is the name of the parameter that the bot API requires (in sendDocument is "document, in sendPhoto is "photo"...).

    bot.sendDocument(chat_id=1234567, files={"document": open("document.txt", "r")})

## Tgbot
It implements the same things, but adds command and polling support. Additional documentation will come soon.

    import tgbot

    bot = tgbot.tgCommandBot("YourTokenHere")

    @bot.messageHandler(command="/help", chat_type="private")
    def helpCommandHandler(bot, message, result):
        bot.sendMessage(text="This is the help message!!", chat_id=message["chat"]["id"])

    while True:
        current_offset = 0
        u = bot.getUpdates(offset=current_offset, limit=1)
        if u:
            update = u[0]
            current_offset = update["update_id"]+1
            bot.proccessMessage(update["message"])
