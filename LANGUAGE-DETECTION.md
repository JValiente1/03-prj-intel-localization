# Auto-Detect Language & Adjust Layout Documentation

## Overview

This JavaScript solution automatically detects language changes on the webpage and dynamically adjusts the layout between RTL (Right-to-Left) and LTR (Left-to-Right) text flow. It works seamlessly with Google Translate and other translation tools.

## Features

✅ **Automatic Detection**: Monitors the page for language changes  
✅ **RTL/LTR Support**: Recognizes 14+ RTL languages  
✅ **Bootstrap CSS Swapping**: Dynamically loads RTL or LTR Bootstrap  
✅ **Google Translate Compatible**: Detects translations in real-time  
✅ **Manual Control**: Provides API for programmatic language switching  
✅ **Event System**: Dispatches custom events when layout changes  

## How It Works

### 1. Language Detection

The script uses multiple detection methods:

```javascript
// Priority order:
1. HTML lang attribute (set by Google Translate)
2. Google Translate iframe detection
3. Browser language (navigator.language)
```

### 2. RTL Language Recognition

Supports these RTL languages:

| Code | Language |
|------|----------|
| `ar` | Arabic |
| `he` | Hebrew |
| `fa` | Persian/Farsi |
| `ur` | Urdu |
| `yi` | Yiddish |
| `ps` | Pashto |
| `sd` | Sindhi |
| `ug` | Uyghur |
| `ku` | Kurdish |
| `dv` | Dhivehi |

### 3. Layout Adjustment

When a language change is detected:

1. **Updates `dir` attribute** on `<html>` element (`rtl` or `ltr`)
2. **Swaps Bootstrap CSS**:
   - RTL: `bootstrap.rtl.min.css`
   - LTR: `bootstrap.min.css`
3. **Dispatches custom event**: `directionChanged`

## Implementation

### Required HTML Setup

```html
<!-- Give Bootstrap link an ID for dynamic swapping -->
<link id="bootstrap-css" 
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.rtl.min.css" 
      rel="stylesheet">
```

### The JavaScript Code

The script is added before the closing `</body>` tag. It includes:

#### Core Functions

1. **`isRTLLanguage(langCode)`**
   - Checks if a language code is RTL
   - Returns: `boolean`

2. **`applyTextDirection(langCode)`**
   - Applies the appropriate layout
   - Updates HTML dir attribute
   - Swaps Bootstrap CSS

3. **`detectLanguage()`**
   - Detects current page language
   - Returns: language code string

4. **`setupLanguageObserver()`**
   - Uses MutationObserver to watch for lang attribute changes
   - Automatically triggers layout updates

5. **`setupGoogleTranslateDetection()`**
   - Watches for Google Translate widget
   - Triggers re-detection when translation occurs

## Usage

### Automatic (With Google Translate)

1. User opens the page
2. Right-clicks → "Translate to [Language]"
3. Script automatically detects the change
4. Layout adjusts to RTL if needed

### Manual Language Switching

Use the global `switchLanguage()` function:

```javascript
// Switch to Arabic (RTL)
switchLanguage('ar');

// Switch to English (LTR)
switchLanguage('en');

// Switch to Hebrew (RTL)
switchLanguage('he');
```

### Listen for Direction Changes

```javascript
window.addEventListener('directionChanged', function(event) {
  console.log('Direction:', event.detail.direction); // 'rtl' or 'ltr'
  console.log('Language:', event.detail.language);   // e.g., 'ar'
  
  // Your custom code here
  // Example: Update images, adjust custom CSS, etc.
});
```

## Testing

### Method 1: Use the Demo Page

Open `language-demo.html` to see an interactive demonstration with:
- Manual language switcher buttons
- Real-time status display
- Event logging
- Sample responsive content

### Method 2: Browser DevTools

```javascript
// In browser console:
switchLanguage('ar'); // Test Arabic (RTL)
switchLanguage('en'); // Test English (LTR)
```

### Method 3: Google Translate

1. Open the page in Chrome or Edge
2. Right-click → "Translate to Arabic" (or any RTL language)
3. Observe automatic layout adjustment

## Browser Compatibility

- ✅ Chrome/Edge 51+
- ✅ Firefox 14+
- ✅ Safari 10+
- ✅ Opera 38+

Uses standard Web APIs:
- MutationObserver
- CustomEvent
- querySelector

## Console Logging

The script logs key events for debugging:

```
RTL/LTR Auto-Detection Script Loaded
Language detection initialized. Current language: ar
Language observer started - watching for language changes
Language changed to: en - Applying LTR layout
```

## Performance

- **Lightweight**: ~4KB minified
- **Efficient**: Only updates when language actually changes
- **Non-blocking**: Uses async MutationObserver
- **No dependencies**: Pure JavaScript (except Bootstrap CSS)

## Customization

### Adding More RTL Languages

```javascript
const RTL_LANGUAGES = [
  'ar',  // Arabic
  'he',  // Hebrew
  // Add your language code here
  'syr', // Syriac
];
```

### Custom Bootstrap URLs

```javascript
const BOOTSTRAP_LTR = 'https://your-cdn.com/bootstrap.min.css';
const BOOTSTRAP_RTL = 'https://your-cdn.com/bootstrap.rtl.min.css';
```

### Disable Console Logging

Remove or comment out all `console.log()` statements for production.

## Integration with Other Tools

### With i18next

```javascript
i18next.on('languageChanged', function(lng) {
  switchLanguage(lng);
});
```

### With React

```javascript
useEffect(() => {
  switchLanguage(currentLanguage);
}, [currentLanguage]);
```

### With Vue

```javascript
watch(() => locale.value, (newLocale) => {
  window.switchLanguage(newLocale);
});
```

## Troubleshooting

### Layout doesn't change
- Check console for errors
- Verify Bootstrap link has `id="bootstrap-css"`
- Ensure language code is correct

### Google Translate not detected
- Check if translation is actually active
- Look for `.goog-te-banner-frame` element
- Try refreshing after translation completes

### CSS not loading
- Check network tab for 404 errors
- Verify Bootstrap CDN URLs are accessible
- Ensure CORS is not blocking requests

## Best Practices

1. **Set initial language**: Use correct `lang` attribute in HTML
2. **Test both directions**: Verify layout in RTL and LTR
3. **Use Bootstrap utilities**: Leverage `me-*`, `ms-*` for spacing
4. **Avoid fixed directions**: Don't hardcode `left` or `right` in CSS
5. **Test accessibility**: Ensure RTL works with screen readers

## Example Implementation

See the main project files:
- `index.html` - Full implementation with Intel sustainability content
- `language-demo.html` - Interactive demo with testing controls

## Credits

Built using:
- Bootstrap 5.3.2 RTL support
- MutationObserver API
- ISO 639-1 language codes

---

**Last Updated**: February 2026  
**Version**: 1.0
