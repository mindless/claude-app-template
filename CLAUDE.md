# [Project Name]

## Technical Stack

- **Framework**: Next.js 16.1.1 with App Router
- **Language**: TypeScript
- **Styling**: Tailwind CSS 4.1.18
- **Package Manager**: npm

## Architecture

- App Router (`app/` directory)
- Server Components by default
- Client Components (`'use client'`) only when needed for interactivity

## File Structure

```
app/           # Routes and pages
components/    # React components
  ui/          # Reusable UI primitives
  sections/    # Page sections
  layout/      # Layout components
lib/           # Utilities and helpers
public/        # Static assets
```

## Development

```bash
npm run dev    # Start development server
npm run build  # Production build
npm run lint   # Run linter
```

---

# Reference Patterns (Optional)

> These patterns are available if your project needs them. Delete sections you don't use.

## Animation Patterns (if using Framer Motion)

### Scroll Animations

- **Fade Up**: Elements start hidden and translate up when entering viewport
- **Stagger**: Children elements animate sequentially with configurable delay

```tsx
"use client";
import { motion } from "framer-motion";

export const FadeIn = ({ children }) => (
  <motion.div
    initial={{ opacity: 0, y: 20 }}
    whileInView={{ opacity: 1, y: 0 }}
    viewport={{ once: true }}
    transition={{ duration: 0.6, ease: "easeOut" }}
  >
    {children}
  </motion.div>
);
```

### Hover Effects

- Opacity change on hover for interactive elements
- Subtle scale transform for cards and buttons

### Timing Guidelines

- Duration: `0.5s` to `0.8s` for entering animations
- Easing: `easeOut` or custom cubic-bezier

## Design System (if defining custom tokens)

### Colors

```
- Primary Background: [your-color]
- Primary Text: [your-color]
- Secondary Text: [your-color]
- Accent: [your-color]
```

### Typography

```
- Heading Font: [your-font]
- Body Font: [your-font]
```

### Spacing

- Use consistent padding scale (e.g., `py-24`, `py-32`)
- Container padding: balanced horizontal spacing

## Image Handling (if using next/image)

- Use `next/image` for all bitmap images
- Use `placeholder="blur"` where possible for better UX
- Maintain aspect ratios (common: 16:9, 4:3)
- Configure `images.remotePatterns` in `next.config.js` for external sources
