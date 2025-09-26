# CPP-Hello-World-Test-Widget
This is a test widget showcasing the features to create your own widgets.

# Widget Plugin System

A simple, iframe-based widget system for creating customizable homepage plugins with automatic theme integration.

## Overview

Widgets are standalone HTML pages loaded in iframes that receive theme colors via URL parameters. Each widget automatically inherits your homepage's theme and can be installed with a single URL.

## Quick Start

### Installing a Widget

Paste a widget URL with its configuration schema into your browser:

```
https://cyberpunk.pizza/?newWidget=https://widgets.example.com/clock.html&clockStyle=select:round,square,digital&showSeconds=bool:true&showDate=bool:false
```

Or encoded:
```
https://cyberpunk.pizza/?newWidget=https%3A%2F%2Fwidgets.example.com%2Fclock.html%26clockStyle%3Dselect%3Around%2Csquare%2Cdigital%26showSeconds%3Dbool%3Atrue
```

The installation URL includes:
- The widget source URL
- Configuration parameters with their types and defaults
- Everything needed to auto-generate settings UI

### Creating a Widget

Create an HTML file with transparent background that reads theme colors from URL parameters:

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    body {
      margin: 0;
      padding: 16px;
      background: transparent;
      color: var(--text, #ffffff);
      font-family: system-ui, -apple-system, sans-serif;
    }
  </style>
</head>
<body>
  <div>Your widget content here</div>
  
  <script>
    // Read theme from URL parameters
    const params = new URLSearchParams(window.location.search);
    const theme = {
      primary: params.get('primary') || '#6366f1',
      secondary: params.get('secondary') || '#8b5cf6', 
      accent: params.get('accent') || '#f59e0b',
      surface: params.get('surface') || '#1e293b',
      text: params.get('text') || '#f1f5f9'
    };
    
    // Apply theme colors
    Object.entries(theme).forEach(([key, value]) => {
      document.documentElement.style.setProperty(`--${key}`, value);
    });
  </script>
</body>
</html>
```

## Theme Parameters

All widgets receive these color parameters via URL query string:

| Parameter | Description | Example |
|-----------|-------------|---------|
| `primary` | Primary brand color | `#6366f1` |
| `secondary` | Secondary brand color | `#8b5cf6` |
| `accent` | Accent color for highlights | `#f59e0b` |
| `surface` | Surface/card background color | `#1e293b` |
| `text` | Main text color | `#f1f5f9` |

### Example URL
```
https://widget.com/clock.html?primary=%236366f1&secondary=%238b5cf6&accent=%23f59e0b&surface=%231e293b&text=%23f1f5f9
```

## Widget Requirements

### ✅ Must Have

1. **Transparent Background**: Widgets must use `background: transparent` on the body
2. **Theme Support**: Must read and apply the five theme color parameters
3. **Responsive**: Should work at any width
4. **Secure**: Served over HTTPS

### ❌ Must NOT

1. **No opaque backgrounds**: Don't set background colors on the body element
2. **No external tracking**: Respect user privacy
3. **No auto-playing audio**: Unless explicitly enabled by user
4. **No popup windows**: Stay within the iframe

## Widget Configuration

### Parameter Type Syntax

When creating an installation URL, specify parameter types and options using this format:

| Type | Syntax | Example | Generated UI |
|------|--------|---------|--------------|
| **select** | `name=select:opt1,opt2,opt3` | `clockStyle=select:round,square,digital` | Dropdown menu |
| **bool** | `name=bool:default` | `showSeconds=bool:true` | Checkbox |
| **text** | `name=text:default` | `title=text:My Clock` | Text input |
| **number** | `name=number:default` | `fontSize=number:16` | Number input |
| **color** | `name=color:default` | `borderColor=color:#ffffff` | Color picker |
| **range** | `name=range:min-max:default` | `opacity=range:0-100:75` | Slider |

### Example Installation URLs

**Clock Widget:**
```
https://cyberpunk.pizza/?newWidget=https://widgets.example.com/clock.html&clockStyle=select:minimal,digital,analog&format=select:12h,24h&showSeconds=bool:false
```

**Weather Widget:**
```
https://cyberpunk.pizza/?newWidget=https://weather.app/widget.html&zipCode=number:10001&units=select:imperial,metric&showForecast=bool:true
```

**Custom Text Widget:**
```
https://cyberpunk.pizza/?newWidget=https://widgets.site/text.html&content=text:Welcome&fontSize=range:12-48:24&alignment=select:left,center,right
```

### How It Works

1. **Installation**: User pastes the full configuration URL
2. **Parsing**: App extracts widget URL and parameter definitions
3. **Storage**: Only the widget URL and user's selected values are saved:
   ```json
   {
     "id": "clock-widget-1",
     "source": "https://widgets.example.com/clock.html",
     "schema": "clockStyle=select:round,square,digital&format=select:12h,24h",
     "settings": {
       "clockStyle": "digital",
       "format": "24h"
     }
   }
   ```
4. **Settings UI**: App generates form inputs based on parameter types
5. **Rendering**: Combines saved settings with theme to build final iframe URL

## Complete Widget Example

### Minimal Clock Widget

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Clock Widget</title>
  <style>
    :root {
      /* Fallback colors */
      --primary: #6366f1;
      --secondary: #8b5cf6;
      --accent: #f59e0b;
      --surface: #1e293b;
      --text: #f1f5f9;
    }
    
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      background: transparent;
      color: var(--text);
      font-family: system-ui, -apple-system, sans-serif;
      padding: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100px;
    }
    
    .clock-container {
      text-align: center;
      background: var(--surface);
      padding: 24px 32px;
      border-radius: 16px;
      border: 1px solid rgba(255, 255, 255, 0.1);
      backdrop-filter: blur(10px);
    }
    
    .time {
      font-size: 2.5rem;
      font-weight: 600;
      font-variant-numeric: tabular-nums;
      color: var(--primary);
      letter-spacing: -0.02em;
    }
    
    .date {
      font-size: 0.875rem;
      color: var(--text);
      opacity: 0.7;
      margin-top: 8px;
      text-transform: uppercase;
      letter-spacing: 0.05em;
    }
    
    .seconds {
      color: var(--accent);
      font-size: 0.8em;
      font-weight: 400;
    }
  </style>
