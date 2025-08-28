---
title: FootballIQ Design System - Complete Style Guide
description: Comprehensive visual specifications and implementation guide for the FootballIQ platform
feature: design-system
last-updated: 2024-12-28
version: 1.0
related-files: 
  - ../tokens/colors.md
  - ../tokens/typography.md
  - ../tokens/spacing.md
  - ../components/README.md
status: draft
---

# FootballIQ Design System - Complete Style Guide

## Design Philosophy

FootballIQ's design system embodies **bold simplicity** with **football-first UX**, creating interfaces that feel as natural as calling plays on the field. Our visual language balances the intensity of football with the clarity needed for effective learning, ensuring every design decision serves the ultimate goal of accelerating player development.

### Core Visual Principles
- **Athletic Confidence**: Bold, purposeful design that inspires trust and performance
- **Cognitive Clarity**: Strategic information hierarchy that reduces mental load during high-pressure moments
- **Mobile Supremacy**: Every design optimized for mobile-first usage during practice and games
- **Accessibility Foundation**: Universal design ensuring all players and coaches can succeed
- **Performance Obsession**: Visual decisions prioritize speed and responsiveness over decoration

## 1. Color System

### Primary Colors - Football Field Inspired

**Primary Green** (Field)
- `Primary`: `#2F6B3B` – Main CTAs, success states, brand elements
- `Primary Dark`: `#1E4525` – Hover states, emphasis, active navigation
- `Primary Light`: `#4A8F5F` – Subtle backgrounds, highlights, disabled states

**Secondary Gold** (Championship)
- `Secondary`: `#D4A574` – Supporting elements, secondary CTAs
- `Secondary Light`: `#E8C5A0` – Backgrounds, subtle accents, section dividers
- `Secondary Pale`: `#F5E5D3` – Selected states, highlights, notification backgrounds

### Accent Colors - Team Spirit

**Accent Blue** (Leadership)
- `Accent Primary`: `#1E3A8A` – Important actions, coach-specific features
- `Accent Secondary`: `#3B82F6` – Links, interactive elements, information highlights

**Gradient System**
- `Gradient Start`: `#2F6B3B` – For hero sections and premium features
- `Gradient End`: `#1E4525` – Creates depth and visual interest

### Semantic Colors - Clear Communication

**Success** (Victory)
- `Success`: `#10B981` – Positive actions, correct answers, achievements
- `Success Light`: `#D1FAE5` – Success backgrounds, positive notifications

**Warning** (Caution)
- `Warning`: `#F59E0B` – Caution states, pending actions, important alerts
- `Warning Light`: `#FEF3C7` – Warning backgrounds, attention sections

**Error** (Penalty)
- `Error`: `#DC2626` – Errors, incorrect answers, destructive actions
- `Error Light`: `#FEE2E2` – Error backgrounds, validation messages

**Info** (Strategy)
- `Info`: `#3B82F6` – Informational messages, tips, neutral actions
- `Info Light`: `#DBEAFE` – Information backgrounds, help sections

### Neutral Palette - Foundation

**Text Hierarchy**
- `Neutral-900`: `#111827` – Primary text, headings
- `Neutral-800`: `#1F2937` – Secondary text, strong emphasis
- `Neutral-700`: `#374151` – Body text, standard content
- `Neutral-600`: `#4B5563` – Muted text, descriptions
- `Neutral-500`: `#6B7280` – Placeholder text, subtle content
- `Neutral-400`: `#9CA3AF` – Border colors, dividers
- `Neutral-300`: `#D1D5DB` – Light borders, inactive states
- `Neutral-200`: `#E5E7EB` – Background colors, card backgrounds
- `Neutral-100`: `#F3F4F6` – Section backgrounds, subtle highlights
- `Neutral-50`: `#F9FAFB` – Page backgrounds, pristine areas

### Accessibility Compliance
- **WCAG AA Standards**: All color combinations meet 4.5:1 contrast ratio (normal text)
- **Enhanced Accessibility**: Critical interactions maintain 7:1 contrast ratio
- **Color-blind Friendly**: Complete palette validated for deuteranopia and protanopia
- **High Contrast Mode**: Alternative color scheme for visual accessibility needs

## 2. Typography System

### Font Stack - Performance Optimized

**Primary Font Family**
```css
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
```

