# Saudi Arabia Demographics Dashboard

![Dashboard Preview](https://img.shields.io/badge/Status-Active-brightgreen)
![Version](https://img.shields.io/badge/Version-1.0.0-blue)

> An interactive, scroll-driven data visualization dashboard presenting Saudi Arabia's demographic data through multiple chart types and animations.

## 🌟 Key Features

- **📊 Six Interactive Visualizations** - Multiple chart types including stacked bars, population pyramids, geographic maps, and comparison charts
- **🔄 Scroll-triggered Animations** - Smooth transitions between different data views using Scrollama.js
- **🌐 Arabic RTL Support** - Proper right-to-left text rendering and layout for Arabic content
- **📱 Responsive Design** - Glassmorphism UI effects with responsive layouts across all devices
- **💡 Real-time Tooltips** - Interactive hover states with detailed information tooltips
- **📖 Progressive Disclosure** - Information revealed gradually through scrolling for engaging storytelling

## 🚀 Live Demo

[View Live Dashboard](https://github.com/Anood3n/Data-Story/blob/main/index.html)

## 📋 Table of Contents

- [Technology Stack](#-technology-stack)
- [Installation](#-installation)
- [Project Structure](#-project-structure)
- [Data Architecture](#-data-architecture)
- [Core Systems](#-core-systems)
- [Chart Implementation](#-chart-implementation)
- [Styling System](#-styling-system)
- [Interactive Features](#-interactive-features)
- [Performance](#-performance)
- [API Reference](#-api-reference)
- [Development Guidelines](#-development-guidelines)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)

## 🛠 Technology Stack

| Technology | Version | Purpose |
|------------|---------|---------|
| **D3.js** | 7.8.5 | Data visualization and DOM manipulation |
| **Scrollama.js** | 3.2.0 | Scroll-triggered events |
| **HTML5** | - | Document structure |
| **CSS3** | - | Styling and animations |
| **JavaScript ES6+** | - | Application logic |

## 💾 Installation

### Prerequisites

- Modern web browser with ES6+ support
- Local web server (for development)

### Quick Start

1. **Clone the repository**
```bash
git clone https://github.com/your-username/saudi-demographics-dashboard.git
cd saudi-demographics-dashboard
```

2. **Serve the files**
```bash
# Using Python
python -m http.server 8000

# Using Node.js
npx serve .

# Using PHP
php -S localhost:8000
```

3. **Open in browser**
```
http://localhost:8000
```

### CDN Dependencies

The project uses these external libraries via CDN:

```html
<!-- D3.js for data visualization -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/7.8.5/d3.min.js"></script>

<!-- Scrollama.js for scroll interactions -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/scrollama/3.2.0/scrollama.min.js"></script>
```

## 📁 Project Structure

```
saudi-demographics-dashboard/
├── index.html              # Main HTML document
├── styles/
│   └── embedded.css        # Embedded in style tags
├── scripts/
│   ├── data.js            # Embedded demographic data
│   ├── charts.js          # Embedded Chart rendering functions
│   └── interactions.js    # Embedded Event handlers
└── assets/
    └── polygon-data.js    # Embedded Saudi Arabia border coordinates

```

## 🗄 Data Architecture

### Primary Data Structure

The dashboard uses a comprehensive data object containing demographic information:

```javascript
const data = {
  // Population composition over time
  composition: {
    2023: { مواطنون: 20, مقيمون: 10, زوار: 5 },
    2024: { مواطنون: 22, مقيمون: 12, زوار: 6 },
    2025: { مواطنون: 24, مقيمون: 14, زوار: 8 }
  },
  
  // Age and gender distribution
  ageGender: {
    ذكور: [5, 8, 7, 5, 3, 2, 1],      // Males by age group
    إناث: [4.5, 7, 6, 4.5, 3, 2, 1.5], // Females by age group
    الفئات: ['0-14', '15-24', '25-34', '35-44', '45-54', '55-64', '65+']
  },
  
  // Regional population data
  regions: {
    الرياض: 8.5,
    مكة: 9.2,
    الشرقية: 5.1,
    المدينة: 2.2,
    عسير: 2.3,
    جازان: 1.9
  },
  
  // Services and infrastructure
  services: {
    الرياض: { السكان: 8.5, مدارس: 2000, مستشفيات: 50 },
    مكة: { السكان: 9.2, مدارس: 1800, مستشفيات: 45 }
    // ... additional regions
  }
};
```

### Geographic Data

```javascript
// City coordinates and population
const saudiCities = [
  {
    name: "الرياض",
    population: 8.5,
    lat: 24.7136,
    lng: 46.6753,
    region: "الرياض"
  }
  // ... additional cities
];

// Saudi Arabia border polygon (1000+ coordinate points)
const saudi_arabia_polygon = [
  {"lng": 34.4571718, "lat": 27.9005095},
  {"lng": 34.4619585, "lat": 27.871923}
  // ... precise border coordinates
];
```

## ⚙ Core Systems

### 1. Scroll Management System

The dashboard uses Scrollama.js for intersection observer-based scroll triggering:

```javascript
// Initialize scroll listener
function init() {
  scroller.setup({
    step: ".step",           // Elements that trigger changes
    offset: 0.5,            // Trigger at 50% visibility
    debug: false            // Development mode
  }).onStepEnter(handleStep);
  
  drawChart(1);             // Initial chart
  window.addEventListener("resize", handleResize);
}

// Handle scroll events
function handleStep(response) {
  // Update active states
  document.querySelectorAll(".step")
    .forEach(s => s.classList.remove("is-active"));
  response.element.classList.add("is-active");
  
  // Trigger chart update
  drawChart(response.index + 1);
}
```

### 2. Chart Rendering Engine

Dynamic chart rendering based on scroll position:

```javascript
function drawChart(step) {
  currentChart = step;
  
  // Clear previous chart
  const container = d3.select("#chart").html("");
  
  // Setup SVG canvas
  const width = container.node().getBoundingClientRect().width;
  const height = container.node().getBoundingClientRect().height;
  const svg = container.append("svg")
    .attr("width", width)
    .attr("height", height);
  
  // Render specific chart based on step
  switch(step) {
    case 1: renderWelcome(svg, width, height); break;
    case 2: renderComposition(svg, width, height); break;
    case 3: renderAgeGender(svg, width, height); break;
    case 4: renderGeographic(svg, width, height); break;
    case 5: renderServices(svg, width, height); break;
    case 6: renderSummary(svg, width, height); break;
  }
}
```

### 3. Animation System

Smooth, staggered animations for enhanced user experience:

```javascript
// Animation configuration
const ANIMATION_CONFIG = {
  duration: {
    fast: 300,
    normal: 800,
    slow: 1500
  },
  easing: {
    bounce: d3.easeBounceOut,
    elastic: d3.easeElasticOut,
    smooth: d3.easeCubicInOut
  },
  stagger: 100  // Delay between elements
};

// Staggered animation pattern
elements.transition()
  .duration(ANIMATION_CONFIG.duration.normal)
  .delay((d, i) => i * ANIMATION_CONFIG.stagger)
  .ease(ANIMATION_CONFIG.easing.bounce)
  .attr("opacity", 1)
  .attr("transform", "scale(1)");
```

## 📊 Chart Implementation

### 1. Stacked Bar Chart (Population Composition)

**Purpose**: Visualizes population composition changes over time (Citizens, Residents, Visitors)

**Key Features**:
- Animated stacked bars
- Color-coded categories
- Interactive tooltips
- Smooth transitions

```javascript
function renderComposition(svg, width, height) {
  // Data preparation
  const years = Object.keys(data.composition);
  const keys = ["مواطنون", "مقيمون", "زوار"];
  const stack = d3.stack().keys(keys);
  const stackedData = stack(years.map(y => ({
    year: y,
    ...data.composition[y]
  })));
  
  // Scales and rendering logic
  // ... implementation details
}
```

### 2. Population Pyramid (Age/Gender Distribution)

**Purpose**: Shows age and gender distribution in horizontal bar format

**Key Features**:
- Males on right (positive values)
- Females on left (negative values)
- Age groups as categories
- Symmetrical layout with center gap

### 3. Geographic Map (Regional Distribution)

**Purpose**: Interactive map showing population distribution across Saudi regions

**Components**:
- **Base map**: SVG path from precise coordinate data
- **Choropleth regions**: Color-coded by population density
- **City markers**: Proportional circles for major cities
- **Interactive tooltips**: Hover information with regional stats

### 4. Services Comparison (Infrastructure Analysis)

**Purpose**: Multi-series bar chart comparing population vs. infrastructure

**Features**:
- Three data series: Population, Schools, Hospitals
- Normalized data for visual comparison
- Regional breakdown
- Gap analysis visualization

### 5. Strategic Summary (Recommendations Dashboard)

**Purpose**: Circular progress indicators with final recommendations

**Components**:
- Animated progress circles
- Key performance indicators
- Strategic recommendations
- Call-to-action elements

## 🎨 Styling System

### Design System

The dashboard implements a comprehensive design system with:

```css
:root {
  /* Color Palette */
  --primary-blue: #1d4289;
  --secondary-green: #0a965f;
  --accent-gold: #c99c2d;
  --text-dark: #222;
  --bg-light: #f0f8ff;
  
  /* Typography */
  --font-family: 'Segoe UI', Tahoma, sans-serif;
  --font-size-sm: 12px;
  --font-size-md: 16px;
  --font-size-lg: 24px;
  
  /* Spacing System */
  --spacing-xs: 8px;
  --spacing-sm: 16px;
  --spacing-md: 24px;
  --spacing-lg: 40px;
  --spacing-xl: 80px;
  
  /* Animation Timing */
  --transition-fast: 0.2s ease;
  --transition-normal: 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  --transition-slow: 0.8s ease-out;
}
```

### Glassmorphism Effects

Modern glass-like effects throughout the interface:

```css
.glass-effect {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
}
```

### RTL (Right-to-Left) Support

Full Arabic language support with proper text direction:

```html
<html lang="ar" dir="rtl">
```

```css
.rtl-text {
  text-align: right;
  direction: rtl;
}
```

## 🖱 Interactive Features

### Tooltip System

Advanced tooltip system with smooth animations:

```javascript
function createTooltip() {
  return d3.select("body").append("div")
    .attr("class", "tooltip")
    .style("position", "absolute")
    .style("padding", "8px 12px")
    .style("background", "rgba(0, 0, 0, 0.9)")
    .style("color", "white")
    .style("border-radius", "8px")
    .style("pointer-events", "none")
    .style("opacity", 0);
}

// Usage pattern
element.on("mouseover", function(event, d) {
  tooltip
    .html(`<strong>${d.name}</strong><br>${d.value} million`)
    .style("left", (event.pageX + 10) + "px")
    .style("top", (event.pageY - 40) + "px")
    .transition()
    .duration(200)
    .style("opacity", 1);
});
```

### Hover Effects

Smooth scale and color transitions:

```javascript
element.on("mouseover", function() {
  d3.select(this)
    .transition()
    .duration(200)
    .attr("transform", "scale(1.1)")
    .style("filter", "brightness(1.2)");
});
```

## ⚡ Performance

### Optimization Strategies

1. **Efficient DOM Updates**: Using D3's join pattern for optimal rendering
2. **Hardware Acceleration**: CSS transforms and GPU layers
3. **Memory Management**: Proper cleanup of event listeners and DOM elements
4. **Batched Updates**: RequestAnimationFrame for smooth animations

### Performance Monitoring

```javascript
function measurePerformance(chartStep) {
  const start = performance.now();
  drawChart(chartStep);
  const end = performance.now();
  console.log(`Chart ${chartStep} rendered in ${end - start}ms`);
}
```

## 📚 API Reference

### Core Functions

#### `init()`
Initializes the application and sets up scroll listeners.

#### `drawChart(step: number)`
Main chart rendering function.
- **Parameters**: `step` (1-6) - Chart step number
- **Returns**: `void`

#### `handleStep(response: Object)`
Scroll event callback function.
- **Parameters**: `response.index`, `response.element`

#### `createTooltip(): D3Selection`
Creates reusable tooltip element.

### Chart Functions

| Function | Purpose | Parameters |
|----------|---------|------------|
| `renderComposition()` | Stacked bar chart | svg, width, height |
| `renderAgeGender()` | Population pyramid | svg, width, height |
| `renderGeographic()` | Interactive map | svg, width, height |
| `renderServices()` | Services comparison | svg, width, height |

## 🛠 Development Guidelines

### Code Organization

```javascript
// 1. Constants and configuration
const CONFIG = { /* ... */ };

// 2. Data definitions
const data = { /* ... */ };

// 3. Utility functions
function createTooltip() { /* ... */ }

// 4. Chart implementations
function renderComposition() { /* ... */ }

// 5. Event handlers
function handleStep() { /* ... */ }

// 6. Initialization
function init() { /* ... */ }
```

### Naming Conventions

- **Functions**: camelCase (`drawChart`, `handleStep`)
- **Constants**: UPPER_SNAKE_CASE (`ANIMATION_CONFIG`)
- **Variables**: camelCase (`currentChart`, `tooltipElement`)
- **CSS Classes**: kebab-case (`.chart-element`, `.is-active`)

### Error Handling

```javascript
function safeDrawChart(step) {
  try {
    drawChart(step);
  } catch (error) {
    console.error('Chart rendering failed:', error);
    showFallbackContent(step);
  }
}
```

## 🐛 Troubleshooting

### Common Issues

#### Charts Not Rendering
**Symptoms**: Empty chart container
**Solutions**:
```javascript
// Check library availability
if (typeof d3 === 'undefined') {
  console.error('D3.js not loaded');
  return;
}

// Validate container dimensions
const container = d3.select("#chart");
const rect = container.node().getBoundingClientRect();
if (rect.width === 0 || rect.height === 0) {
  console.error('Container has no dimensions');
}
```

#### Scroll Events Not Triggering
**Symptoms**: Charts don't change on scroll
**Solutions**:
```javascript
// Enable debug mode
scroller.setup({
  step: ".step",
  offset: 0.5,
  debug: true  // Shows step boundaries
});
```

#### Performance Issues
**Solutions**:
```javascript
// Reduce motion for users who prefer it
const REDUCED_MOTION = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

function optimizedTransition(selection) {
  return REDUCED_MOTION ? selection.duration(0) : selection.duration(800);
}
```

### Browser Compatibility

| Feature | Chrome | Firefox | Safari | Edge |
|---------|--------|---------|--------|------|
| CSS Grid | ✅ 57+ | ✅ 52+ | ✅ 10.1+ | ✅ 16+ |
| Backdrop Filter | ✅ 76+ | ❌ | ✅ 9+ | ✅ 17+ |
| Intersection Observer | ✅ 51+ | ✅ 55+ | ✅ 12.1+ | ✅ 15+ |

## 🤝 Contributing

We welcome contributions!.

### Development Setup

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Make your changes
4. Test thoroughly
5. Submit a pull request

### Code Style

- Use ESLint and Prettier for code formatting
- Follow the existing naming conventions
- Add comments for complex logic
- Write meaningful commit messages

## 🙏 Acknowledgments

- **D3.js Community** for the powerful visualization library
- **Scrollama.js** for smooth scroll interactions
- **Saudi Arabia Vision 2030** for inspiring the demographic focus

## 📞 Support
- **Email**: alhenakiaa@gmail.com

---

<div align="center">


[⭐ Star this repo](https://github.com/Anood3n/Data-Story) | [🐛 Report Bug](https://github.com/Anood3n/Data-Story/issues) | [💡 Request Feature](https://github.com/Anood3n/Data-Story/issues)

</div>
