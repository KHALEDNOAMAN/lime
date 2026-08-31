# Lime - Performance & Deployment Guide

## Performance Optimization

### Image Optimization
- Convert images to WebP format (30-50% smaller)
- Use responsive images with srcset
- Lazy load below-the-fold images
```html
<img loading="lazy" src="hero.webp" alt="Hero" />
```

### Bundle Size
```bash
# Analyze bundle
npm run build -- --stats
npx webpack-bundle-analyzer dist/stats.json
```

### Lighthouse Targets
| Metric | Target | How |
|--------|--------|-----|
| Performance | > 95 | Optimize images, lazy load |
| Accessibility | > 95 | Alt text, contrast, focus |
| Best Practices | > 95 | HTTPS, no console errors |
| SEO | > 95 | Meta tags, sitemap |

## Deployment

### Vercel (Recommended)
```bash
npm i -g vercel
vercel --prod
```

### Netlify
```bash
npm run build
# Drag dist/ to Netlify dashboard
```

### Docker
```dockerfile
FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
```

## Tailwind CSS Tips
- Purge unused styles in production (built-in with Tailwind 3+)
- Use `@apply` sparingly — prefer utility classes
- Group responsive variants: `sm:` → `md:` → `lg:`

## Animation Performance
- Use `transform` and `opacity` for GPU-accelerated animations
- Avoid animating `width`, `height`, `margin`
- Use `will-change` sparingly for known animations