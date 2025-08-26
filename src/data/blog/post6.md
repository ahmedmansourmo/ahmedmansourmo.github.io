---
title: SSR vs CSR — Which Rendering Strategy Should You Use?
description: A practical comparison of Server-Side Rendering (SSR) and Client-Side Rendering (CSR)—how they work, trade-offs, performance, SEO, and when to choose each.
date: "2025-08-24"
image: "/images/posts/post-1.jpg"
tags:
  - Web
  - SSR
  - CSR
  - Performance
---

Understanding rendering strategies helps you deliver fast, SEO-friendly, and maintainable web apps. Here’s a concise guide to SSR vs CSR.

## What is CSR (Client-Side Rendering)?

The server sends a minimal HTML shell and a JavaScript bundle. The browser downloads JS, builds the UI, and fetches data. First paint can be fast, but content may be blank until hydration completes.

Pros:
- Great for rich, highly interactive apps
- Can cache static assets on CDN
- Simple hosting (static + API)

Cons:
- Slower Time to First Byte (TTFB) and First Contentful Paint (FCP)
- SEO depends on prerendering/bot support
- Heavier JS on the client

CSR example (React):

```jsx
function Products() {
  const [items, setItems] = React.useState([]);
  React.useEffect(() => {
    fetch('/api/products').then(r => r.json()).then(setItems);
  }, []);
  return items.map(p => <div key={p.id}>{p.name}</div>);
}
```

## What is SSR (Server-Side Rendering)?

HTML is rendered on the server per request (or edge) and sent to the browser. Users see content earlier; hydration attaches interactivity on the client.

Pros:
- Better FCP/LCP and perceived performance
- SEO-friendly out of the box
- Personalized content at request time

Cons:
- Higher server cost/complexity
- Longer TTFB if server/data is slow
- Still ships client JS for interactivity

SSR example (Next.js):

```jsx
// pages/products.js (Next 12/13 pages router)
export async function getServerSideProps() {
  const res = await fetch('https://api.example.com/products');
  const items = await res.json();
  return { props: { items } };
}
export default function Page({ items }) {
  return items.map(p => <div key={p.id}>{p.name}</div>);
}
```

## Performance and SEO

- CSR: use code-splitting, lazy, and route-based bundles; prefetch data and cache aggressively.
- SSR: stream HTML, cache at the edge, and avoid slow blocking calls. Hydrate only what’s interactive.

Key metrics: TTFB (server), FCP/LCP (paint), TTI (hydration/JS), CLS (layout stability).

## When to choose which?

- Choose CSR when: app is SPA-like, authentication heavy, and SEO is limited or handled via prerendering.
- Choose SSR when: SEO matters, content must be visible fast, or you need per-request personalization.

## Hybrids and modern options

- SSG/ISR: pre-render at build time and revalidate for scale.
- Islands/Partial Hydration (Astro): render most HTML on server, hydrate only interactive islands—great DX and performance.
- Edge SSR/Streaming: reduce latency and show content progressively.

## Summary

There’s no one-size-fits-all. Use SSR for content-first and SEO; use CSR for app-first interactivity. Many stacks mix both for the best user experience.