**Monospace Font Family** (Code/Stats)
```css
font-family: 'JetBrains Mono', Consolas, 'Courier New', monospace;
```

### Font Weights - Semantic Hierarchy
- **Light**: 300 – Subtle emphasis, large display text
- **Regular**: 400 – Body text, standard content
- **Medium**: 500 – Section headers, emphasized content
- **Semibold**: 600 – Important labels, navigation items
- **Bold**: 700 – Headlines, critical information

### Type Scale - Responsive Design

**Desktop Type Scale**
- **H1**: `48px/56px, Bold, -0.025em` – Page titles, major sections
- **H2**: `36px/44px, Semibold, -0.02em` – Section headers, feature titles
- **H3**: `28px/36px, Semibold, -0.015em` – Subsection headers, card titles
- **H4**: `22px/30px, Medium, -0.01em` – Component headers, labels
- **H5**: `18px/26px, Medium, -0.005em` – Minor headers, emphasized content
- **Body Large**: `18px/28px, Regular` – Primary reading text, descriptions
- **Body**: `16px/24px, Regular` – Standard UI text, form labels
- **Body Small**: `14px/20px, Regular` – Secondary information, captions
- **Caption**: `12px/16px, Medium` – Metadata, timestamps, fine print
- **Label**: `14px/20px, Semibold, uppercase, 0.05em` – Form labels, categories
- **Code**: `14px/20px, monospace, Regular` – Technical content, statistics

**Mobile Type Scale** (Responsive Adjustments)
- **H1**: `36px/44px` – Reduced for mobile screens
- **H2**: `28px/36px` – Optimized mobile hierarchy
- **H3**: `22px/30px` – Clear mobile section headers
- **Body Large**: `16px/26px` – Enhanced mobile readability
- **Body**: `15px/23px` – Optimal mobile body text

### Typography Usage Guidelines

**Headings**
- Use semantic HTML heading structure (H1 → H2 → H3)
- Maintain visual hierarchy with size and weight progression
- Limit line length to 60-70 characters for optimal readability
- Apply appropriate letter-spacing for enhanced legibility

**Body Text**
- Standard body text uses 16px base size for accessibility
- Maintain 1.5x line height minimum for comfortable reading
- Use sufficient contrast ratios for all text combinations
- Implement responsive scaling for mobile optimization

**Interactive Text**
- Links use `Accent Primary` color with 2px underline on hover
- Button text uses appropriate contrast for background colors
- Form labels use `Semibold` weight for clear identification
- Error text combines `Error` color with medium weight

## 3. Spacing & Layout System

### Base Unit System
**Base Unit**: `8px` (rem equivalent: `0.5rem`)

### Spacing Scale - Mathematical Progression
- `xs`: `4px` (0.25rem) – Micro spacing, icon padding
- `sm`: `8px` (0.5rem) – Small spacing, internal component padding
- `md`: `16px` (1rem) – Default spacing, standard margins
- `lg`: `24px` (1.5rem) – Medium spacing, section separation
- `xl`: `32px` (2rem) – Large spacing, major component separation
- `2xl`: `48px` (3rem) – Extra large spacing, page section padding
- `3xl`: `64px` (4rem) – Huge spacing, hero sections, major layouts

### Grid System - Responsive Foundation

**Desktop Grid** (1024px+)
- **Columns**: 12-column grid with 24px gutters
- **Container Max Width**: 1200px
- **Side Margins**: 48px minimum

**Tablet Grid** (768px - 1023px)
- **Columns**: 8-column grid with 20px gutters
- **Container Max Width**: 100% with 32px margins
- **Flexible Layout**: Adaptive column spanning

**Mobile Grid** (320px - 767px)
- **Columns**: 4-column grid with 16px gutters
- **Container Max Width**: 100% with 20px margins
- **Single Column Priority**: Stack for optimal readability

### Responsive Breakpoints
```css
/* Mobile First Approach */
Mobile: 320px - 767px    /* Primary target: 375px */
Tablet: 768px - 1023px   /* Primary target: 768px */
Desktop: 1024px - 1439px /* Primary target: 1280px */
Wide: 1440px+            /* Primary target: 1440px */
```

### Layout Principles

**Mobile-First Strategy**
- Design starts with mobile constraints
- Progressive enhancement for larger screens
- Touch-friendly interface elements (44px minimum)
- Thumb-reachable navigation zones