</head>
<body>
  <div class="clock-container">
    <div class="time" id="time"></div>
    <div class="date" id="date"></div>
  </div>

  <script>
    // Parse theme from URL parameters
    const params = new URLSearchParams(window.location.search);
    const root = document.documentElement;
    
    // Apply theme colors
    ['primary', 'secondary', 'accent', 'surface', 'text'].forEach(color => {
      const value = params.get(color);
      if (value) {
        root.style.setProperty(`--${color}`, value);
      }
    });
    
    // Widget configuration from URL params
    const config = {
      format: params.get('format') || '12h',
      showSeconds: params.get('showSeconds') === 'true',
      showDate: params.get('showDate') !== 'false'
    };
    
    function updateClock() {
      const now = new Date();
      
      // Format time
      let hours = now.getHours();
      let minutes = now.getMinutes();
      let seconds = now.getSeconds();
      let period = '';
      
      if (config.format === '12h') {
        period = hours >= 12 ? ' PM' : ' AM';
        hours = hours % 12 || 12;
      }
      
      const timeStr = `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}`;
      const secondsStr = config.showSeconds ? `<span class="seconds">:${seconds.toString().padStart(2, '0')}</span>` : '';
      
      document.getElementById('time').innerHTML = timeStr + secondsStr + period;
      
      // Format date
      if (config.showDate) {
        const dateStr = now.toLocaleDateString('en-US', { 
          weekday: 'short', 
          month: 'short', 
          day: 'numeric' 
        });
        document.getElementById('date').textContent = dateStr;
      }
    }
    
    // Update immediately and then every second
    updateClock();
    setInterval(updateClock, 1000);
    
    // Listen for theme updates from parent
    window.addEventListener('message', (event) => {
      if (event.data.type === 'THEME_UPDATE') {
        Object.entries(event.data.theme).forEach(([key, value]) => {
          root.style.setProperty(`--${key}`, value);
        });
      }
    });
  </script>
