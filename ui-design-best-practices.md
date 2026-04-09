# UI Design Best Practices for Dailbysha

**Version:** 1.0  
**Last Updated:** 2026-04-10  
**Audience:** Frontend Developers, UI/UX Designers, Product Managers

---

## 🎨 **Theme Compatibility Principles**

### Core Philosophy
Build UI components that work seamlessly across **all themes** (light, dark, high-contrast, custom) without hardcoding colors or assuming specific theme values.

---

## 📐 **1. Color System**

### ✅ DO: Use Semantic Color Tokens
```css
/* GOOD - Theme agnostic */
--color-surface-primary
--color-surface-secondary
--color-text-primary
--color-text-secondary
--color-border-default
--color-accent-brand
--color-status-success
--color-status-error
--color-status-warning
--color-status-info
```

### ❌ DON'T: Hardcode Hex/RGB Values
```css
/* BAD - Breaks in dark theme */
background-color: #ffffff;
color: #1a1a1a;
border: 1px solid #e0e0e0;
```

### Color Contrast Requirements
| Element | Minimum Contrast Ratio | WCAG Level |
|---------|----------------------|------------|
| Normal text | 4.5:1 | AA |
| Large text (18px+ bold) | 3:1 | AA |
| UI components (icons, buttons) | 3:1 | AA |
| Critical indicators (errors, alerts) | 4.5:1 | AAA recommended |

### Theme Color Mapping
```typescript
// Theme token structure
interface ThemeColors {
  // Surfaces
  surface: {
    primary: string;      // Main background
    secondary: string;    // Cards, panels
    tertiary: string;     // Nested surfaces
    elevated: string;     // Dropdowns, modals
  }
  
  // Text
  text: {
    primary: string;      // High emphasis
    secondary: string;    // Medium emphasis
    tertiary: string;     // Low emphasis
    disabled: string;     // Disabled state
  }
  
  // Borders
  border: {
    default: string;
    strong: string;
    subtle: string;
  }
  
  // Brand & Status
  brand: { primary: string; secondary: string }
  status: {
    success: string;
    error: string;
    warning: string;
    info: string;
  }
  
  // Interactive
  interactive: {
    hover: string;
    active: string;
    focus: string;
  }
}
```

---

## 🔤 **2. Typography**

### Type Scale (Responsive)
```css
/* Base scale - works with any theme */
--text-xs: 0.75rem;      /* 12px */
--text-sm: 0.875rem;     /* 14px */
--text-base: 1rem;       /* 16px */
--text-lg: 1.125rem;     /* 18px */
--text-xl: 1.25rem;      /* 20px */
--text-2xl: 1.5rem;      /* 24px */
--text-3xl: 1.875rem;    /* 30px */
--text-4xl: 2.25rem;     /* 36px */

/* Font weights */
--font-weight-normal: 400;
--font-weight-medium: 500;
--font-weight-semibold: 600;
--font-weight-bold: 700;

/* Line heights */
--leading-tight: 1.25;
--leading-normal: 1.5;
--leading-relaxed: 1.75;
```

### Typography Best Practices
- [ ] Use relative units (rem, em) not pixels for font sizes
- [ ] Maintain 1.5 line height for body text (readability)
- [ ] Limit to 2-3 font families max
- [ ] Ensure font renders in all themes (test with custom fonts)
- [ ] Support dynamic text sizing (accessibility settings)

---

## 📏 **3. Spacing & Layout**

### Spacing Scale (8pt Grid)
```css
--space-0: 0;
--space-1: 0.25rem;   /* 4px */
--space-2: 0.5rem;    /* 8px */
--space-3: 0.75rem;   /* 12px */
--space-4: 1rem;      /* 16px */
--space-5: 1.25rem;   /* 20px */
--space-6: 1.5rem;    /* 24px */
--space-8: 2rem;      /* 32px */
--space-10: 2.5rem;   /* 40px */
--space-12: 3rem;     /* 48px */
--space-16: 4rem;     /* 64px */
```

### Layout Principles
- [ ] Use consistent spacing scale (multiples of 4px)
- [ ] Maintain visual hierarchy through spacing, not just size
- [ ] Ensure touch targets are 44x44px minimum (mobile)
- [ ] Test layouts at all breakpoints (320px → 1920px+)
- [ ] Support RTL (Right-to-Left) layouts if needed

---

## 🖱️ **4. Interactive States**

