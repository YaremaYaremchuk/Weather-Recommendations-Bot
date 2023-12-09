# WeatherRecommendationsBot
🌤️ Telegram Weather Recommendation Bot.
This Python-based Telegram bot utilizes the OpenWeather JSON Database API to provide personalized weather forecasts and recommendations. Seamlessly integrated with Telegram, it offers location-based weather updates and helpful suggestions based on current weather conditions. 

# What can the bot do
- Location-based forecasts📍.
- Real-time weather updates🕚.
- Personalized recommendations based on weather conditions⛈️.
- Help with debugging through error logging💻.

# How to try out the bot
The bot may be easily searched in telegram by its username **@weatherrecommendbot**, where you can try out its functions.

# Showcase
<img src="https://github.com/YaremaYaremchuk/WeatherRecommendationsBot/blob/main/showcase.jpeg" width="408">


# User guide

1. Initiate the bot by using the "/start" command.
2. Inputhttps://github.com/[username]/[reponame]/blob/[branch]/image.jpg?raw=true your city's name.
3. View the displayed weather recommendations based on current conditions.


# Code Overview
```
from typing import Final
from telegram import Update
from telegram.ext import Application, CommandHandler, MessageHandler, filters, ContextTypes
import requests
import json

TOKEN_TELEGRAM = "YOUR_TELEGRAM_TOKEN"
Bot_username : Final= "YOUR_BOT_USERNAME"
TOKEN_WEATHER = "YOUR_OPENWEATHER_API_TOKEN"

async def start_com(update: Update, context: ContextTypes):
    await update.message.reply_text("Hello! I may suggest you reccommendation for current weather in your city☁️. Just type the name of it.")




async def response(update: Update, context: ContextTypes):
    text_user: str = update.message.text
    city = text_user.lower()
    if text_user=="Zair Sabino":
        text: str = "Zair Sabino is the love of my life❤️"
        await update.message.reply_text(text)
    else:
        response_weather = requests.get(f"https://api.openweathermap.org/data/2.5/weather?q={city}&appid={TOKEN_WEATHER}&units=metric")
        if response_weather.status_code == 200:
            data = response_weather.json()
            print(data)
            main = data['main']
            weather = data['weather'][0]
            curweather = weather['main']
            description = weather['description']
            curtemp = main['temp']

            curtemp = float(curtemp)
            

            if (curweather=="Thunderstorm" or curweather=="Drizzle" or curweather=="Drizzle"):
                if(curtemp>8):
                    rec_text: str = "Right now is quite rainy, you may want to take an umbrella☔️"
                else:
                    rec_text: str = "Right now is quite rainy and cold, you should take an umbrella and a coat🧥"
                

            elif(curweather=="Snow"):
                rec_text: str = "Right now is snowing, you should dress up warm🌨️🧣"

            elif(description=="scattered clouds" or description=="broken clouds" or description=="overcast clouds"):
                if(curtemp>5):
                    rec_text: str = "Right now is quite cloudy, you should wear a jacket☁️"
                else:
                    rec_text: str = "Right now is quite cold and cloudy, you should wear a coat🧥"

            elif(curweather=="Fog" or curweather=="Mist"):
                rec_text: str = "Right now is foggy, be careful on the road!🌫️"
            
            else:
                if(curtemp>8):
                    rec_text: str = "Right now is warm and sunny, you may wear a light clothes👕"
                else:
                    rec_text: str = "Right now is sunny, but quite cold, you should wear a coat or a jacket🧥"

            
            await update.message.reply_text(rec_text)
            
            
        else:
            text: str = "Can't find information for selected city"
            await update.message.reply_text(text)
        
        
        
        
    


async def error(update: Update, context: ContextTypes):
    print(f"Update {update} caused error {context.error}")


if __name__== "__main__":
    print("Start bot")
    app = Application.builder().token(TOKEN_TELEGRAM).build()
    
    app.add_handler(CommandHandler("start", start_com))
    app.add_handler(MessageHandler(filters.TEXT, response))
    app.add_error_handler(error)


    print("Start polling")
    app.run_polling(poll_interval=3)
```
# Setup & Installation

1. Copy this repository to your local machine using Git.
2. Install necessary dependencies by running the following commands:
```
pip install python-telegram-bot
pip install requests
```
3. Change personal tokens for your Telegram API token and your OpenWeather API key.
4. Now run the bot using:
```
python start.py
```
5. Go to the telegram chat with the bot and start talking with it💬

Contributions

Feel free to fork this project, open an issue, or submit pull requests. All contributions are welcome!