**Content Strategy**
- Maximum line length: 75 characters
- Comfortable reading zones with adequate whitespace
- Strategic content prioritization for mobile screens
- Progressive disclosure of complex information

## 4. Component Specifications

### Button System - Athletic Confidence

**Primary Button** (Main Actions)
- **Height**: `44px` (mobile), `40px` (desktop)
- **Padding**: `12px 24px` internal spacing
- **Border Radius**: `8px` modern, approachable corner treatment
- **Typography**: `Body, Semibold` for clear action communication
- **Background**: `Primary` with hover transition to `Primary Dark`
- **Shadow**: `0 2px 4px rgba(47, 107, 59, 0.1)` subtle depth

**Secondary Button** (Supporting Actions)
- **Border**: `2px solid Primary` with transparent background
- **Text Color**: `Primary` changing to white on hover
- **Hover State**: Background fills with `Primary` color
- **Same dimensions** as Primary for consistent interface rhythm

**Ghost Button** (Subtle Actions)
- **No border or background** in default state
- **Text Color**: `Neutral-700` with `Primary` on hover
- **Hover Background**: `Primary` with 10% opacity
- **Used for**: Navigation, secondary options, cancel actions

### Form Elements - Clear Communication

**Input Fields**
- **Height**: `48px` for optimal touch targets
- **Border**: `2px solid Neutral-300` with `Primary` focus state
- **Border Radius**: `6px` subtle, professional appearance
- **Padding**: `12px 16px` comfortable text entry area
- **Typography**: `Body, Regular` for legible user input
- **Placeholder**: `Neutral-500` color for clear guidance

**Label System**
- **Typography**: `Label` style (14px, Semibold, uppercase)
- **Color**: `Neutral-800` for strong identification
- **Spacing**: `8px` margin below label, `4px` above input
- **Required Indicators**: `*` in `Error` color for mandatory fields

### Card Components - Information Containers

**Standard Card**
- **Background**: `White` with `Neutral-100` subtle tint
- **Border**: `1px solid Neutral-200` clean separation
- **Border Radius**: `12px` modern, friendly appearance
- **Shadow**: `0 4px 8px rgba(0, 0, 0, 0.1)` gentle elevation
- **Padding**: `24px` desktop, `20px` mobile
- **Hover State**: Shadow intensifies, slight scale (1.02)

**Interactive Card**
- **Cursor**: `pointer` for clear interactivity
- **Hover Background**: `Neutral-50` subtle state change
- **Active State**: `Primary Light` background with border change
- **Transition**: `200ms ease-out` for smooth interactions

## 5. Motion & Animation System

### Timing Functions - Athletic Performance
- **Ease-out**: `cubic-bezier(0.0, 0, 0.2, 1)` – Entrances, expansions, success states
- **Ease-in-out**: `cubic-bezier(0.4, 0, 0.6, 1)` – Transitions, smooth movements
- **Spring**: `cubic-bezier(0.175, 0.885, 0.32, 1.275)` – Playful interactions, celebrations

### Duration Scale - Performance Optimized
- **Micro**: `150ms` – State changes, hover effects, immediate feedback
- **Short**: `250ms` – Local transitions, dropdown menus, tooltips
- **Medium**: `400ms` – Modal appearances, page transitions
- **Long**: `600ms` – Complex animations, onboarding flows

### Animation Principles

**Performance Requirements**
- **60fps minimum** for all animations
- **Hardware acceleration** using `transform` and `opacity`
- **Reduced motion respect** via `prefers-reduced-motion` media query
- **Battery consideration** with efficient animation techniques

**Functional Animation**
- **Loading States**: Skeleton screens, progress bars, spinners
- **State Feedback**: Button presses, form validation, success confirmations
- **Spatial Awareness**: Modal entrances, navigation transitions
- **Attention Direction**: Subtle pulses for important actions

### Specific Animation Patterns

**Button Interactions**
```css
/* Hover State */
transform: translateY(-1px);
box-shadow: 0 4px 8px rgba(47, 107, 59, 0.15);
transition: all 150ms ease-out;

/* Active State */
transform: translateY(0px);
transition: all 100ms ease-out;
```

**Card Hover Effects**
```css
transform: translateY(-2px) scale(1.01);
box-shadow: 0 8px 16px rgba(0, 0, 0, 0.12);
transition: all 250ms ease-out;
```