</body>
</html>
```

## Sharing Widgets

### One-Line Install with Configuration

Share your widget with a complete configuration URL:

```
https://cyberpunk.pizza/?newWidget=https://your-widget.com/clock.html&format=select:12h,24h&showDate=bool:true
```

### Creating Your Installation URL

1. Start with your widget URL
2. Add parameter definitions using `&param=type:options` format
3. URL encode if needed
4. Share!

Example builder:
```javascript
const widgetUrl = "https://my-widget.com/timer.html";
const params = [
  "duration=number:60",
  "sound=select:none,bell,chime",
  "autoRepeat=bool:false"
];
const installUrl = `https://cyberpunk.pizza/?newWidget=${widgetUrl}&${params.join('&')}`;
```

### Schema Storage Efficiency

The app stores configuration efficiently:
- **On Install**: Saves the schema string and widget URL
- **Settings Modal**: Parses schema to generate UI dynamically
- **User Changes**: Only stores actual values, not the schema
- **Widget Render**: Merges theme + user settings into final URL

Example localStorage structure:
```json
{
  "widgets": [
    {
      "id": "widget-1730123456",
      "source": "https://widgets.com/clock.html",
      "schema": "format=select:12h,24h&style=select:minimal,digital",
      "zone": "top-right",
      "enabled": true,
      "settings": {
        "format": "24h",
        "style": "minimal"
      }
    }
  ]
}
```

The schema string is tiny (usually <100 bytes) and only stored once per widget type.

## Advanced Features

### Widget State Storage

Widgets can store persistent data in the parent app's localStorage, ensuring data travels with the user's homepage configuration during export/import.

#### Storage API

Each widget gets one storage slot automatically tied to its URL. No key management needed:

```javascript
// Save data to parent storage
window.parent.postMessage({
  type: 'STORAGE_SET',
  value: JSON.stringify({
    todos: [
      { id: 1, text: 'Build a widget', done: false },
      { id: 2, text: 'Share with friends', done: true }
    ],
    settings: { sortBy: 'date' }
  })
}, '*');

// Request data from parent storage
window.parent.postMessage({
  type: 'STORAGE_GET'
}, '*');

// Clear stored data
window.parent.postMessage({
  type: 'STORAGE_CLEAR'
}, '*');

