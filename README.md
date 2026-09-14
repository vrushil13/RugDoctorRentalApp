# Vrushil Patel — Portfolio Website

A cinematic, interactive, season-switching developer portfolio built with pure HTML/CSS/JS. Zero build step. One file. Deploy in 60 seconds.

## Features

- **4 Season Themes** — Spring 🌸, Summer ☀️, Autumn 🍂, Winter ❄️ with animated particle effects, smooth transitions, and persistent state (localStorage)
- **Animated Canvas Particles** — season-aware particle shapes: snowflakes, petals, leaves, rays
- **Typewriter Hero** — cycling role descriptors with cursor animation
- **Interactive Terminal** — typewriter-animated terminal section with your info
- **Scroll Reveal Animations** — sections and timeline items animate in on scroll
- **Animated Timeline** — vertical work history with dot progression
- **Project Cards** — hover tilt + glass-glow effect
- **Contact Form** — with honeypot anti-spam, rate limiting (30s cooldown), input sanitization, and validation
- **Custom Cursor** — accent-colored dot that reacts to interactive elements
- **Scroll Progress Bar** — thin accent line at top of viewport
- **Fully Accessible** — ARIA labels, skip link, reduced-motion support, semantic HTML
- **Responsive** — mobile-first with hamburger nav

---

## Deployment (3 options)

### Option 1 — Netlify Drop (Fastest, ~30 seconds)

1. Go to [netlify.com/drop](https://app.netlify.com/drop)
2. Drag and drop `index.html` onto the page
3. Your site is live — done!

To use a custom domain: Site settings → Domain management → Add custom domain

---

### Option 2 — Vercel (Recommended for custom domain + analytics)

1. Push `index.html` to a GitHub repo (can be a new one named `portfolio`)
2. Go to [vercel.com](https://vercel.com) → New Project → Import your repo
3. Vercel auto-detects it's a static site — click **Deploy**
4. Add your custom domain under Project → Domains

No `vercel.json` needed — Vercel serves `index.html` automatically.

---

### Option 3 — GitHub Pages (Free forever)

1. Create a repo named `yourusername.github.io` (e.g. `vrushil13.github.io`)
2. Add `index.html` to the root
3. Go to Settings → Pages → Source: main branch → Save
4. Visit `https://vrushil13.github.io` — live in ~2 minutes

---

## Connecting a Real Contact Form

The form currently simulates a send (demo mode). To make it actually send emails:

### Option A — EmailJS (No backend needed, free tier available)

1. Sign up at [emailjs.com](https://www.emailjs.com)
2. Create a service (Gmail) and an email template
3. Add to `<head>`:
   ```html
   <script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
   <script>emailjs.init('YOUR_PUBLIC_KEY');</script>
   ```
4. Replace the `setTimeout` block in `handleContact()` with:
   ```javascript
   emailjs.send('YOUR_SERVICE_ID', 'YOUR_TEMPLATE_ID', {
     from_name: name,
     from_email: email,
     message: msg
   }).then(() => {
     showStatus("Message sent! I'll get back to you soon.", true);
   }).catch(() => {
     showStatus('Something went wrong. Please email me directly.', false);
   }).finally(() => {
     btn.textContent = 'Send message →';
     btn.disabled = false;
   });
   ```

### Option B — Formspree (Paste one line)

1. Sign up at [formspree.io](https://formspree.io) and create a form
2. Change `<form id="contact-form" novalidate>` to:
   ```html
   <form id="contact-form" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
   ```
3. Change the button type to `type="submit"` and remove the `onclick`

---

## Customization Guide

### Update personal info
All content is in the HTML. Search for these strings to find and update them:

| What to update | Search for |
|---|---|
| Name | `Vrushil Patel` / `VP` |
| Email | `vrushil13@gmail.com` |
| GitHub | `github.com/vrushil13` |
| LinkedIn | `linkedin.com/in/vrushil13` |
| Phone | `+14313741787` |
| Location | `Winnipeg, MB` |
| Hero descriptors | `words = [...]` array in script |

### Change default season
Find `body.winter` near the top and change to `body.spring`, `body.summer`, or `body.autumn`.

In the JS, change `let currentSeason = 'winter';` to match.

### Add a real resume download button
Replace the ghost button in the hero with:
```html
<a href="/VrushilPatelResume2026.pdf" class="btn-ghost" download>Download Resume</a>
```
Upload your PDF to the same folder as `index.html`.

### Add Google Analytics
Paste your GA4 snippet just before `</head>`:
```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer=window.dataLayer||[];
  function gtag(){dataLayer.push(arguments);}
  gtag('js',new Date()); gtag('config','G-XXXXXXXXXX');
</script>
```

---

## File Structure

```
portfolio/
├── index.html          ← Everything (HTML + CSS + JS, single file)
├── README.md           ← This file
└── VrushilPatelResume.pdf   ← (optional) for download button
```

---

## Browser Support

Works in all modern browsers: Chrome, Firefox, Safari, Edge (last 2 versions).

The custom cursor is hidden on touch devices automatically. Reduced-motion is respected via `prefers-reduced-motion` media query — particles and typewriter effects are disabled for users who prefer less motion.

---

## Performance

- Zero build step, zero dependencies loaded at runtime (only Google Fonts)
- Canvas particle system uses `requestAnimationFrame` with `cancelAnimationFrame` cleanup
- Intersection Observer for scroll reveals (no scroll event spam)
- Resize debounced (200ms) for canvas reinitialization
- Images: none (pure CSS/SVG design — loads instantly)
- Estimated Lighthouse score: 95+ Performance, 100 Accessibility, 100 Best Practices, 90+ SEO

---

## Security Notes (Contact Form)

The form includes:
- **Honeypot field** — hidden input that bots fill but humans don't; filled = rejected
- **Rate limiting** — 30-second cooldown between sends (client-side)
- **Input sanitization** — strips HTML tags, enforces 2000-char limit
- **Email validation** — regex check before send
- **No email exposed in source** — when using EmailJS/Formspree, your real email stays server-side

For production, pair with a server-side rate limiter (Cloudflare Workers, Vercel Edge Functions) for stronger protection.