**Loading Animation**
```css
/* Skeleton Screen Shimmer */
background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
background-size: 200% 100%;
animation: shimmer 1.5s infinite ease-in-out;
```

## 6. Accessibility Specifications

### WCAG 2.1 AA Compliance

**Color Contrast Requirements**
- **Normal Text**: 4.5:1 minimum contrast ratio
- **Large Text**: 3:1 minimum contrast ratio
- **Interactive Elements**: 4.5:1 for all states
- **Critical Information**: 7:1 enhanced contrast ratio

**Keyboard Navigation**
- **Tab Order**: Logical progression through interactive elements
- **Focus Indicators**: 2px solid `Accent Primary` outline with 2px offset
- **Skip Links**: Available for efficient navigation
- **Keyboard Shortcuts**: Documented and non-conflicting

**Screen Reader Support**
- **Semantic HTML**: Proper heading structure, landmark roles
- **ARIA Labels**: Descriptive labels for complex interactions
- **Alternative Text**: Meaningful descriptions for all images
- **Live Regions**: Dynamic content announcements

**Touch Accessibility**
- **Minimum Touch Target**: 44×44px for all interactive elements
- **Target Spacing**: 8px minimum between adjacent touch targets
- **Gesture Alternatives**: All gesture-based interactions have alternatives
- **Haptic Feedback**: Appropriate vibration patterns for mobile interactions

## 7. Implementation Guidelines

### CSS Custom Properties (Design Tokens)
```css
:root {
  /* Colors */
  --color-primary: #2F6B3B;
  --color-primary-dark: #1E4525;
  --color-primary-light: #4A8F5F;
  
  /* Typography */
  --font-size-h1: 48px;
  --line-height-h1: 56px;
  --font-weight-bold: 700;
  
  /* Spacing */
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  
  /* Motion */
  --duration-micro: 150ms;
  --easing-ease-out: cubic-bezier(0.0, 0, 0.2, 1);
}
```

### Tailwind CSS Integration
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: {
          DEFAULT: '#2F6B3B',
          dark: '#1E4525',
          light: '#4A8F5F'
        },
        // ... additional colors
      },
      fontSize: {
        'h1': ['48px', '56px'],
        'h2': ['36px', '44px'],
        // ... additional sizes
      }
    }
  }
}
```

### Component Usage Guidelines

**Do's**
- Use semantic color names (`primary`, `success`, `error`) instead of specific hex values
- Maintain consistent spacing using the 8px base unit system
- Apply responsive typography scaling for mobile optimization
- Implement proper focus states for all interactive elements
- Use loading states to communicate system status

**Don'ts**
- Don't use arbitrary spacing values outside the systematic scale
- Don't compromise contrast ratios for visual aesthetics
- Don't create custom components without referencing the design system
- Don't ignore mobile-first responsive design principles
- Don't implement animations without performance consideration

## 8. Quality Assurance Checklist

### Design System Compliance
- [ ] Colors match defined palette with proper contrast ratios verified
- [ ] Typography follows established hierarchy and responsive scaling
- [ ] Spacing uses systematic 8px base unit scale consistently
- [ ] Components match documented specifications exactly
- [ ] Motion follows timing and easing standards with performance optimization
- [ ] Accessibility requirements met for WCAG 2.1 AA compliance

### Implementation Verification
- [ ] CSS custom properties implemented correctly
- [ ] Tailwind configuration matches design specifications
- [ ] Component library includes all documented variants and states
- [ ] Responsive behavior verified across all breakpoints
- [ ] Cross-browser compatibility tested (Chrome, Safari, Firefox, Edge)
- [ ] Performance impact assessed and optimized

## Related Documentation
- [Color System Details](../tokens/colors.md)
- [Typography Specifications](../tokens/typography.md)
- [Spacing System](../tokens/spacing.md)
- [Component Library](../components/README.md)
- [Accessibility Guidelines](../../accessibility/guidelines.md)

## Implementation Notes
This style guide serves as the single source of truth for all visual design decisions in the FootballIQ platform. All components, features, and interfaces should reference and adhere to these specifications to maintain consistency and quality throughout the user experience.

## Last Updated
Created: December 28, 2024  
Version: 1.0  
Next Review: January 15, 2025