---
title: FootballIQ Typography System - Responsive Type Scale
description: Complete typography specifications optimized for mobile-first football education platform
feature: design-tokens
last-updated: 2024-12-28
version: 1.0
related-files: 
  - ../style-guide.md
  - colors.md
  - spacing.md
dependencies:
  - Inter font family
  - JetBrains Mono for monospace
status: draft
---

# FootballIQ Typography System - Responsive Type Scale

## Overview
The FootballIQ typography system prioritizes **mobile readability** and **cognitive clarity** for teenage athletes and time-pressed coaches. Every typographic decision serves the goal of making complex football concepts easily digestible across all device sizes, with particular attention to outdoor visibility and quick comprehension during high-pressure moments.

## Table of Contents
1. [Font Stack](#font-stack)
2. [Type Scale System](#type-scale-system)
3. [Responsive Typography](#responsive-typography)
4. [Typography Hierarchy](#typography-hierarchy)
5. [Usage Guidelines](#usage-guidelines)
6. [Accessibility Specifications](#accessibility-specifications)
7. [Implementation](#implementation)

## Font Stack

### Primary Font Family - Inter
**System Font Stack**:
```css
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
```

**Why Inter?**
- **Optimized for UI**: Designed specifically for user interfaces and digital screens
- **Excellent Readability**: Superior legibility at small sizes on mobile devices
- **Variable Font Support**: Efficient loading with weight variations
- **Athletic Aesthetic**: Clean, confident letterforms that convey professionalism
- **Wide Language Support**: Comprehensive character set for international expansion

### Monospace Font Family - JetBrains Mono
**System Font Stack**:
```css
font-family: 'JetBrains Mono', Consolas, 'SF Mono', 'Monaco', 'Cascadia Code', 'Roboto Mono', Courier, monospace;
```

**Usage Context**:
- Statistics and performance metrics
- Code snippets in technical documentation
- Player numbers and jersey identifiers
- Time stamps and precise measurements
- Technical configuration displays

### Font Loading Strategy
```css
@font-face {
  font-family: 'Inter';
  font-style: normal;
  font-weight: 100 900;
  font-display: swap;
  src: url('/fonts/Inter-Variable.woff2') format('woff2-variations');
}

@font-face {
  font-family: 'JetBrains Mono';
  font-style: normal;
  font-weight: 100 800;
  font-display: swap;
  src: url('/fonts/JetBrainsMono-Variable.woff2') format('woff2-variations');
}
```

## Font Weight System

### Semantic Weight Mapping
- **Light (300)**: Large display text, subtle emphasis, decorative headers
- **Regular (400)**: Body text, standard content, general reading material
- **Medium (500)**: Section headers, emphasized content, important information
- **Semibold (600)**: Component headers, navigation labels, call-to-action text
- **Bold (700)**: Page titles, critical information, high emphasis content

### Weight Usage Guidelines
```css
/* Hierarchy from lightest to boldest emphasis */
.subtle-text { font-weight: 300; }      /* Light emphasis */
.body-text { font-weight: 400; }        /* Standard content */
.emphasized { font-weight: 500; }       /* Medium emphasis */
.important { font-weight: 600; }        /* High importance */
.critical { font-weight: 700; }         /* Maximum emphasis */
```

## Type Scale System

### Desktop Type Scale (1024px+)

**Display Typography**
- **H1**: `48px/56px` (3rem/3.5rem), Bold 700, -0.025em letter-spacing
  - Usage: Page titles, hero headlines, major section headers
  - Context: Dashboard welcomes, feature announcements, primary navigation

- **H2**: `36px/44px` (2.25rem/2.75rem), Semibold 600, -0.02em letter-spacing  
  - Usage: Section headers, feature titles, content area headers
  - Context: Playbook sections, signal categories, learning modules

- **H3**: `28px/36px` (1.75rem/2.25rem), Semibold 600, -0.015em letter-spacing
  - Usage: Subsection headers, card titles, component headers
  - Context: Individual plays, drill instructions, progress sections

**Interface Typography**  
- **H4**: `22px/30px` (1.375rem/1.875rem), Medium 500, -0.01em letter-spacing
  - Usage: Component headers, form section titles, sidebar headers
  - Context: Settings sections, profile areas, navigation groups

- **H5**: `18px/26px` (1.125rem/1.625rem), Medium 500, -0.005em letter-spacing
  - Usage: Minor headers, emphasized content, list headers
  - Context: Drill names, player names, metric labels

**Body Typography**
- **Body Large**: `18px/28px` (1.125rem/1.75rem), Regular 400
  - Usage: Primary reading text, descriptions, introductory content
  - Context: Play descriptions, learning objectives, welcome messages

- **Body**: `16px/24px` (1rem/1.5rem), Regular 400
  - Usage: Standard UI text, form labels, navigation items
  - Context: Button text, menu items, general interface content

- **Body Small**: `14px/20px` (0.875rem/1.25rem), Regular 400
  - Usage: Secondary information, captions, metadata
  - Context: Timestamps, authorship, additional details

**Utility Typography**
- **Caption**: `12px/16px` (0.75rem/1rem), Medium 500
  - Usage: Metadata, timestamps, fine print, helper text
  - Context: Last updated, created by, terms links

- **Label**: `14px/20px` (0.875rem/1.25rem), Semibold 600, uppercase, 0.05em letter-spacing
  - Usage: Form labels, categories, section identifiers
  - Context: Input labels, filter categories, status indicators

- **Code**: `14px/20px` (0.875rem/1.25rem), JetBrains Mono Regular 400
  - Usage: Technical content, statistics, player numbers
  - Context: Performance metrics, configuration values, IDs

### Mobile Type Scale (320px - 767px)

**Mobile-Optimized Hierarchy**
- **H1**: `36px/44px` (2.25rem/2.75rem), Bold 700, -0.02em letter-spacing
- **H2**: `28px/36px` (1.75rem/2.25rem), Semibold 600, -0.015em letter-spacing  
- **H3**: `22px/30px` (1.375rem/1.875rem), Semibold 600, -0.01em letter-spacing
- **H4**: `20px/28px` (1.25rem/1.75rem), Medium 500, -0.005em letter-spacing
- **H5**: `18px/26px` (1.125rem/1.625rem), Medium 500, normal letter-spacing

**Mobile Body Text** (Optimized for thumb-scrolling)
- **Body Large**: `16px/26px` (1rem/1.625rem), Regular 400
- **Body**: `15px/23px` (0.9375rem/1.4375rem), Regular 400  
- **Body Small**: `13px/19px` (0.8125rem/1.1875rem), Regular 400

**Mobile Utility Text**
- **Caption**: `11px/15px` (0.6875rem/0.9375rem), Medium 500
- **Label**: `13px/19px` (0.8125rem/1.1875rem), Semibold 600, uppercase
- **Code**: `13px/19px` (0.8125rem/1.1875rem), JetBrains Mono Regular 400

### Tablet Type Scale (768px - 1023px)

**Tablet Intermediate Scaling**
- **H1**: `42px/50px` (2.625rem/3.125rem), Bold 700
- **H2**: `32px/40px` (2rem/2.5rem), Semibold 600
- **H3**: `25px/33px` (1.5625rem/2.0625rem), Semibold 600
- **Body Large**: `17px/27px` (1.0625rem/1.6875rem), Regular 400
- **Body**: `15px/23px` (0.9375rem/1.4375rem), Regular 400

## Responsive Typography Implementation

### CSS Clamp() for Fluid Scaling
```css
/* Fluid typography that scales smoothly between breakpoints */
.h1 {
  font-size: clamp(2.25rem, 4vw + 1rem, 3rem);
  line-height: clamp(2.75rem, 4vw + 1.5rem, 3.5rem);
  font-weight: 700;
  letter-spacing: -0.025em;
}

.h2 {
  font-size: clamp(1.75rem, 3vw + 0.5rem, 2.25rem);
  line-height: clamp(2.25rem, 3vw + 1rem, 2.75rem);
  font-weight: 600;
  letter-spacing: -0.02em;
}

.body {
  font-size: clamp(0.9375rem, 1vw + 0.75rem, 1rem);
  line-height: clamp(1.4375rem, 1vw + 1.25rem, 1.5rem);
  font-weight: 400;
}
```

### Responsive Breakpoint Scaling
```css
/* Mobile First Approach */
.typography-responsive {
  /* Mobile: 320px - 767px */
  font-size: 0.9375rem;    /* 15px */
  line-height: 1.4375rem;  /* 23px */
}

@media (min-width: 768px) {
  /* Tablet: 768px - 1023px */
  .typography-responsive {
    font-size: 0.9375rem;   /* 15px */
    line-height: 1.4375rem; /* 23px */
  }
}

@media (min-width: 1024px) {
  /* Desktop: 1024px+ */
  .typography-responsive {
    font-size: 1rem;        /* 16px */
    line-height: 1.5rem;    /* 24px */
  }
}
```

## Typography Hierarchy

### Information Architecture Through Type

**Primary Hierarchy (Page Level)**
1. **H1**: Page title, primary purpose identification
2. **H2**: Major section divisions, content area headers  
3. **H3**: Subsection organization, grouped content headers

**Secondary Hierarchy (Component Level)**  
4. **H4**: Component titles, card headers, form sections
5. **H5**: Item titles, list headers, secondary emphasis
6. **Body Large**: Introductory content, primary descriptions

**Tertiary Hierarchy (Content Level)**
7. **Body**: Standard content, interface text, general information
8. **Body Small**: Secondary details, supporting information
9. **Caption**: Metadata, attribution, supplementary details

### Semantic HTML Integration
```html
<!-- Proper heading structure for accessibility -->
<h1>Team Dashboard</h1>
  <h2>Recent Activity</h2>
    <h3>Signal Practice Results</h3>
    <h3>Playbook Updates</h3>
  <h2>Learning Progress</h2>
    <h3>Individual Performance</h3>
      <h4>John Smith - Quarterback</h4>
      <h4>Mike Johnson - Running Back</h4>
```

## Usage Guidelines

### Reading Optimization

**Line Length Recommendations**
- **Desktop**: 60-75 characters per line (optimal: 66)
- **Tablet**: 50-65 characters per line (optimal: 58) 
- **Mobile**: 35-50 characters per line (optimal: 42)

**Line Height Ratios**
- **Body text**: 1.5x font size minimum (1.6x optimal)
- **Headings**: 1.2x font size minimum (1.3x optimal)  
- **Captions**: 1.3x font size minimum (1.4x optimal)

### Emphasis Patterns

**Progressive Emphasis Techniques**
1. **Font Weight**: Increase weight for emphasis (400 → 500 → 600 → 700)
2. **Font Size**: Increase size within hierarchy level
3. **Color**: Use accent colors for interactive elements
4. **Letter Spacing**: Tighter spacing for premium feel (-0.025em to -0.05em)

**Emphasis Hierarchy Example**
```css
/* Low emphasis */
.text-muted {
  color: var(--color-neutral-600);
  font-weight: 400;
}

/* Standard emphasis */
.text-standard {
  color: var(--color-neutral-700);
  font-weight: 400;
}

/* Medium emphasis */
.text-emphasized {
  color: var(--color-neutral-800);
  font-weight: 500;
}

/* High emphasis */
.text-strong {
  color: var(--color-neutral-900);
  font-weight: 600;
}

/* Maximum emphasis */
.text-critical {
  color: var(--color-primary);
  font-weight: 700;
}
```

### Context-Specific Applications

**Coach Dashboard Interface**
- **Primary Actions**: Semibold 600, Accent Primary color
- **Data Tables**: Regular 400, Neutral-700, monospace for numbers
- **Status Indicators**: Medium 500, semantic colors
- **Navigation**: Semibold 600, appropriate contrast ratios

**Player Learning Interface**  
- **Instructions**: Body Large 18px for clear readability
- **Progress Text**: Medium 500 for encouragement emphasis
- **Quiz Questions**: Body 16px with clear line spacing
- **Feedback Messages**: Bold 700 with semantic colors

**Mobile Practice Interface**
- **Signal Names**: H4 22px for quick recognition
- **Timer Display**: JetBrains Mono Bold for precision
- **Action Buttons**: Semibold 600 with high contrast
- **Status Updates**: Medium 500 with clear color coding

## Accessibility Specifications

### WCAG 2.1 Compliance

**Text Size Requirements**
- **Normal Text**: 16px minimum (18px recommended for mobile)
- **Large Text**: 18px minimum (20px recommended)  
- **Touch Targets**: Text within buttons must meet 44×44px minimum area
- **Reading Distance**: Optimized for 20-24 inch viewing distance

**Contrast Requirements** (see [Color System](colors.md) for ratios)
- **Normal Text**: 4.5:1 contrast ratio minimum
- **Large Text**: 3:1 contrast ratio minimum
- **Enhanced Accessibility**: 7:1 contrast ratio for critical information

### Responsive Accessibility

**Mobile Accessibility**
- **Base Size**: 15px minimum for outdoor visibility
- **Touch-Friendly**: Button text sized for 44px minimum touch targets
- **Scanning Optimized**: Clear hierarchy for quick information scanning
- **Thumb Navigation**: Text positioned within comfortable reach zones

**Screen Reader Optimization**
- **Semantic HTML**: Proper heading structure (H1 → H6)
- **ARIA Labels**: Descriptive labels for complex typography elements
- **Reading Flow**: Logical content progression for screen readers
- **Skip Links**: Typography-based navigation shortcuts

### Cognitive Accessibility

**Information Processing**
- **Chunking**: Break complex information into digestible segments
- **Scanning**: Clear visual hierarchy for quick information retrieval
- **Comprehension**: Simple language with consistent terminology
- **Memory Load**: Reduce cognitive burden through clear typography patterns

## Implementation

### CSS Custom Properties
```css
:root {
  /* Font Families */
  --font-family-primary: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  --font-family-mono: 'JetBrains Mono', Consolas, 'SF Mono', monospace;
  
  /* Font Weights */
  --font-weight-light: 300;
  --font-weight-regular: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;
  
  /* Desktop Type Scale */
  --font-size-h1: 3rem;        /* 48px */
  --line-height-h1: 3.5rem;    /* 56px */
  --font-size-h2: 2.25rem;     /* 36px */
  --line-height-h2: 2.75rem;   /* 44px */
  --font-size-h3: 1.75rem;     /* 28px */
  --line-height-h3: 2.25rem;   /* 36px */
  
  --font-size-body-large: 1.125rem;  /* 18px */
  --line-height-body-large: 1.75rem; /* 28px */
  --font-size-body: 1rem;            /* 16px */
  --line-height-body: 1.5rem;        /* 24px */
  --font-size-body-small: 0.875rem;  /* 14px */
  --line-height-body-small: 1.25rem; /* 20px */
  
  /* Letter Spacing */
  --letter-spacing-tight: -0.025em;
  --letter-spacing-normal: 0em;
  --letter-spacing-wide: 0.05em;
}

/* Mobile Adjustments */
@media (max-width: 767px) {
  :root {
    --font-size-h1: 2.25rem;     /* 36px */
    --line-height-h1: 2.75rem;   /* 44px */
    --font-size-h2: 1.75rem;     /* 28px */
    --line-height-h2: 2.25rem;   /* 36px */
    --font-size-body: 0.9375rem; /* 15px */
    --line-height-body: 1.4375rem; /* 23px */
  }
}
```

### Utility Classes
```css
/* Typography Utility Classes */
.text-h1 {
  font-size: var(--font-size-h1);
  line-height: var(--line-height-h1);
  font-weight: var(--font-weight-bold);
  letter-spacing: var(--letter-spacing-tight);
}

.text-h2 {
  font-size: var(--font-size-h2);
  line-height: var(--line-height-h2);
  font-weight: var(--font-weight-semibold);
  letter-spacing: var(--letter-spacing-tight);
}

.text-body {
  font-size: var(--font-size-body);
  line-height: var(--line-height-body);
  font-weight: var(--font-weight-regular);
}

.text-body-large {
  font-size: var(--font-size-body-large);
  line-height: var(--line-height-body-large);
  font-weight: var(--font-weight-regular);
}

.text-caption {
  font-size: 0.75rem;
  line-height: 1rem;
  font-weight: var(--font-weight-medium);
}

.text-label {
  font-size: 0.875rem;
  line-height: 1.25rem;
  font-weight: var(--font-weight-semibold);
  text-transform: uppercase;
  letter-spacing: var(--letter-spacing-wide);
}

.text-mono {
  font-family: var(--font-family-mono);
}
```

### Tailwind CSS Configuration
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      fontFamily: {
        'sans': ['Inter', 'system-ui', 'sans-serif'],
        'mono': ['JetBrains Mono', 'Consolas', 'monospace']
      },
      fontSize: {
        'h1': ['3rem', { lineHeight: '3.5rem', letterSpacing: '-0.025em' }],
        'h2': ['2.25rem', { lineHeight: '2.75rem', letterSpacing: '-0.02em' }],
        'h3': ['1.75rem', { lineHeight: '2.25rem', letterSpacing: '-0.015em' }],
        'h4': ['1.375rem', { lineHeight: '1.875rem', letterSpacing: '-0.01em' }],
        'h5': ['1.125rem', { lineHeight: '1.625rem', letterSpacing: '-0.005em' }],
        'body-large': ['1.125rem', '1.75rem'],
        'body': ['1rem', '1.5rem'],
        'body-small': ['0.875rem', '1.25rem'],
        'caption': ['0.75rem', '1rem'],
        'label': ['0.875rem', { lineHeight: '1.25rem', letterSpacing: '0.05em' }]
      },
      fontWeight: {
        'light': 300,
        'regular': 400,
        'medium': 500,
        'semibold': 600,
        'bold': 700
      }
    }
  }
}
```

### Component Examples
```css
/* Button Typography */
.btn {
  font-size: var(--font-size-body);
  line-height: var(--line-height-body);
  font-weight: var(--font-weight-semibold);
  font-family: var(--font-family-primary);
}

/* Card Title Typography */
.card-title {
  font-size: var(--font-size-h4);
  line-height: var(--line-height-h4);
  font-weight: var(--font-weight-medium);
  letter-spacing: var(--letter-spacing-tight);
  color: var(--color-neutral-900);
}

/* Form Label Typography */
.form-label {
  font-size: 0.875rem;
  line-height: 1.25rem;
  font-weight: var(--font-weight-semibold);
  color: var(--color-neutral-800);
  text-transform: uppercase;
  letter-spacing: var(--letter-spacing-wide);
}

/* Statistics Display Typography */
.stat-value {
  font-family: var(--font-family-mono);
  font-size: 1.5rem;
  line-height: 2rem;
  font-weight: var(--font-weight-bold);
  color: var(--color-primary);
}
```

## Performance Considerations

### Font Loading Optimization
```html
<!-- Preload critical fonts for performance -->
<link rel="preload" href="/fonts/Inter-Variable.woff2" as="font" type="font/woff2" crossorigin>
<link rel="preload" href="/fonts/JetBrainsMono-Variable.woff2" as="font" type="font/woff2" crossorigin>
```

### Variable Font Benefits
- **Reduced File Size**: Single file contains all font weights
- **Smooth Weight Transitions**: Enables precise font-weight animations
- **Performance**: Fewer HTTP requests, faster loading
- **Flexibility**: Fine-tuned weight control (font-weight: 450)

## Related Documentation
- [Color System](colors.md) - Color applications for typography
- [Spacing System](spacing.md) - Spacing around typographic elements
- [Accessibility Guidelines](../../accessibility/guidelines.md) - Complete accessibility specifications
- [Component Library](../components/README.md) - Typography in component context

## Implementation Notes
This typography system provides the foundation for all text-based communication in FootballIQ. Consistent application ensures optimal readability across all devices and contexts, particularly for mobile-first usage during practice and competitive situations.

## Last Updated
Created: December 28, 2024  
Version: 1.0  
Next Review: January 15, 2025

---

**Performance Note**: This typography system is optimized for Core Web Vitals, with particular attention to Cumulative Layout Shift (CLS) prevention through proper font loading strategies.