// Listen for storage responses
window.addEventListener('message', (event) => {
  if (event.data.type === 'STORAGE_RESPONSE') {
    if (event.data.value) {
      const myData = JSON.parse(event.data.value);
      // Update your widget with retrieved data
      updateWidget(myData);
    }
  }
});
```

#### Storage Limits

- **Maximum storage**: 100KB per widget
- **Data type**: Must be JSON-serializable (strings, numbers, arrays, objects - no functions or circular references)
- **One slot per widget**: Automatically keyed to your widget URL

#### Complete Todo Widget Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Todo Widget</title>
  <style>
    :root {
      --primary: #6366f1;
      --secondary: #8b5cf6;
      --accent: #f59e0b;
      --surface: #1e293b;
      --text: #f1f5f9;
    }
    
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }
    
    body {
      background: transparent;
      color: var(--text);
      font-family: system-ui, -apple-system, sans-serif;
      padding: 20px;
    }
    
    .todo-container {
      background: var(--surface);
      border-radius: 12px;
      padding: 20px;
      border: 1px solid rgba(255, 255, 255, 0.1);
    }
    
    .todo-header {
      display: flex;
      gap: 8px;
      margin-bottom: 16px;
    }
    
    .todo-input {
      flex: 1;
      padding: 8px 12px;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 255, 255, 0.1);
      border-radius: 6px;
      color: var(--text);
      font-size: 14px;
    }
    
    .add-btn {
      padding: 8px 16px;
      background: var(--primary);
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-weight: 500;
    }
    
    .add-btn:hover {
      background: var(--accent);
    }
    
    .todo-list {
      list-style: none;
    }
    
    .todo-item {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 12px;
      background: rgba(255, 255, 255, 0.03);
      border-radius: 6px;
      margin-bottom: 8px;
    }
    
    .todo-item.done {
      opacity: 0.5;
    }
    
    .todo-item.done .todo-text {
      text-decoration: line-through;
    }
    
    .todo-checkbox {
      width: 20px;
      height: 20px;
      cursor: pointer;
    }
    
    .todo-text {
      flex: 1;
    }
    
    .delete-btn {
      padding: 4px 8px;
      background: rgba(239, 68, 68, 0.2);
      color: #ef4444;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 12px;
    }
    
    .empty-state {
      text-align: center;
      padding: 32px;
      opacity: 0.5;
    }
  </style>
</head>
<body>
  <div class="todo-container">
    <div class="todo-header">
      <input type="text" class="todo-input" id="todoInput" placeholder="Add a new task...">
      <button class="add-btn" onclick="addTodo()">Add</button>
    </div>
    <ul class="todo-list" id="todoList"></ul>
    <div class="empty-state" id="emptyState" style="display: none;">
      No tasks yet. Add one above!
    </div>
  </div>

  <script>
    // Parse theme from URL parameters
    const params = new URLSearchParams(window.location.search);
    const root = document.documentElement;
    
    // Apply theme colors
    ['primary', 'secondary', 'accent', 'surface', 'text'].forEach(color => {
      const value = params.get(color);
      if (value) {
        root.style.setProperty(`--${color}`, value);
      }
    });
    
    // Widget state - all data in one object
    let widgetData = {
      todos: [],
      lastUpdated: null
    };
    
    // Load widget data on startup
    function loadData() {
      window.parent.postMessage({
        type: 'STORAGE_GET'
      }, '*');
    }
    
    // Save all widget data
    function saveData() {
      widgetData.lastUpdated = Date.now();
      window.parent.postMessage({
        type: 'STORAGE_SET',
        value: JSON.stringify(widgetData)
      }, '*');
    }
    
    // Add new todo
    function addTodo() {
      const input = document.getElementById('todoInput');
      const text = input.value.trim();
      
      if (text) {
        const todo = {
          id: Date.now(),
          text: text,
          done: false
        };
        widgetData.todos.push(todo);
        input.value = '';
        renderTodos();
        saveData();
      }
    }
    
    // Toggle todo completion
    function toggleTodo(id) {
      const todo = widgetData.todos.find(t => t.id === id);
      if (todo) {
        todo.done = !todo.done;
        renderTodos();
        saveData();
      }
    }
    
    // Delete todo
    function deleteTodo(id) {
      widgetData.todos = widgetData.todos.filter(t => t.id !== id);
      renderTodos();
      saveData();
    }
    
    // Render todos to DOM
    function renderTodos() {
      const list = document.getElementById('todoList');
      const emptyState = document.getElementById('emptyState');
      
      if (widgetData.todos.length === 0) {
        list.innerHTML = '';
        emptyState.style.display = 'block';
        return;
      }
      
      emptyState.style.display = 'none';
      list.innerHTML = widgetData.todos.map(todo => `
        <li class="todo-item ${todo.done ? 'done' : ''}">
          <input type="checkbox" 
                 class="todo-checkbox" 
                 ${todo.done ? 'checked' : ''} 
                 onchange="toggleTodo(${todo.id})">
          <span class="todo-text">${todo.text}</span>
          <button class="delete-btn" onclick="deleteTodo(${todo.id})">Delete</button>
        </li>
      `).join('');
    }
    
    // Listen for storage responses and theme updates
    window.addEventListener('message', (event) => {
      if (event.data.type === 'STORAGE_RESPONSE') {
        if (event.data.value) {
          try {
            widgetData = JSON.parse(event.data.value);
            renderTodos();
          } catch (e) {
            console.error('Failed to parse saved data:', e);
            renderTodos();
          }
        } else {
          // No saved data yet, use defaults
          renderTodos();
        }
      } else if (event.data.type === 'THEME_UPDATE') {
        Object.entries(event.data.theme).forEach(([key, value]) => {
          root.style.setProperty(`--${key}`, value);
        });
      }
    });
    
    // Allow Enter key to add todo
    document.getElementById('todoInput').addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        addTodo();
      }
    });
    
    // Load data on widget init
    loadData();
  </script>
</body>
</html>
```

