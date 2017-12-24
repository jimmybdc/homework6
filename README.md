# Dependencies
import csv
import matplotlib.pyplot as plt
import pandas as pd
import requests as req
from random import uniform
from citipy import citipy

# Save config information.
api_key = "25bc90a1196e6f153eece0bc0b0fc9eb"
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "metric"

# Build partial query URL
query_url = url + "appid=" + api_key + "&units=" + units + "&q="
query_url

# creating list
weather_data = []
cities = []

while len(cities)<600:
    latitude, longitude = uniform(-180,180), uniform(-90,90)
    city = citipy.nearest_city(latitude, longitude).city_name
    if city not in cities : 
        cities.append(city)

# creating dictionary and log
count = 1
weather_d = {"City":[],"Cloudiness":[],"Country":[],"Date":[],"Humidity":[],"Lat":[],"Lng":[],"Temp":[],"Wind Speed":[]}
for city in cities:
    temp = req.get(query_url + city).json()
    try:
        if temp["message"]=="city not found":
            continue
    except KeyError:
        print(str(count) + " of 500")
        print(temp.get("id"), city)
        print(query_url + city)
        weather_d["City"].append(temp["name"])
        weather_d["Cloudiness"].append(temp["clouds"]["all"])
        weather_d["Country"].append(temp["sys"]["country"])
        weather_d["Date"].append(temp["dt"])
        weather_d["Humidity"].append(temp["main"]["humidity"])
        weather_d["Lat"].append(temp["coord"]["lat"])
        weather_d["Lng"].append(temp["coord"]["lon"])
        weather_d["Temp"].append(temp["main"]["temp"])
        weather_d["Wind Speed"].append(temp["wind"]["speed"])
        weather_d
        count+=1

# Temperature (F) vs. Latitude Scatterplot
plt.scatter(weather["Temp"], weather["Lat"], c='blue', alpha=0.9, linewidth=.5)
plt.title("Temperature (F) vs. Latitude (12/23/2017)")
plt.xlabel('Temperature')
plt.ylabel('Lattitude')
plt.savefig('temperature_vs_latitude.png')
plt.show()

# Humidity (%) vs. Latitude Scatterplot
plt.scatter(weather["Humidity"], weather["Lat"], c='blue', alpha=0.9, linewidth=.5)
plt.title("Humidity (%) vs. Latitude (12/23/2017)")
plt.xlabel('Humidity')
plt.ylabel('Lattitude')
plt.savefig('humidity_vs_latitude.png')
plt.show()

# Cloudiness (%) vs. Latitude Scatterplot
plt.scatter(weather["Cloudiness"], weather["Lat"], c='blue', alpha=0.9, linewidth=.5)
plt.title("Cloudiness (%) vs. Latitude (12/23/2017)")
plt.xlabel('Cloudiness')
plt.ylabel('Lattitude')
plt.savefig('cloudiness_vs_latitude.png')
plt.show()

# Wind Speed (mph) vs. Latitude Scatterplot
plt.scatter(weather["Wind Speed"], weather["Lat"], c='blue', alpha=0.9, linewidth=.5)
plt.title("Wind Speed (mph) vs. Latitude (12/23/2017)")
plt.xlabel('Wind Speed')
plt.ylabel('Lattitude')
plt.savefig('windspeed_vs_latitude.png')
plt.show()

# creating weather data out put file
weather.to_csv("weather.csv")
