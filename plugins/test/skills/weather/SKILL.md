---
name: weather
description: Get the current weather and a short forecast for a city or place. Use when the user asks about the weather, temperature, rain, or whether they need a jacket or umbrella.
---

# Weather

Look up the current conditions and a short forecast for a location using the free Open-Meteo API (no API key needed).

## Steps

1. Get the location from the user. If they did not give one, ask for a city.
2. Geocode it:

   ```bash
   curl -s "https://geocoding-api.open-meteo.com/v1/search?name=<CITY>&count=1"
   ```

   Take `latitude`, `longitude`, `name`, and `country` from the first result. If there are no results, tell the user and ask for a different spelling.
3. Fetch the weather:

   ```bash
   curl -s "https://api.open-meteo.com/v1/forecast?latitude=<LAT>&longitude=<LON>&current=temperature_2m,apparent_temperature,relative_humidity_2m,precipitation,wind_speed_10m,weather_code&daily=temperature_2m_max,temperature_2m_min,precipitation_probability_max&forecast_days=3&temperature_unit=fahrenheit&wind_speed_unit=mph&timezone=auto"
   ```

   Use `temperature_unit=celsius` and `wind_speed_unit=kmh` if the user prefers metric or the location is outside the US.
4. Translate `weather_code` into plain words (0 clear, 1-3 partly cloudy to overcast, 45/48 fog, 51-67 drizzle or rain, 71-77 snow, 80-82 showers, 95-99 thunderstorms).
5. Reply in a few short sentences: current temperature and feels-like, conditions, wind, then the next two days' highs and lows with rain chances. Add a one-line suggestion if it is useful (jacket, umbrella).

## Notes

- Keep the answer brief and conversational.
- State the resolved place name so the user can catch a wrong match (for example, Paris, Texas vs Paris, France).