#### Simple Storage Pattern

For widget developers, here's the simplest pattern:

```javascript
// Your widget's entire state in one object
let myWidgetState = {
  userPreferences: {},
  cachedData: [],
  lastSync: null
};

// Save everything
function save() {
  window.parent.postMessage({
    type: 'STORAGE_SET',
    value: JSON.stringify(myWidgetState)
  }, '*');
}

// Load everything
function load() {
  window.parent.postMessage({
    type: 'STORAGE_GET'
  }, '*');
}

// Handle the response
window.addEventListener('message', (e) => {
  if (e.data.type === 'STORAGE_RESPONSE' && e.data.value) {
    myWidgetState = JSON.parse(e.data.value);
    updateUI();
  }
});
```

#### Benefits of Single-Slot Storage

- ✅ **Dead simple**: No key management, just save and load
- ✅ **No conflicts**: Each widget URL gets its own automatic namespace  
- ✅ **Export-friendly**: Widget data travels with homepage configuration
- ✅ **Transparent**: Widget devs control their entire data structure
- ✅ **Fast**: One read/write operation for all widget data

### Auto-Resize

Widgets can request height adjustments:

```javascript
// In your widget
function requestResize() {
  const height = document.body.scrollHeight;
  window.parent.postMessage({
    type: 'RESIZE',
    height: height
  }, '*');
}

// Call when content changes
requestResize();
```

### Theme Change Listener

React to live theme changes:

```javascript
window.addEventListener('message', (event) => {
  if (event.data.type === 'THEME_UPDATE') {
    const newTheme = event.data.theme;
    // Update your widget's appearance
    applyTheme(newTheme);
  }
});
```

## Implementation Example

### App-Side Parser

```javascript
// Parse installation URL
function parseWidgetInstallUrl(url) {
  const params = new URLSearchParams(url.split('?')[1]);
  const widgetUrl = params.get('newWidget');
  
  // Extract widget URL and its parameters
  const [source, ...configParts] = widgetUrl.split('&');
  const schema = configParts.join('&');
  
  // Parse schema to generate settings
  const settings = {};
  const fields = [];
  
  configParts.forEach(part => {
    const [name, definition] = part.split('=');
    const [type, ...options] = definition.split(':');
    
    let defaultValue, fieldConfig;
    
    switch(type) {
      case 'select':
        const opts = options[0].split(',');
        fields.push({ name, type, options: opts });
        settings[name] = opts[0]; // First option as default
        break;
      case 'bool':
        settings[name] = options[0] === 'true';
        fields.push({ name, type });
        break;
      case 'number':
        settings[name] = parseInt(options[0]) || 0;
        fields.push({ name, type });
        break;
      case 'text':
        settings[name] = options[0] || '';
        fields.push({ name, type });
        break;
      case 'range':
        const [range, def] = options[0].split(':');
        const [min, max] = range.split('-');
        settings[name] = parseInt(def) || parseInt(min);
        fields.push({ name, type, min, max });
        break;
    }
  });
  
  return {
    source,
    schema, // Store original schema string
    settings,
    fields // For generating UI
  };
}

// Build final widget URL with theme and settings
function buildWidgetUrl(widget, theme) {
  const params = new URLSearchParams({
    // Theme colors
    ...theme,
    // User settings
    ...widget.settings
  });
  return `${widget.source}?${params.toString()}`;
}
```

### Settings Modal Generator

