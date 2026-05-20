# Design System

Visual language and component specifications for breakDown.

## Colors

### Dark Mode (Default)
- **Base**: `#0d1117` → `#1a0f2e` (gradient)
- **Text**: `#e5e7eb` (primary), `#9ca3af` (secondary)
- **Interactive**: `#6366f1` (indigo), `#a78bfa` (lavender hover)
- **Status**: `#22c55e` (owed to you), `#ef4444` (you owe)
- **Cards**: Glassmorphism (blur 10px, rgba indigo 0.1)

### Light Mode (Alternative)
- **Base**: `#f5f3ff` → `#faf8ff` (gradient)
- **Text**: `#1f2937` (primary), `#4b5563` (secondary)
- **Same colors**: Indigo, green, red (unchanged for contrast)
- **Accessibility**: WCAG AA (4.5:1+ contrast)

## Typography

- **Sans-serif**: `-apple-system`, `BlinkMacSystemFont`, `Segoe UI`, `Roboto`
- **Monospace**: `Courier New`, `SF Mono`, `Monaco` (for data/amounts)
- **Scale**: H1 (32px, 600wt), H2 (24px), Body (14px), Labels (12px)

## Spacing

- **Base**: 4px unit (xs=4, sm=8, md=12, lg=16, xl=20, 2xl=24, 3xl=32, 4xl=40)
- **Border radius**: sm=4px, md=8px, lg=12px

## Components

### Buttons
- **Primary**: `#6366f1` bg, `#e5e7eb` text, hover→`#a78bfa`
- **States**: Default, Hover, Active/Pressed, Loading (spinner), Disabled
- **Size**: 12px padding, 8px border-radius, min-height 44px (touch)

### Form Inputs
- **Padding**: 12px
- **Border**: `rgba(99,102,241,0.2)` default, `#6366f1` focus
- **Error**: Red border + error message below
- **Label**: 12px, uppercase, 600 weight

### Cards
- **Background**: `rgba(99,102,241,0.1)` with blur(10px)
- **Border**: `rgba(99,102,241,0.2)`
- **Hover**: Elevated, increased opacity
- **Padding**: 24px

## Responsive Breakpoints

- **Mobile**: <768px (full-width layout)
- **Tablet**: 768px-1023px (2-column grid)
- **Desktop**: 1024px+ (3-4 column grid, side-by-side layouts)

## Accessibility

- ✅ **WCAG AA Compliant**
- ✅ **Keyboard Navigation**: Tab order, Enter/Escape support
- ✅ **Screen Reader**: Semantic HTML, aria labels
- ✅ **Color Contrast**: 4.5:1+ for normal text
- ✅ **Touch Targets**: 44x48px minimum

---

This document covers the core design tokens. Full component and platform-specific specifications are defined in the main project's `branding/frontend-implementation-guide.md`.
