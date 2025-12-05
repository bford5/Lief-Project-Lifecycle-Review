# Lief Platform — Comprehensive Project & Timeline Analysis

**Brandon Ford** | Lead Developer & Architect  
**Role:** Sole technical lead responsible for architecture decisions, implementation, and coordinating a 3-person development team, 1 designer, and 1 marketing specialist
**Duration:** January 2022 – February 2025 (3 years)

---

## The Challenge

Cannabis dispensaries face unique digital marketing constraints — advertising restrictions on major platforms force them to rely heavily on organic web presence. Most dispensaries lack the technical resources or budget for custom website development, yet they need professional, SEO-optimized sites to compete.

Lief Technologies LLC needed two things: a marketing website to attract dispensary clients, and a scalable template system to serve them efficiently.

---

## My Solution

I led development of a dual-product platform:

**Company Website (B2B Marketing)**

- Built a high-performance Astro + React site with ISR caching
- Integrated Directus CMS for lead capture via Zod-validated forms
- Achieved excellent Lighthouse scores through Core Web Vitals optimization

**Template System (White-Label Platform)**

- Designed "Shipyard" — a 3-tier component architecture (Widgets → Components → Layouts)
- Created a 50+ variable CSS theming system with 6 pre-built color schemes
- Implemented multi-tenant architecture enabling single codebase to serve multiple clients
- Led CMS migration from PayloadCMS to Directus mid-project

---

## Results

| Metric                   | Outcome                                          |
| ------------------------ | ------------------------------------------------ |
| **Components Built**     | 70+ reusable parts in organized hierarchy        |
| **Development Velocity** | Per-client deployment reduced from weeks to days |
| **Total Commits**        | 230+ across 17 development phases                |
| **Team Coordination**    | 3-person team with established coding standards  |

---

## Technology Stack

`Astro` `React` `TypeScript` `Tailwind CSS` `Directus CMS` `Vercel ISR` `HeadlessUI` `Zod` `Sentry`

---

## Key Insight

> "The most valuable architectural decision was building theming as a first-class concern. CSS variables from day one enabled 6 distinct visual themes without touching component logic — transforming per-client customization from a development task into a configuration change."

---

## Learn More

- **Full Portfolio Review:** `regreview-v1.md`
- **Technical Deep Dive:** `codereview-v1.md`
- **Repository History:** `reporeview-v1.md`

---

_This case study summarizes 3 years of development work on two production web applications serving the U.S. cannabis dispensary market._