### State Styling Requirements
```css
/* All interactive elements MUST have these states */
.button {
  /* Default state */
  background-color: var(--color-interactive-default);
  
  /* Hover state */
  &:hover {
    background-color: var(--color-interactive-hover);
  }
  
  /* Active/Pressed state */
  &:active {
    background-color: var(--color-interactive-active);
  }
  
  /* Focus state - CRITICAL for accessibility */
  &:focus-visible {
    outline: 2px solid var(--color-interactive-focus);
    outline-offset: 2px;
  }
  
  /* Disabled state */
  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
}
```

### Focus Indicators
- [ ] Never remove `:focus-visible` styles
- [ ] Use 2px minimum outline width
- [ ] Ensure focus color contrasts 3:1 against background
- [ ] Test keyboard navigation (Tab, Shift+Tab, Enter, Space, Esc)
- [ ] Focus should be visible in ALL themes

---

## 🎭 **5. Component Patterns**

### Buttons
```css
/* Primary button - theme compatible */
.btn-primary {
  background-color: var(--color-brand-primary);
  color: var(--color-text-inverse); /* Always readable on brand color */
  padding: var(--space-2) var(--space-4);
  border-radius: var(--radius-md);
  font-weight: var(--font-weight-medium);
  
  &:hover {
    background-color: var(--color-brand-primary-hover);
  }
  
  &:disabled {
    background-color: var(--color-brand-disabled);
    color: var(--color-text-disabled);
  }
}
```

### Cards & Surfaces
```css
/* Card - works in light/dark themes */
.card {
  background-color: var(--color-surface-secondary);
  border: 1px solid var(--color-border-default);
  border-radius: var(--radius-lg);
  padding: var(--space-6);
  box-shadow: var(--shadow-sm);
  
  /* Elevated state (hover, selected) */
  &.elevated {
    box-shadow: var(--shadow-md);
    border-color: var(--color-border-strong);
  }
}
```

### Form Inputs
```css
/* Input field - accessible in all themes */
.input {
  background-color: var(--color-surface-primary);
  border: 1px solid var(--color-border-default);
  color: var(--color-text-primary);
  padding: var(--space-2) var(--space-3);
  border-radius: var(--radius-md);
  
  /* Focus state */
  &:focus {
    border-color: var(--color-brand-primary);
    box-shadow: 0 0 0 3px var(--color-brand-light);
    outline: none;
  }
  
  /* Error state */
  &.error {
    border-color: var(--color-status-error);
  }
  
  /* Disabled state */
  &:disabled {
    background-color: var(--color-surface-tertiary);
    color: var(--color-text-disabled);
    cursor: not-allowed;
  }
  
  /* Placeholder - must have sufficient contrast */
  &::placeholder {
    color: var(--color-text-tertiary);
  }
}
```

### Tables (Critical for Dailbysha)
```css
/* Data table - patient lists, sessions, etc. */
.data-table {
  width: 100%;
  border-collapse: collapse;
  
  th {
    background-color: var(--color-surface-tertiary);
    color: var(--color-text-primary);
    font-weight: var(--font-weight-semibold);
    text-align: left;
    padding: var(--space-3) var(--space-4);
    border-bottom: 2px solid var(--color-border-strong);
  }
  
  td {
    padding: var(--space-3) var(--space-4);
    border-bottom: 1px solid var(--color-border-default);
    color: var(--color-text-primary);
  }
  
  tr:hover {
    background-color: var(--color-surface-secondary);
  }
  
  /* Striped rows for readability */
  tr:nth-child(even) {
    background-color: var(--color-surface-secondary);
  }
}
```

### Alerts & Status Indicators
```css
/* Alert component */
.alert {
  padding: var(--space-4);
  border-radius: var(--radius-md);
  border-left: 4px solid;
  
  &.success {
    background-color: var(--color-status-success-bg);
    border-color: var(--color-status-success);
    color: var(--color-status-success-text);
  }
  
  &.error {
    background-color: var(--color-status-error-bg);
    border-color: var(--color-status-error);
    color: var(--color-status-error-text);
  }
  
  &.warning {
    background-color: var(--color-status-warning-bg);
    border-color: var(--color-status-warning);
    color: var(--color-status-warning-text);
  }
  
  &.info {
    background-color: var(--color-status-info-bg);
    border-color: var(--color-status-info);
    color: var(--color-status-info-text);
  }
}
```

