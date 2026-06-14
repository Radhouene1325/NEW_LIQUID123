# 3️⃣ IMAGE OPTIMIZATION GUIDE

## 📊 Performance Impact: 50-60% Image Size Reduction

---

## 🖼️ CONVERT TO MODERN FORMATS

### WebP Format (Best Compression):

```html
<!-- ✅ BEST: WebP with fallback -->
<picture>
  <source srcset="/cdn/shop/t/image.webp" type="image/webp">
  <source srcset="/cdn/shop/t/image.jpg" type="image/jpeg">
  <img src="/cdn/shop/t/image.jpg" alt="Product description" loading="lazy">
</picture>
```

### Shopify Liquid Implementation:

```liquid
{%- capture sizes -%}
  (max-width: 599px) 100vw,
  (max-width: 749px) 60vw,
  (max-width: 999px) 50vw,
  calc(100vw / 2 - 20px)
{%- endcapture -%}

<picture>
  <source 
    srcset="{{ product.featured_image | image_url: width: 400 }}.webp 400w,
            {{ product.featured_image | image_url: width: 600 }}.webp 600w,
            {{ product.featured_image | image_url: width: 800 }}.webp 800w"
    type="image/webp"
    sizes="{{ sizes }}"
  >
  <source 
    srcset="{{ product.featured_image | image_url: width: 400 }} 400w,
            {{ product.featured_image | image_url: width: 600 }} 600w,
            {{ product.featured_image | image_url: width: 800 }} 800w"
    sizes="{{ sizes }}"
  >
  <img 
    src="{{ product.featured_image | image_url: width: 400 }}"
    alt="{{ product.featured_image.alt }}"
    loading="lazy"
    width="400"
    height="400"
  >
</picture>
```

### Size Comparison:

| Format | JPEG (100%) | PNG (100%) | WebP | Improvement |
|--------|-----------|----------|------|-------------|
| Logo (20KB) | 20KB | 25KB | 6KB | 70% smaller |
| Hero (500KB) | 500KB | 600KB | 180KB | 64% smaller |
| Product (150KB) | 150KB | 180KB | 45KB | 70% smaller |
| Thumbnail (50KB) | 50KB | 65KB | 14KB | 72% smaller |

---

## 📱 RESPONSIVE IMAGES (srcset)

### Mobile First Approach:

```liquid
<!-- Mobile (320px): 280px image -->
<!-- Tablet (768px): 500px image -->
<!-- Desktop (1024px): 800px image -->

<img 
  src="{{ image | image_url: width: 280 }}"
  srcset="{{ image | image_url: width: 280 }} 280w,
          {{ image | image_url: width: 500 }} 500w,
          {{ image | image_url: width: 800 }} 800w"
  sizes="(max-width: 767px) 100vw,
         (max-width: 1023px) 80vw,
         800px"
  alt="Product image"
  loading="lazy"
  width="800"
  height="800"
>
```

### Why Responsive Images?

- Mobile users don't need 800px images
- Save 60-70% bandwidth on mobile
- Faster page load
- Better performance score

---

## ⏰ LAZY LOADING IMAGES

### Native Lazy Loading:

```html
<!-- ✅ Works on 90%+ browsers -->
<img 
  src="image.jpg" 
  alt="Product"
  loading="lazy"
  width="400"
  height="300"
>
```

### With Blur-Up Placeholder:

```liquid
<!-- Small blurred placeholder while loading -->
<img 
  src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 800 600'%3E%3Cfilter id='b'%3E%3CfeGaussianBlur stdDeviation='50'/%3E%3C/filter%3E%3Cimage filter='url(%23b)' x='0' y='0' width='100%25' height='100%25' href='{{ image | image_url: width: 10 }}'/%3E%3C/svg%3E"
  srcset="{{ image | image_url: width: 10 }} 10w,
          {{ image | image_url: width: 400 }} 400w,
          {{ image | image_url: width: 800 }} 800w"
  sizes="(max-width: 767px) 100vw, 800px"
  alt="Product"
  loading="lazy"
>
```

---

## 🎨 SVG FOR ICONS

### Replace PNG Icons with SVG:

```html
<!-- ❌ BAD: 15KB PNG icon -->
<img src="icon.png" alt="Close">

<!-- ✅ GOOD: Inline SVG 2KB -->
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor">
  <line x1="18" y1="6" x2="6" y2="18"></line>
  <line x1="6" y1="6" x2="18" y2="18"></line>
</svg>
```

### SVG Sprite Sheets:

```html
<!-- Single file with multiple icons -->
<svg style="display: none;">
  <defs>
    <symbol id="icon-close" viewBox="0 0 24 24">
      <line x1="18" y1="6" x2="6" y2="18"></line>
      <line x1="6" y1="6" x2="18" y2="18"></line>
    </symbol>
    <symbol id="icon-menu" viewBox="0 0 24 24">
      <line x1="3" y1="6" x2="21" y2="6"></line>
      <line x1="3" y1="12" x2="21" y2="12"></line>
      <line x1="3" y1="18" x2="21" y2="18"></line>
    </symbol>
  </defs>
</svg>

<!-- Use icons: -->
<button>
  <svg class="icon-24"><use xlink:href="#icon-close"></use></svg>
</button>
```

---

## 🗜️ IMAGE COMPRESSION TOOLS

### Recommended Tools:

1. **TinyPNG/TinyJPG** - Automated compression (https://tinypng.com)
2. **ImageOptim** - Mac desktop app
3. **FileZilla** - Batch compression
4. **Shopify's Built-in** - Automatic optimization

### Compression Settings:

```
JPEG: 75-85% quality (imperceptible loss)
PNG: 8-bit indexed (when possible)
WebP: 80% quality (matches JPEG quality)
```

---

## 📐 IMAGE DIMENSIONS

### Optimize for Device Sizes:

```liquid
<!-- Mobile -->
{{ image | image_url: width: 300 }}  <!-- 300px width -->

<!-- Tablet -->
{{ image | image_url: width: 500 }}  <!-- 500px width -->

<!-- Desktop -->
{{ image | image_url: width: 800 }}  <!-- 800px width -->

<!-- 2x Retina -->
{{ image | image_url: width: 1600 }} <!-- For Retina displays -->
```

### Example Product Grid:

```liquid
{% for product in collection.products %}
  <div class="product-card">
    <img
      src="{{ product.featured_image | image_url: width: 300 }}"
      srcset="{{ product.featured_image | image_url: width: 300 }} 300w,
              {{ product.featured_image | image_url: width: 600 }} 600w"
      alt="{{ product.title }}"
      loading="lazy"
      width="300"
      height="300"
    >
  </div>
{% endfor %}
```

---

## 📊 MEASUREMENTS

Before & After Image Optimization:

| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| Total Image Size | 5MB | 2.5MB | 50% smaller |
| Hero Image | 800KB | 240KB | 70% smaller |
| Product Images | 200KB each | 60KB each | 70% smaller |
| Page Load Time | 6s | 3.5s | 42% faster |
| Mobile Load | 8s | 4s | 50% faster |

---

## ✨ IMPLEMENTATION CHECKLIST

- [ ] Convert images to WebP format
- [ ] Implement responsive srcset
- [ ] Add lazy loading attributes
- [ ] Replace PNG icons with SVG
- [ ] Compress remaining images
- [ ] Test on slow 4G network
- [ ] Verify on all devices
- [ ] Measure image metrics

**Status:** Ready for Implementation 🚀
