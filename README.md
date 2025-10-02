# Cyberpunk.Pizza Widget Examples

This repository contains two example widgets for the Cyberpunk.Pizza homepage app, built according to the widget plugin system documentation.

These files are intended to be hosted on a service like GitHub Pages to serve as live widgets.

## Available Widgets

Here is a list of the available widgets in this repository.

*   **Theme Test (`index.html`)**: A simple utility to visualize theme colors.
*   **Weather (`weather.htm`)**: A mock weather widget with configuration options.
*   **Quick Notes (`notes.html`)**: A persistent notes widget with markdown support.
*   **Calendar (`calendar.html`)**: A monthly calendar with theme and configuration options.
*   **Clock (`clock.html`)**: A digital/analog clock that is themeable and configurable.

---

## Widget Details & Installation Links

To install these widgets on your Cyberpunk.Pizza homepage, use the example "magic links" below.

### Theme Test Widget
A simple utility widget to visualize the five theme colors.
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

### Quick Notes Widget
A quick notes widget with autosave and markdown functionality.
**Installation Link:**
```
https://cyberpunk.pizza/?newWidget=https://crypto-genik.github.io/CPP-Hello-World-Test-Widget/notes.html&markdown=bool:false&autosave=bool:true&placeholder=text:Type your notes here...
```

### Calendar Widget
A monthly calendar that can be themed and configured.
**Installation Link:**
```
https://cyberpunk.pizza/?newWidget=https://crypto-genik.github.io/CPP-Hello-World-Test-Widget/calendar.html&startOfWeek=select:sunday,monday&highlightWeekends=bool:true
```

### Clock Widget
A themeable clock with analog and digital faces.
**Installation Link:**
```
https://cyberpunk.pizza/?newWidget=https://crypto-genik.github.io/CPP-Hello-World-Test-Widget/clock.html&style=select:digital,analog&showSeconds=bool:true&hourFormat=select:12,24
```
