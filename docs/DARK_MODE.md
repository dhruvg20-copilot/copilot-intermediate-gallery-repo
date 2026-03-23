# Dark Mode Implementation

This project implements a complete dark mode feature using `next-themes` with persistent theme preferences.

## Features

- **Theme Toggle**: Sun/moon icon button in the global navigation header
- **Persistence**: Theme choice is saved to localStorage and persists across sessions
- **System Preference**: Falls back to OS dark mode preference if no saved choice exists
- **No Flash**: Prevents flash of unstyled content (FOUC) on page load
- **Accessibility**: Fully keyboard accessible with proper ARIA labels

## How It Works

### Theme Provider

The app uses `next-themes` which provides:
- Automatic localStorage persistence
- System preference detection
- No-flash hydration handling
- Type-safe theme hooks

The provider is configured in `src/app/layout.tsx`:

```tsx
<ThemeProvider attribute="class" defaultTheme="system" enableSystem>
  {children}
</ThemeProvider>
```

### Theme Toggle Component

Located at `src/components/ui/ThemeToggle.tsx`, this component:
- Uses the `useTheme` hook from next-themes
- Renders sun icon in light mode, moon icon in dark mode
- Handles client-side mounting to prevent hydration issues
- Toggles between light and dark themes

### CSS Theming Strategy

All dark mode styles use class-based selectors:

```css
/* Light mode (default) */
.card-base {
  background-color: #ffffff;
}

/* Dark mode */
.dark .card-base {
  background-color: #1e293b;
}
```

This approach works with Tailwind's `dark:` variants and custom CSS classes.

## Using Dark Mode in Components

### With Tailwind

Use the `dark:` variant for any utility class:

```tsx
<div className="bg-white dark:bg-slate-800 text-slate-900 dark:text-white">
  Content adapts to theme
</div>
```

### With Custom CSS Classes

All standard CSS classes in `src/app/globals.css` support both themes:

```tsx
<div className="card-base">
  {/* Automatically adapts to current theme */}
</div>
```

### Accessing Theme in JavaScript

```tsx
import { useTheme } from "next-themes";

export function MyComponent() {
  const { theme, setTheme } = useTheme();

  return (
    <button onClick={() => setTheme(theme === "dark" ? "light" : "dark")}>
      Current theme: {theme}
    </button>
  );
}
```

## Available Themes

The system supports three theme values:

- `"light"` - Force light mode
- `"dark"` - Force dark mode
- `"system"` - Use OS preference (default)

## Theme Persistence

Themes are automatically persisted to `localStorage` under the key `theme`. Users' theme choices persist across:
- Page refreshes
- Browser restarts
- Navigation between routes

## Preventing FOUC

The `suppressHydrationWarning` attribute on `<html>` prevents React warnings from `next-themes`' inline script that sets the theme before paint.

## Adding New Themed Components

When creating new components:

1. **For Tailwind styling**: Add `dark:` variants to all color/background classes
2. **For custom CSS**: Define both light and dark versions using `.dark` selector
3. **Test both themes**: Verify your component in both light and dark modes

Example:

```tsx
// Component with Tailwind
export function MyCard() {
  return (
    <div className="bg-white dark:bg-slate-800 border border-slate-200 dark:border-slate-700">
      <h2 className="text-slate-900 dark:text-white">Title</h2>
      <p className="text-slate-600 dark:text-slate-400">Description</p>
    </div>
  );
}
```

## Browser Support

Dark mode works in all modern browsers that support:
- CSS custom properties
- localStorage
- Tailwind CSS v4

No polyfills required.
