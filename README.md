# Cloudy Pie
`Python` `Raspberry Pi` `IoT` `LED Control` `Weather API`

**Card Summary:**
A DIY weather forecast lamp that uses Raspberry Pi and LED strips to display weather conditions visually.

## Context & Story
Cloudy Pie is a remake of DJAkbar's Cloudy-A project, designed to create a visually appealing weather forecast lamp. The project uses a Raspberry Pi to control LED strips, which change colors and patterns based on weather data fetched from the Weather Underground API. This project is ideal for hobbyists interested in IoT and creative lighting solutions.

## Architecture & Decisions
- **Python**: Chosen for its simplicity and extensive library support.
- **Raspberry Pi**: Selected for its GPIO capabilities and affordability.
- **Weather Underground API**: Provides reliable weather data for the lamp.
- **LED Control**: Utilizes `pigpio` for precise control of RGB LED strips.
- **DIY Design**: Focused on accessibility with common household and craft materials.

## Key Features
- Fetches real-time weather data using the Weather Underground API.
- Displays weather conditions (e.g., sunny, cloudy, rainy) with dynamic LED animations.
- Supports customization of brightness and update intervals.
- Includes a web interface for remote control (optional).
- Modular design for easy hardware and software modifications.

## Quick Start
1. **Hardware Setup**:
   - Assemble the Raspberry Pi, LED strips, and other components as per the [tutorial](http://popoklopsi.github.io/RaspberryPi-LedStrip/#!/).
   - Ensure the Raspberry Pi has internet connectivity.

2. **Software Setup**:
   - Install Python and `pigpio` on the Raspberry Pi.
   - Clone this repository:
   - Install dependencies:
     ```bash
     pip install -r requirements.txt
     ```

3. **Configuration**:
   - Add your Weather Underground API key and location in the script.
   - Adjust brightness and update intervals as needed.

4. **Run the Script**:
   ```bash
   python cloudy-3.py
   ```

5. **Optional**:
   - Set up the web interface for remote control.

For detailed instructions, refer to the original [Cloudy-A project](https://github.com/DJAkbar/cloudy-a).
