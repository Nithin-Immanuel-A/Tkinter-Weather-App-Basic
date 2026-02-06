# Tkinter-Weather-App-Basic
A simple desktop Weather Application built using **Python Tkinter GUI** and **OpenWeatherMap API**. eatures - Search weather by city name - Shows: Temperature,Feels Like Temperature,Pressure,Humidity,Wind Speed,Sunrise &amp; Sunset Time,Cloud Percentage &amp; Weather Description. Technologies Used : Python,Tkinter,Requests,OpenWeatherMap API

Python Code:

from tkinter import *
import requests
from datetime import datetime

# -------------------- Window Setup --------------------
root = Tk()
root.geometry("800x600")
root.resizable(0, 0)
root.title("Weather App")

city_value = StringVar()


# -------------------- Time Format --------------------
def time_format_for_location(utc_with_tz):
    local_time = datetime.utcfromtimestamp(utc_with_tz)
    return local_time.strftime("%H:%M:%S")


# -------------------- Weather Function --------------------
def showWeather():
    api_key = "YOUR_API_KEY_HERE"

    city_name = city_value.get()

    weather_url = (
        "http://api.openweathermap.org/data/2.5/weather?q="
        + city_name
        + "&appid="
        + api_key
    )

    response = requests.get(weather_url)
    weather_info = response.json()

    tfield.delete("1.0", END)

    if weather_info["cod"] == 200:
        kelvin = 273.15

        temp = int(weather_info["main"]["temp"] - kelvin)
        feels_like_temp = int(weather_info["main"]["feels_like"] - kelvin)
        pressure = weather_info["main"]["pressure"]
        humidity = weather_info["main"]["humidity"]
        wind_speed = weather_info["wind"]["speed"] * 3.6
        sunrise = weather_info["sys"]["sunrise"]
        sunset = weather_info["sys"]["sunset"]
        timezone = weather_info["timezone"]
        cloudy = weather_info["clouds"]["all"]
        description = weather_info["weather"][0]["description"]

        sunrise_time = time_format_for_location(sunrise + timezone)
        sunset_time = time_format_for_location(sunset + timezone)

        weather = f"""
Weather of: {city_name}

Temperature: {temp} °C
Feels Like: {feels_like_temp} °C
Pressure: {pressure} hPa
Humidity: {humidity} %
Wind Speed: {round(wind_speed,2)} km/h
Cloudiness: {cloudy} %

Sunrise: {sunrise_time}
Sunset: {sunset_time}

Description: {description}
"""

    else:
        weather = f"\nWeather for '{city_name}' not found!\nPlease enter a valid city name."

    tfield.insert(INSERT, weather)


# -------------------- UI Elements --------------------
Label(root, text="Enter City Name", font="Arial 12 bold").pack(pady=10)

Entry(root, textvariable=city_value, width=30, font="Arial 14 bold").pack()

Button(
    root,
    command=showWeather,
    text="Check Weather",
    font="Arial 10",
    bg="lightblue",
    fg="black",
    activebackground="teal",
    padx=5,
    pady=5,
).pack(pady=20)

Label(root, text="The Weather is:", font="Arial 12 bold").pack(pady=10)

tfield = Text(root, width=46, height=20)
tfield.pack()

root.mainloop()


Also do install:
pip install -r requirements.txt

1.Go to https://openweathermap.org/api

2.Create account

3.Generate API Key

