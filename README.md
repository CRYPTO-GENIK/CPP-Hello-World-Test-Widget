# Cyberpunk.Pizza Widget Examples

This repository contains two example widgets for the Cyberpunk.Pizza homepage app, built according to the widget plugin system documentation.

These files are intended to be hosted on a service like GitHub Pages to serve as live widgets.

## Widgets

1.  **`index.html` - Theme Test Widget**: A simple utility widget to visualize the five theme colors (primary, secondary, accent, surface, text) sent by the homepage. It includes a configuration option to show or hide the hex color codes.

2.  **`weather.htm` - Mock Weather Widget**: A more complex example demonstrating various configuration types. It includes settings for a zip code, temperature units, and a forecast toggle. The weather data is static but changes based on the provided settings.

## Installation

To install these widgets on your Cyberpunk.Pizza homepage, use the "magic links" below.

### Theme Test Widget

This widget helps you test and visualize the theme colors.

**Installation Link:**
```
https://cyberpunk.pizza/?newWidget=https://crypto-genik.github.io/CPP-Hello-World-Test-Widget/index.html&showHex=bool:true
```

### Weather Widget

A mock weather widget with several configuration options.

**Installation Link:**
```
https://cyberpunk.pizza/?newWidget=https://crypto-genik.github.io/CPP-Hello-World-Test-Widget/weather.htm&zipCode=number:90210&units=select:imperial,metric&showForecast=bool:true
```

### Quicknotes Widget

A quick notes widget with autosave and markdown functionality.

**Installation Link:**
```
https://cyberpunk.pizza/?newWidget=https://crypto-genik.github.io/CPP-Hello-World-Test-Widget/notes.html&markdown=bool:false&autosave=bool:true&placeholder=text:Type your notes here...
```

## How to Use

1.  Paste one of the complete installation links above into your Cyberpunk.Pizza homepage to add the corresponding widget.
