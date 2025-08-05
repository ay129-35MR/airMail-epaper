# airMail-epaper
Automated system that parses Airbnb booking emails, extracts guest info, and displays it on e-paper tags via Home Assistant.  Ideal for short-term rental hosts who want a stylish, automated welcome display.

### System Overview

This project connects Airbnb guest booking data to e-paper tags in your home.  Airbnb API isnt available to me and I cant justify the cost of Hospitable or other such services.   I already use the excellent Rental control https://github.com/tykeal/homeassistant-rental-control integration, but that does not show guest names.  However, I use Gmail as my airbnb inbox and get emails from airbnb there, so an automated way of getting this info is helpful.
I run home assistant on raspberry Pi 4B with attached SSD, and have a separate plex server on an always-on Windows 11 n150 NUC.  

### Flow:

1. **Source: Airbnb Emails**
   - Google Apps Script scans Gmail inbox once daily.
   - Parses Airbnb confirmation/reminder emails.

2. **Trigger: Google Apps Script**
   - Runs on a daily schedule (e.g., 11:50 AM).
   - Sends current booking info to a Flask webhook (JSON payload).

3. **Receiver: Flask Web Server on my n150 NUC - on the same network as the Pi running home assistant**
   - Runs on local device (e.g., Raspberry Pi).
   - Receives JSON and updates Home Assistant via REST API.

4. **Tunnel: ngrok (or alternative)**
   - Exposes local Flask server to the internet.
   - Used as the webhook URL for Google Apps Script.

5. **Automation: Home Assistant running on Pi 4b**
   - Detects webhook changes.
   - Updates e-paper tags using:
     - `gicisky.write` (for B/W tags)
     - `openepaperlink` (for color tags)
