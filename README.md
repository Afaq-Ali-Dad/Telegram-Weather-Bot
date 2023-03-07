# Telegram-Weather-Bot
This is a telegram which interact with people on telegram chat and can give you information about current weather of any city. Basic concept of using APIs and HTTP request is required to complete this project.
here is the code:

REMEMBER TO GET TELEGRAM API FROM BOTFATHER OF TELEGRAM APPLICATION

import logging
from telegram.ext import ApplicationBuilder, CommandHandler, MessageHandler, filters
import requests
from KEYWORD_RESPONSES import KEYWORD_RESPONSES
import tracemalloc
import traceback


def error_handler(e):
    print("An error has occurred:", e)
    traceback.print_exc()


# try:
# Code that may raise an exception
# raise Exception("This is a test exception")
# except Exception as e:
# error_handler(e)


# Start tracemalloc
tracemalloc.start()

# Get the current traceback
current, peak = tracemalloc.get_traced_memory()
print(f"Current memory usage is {current / 10 ** 6}MB; Peak was {peak / 10 ** 6}MB")

API_KEY = "Telegram API token"
api_key = "open weather api key"
base_url = "http://api.openweathermap.org/data/2.5/weather?"



# Set up the logging.basic-config
logging.basicConfig(
    format="%(asctime)s - %(name)s - %(levelname)s - %(message)s", level=logging.INFO
)
logging.info("Starting Bot...")


async def start_command(update,):
    await update.message.reply_text(
        "Hello and welcome, I am a Weather Bot. If you wanna know weather, enter city name")


async def handle_message(update, context):
    text = str(update.message.text).lower()
    logging.info(f"User ({update.message.chat.id}) says: {text}")

    response = KEYWORD_RESPONSES.get(text)
    if response:
        await context.bot.send_message(chat_id=update.effective_chat.id, text=response)
    else:
        city = text.strip()
        url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={api_key}"
        weather_response = requests.get(url)
        weather_data = weather_response.json()

        if weather_data["cod"] != "404":
            temp = int(weather_data["main"]["temp"] - 273.15)  # rounding to whole numbers
            humidity = weather_data["main"]["humidity"]
            wind_speed = weather_data["wind"]["speed"]
            weather = weather_data["weather"][0]["description"]

            response = f"Temperature: {temp}°C\nHumidity: {humidity}%\nWind Speed: {wind_speed} m/s\nWeather: {weather}"
        else:
            response = "I am sorry I couldn't understand, please write the correct name of city to find weather details."

        await context.bot.send_message(chat_id=update.effective_chat.id, text=response)




# Run the programme
if __name__ == "__main__":
    application = ApplicationBuilder().token(API_KEY).build()
    # Commands

    application.add_handler(CommandHandler("start", start_command))

    # Messages
    application.add_handler(MessageHandler(filters.TEXT, handle_message))

    # Run the bot
    application.run_polling()
    # updater.idle()
    
    
    
    
    Here are the KEYWORD RESPONSES
    
    KEYWORD_RESPONSES = {
    "hello": "Hello there!",
    "hi": "hey there",
    "how are you?": "I'm doing well, thank you! what about you?",
    "what is your name?": "My name is The Mosam Bot",
    "what is the weather now?": "May I know the name of your city please",
    "thank you": "You are welcome",
    "thanks": "welcome",
    "good": "okay",
    "bye": "good bye",
    "how are you": "I'm doing well, thank you! what about you?",
    "what is your name": "My name is The Mosam Bot",
    "what is the weather now": "May I know the name of your city please",
    "I am good": "okay",
    "good bye": "good bye"

}

Now you can run and start your bot.
