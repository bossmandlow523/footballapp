---
title: FootballIQ Animation System - Motion & Performance Specifications
description: Complete motion design specifications optimized for mobile performance and athletic context
feature: design-tokens
last-updated: 2024-12-28
version: 1.0
related-files: 
  - ../style-guide.md
  - typography.md
  - colors.md
dependencies:
  - CSS transforms and transitions
  - Hardware acceleration support
status: draft
---

# FootballIQ Animation System - Motion & Performance Specifications

## Overview
The FootballIQ animation system embodies **athletic confidence** through purposeful motion that enhances usability without compromising performance. Every animation serves a functional purpose: providing feedback, guiding attention, or communicating system status. All animations are optimized for 60fps performance on mobile devices used during practice and competitive situations.

## Table of Contents
1. [Animation Philosophy](#animation-philosophy)
2. [Timing Functions](#timing-functions)
3. [Duration Scale](#duration-scale)
4. [Animation Patterns](#animation-patterns)
5. [Performance Requirements](#performance-requirements)
6. [Accessibility Considerations](#accessibility-considerations)
7. [Component-Specific Animations](#component-specific-animations)
8. [Implementation](#implementation)

## Animation Philosophy

### Athletic Performance Principles
- **Confident Motion**: Animations reflect the decisive nature of football
- **Purposeful Movement**: Every animation serves user understanding or system feedback
- **Performance First**: 60fps minimum on practice-field mobile devices
- **Battery Conscious**: Efficient animations that preserve device battery life
- **Context Awareness**: Reduced motion during high-stress game situations

### Functional Animation Categories

**Feedback Animations** (Immediate Response)
- Button presses and touch interactions
- Form validation and success states
- Loading acknowledgments and progress indicators
- Achievement celebrations and milestone recognition

**Spatial Animations** (Navigation Understanding)
- Modal and drawer entrances/exits
- Page transitions and navigation flow
- Content expansion and collapse
- Hierarchical relationship indication

**Attention Animations** (User Guidance)  
- Important notification pulses
- Call-to-action emphasis
- Error state highlighting
- New content indicators

**Status Animations** (System Communication)
- Loading states and progress indication
- Processing feedback and completion
- Network status and connectivity
- Real-time data updates

## Timing Functions

### Athletic-Inspired Easing Curves

**Ease-Out** `cubic-bezier(0.0, 0, 0.2, 1)` - "Explosive Start"
- **Usage**: Entrances, expansions, success celebrations
- **Athletic Context**: Like a sprint start - immediate power, controlled finish
- **Examples**: Modal appearances, button hover states, success notifications

**Ease-In-Out** `cubic-bezier(0.4, 0, 0.6, 1)` - "Smooth Transition"  
- **Usage**: Transitions, movements, state changes
- **Athletic Context**: Like a smooth handoff - controlled throughout
- **Examples**: Page transitions, content sliding, navigation changes

**Spring** `cubic-bezier(0.175, 0.885, 0.32, 1.275)` - "Playful Bounce"
- **Usage**: Celebrations, achievements, positive interactions
- **Athletic Context**: Like a touchdown celebration - energetic and joyful
- **Examples**: Achievement badges, successful completions, milestone celebrations

**Sharp** `cubic-bezier(0.4, 0, 1, 1)` - "Quick Cut"
- **Usage**: Instant feedback, micro-interactions, utility animations
- **Athletic Context**: Like a sharp route cut - precise and immediate
- **Examples**: Button active states, selection feedback, toggle switches

### Custom Timing Functions
```css
:root {
  /* Athletic-inspired easing */
  --timing-explosive: cubic-bezier(0.0, 0, 0.2, 1);
  --timing-smooth: cubic-bezier(0.4, 0, 0.6, 1);  
  --timing-bounce: cubic-bezier(0.175, 0.885, 0.32, 1.275);
  --timing-sharp: cubic-bezier(0.4, 0, 1, 1);
  
  /* Specialized curves */
  --timing-entrance: cubic-bezier(0.0, 0, 0.2, 1);
  --timing-exit: cubic-bezier(0.4, 0, 1, 1);
  --timing-emphasis: cubic-bezier(0.175, 0.885, 0.32, 1.275);
}
```

## Duration Scale

### Performance-Optimized Durations

**Micro (100-150ms)** - "Instant Feedback"
- **Usage**: State changes, hover effects, micro-interactions
- **Context**: Button presses, selection highlights, toggle switches
- **Athletic Context**: Reaction time - immediate response to user input
- **Battery Impact**: Minimal - short duration preserves energy

**Short (200-300ms)** - "Quick Response"
- **Usage**: Local transitions, dropdown menus, tooltip appearances
- **Context**: Form validation, menu expansions, content reveals
- **Athletic Context**: Quick decision - fast but noticeable action
- **Battery Impact**: Low - brief animations with minimal resource usage

**Medium (400-500ms)** - "Smooth Transition"
- **Usage**: Page transitions, modal appearances, significant state changes
- **Context**: Navigation changes, content loading, major UI updates
- **Athletic Context**: Play execution - coordinated, purposeful movement
- **Battery Impact**: Moderate - balanced performance and visual appeal

**Long (600-800ms)** - "Dramatic Effect"
- **Usage**: Complex animations, onboarding flows, celebration sequences
- **Context**: Achievement celebrations, first-time user experiences
- **Athletic Context**: Touchdown celebration - memorable, impactful moment
- **Battery Impact**: Higher - use sparingly for special moments

### Context-Specific Duration Guidelines

**Mobile Practice Environment**
- Prioritize **Micro** (100-150ms) for immediate feedback
- Use **Short** (200-300ms) for essential transitions
- Limit **Medium** (400-500ms) to critical user flows
- Reserve **Long** (600-800ms) for achievements only

**Desktop Planning Environment**
- Standard durations across all categories
- **Medium** animations for enhanced user experience
- **Long** animations for engaging administrative tasks

## Animation Patterns

### Button Interactions

**Primary Button Animation**
```css
.btn-primary {
  transform: translateY(0);
  box-shadow: 0 2px 4px rgba(47, 107, 59, 0.1);
  transition: all 150ms var(--timing-explosive);
}

.btn-primary:hover {
  transform: translateY(-1px);
  box-shadow: 0 4px 8px rgba(47, 107, 59, 0.15);
}

.btn-primary:active {
  transform: translateY(0);
  box-shadow: 0 1px 2px rgba(47, 107, 59, 0.2);
  transition: all 100ms var(--timing-sharp);
}
```

**Loading Button State**
```css
.btn-loading {
  position: relative;
  color: transparent;
}

.btn-loading::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 16px;
  height: 16px;
  margin: -8px 0 0 -8px;
  border: 2px solid currentColor;
  border-right-color: transparent;
  border-radius: 50%;
  animation: btn-spin 600ms linear infinite;
}

@keyframes btn-spin {
  to { transform: rotate(360deg); }
}
```

### Card Component Animations

**Card Hover Effects**
```css
.card {
  transform: translateY(0) scale(1);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: all 250ms var(--timing-smooth);
}

.card:hover {
  transform: translateY(-2px) scale(1.01);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.12);
}

/* Mobile: Remove hover effects for touch devices */
@media (hover: none) {
  .card:hover {
    transform: none;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
}
```

### Modal and Drawer Animations

**Modal Entrance Animation**
```css
.modal-overlay {
  background: rgba(0, 0, 0, 0);
  transition: background 300ms var(--timing-smooth);
}

.modal-overlay.open {
  background: rgba(0, 0, 0, 0.5);
}

.modal-content {
  transform: translateY(100px) scale(0.95);
  opacity: 0;
  transition: all 300ms var(--timing-explosive);
}

.modal-content.open {
  transform: translateY(0) scale(1);
  opacity: 1;
}
```

**Mobile Drawer Animation**
```css
.drawer {
  transform: translateX(-100%);
  transition: transform 250ms var(--timing-smooth);
}

.drawer.open {
  transform: translateX(0);
}

/* Smooth close animation */
.drawer.closing {
  transform: translateX(-100%);
  transition: transform 200ms var(--timing-sharp);
}
```

### Loading States

**Skeleton Screen Animation**
```css
.skeleton {
  background: linear-gradient(
    90deg,
    var(--color-neutral-200) 25%,
    var(--color-neutral-100) 50%,
    var(--color-neutral-200) 75%
  );
  background-size: 200% 100%;
  animation: skeleton-shimmer 1.5s ease-in-out infinite;
}

@keyframes skeleton-shimmer {
  0% { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}
```

**Progress Bar Animation**
```css
.progress-bar {
  width: 0%;
  height: 4px;
  background: var(--color-primary);
  border-radius: 2px;
  transition: width 400ms var(--timing-smooth);
}

.progress-bar[aria-valuenow="100"] {
  background: var(--color-success);
  transition: all 500ms var(--timing-bounce);
}
```

### Success and Achievement Animations

**Achievement Badge Animation**
```css
.achievement-badge {
  transform: scale(0) rotate(180deg);
  opacity: 0;
  animation: achievement-appear 600ms var(--timing-bounce) forwards;
}

@keyframes achievement-appear {
  0% {
    transform: scale(0) rotate(180deg);
    opacity: 0;
  }
  50% {
    transform: scale(1.1) rotate(0deg);
    opacity: 1;
  }
  100% {
    transform: scale(1) rotate(0deg);
    opacity: 1;
  }
}
```

**Success Checkmark Animation**
```css
.success-checkmark {
  stroke-dasharray: 20;
  stroke-dashoffset: 20;
  animation: checkmark-draw 400ms var(--timing-explosive) forwards;
}

@keyframes checkmark-draw {
  to {
    stroke-dashoffset: 0;
  }
}
```

### Attention and Notification Animations

**Pulse Animation for Important Actions**
```css
.pulse-attention {
  animation: pulse 2s var(--timing-smooth) infinite;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.05);
    opacity: 0.8;
  }
}
```

**Notification Slide-in Animation**
```css
.notification {
  transform: translateX(100%);
  animation: notification-enter 300ms var(--timing-explosive) forwards;
}

@keyframes notification-enter {
  to {
    transform: translateX(0);
  }
}

.notification.exit {
  animation: notification-exit 250ms var(--timing-sharp) forwards;
}

@keyframes notification-exit {
  to {
    transform: translateX(100%);
    opacity: 0;
  }
}
```

## Performance Requirements

### Hardware Acceleration
```css
/* Force hardware acceleration for smooth animations */
.gpu-accelerated {
  transform: translateZ(0);
  backface-visibility: hidden;
  perspective: 1000px;
}

/* Preferred properties for 60fps performance */
.performant-animation {
  /* ✅ GOOD: These properties are GPU-accelerated */
  transform: translateX(100px);
  opacity: 0.5;
  filter: blur(4px);
  
  /* ❌ AVOID: These properties cause repaints/reflows */
  /* width: 200px; */
  /* height: 100px; */
  /* top: 50px; */
  /* background-color: red; */
}
```

### Battery-Conscious Animation
```css
/* Reduce animations when battery is low */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

/* Pause animations when page is hidden */
.page-hidden * {
  animation-play-state: paused;
}
```

### Performance Monitoring
```javascript
// Monitor animation performance
const observer = new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    if (entry.duration > 16.67) { // Over 60fps threshold
      console.warn(`Animation frame took ${entry.duration}ms`);
    }
  }
});

observer.observe({ type: 'measure', buffered: true });
```

## Accessibility Considerations

### Reduced Motion Support
```css
/* Respect user preferences for reduced motion */
@media (prefers-reduced-motion: reduce) {
  /* Replace animations with instant transitions */
  .btn-primary {
    transition: none;
  }
  
  .btn-primary:hover {
    /* Maintain visual feedback without motion */
    box-shadow: 0 0 0 3px rgba(47, 107, 59, 0.2);
  }
  
  /* Disable decorative animations completely */
  .skeleton {
    animation: none;
    background: var(--color-neutral-200);
  }
  
  /* Keep functional animations but reduce duration */
  .modal-content {
    transition-duration: 0.001s;
  }
}
```

### Focus Management
```css
/* Animate focus indicators for accessibility */
.focusable {
  outline: 0;
  box-shadow: 0 0 0 0 rgba(30, 58, 138, 0.4);
  transition: box-shadow 200ms var(--timing-explosive);
}

.focusable:focus-visible {
  box-shadow: 0 0 0 3px rgba(30, 58, 138, 0.4);
}

/* Ensure focus is visible during animations */
.animated-element:focus-visible {
  animation-play-state: paused;
}
```

### Screen Reader Considerations
```html
<!-- Announce animation states for screen readers -->
<div aria-live="polite" aria-atomic="true">
  <span class="sr-only" data-animation-status>
    <!-- JavaScript updates this with animation status -->
  </span>
</div>
```

## Component-Specific Animations

### Navigation Animations

**Mobile Navigation Slide**
```css
.mobile-nav {
  transform: translateX(-100%);
  transition: transform 250ms var(--timing-smooth);
}

.mobile-nav.open {
  transform: translateX(0);
}

/* Individual nav items stagger animation */
.nav-item {
  opacity: 0;
  transform: translateX(-20px);
  transition: all 200ms var(--timing-explosive);
}

.nav-item.animate {
  opacity: 1;
  transform: translateX(0);
}

/* Stagger delay */
.nav-item:nth-child(1) { transition-delay: 50ms; }
.nav-item:nth-child(2) { transition-delay: 100ms; }
.nav-item:nth-child(3) { transition-delay: 150ms; }
```

### Form Animations

**Input Focus Animation**
```css
.form-input {
  border: 2px solid var(--color-neutral-300);
  transition: all 200ms var(--timing-explosive);
}

.form-input:focus {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 3px rgba(47, 107, 59, 0.1);
}

/* Label float animation */
.form-label {
  transform: translateY(0);
  font-size: 1rem;
  transition: all 200ms var(--timing-explosive);
}

.form-input:focus + .form-label,
.form-input:not(:placeholder-shown) + .form-label {
  transform: translateY(-24px);
  font-size: 0.875rem;
  color: var(--color-primary);
}
```

**Form Validation Animations**
```css
.form-error {
  max-height: 0;
  opacity: 0;
  overflow: hidden;
  transition: all 250ms var(--timing-explosive);
}

.form-error.show {
  max-height: 60px;
  opacity: 1;
}

/* Input shake animation for errors */
.form-input.error {
  animation: input-shake 400ms var(--timing-sharp);
  border-color: var(--color-error);
}

@keyframes input-shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-4px); }
  75% { transform: translateX(4px); }
}
```

### Dashboard Animations

**Widget Loading Animation**
```css
.dashboard-widget {
  opacity: 0;
  transform: translateY(20px);
  animation: widget-enter 400ms var(--timing-explosive) forwards;
}

@keyframes widget-enter {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Staggered widget loading */
.dashboard-widget:nth-child(1) { animation-delay: 0ms; }
.dashboard-widget:nth-child(2) { animation-delay: 100ms; }
.dashboard-widget:nth-child(3) { animation-delay: 200ms; }
.dashboard-widget:nth-child(4) { animation-delay: 300ms; }
```

## Implementation

### CSS Animation Library
```css
/* Animation Utility Classes */
.animate-fade-in {
  opacity: 0;
  animation: fade-in 300ms var(--timing-explosive) forwards;
}

.animate-slide-up {
  transform: translateY(20px);
  opacity: 0;
  animation: slide-up 400ms var(--timing-explosive) forwards;
}

.animate-scale-in {
  transform: scale(0.95);
  opacity: 0;
  animation: scale-in 250ms var(--timing-bounce) forwards;
}

.animate-bounce {
  animation: bounce 600ms var(--timing-bounce);
}

/* Keyframe Definitions */
@keyframes fade-in {
  to { opacity: 1; }
}

@keyframes slide-up {
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

@keyframes scale-in {
  to {
    transform: scale(1);
    opacity: 1;
  }
}

@keyframes bounce {
  0%, 20%, 53%, 80%, 100% {
    transform: scale(1);
  }
  40%, 43% {
    transform: scale(1.08);
  }
  70% {
    transform: scale(1.04);
  }
  90% {
    transform: scale(1.02);
  }
}
```

### JavaScript Animation Controller
```javascript
// Animation utility functions
class AnimationController {
  static async animate(element, animation, options = {}) {
    const defaults = {
      duration: 300,
      easing: 'var(--timing-explosive)',
      fill: 'forwards'
    };
    
    const settings = { ...defaults, ...options };
    
    return element.animate(animation, settings).finished;
  }
  
  static respectsReducedMotion() {
    return window.matchMedia('(prefers-reduced-motion: reduce)').matches;
  }
  
  static async slideIn(element, direction = 'up') {
    if (this.respectsReducedMotion()) {
      element.style.opacity = '1';
      return Promise.resolve();
    }
    
    const transforms = {
      up: [{ transform: 'translateY(20px)', opacity: 0 }, { transform: 'translateY(0)', opacity: 1 }],
      down: [{ transform: 'translateY(-20px)', opacity: 0 }, { transform: 'translateY(0)', opacity: 1 }],
      left: [{ transform: 'translateX(20px)', opacity: 0 }, { transform: 'translateX(0)', opacity: 1 }],
      right: [{ transform: 'translateX(-20px)', opacity: 0 }, { transform: 'translateX(0)', opacity: 1 }]
    };
    
    return this.animate(element, transforms[direction]);
  }
  
  static async pulse(element, intensity = 1.05) {
    if (this.respectsReducedMotion()) return Promise.resolve();
    
    return this.animate(element, [
      { transform: 'scale(1)' },
      { transform: `scale(${intensity})` },
      { transform: 'scale(1)' }
    ], { duration: 400 });
  }
}

// Usage examples
AnimationController.slideIn(document.querySelector('.modal'), 'up');
AnimationController.pulse(document.querySelector('.achievement-badge'));
```

### React Animation Hooks
```javascript
// Custom React hooks for animations
import { useEffect, useReducer } from 'react';

export function useAnimationState(trigger) {
  const [state, dispatch] = useReducer(
    (state, action) => ({ ...state, ...action }),
    { isAnimating: false, hasAnimated: false }
  );
  
  useEffect(() => {
    if (trigger && !state.hasAnimated) {
      dispatch({ isAnimating: true });
      
      const timer = setTimeout(() => {
        dispatch({ isAnimating: false, hasAnimated: true });
      }, 400);
      
      return () => clearTimeout(timer);
    }
  }, [trigger, state.hasAnimated]);
  
  return state;
}

export function useReducedMotion() {
  const [prefersReducedMotion, setPrefersReducedMotion] = useState(false);
  
  useEffect(() => {
    const mediaQuery = window.matchMedia('(prefers-reduced-motion: reduce)');
    setPrefersReducedMotion(mediaQuery.matches);
    
    const handler = (e) => setPrefersReducedMotion(e.matches);
    mediaQuery.addEventListener('change', handler);
    
    return () => mediaQuery.removeEventListener('change', handler);
  }, []);
  
  return prefersReducedMotion;
}
```

## Related Documentation
- [Color System](colors.md) - Color applications in animated states
- [Typography System](typography.md) - Typography animations and transitions
- [Component Library](../components/README.md) - Component-specific animation implementations
- [Accessibility Guidelines](../../accessibility/guidelines.md) - Animation accessibility requirements

## Implementation Notes
This animation system balances visual appeal with performance requirements, ensuring smooth 60fps animations on mobile devices used during football practice. All animations respect user accessibility preferences and provide meaningful feedback without overwhelming the interface.

## Last Updated
Created: December 28, 2024  
Version: 1.0  
Next Review: January 15, 2025

---

**Performance Guarantee**: All animations in this system are tested to maintain 60fps on iPhone 12 and equivalent Android devices, ensuring smooth operation during football practice and competitive situations.