```javascript
// Generate settings UI from schema
function generateSettingsUI(widget) {
  const fields = parseSchema(widget.schema);
  
  return fields.map(field => {
    switch(field.type) {
      case 'select':
        return `
          <label>${field.name}</label>
          <select data-field="${field.name}">
            ${field.options.map(opt => 
              `<option value="${opt}" ${widget.settings[field.name] === opt ? 'selected' : ''}>${opt}</option>`
            ).join('')}
          </select>
        `;
      case 'bool':
        return `
          <label>
            <input type="checkbox" data-field="${field.name}" 
                   ${widget.settings[field.name] ? 'checked' : ''}>
            ${field.name}
          </label>
        `;
      case 'number':
        return `
          <label>${field.name}</label>
          <input type="number" data-field="${field.name}" 
                 value="${widget.settings[field.name]}">
        `;
      // ... other types
    }
  }).join('');
}
```

### Parent App Storage Handler

```javascript
// In your main app
const STORAGE_LIMIT = 100 * 1024; // 100KB per widget

window.addEventListener('message', (event) => {
  const iframe = Array.from(document.querySelectorAll('iframe'))
    .find(f => f.contentWindow === event.source);
  
  if (!iframe) return;
  
  // Use the iframe's src URL as the storage key
  const widgetUrl = new URL(iframe.src).origin + new URL(iframe.src).pathname;
  const storageKey = `widget_data_${btoa(widgetUrl)}`;
  
  switch(event.data.type) {
    case 'STORAGE_SET':
      const value = event.data.value;
      
      // Check size limit
      if (value && value.length > STORAGE_LIMIT) {
        console.error('Storage limit exceeded (100KB max)');
        return;
      }
      
      // Save data
      try {
        localStorage.setItem(storageKey, value);
      } catch (e) {
        console.error('Failed to save widget data:', e);
      }
      break;
      
    case 'STORAGE_GET':
      // Retrieve and send back data
      const storedValue = localStorage.getItem(storageKey);
      event.source.postMessage({
        type: 'STORAGE_RESPONSE',
        value: storedValue
      }, '*');
      break;
      
    case 'STORAGE_CLEAR':
      // Clear widget data
      localStorage.removeItem(storageKey);
      break;
  }
});
```

## Security Considerations

### For Widget Users

⚠️ **Only install widgets from trusted sources**

Widgets run in sandboxed iframes but can still:
- Make network requests
- Store cookies/localStorage (in their own origin)
- Run JavaScript code

### For Widget Developers

Best practices:
- Don't collect user data without permission
- Use HTTPS for all resources
- Keep widgets lightweight (< 100KB)
- Test with different theme colors
- Handle errors gracefully

## Hosting Widgets

### Free Hosting Options

1. **GitHub Pages**: Free, reliable, perfect for open-source widgets
2. **Netlify/Vercel**: Great for widgets that need build steps
3. **CodePen/JSFiddle**: Quick prototypes (use full page view URL)
4. **Cloudflare Pages**: Fast, global CDN

### Example: GitHub Pages

1. Create a new repository
2. Add your widget HTML file as `index.html`
3. Enable GitHub Pages in settings
4. Share your widget URL:
   ```
   https://cyberpunk.pizza/?newWidget=https://username.github.io/clock-widget/
   ```

## Widget Gallery

Submit your widget to our gallery by opening a PR:

```markdown
## Widget Name
- **Author**: @yourusername
- **Install**: `https://cyberpunk.pizza/?newWidget=YOUR_WIDGET_URL`
- **Description**: Brief description
- **Settings**: List any configuration options
```

## Troubleshooting

### Widget Not Loading

- Check browser console for errors
- Ensure widget is served over HTTPS
- Verify URL encoding if using special characters

### Theme Colors Not Applying

- Ensure you're reading parameters from `window.location.search`
- Check parameter names match exactly: `primary`, `secondary`, `accent`, `surface`, `text`
- Use fallback colors in your CSS

### Widget Looks Wrong

- Test with different theme colors
- Ensure transparent background on body element
- Check responsive design at different widths

## Support

- **Issues**: Open an issue on our GitHub repo
- **Discord**: Join our community for help
- **Examples**: Check `/examples` folder for more widget templates