---

## 🌗 **6. Dark Mode Specific Guidelines**

### Elevate with Light, Not Darkness
```css
/* GOOD - Dark mode elevation */
--shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.3);
--shadow-md: 0 4px 6px rgba(0, 0, 0, 0.4);
--shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.5);

/* Use lighter surfaces for elevation, not shadows */
.surface-elevated {
  background-color: var(--color-surface-elevated); /* Lighter than parent */
}
```

### Dark Mode Adjustments
- [ ] Reduce saturation of brand colors (less eye strain)
- [ ] Use lighter borders instead of shadows for depth
- [ ] Avoid pure white (#fff) text on pure black (#000) backgrounds
- [ ] Test with OLED screens (true black can cause smearing)
- [ ] Ensure images have proper contrast overlays

### Light Mode Adjustments
- [ ] Avoid pure black text on pure white (use #1a1a1a on #fafafa)
- [ ] Use subtle shadows for depth
- [ ] Ensure bright brand colors don't cause glare

---

## ♿ **7. Accessibility (A11y) Requirements**

### WCAG 2.1 AA Compliance Checklist
- [ ] Color contrast 4.5:1 for normal text, 3:1 for large text
- [ ] All functionality available via keyboard
- [ ] Focus indicators visible on all interactive elements
- [ ] Form inputs have associated labels
- [ ] Error messages are clear and actionable
- [ ] Icons have `aria-label` or `aria-hidden` as appropriate
- [ ] Loading states use `aria-live` regions
- [ ] Skip links for keyboard users
- [ ] Semantic HTML (nav, main, article, section, aside)

### Screen Reader Support
```jsx
// GOOD - Accessible button
<button
  aria-label="Delete patient record"
  aria-describedby="delete-warning"
>
  <TrashIcon aria-hidden="true" />
</button>
<span id="delete-warning" className="sr-only">
  This action cannot be undone
</span>

// GOOD - Status announcements
<div
  role="status"
  aria-live="polite"
  aria-atomic="true"
>
  Patient record saved successfully
</div>
```

### Color Independence
- [ ] Never convey information with color alone
- [ ] Use icons + color for status (error icon + red)
- [ ] Use patterns/textures in charts (not just colors)
- [ ] Underline links (or have distinct non-color indicator)

---

## 📱 **8. Responsive Design**

### Breakpoint Strategy
```css
/* Mobile-first breakpoints */
--breakpoint-sm: 640px;   /* Small tablets */
--breakpoint-md: 768px;   /* Tablets */
--breakpoint-lg: 1024px;  /* Laptops */
--breakpoint-xl: 1280px;  /* Desktops */
--breakpoint-2xl: 1536px; /* Large screens */
```

### Responsive Patterns
- [ ] Use fluid typography (`clamp()` function)
- [ ] Test on actual devices, not just browser resize
- [ ] Consider touch vs. mouse interaction differences
- [ ] Optimize for landscape and portrait orientations
- [ ] Support zoom up to 200% without breaking layout

---

## 🏥 **9. Dailbysha-Specific Patterns**

### Medical Data Display
```css
/* Vital signs display - high readability */
.vital-sign {
  display: flex;
  align-items: baseline;
  gap: var(--space-2);
  
  &.value {
    font-size: var(--text-3xl);
    font-weight: var(--font-weight-bold);
    color: var(--color-text-primary);
    font-variant-numeric: tabular-nums; /* Align numbers */
  }
  
  &.unit {
    font-size: var(--text-sm);
    color: var(--color-text-secondary);
  }
  
  &.critical {
    color: var(--color-status-error);
    animation: pulse 2s infinite;
  }
}
```

### Session Timer (Dialysis)
```css
/* Session timer - visible in peripheral vision */
.session-timer {
  font-size: var(--text-4xl);
  font-weight: var(--font-weight-bold);
  font-variant-numeric: tabular-nums;
  padding: var(--space-4);
  border-radius: var(--radius-lg);
  background-color: var(--color-surface-tertiary);
  
  &.warning {
    /* 15 minutes remaining */
    color: var(--color-status-warning);
    background-color: var(--color-status-warning-bg);
  }
  
  &.critical {
    /* 5 minutes remaining */
    color: var(--color-status-error);
    background-color: var(--color-status-error-bg);
    animation: pulse 1s infinite;
  }
}
```

### Patient Status Indicators
```css
/* Status badge - works in all themes */
.status-badge {
  display: inline-flex;
  align-items: center;
  gap: var(--space-1);
  padding: var(--space-1) var(--space-2);
  border-radius: var(--radius-full);
  font-size: var(--text-xs);
  font-weight: var(--font-weight-medium);
  
  &.active {
    background-color: var(--color-status-success-bg);
    color: var(--color-status-success-text);
    
    &::before {
      content: '';
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background-color: var(--color-status-success);
    }
  }
  
  &.critical {
    background-color: var(--color-status-error-bg);
    color: var(--color-status-error-text);
  }
}
```

---

## 🔍 **10. Testing Checklist**

### Before Ship - Theme Testing
- [ ] Test in light theme
- [ ] Test in dark theme
- [ ] Test in high contrast mode (Windows/MacOS)
- [ ] Test with custom theme (if available)
- [ ] Test with browser zoom (100%, 150%, 200%)
- [ ] Test with custom fonts (user preference)
- [ ] Test with reduced motion preference
- [ ] Test with increased text size preference

### Accessibility Testing
- [ ] Test with keyboard only (no mouse)
- [ ] Test with screen reader (NVDA, VoiceOver, JAWS)
- [ ] Run automated a11y audit (axe, Lighthouse)
- [ ] Test color contrast with tool (Stark, Color Oracle)
- [ ] Verify focus order matches visual order
- [ ] Test with JavaScript disabled (progressive enhancement)

### Cross-Browser Testing
- [ ] Chrome (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest)
- [ ] Edge (latest)
- [ ] Mobile Safari (iOS)
- [ ] Chrome Mobile (Android)

### Device Testing
- [ ] Desktop (1920x1080)
- [ ] Laptop (1366x768)
- [ ] Tablet (768x1024)
- [ ] Mobile (375x667)
- [ ] Large monitor (2560x1440)

---

## 📚 **11. Resources**

### Design Systems
- [Material Design](https://material.io/design)
- [Apple Human Interface Guidelines](https://developer.apple.com/design/)
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)

### Tools
- **Color Contrast:** WebAIM Contrast Checker, Stark
- **Accessibility:** axe DevTools, Lighthouse, WAVE
- **Color Blindness:** Color Oracle, Stark
- **Responsive Testing:** BrowserStack, Responsively App

### Dailbysha Theme Tokens
```typescript
// Import from @dailbysha/design-tokens
import {
  // Colors
  colors,
  // Typography
  typography,
  // Spacing
  spacing,
  // Shadows
  shadows,
  // Border radius
  radii,
  // Breakpoints
  breakpoints,
} from '@dailbysha/design-tokens';
```

---

## ⚠️ **Common Mistakes to Avoid**

| Mistake | Impact | Fix |
|---------|--------|-----|
| Hardcoded colors | Breaks in dark theme | Use semantic tokens |
| Removing focus styles | Keyboard users can't navigate | Keep `:focus-visible` |
| Color-only indicators | Color blind users can't understand | Add icons/patterns |
| Small touch targets | Mobile users can't tap accurately | 44x44px minimum |
| Low contrast text | Users can't read content | 4.5:1 ratio minimum |
| Fixed font sizes | Users can't scale text | Use rem units |
| Mouse-only interactions | Keyboard users locked out | Add keyboard support |
| Auto-playing media | Vestibular disorders triggered | Respect `prefers-reduced-motion` |

---

## 🎯 **Quick Reference: Theme-Compatible CSS**

```css
/* Copy-paste template for any component */
.component {
  /* Surface */
  background-color: var(--color-surface-secondary);
  border: 1px solid var(--color-border-default);
  
  /* Content */
  color: var(--color-text-primary);
  padding: var(--space-4);
  border-radius: var(--radius-md);
  
  /* Typography */
  font-size: var(--text-base);
  line-height: var(--leading-normal);
  
  /* Interactive */
  &:hover {
    background-color: var(--color-surface-tertiary);
  }
  
  &:focus-visible {
    outline: 2px solid var(--color-brand-primary);
    outline-offset: 2px;
  }
  
  /* States */
  &.disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
  
  /* Responsive */
  @media (min-width: 768px) {
    padding: var(--space-6);
  }
}
```

---

**Remember:** Every UI decision should answer: *"Does this work in light mode, dark mode, high contrast, and with assistive technologies?"* If not, revise until it does.
