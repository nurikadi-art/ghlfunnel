# Smartworker TripWire - GHL Development Guide

This document explains the HTML/CSS patterns, file structure, and conventions used in this GoHighLevel (GHL) funnel project. Use this as a reference when making changes or creating new components.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [File Structure](#file-structure)
3. [GHL Custom HTML Injection](#ghl-custom-html-injection)
4. [Button URL Mapping](#button-url-mapping)
5. [Design System - Titanium Blue Theme](#design-system---titanium-blue-theme)
6. [Popup HTML Section Format](#popup-html-section-format)
7. [Dynamic Currency Implementation](#dynamic-currency-implementation)
8. [Code Patterns & Examples](#code-patterns--examples)

---

## Project Overview

**Product**: AI Operator Bundle ("Комплект ИИ-Оператора")
**Price**: $9 USD tripwire offer
**Market**: Russian-speaking CIS countries (Russia, Kazakhstan, Ukraine, Belarus, etc.)
**Platform**: GoHighLevel (GHL) page builder with custom HTML/CSS injection

The funnel consists of:
- Landing/Tripwire pages (sales copy)
- Order form pages (checkout)
- Popup components (exit intent, quiz)

---

## File Structure

```
Smartworker-TripWire/
├── ai-operator-tripwire-RU-9usd.html      # Main tripwire landing page (dark theme)
├── ai-operator-tripwire-RU-9usd-edtech.html # EdTech variation (blue theme)
├── fe-order-form-top.html                  # Order form header section
├── fe-order-form-bottom.html               # Order form footer (refund info)
├── ghl-order-form-inject.html              # GHL form styling injection
├── exit-popup-inject.html                  # Exit intent popup (standalone)
├── quiz-popup-inject.html                  # Interactive quiz popup
└── GHL-DEVELOPMENT-GUIDE.md                # This documentation
```

### File Purposes

| File | Purpose | Where to Use in GHL |
|------|---------|---------------------|
| `ai-operator-tripwire-RU-9usd.html` | Main sales page content | Page body - Custom HTML element |
| `ai-operator-tripwire-RU-9usd-edtech.html` | Alternative EdTech-focused variation | Page body - Custom HTML element |
| `fe-order-form-top.html` | Product summary, book stack visual, receipt | Order form page - Above form |
| `fe-order-form-bottom.html` | Refund/guarantee info | Order form page - Below form |
| `ghl-order-form-inject.html` | CSS/JS to style GHL's native form | Order form page - Custom Code section |
| `exit-popup-inject.html` | Full exit intent popup with overlay | Page Custom Code (before `</body>`) |
| `quiz-popup-inject.html` | Quiz popup for GHL popup HTML section | GHL Popup Builder - HTML section |

---

## GHL Custom HTML Injection

### Where to Add Custom Code in GHL

1. **Page Body Content**: Use "Custom HTML" element in the page builder
2. **Page-Level Scripts/Styles**: Settings → Custom Code → Header or Footer
3. **Popup HTML Section**: Popup Builder → Add Section → HTML

### Important Notes

- GHL strips some attributes, so use inline styles when CSS classes don't work
- Always include `!important` on critical styles to override GHL defaults
- GHL handles popup overlay/close logic - don't add redundant overlay wrappers in popup HTML sections

---

## Button URL Mapping

### Primary URLs

| Button Purpose | URL |
|----------------|-----|
| Main CTA (Order Form) | `https://smartworkers.kz/fe---order-form-page-6628-3000-5628` |
| Alternative Order Form | `https://smartworkers.kz/orderform-6709` |

### Button Classes

```css
/* Primary CTA Button */
.cta-button, .tp-btn, .hero-cta {
    /* Green gradient button */
    background: linear-gradient(135deg, #28cd41 0%, #1fa82e 100%);
}

/* Secondary/Decline Link */
.tp-no, .decline-link {
    /* Subtle underlined text */
    color: #52525b;
    text-decoration: underline;
}
```

### Button HTML Pattern

```html
<!-- Primary CTA -->
<a href="https://smartworkers.kz/fe---order-form-page-6628-3000-5628" class="tp-btn">
    Да, хочу Комплект ИИ-Оператора за $9
</a>

<!-- Decline/Close (for popups) -->
<div class="tp-no" onclick="closeExitPopup()">
    Нет, я продолжу делать всё вручную
</div>
```

---

## Design System - Titanium Blue Theme

### Color Palette

```css
:root {
    /* Primary Colors */
    --primary-blue: #0a84ff;
    --primary-green: #28cd41;
    --success-green: #1fa82e;

    /* Background */
    --bg-dark: #050505;
    --bg-card: #0f0f12;
    --bg-subtle: rgba(255, 255, 255, 0.03);

    /* Text */
    --text-primary: #f5f5f7;
    --text-secondary: #a1a1aa;
    --text-muted: #86868b;
    --text-dim: #52525b;

    /* Borders */
    --border-subtle: rgba(255, 255, 255, 0.08);
    --border-blue: rgba(10, 132, 255, 0.3);
    --border-green: rgba(40, 205, 65, 0.2);

    /* Trustpilot Green */
    --trustpilot-green: #00b67a;
}
```

### Typography

```css
/* Font Stack */
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;

/* Font Smoothing (important for consistency) */
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;

/* Heading Sizes */
.headline { font-size: 30px; font-weight: 800; }
.subheadline { font-size: 15px; color: #a1a1aa; }
.label { font-size: 12px; letter-spacing: 2.5px; text-transform: uppercase; }
```

### Card/Box Patterns

```css
/* Titanium Box (main container) */
.titanium-box {
    background: linear-gradient(145deg, #0f0f12 0%, #050505 100%);
    border: 1px solid rgba(10, 132, 255, 0.3);
    box-shadow: 0 0 80px rgba(10, 132, 255, 0.15);
    border-radius: 24px;
    padding: 48px;
}

/* Inner Panel */
.inner-panel {
    background: rgba(255, 255, 255, 0.03);
    border: 1px solid rgba(255, 255, 255, 0.08);
    border-radius: 16px;
    padding: 28px;
}

/* Guarantee/Info Box */
.guarantee-box {
    background: rgba(40, 205, 65, 0.08);
    border: 1px solid rgba(40, 205, 65, 0.2);
    border-radius: 10px;
    padding: 12px 18px;
    color: #28cd41;
}
```

---

## Popup HTML Section Format

### For GHL Popup Builder

When creating popups for GHL's built-in popup system, **do NOT include overlay wrappers**. GHL handles the overlay, backdrop, and close button logic.

### Correct Format (for GHL Popup HTML Section)

```html
<!-- Just the content box - no overlay wrapper -->
<div class="titanium-box">
    <div class="tp-content-grid">
        <div class="tp-left">
            <div class="tp-header">ПОДОЖДИ! НЕ УХОДИ...</div>
            <div class="tp-headline">Your headline here</div>
            <div class="tp-subtext">
                Description text with <span class="tp-highlight">$9</span> price highlight.
            </div>
            <div class="tp-guarantee">
                <span class="tp-guarantee-icon">🛡️</span>
                <span>30-дневная гарантия возврата</span>
            </div>
        </div>
        <div class="tp-right">
            <div class="tp-widget">
                <!-- Trustpilot stars and review -->
            </div>
            <a href="ORDER_URL" class="tp-btn">
                CTA Button Text
            </a>
            <div class="tp-no" onclick="document.querySelector('.close-popup').click()">
                Decline text here
            </div>
        </div>
    </div>
</div>

<style>
/* Include all necessary styles inline */
</style>
```

### For Standalone Popups (with own overlay)

Use `exit-popup-inject.html` as reference - includes full overlay, show/hide logic, and exit intent detection.

```html
<div id="exitPopupOverlay" class="exit-overlay">
    <div class="titanium-box">
        <!-- Content here -->
    </div>
</div>

<style>
.exit-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.92);
    backdrop-filter: blur(8px);
    z-index: 999999;
}
.exit-overlay.show { display: flex; }
</style>

<script>
// Exit intent detection logic
</script>
```

---

## Dynamic Currency Implementation

Shows local currency equivalent in parentheses based on visitor's IP location.

### Supported Currencies

```javascript
const currencies = {
    'RU': { code: 'RUB', symbol: '₽', rate: 92 },
    'KZ': { code: 'KZT', symbol: '₸', rate: 450 },
    'UA': { code: 'UAH', symbol: '₴', rate: 41 },
    'BY': { code: 'BYN', symbol: 'Br', rate: 3.2 },
    'UZ': { code: 'UZS', symbol: 'сум', rate: 12500 },
    'AZ': { code: 'AZN', symbol: '₼', rate: 1.7 },
    'GE': { code: 'GEL', symbol: '₾', rate: 2.7 },
    'MD': { code: 'MDL', symbol: 'лей', rate: 18 },
    'KG': { code: 'KGS', symbol: 'сом', rate: 89 },
    'TJ': { code: 'TJS', symbol: 'смн', rate: 11 },
    'AM': { code: 'AMD', symbol: '֏', rate: 400 },
    'TM': { code: 'TMT', symbol: 'ман', rate: 3.5 }
};
```

### Implementation

```html
<!-- Mark elements with data-usd attribute -->
<span class="price" data-usd="9">$9</span>
<span class="price" data-usd="97">$97</span>

<script>
(function() {
    const currencies = { /* ... currency map ... */ };

    async function detectAndShowCurrency() {
        try {
            const response = await fetch('https://ipapi.co/json/');
            const data = await response.json();
            const country = data.country_code;

            if (currencies[country]) {
                const curr = currencies[country];
                document.querySelectorAll('[data-usd]').forEach(el => {
                    const usd = parseFloat(el.getAttribute('data-usd'));
                    const local = Math.round(usd * curr.rate);
                    const formatted = local.toLocaleString('ru-RU');
                    el.innerHTML = `$${usd} <span class="local-price">(~${formatted} ${curr.symbol})</span>`;
                });
            }
        } catch (e) {
            console.log('Currency detection failed, using USD only');
        }
    }

    if (document.readyState === 'loading') {
        document.addEventListener('DOMContentLoaded', detectAndShowCurrency);
    } else {
        detectAndShowCurrency();
    }
})();
</script>

<style>
.local-price {
    font-size: 0.85em;
    color: #a1a1aa;
    font-weight: 400;
}
</style>
```

---

## Code Patterns & Examples

### Trustpilot Widget

```html
<div class="tp-widget">
    <div class="tp-row">
        <div class="tp-stars">
            <div class="tp-star"></div>
            <div class="tp-star"></div>
            <div class="tp-star"></div>
            <div class="tp-star"></div>
            <div class="tp-star"></div>
        </div>
        <div class="tp-logo">Trust<span>pilot</span></div>
    </div>
    <div class="tp-review">
        "Review text here with <strong>highlighted part</strong>."
    </div>
    <div class="tp-author">— Name, Title</div>
</div>

<style>
.tp-stars { display: flex; gap: 3px; }
.tp-star {
    width: 18px;
    height: 18px;
    background: #00b67a;
    clip-path: polygon(50% 0%, 61% 35%, 98% 35%, 68% 57%, 79% 91%, 50% 70%, 21% 91%, 32% 57%, 2% 35%, 39% 35%);
}
.tp-logo { font-weight: 700; color: #fff; }
.tp-logo span { color: #00b67a; }
.tp-review {
    font-style: italic;
    border-left: 3px solid #00b67a;
    padding-left: 14px;
}
</style>
```

### Refund Info Section

```html
<div class="refund-info">
    <div class="refund-icon">✅</div>
    <div class="refund-content">
        <p>Возврат? Без проблем — одно сообщение в Telegram <strong>@incastra31</strong> (Адлет) или Instagram <strong>@nurikadi</strong></p>
    </div>
</div>

<style>
.refund-info {
    display: flex;
    align-items: flex-start;
    gap: 14px;
    background: rgba(40, 205, 65, 0.06);
    border: 1px solid rgba(40, 205, 65, 0.15);
    border-radius: 12px;
    padding: 18px 22px;
}
.refund-icon { font-size: 24px; }
.refund-content p {
    color: #a1a1aa;
    font-size: 14px;
    line-height: 1.6;
}
.refund-content strong { color: #28cd41; }
</style>
```

### Mobile Responsive Pattern

```css
/* Desktop first, then mobile overrides */
.container {
    padding: 48px;
    max-width: 900px;
}

@media (max-width: 768px) {
    .container {
        padding: 32px 24px;
    }

    .grid-2col {
        grid-template-columns: 1fr; /* Stack on mobile */
    }

    .headline {
        font-size: 24px; /* Smaller on mobile */
    }
}

@media (max-width: 480px) {
    .container {
        padding: 28px 20px;
        border-radius: 20px;
    }
}
```

### Exit Intent Detection

```javascript
(function() {
    const POPUP_KEY = 'exitPopupShown';
    const POPUP_DELAY = 3000; // Don't show within first 3 seconds
    let canShowPopup = false;
    let popupShown = false;

    setTimeout(() => { canShowPopup = true; }, POPUP_DELAY);

    // Desktop: Mouse leaves viewport
    document.addEventListener('mouseout', function(e) {
        if (e.clientY < 5 && canShowPopup && !popupShown && !sessionStorage.getItem(POPUP_KEY)) {
            showExitPopup();
        }
    });

    // Mobile: Back button detection
    window.addEventListener('popstate', function() {
        if (canShowPopup && !popupShown && !sessionStorage.getItem(POPUP_KEY)) {
            showExitPopup();
            history.pushState(null, '', window.location.href);
        }
    });

    history.pushState(null, '', window.location.href);

    function showExitPopup() {
        document.getElementById('exitPopupOverlay').classList.add('show');
        popupShown = true;
        sessionStorage.setItem(POPUP_KEY, 'true');
        document.body.style.overflow = 'hidden';
    }

    window.closeExitPopup = function() {
        document.getElementById('exitPopupOverlay').classList.remove('show');
        document.body.style.overflow = '';
    };
})();
```

---

## Quick Reference

### Key Contacts (for refund section)
- Telegram: @incastra31 (Адлет)
- Instagram: @nurikadi

### Product Name (Russian)
- Full: **Комплект ИИ-Оператора**
- Short: **ИИ-Оператор**

### Price Points
- Tripwire: $9
- Original value shown: $97 (crossed out)

### Common Russian CTA Phrases
- "Да, хочу Комплект ИИ-Оператора за $9"
- "Получить доступ сейчас"
- "Забрать комплект за $9"
- Decline: "Нет, я продолжу делать всё вручную"

---

## Checklist for New Components

- [ ] Use Inter font family with font-smoothing
- [ ] Follow Titanium Blue color palette
- [ ] Include mobile responsive styles (@media queries)
- [ ] Use correct order form URL for CTAs
- [ ] Add `!important` to critical styles (GHL override)
- [ ] Test on both desktop and mobile
- [ ] Use Russian text for CIS market
- [ ] Include dynamic currency script if showing prices

---

*Last updated: February 2026*
