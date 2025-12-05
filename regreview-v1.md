# Lief Platform — Project Portfolio Review

**Author:** Brandon Ford (bforddev7)  
**Project Duration:** January 2022 – February 2025 (3 years)  
**Core Development:** April 2023 – November 2024 (20 months)  
**Role:** Lead Developer & Architect  
**Version:** v1

---

## Executive Summary

The Lief Platform is a full-stack web development initiative I led that created two interconnected products for the U.S. cannabis dispensary market:

1. **Lief Company Website** — A B2B marketing platform showcasing services to dispensary owners
2. **Lief Template System** — A white-label, multi-tenant website solution for dispensary clients

Over 3 years — from early research and concept development through core implementation and eventual project closure — I architected, developed, and deployed both products, collaborating with designers, marketing teams, and fellow developers. The project underwent a complete architectural rewrite in January 2024, with 20 months of intensive core development. This work demonstrates expertise in modern web technologies, scalable architecture design, CMS integration, and full product lifecycle management.

> **Timeline Note:** The detailed repository analysis documents (`reporeview-v1.md` and `codereview-v1.md`) focus primarily on the 2024 repository era, when the current Astro-based codebases were created. This portfolio document reflects the broader 3-year project lifecycle, including earlier concept work, research, and prototyping phases that preceded the current repositories.

> **Repository Note:** Technical details referenced throughout this document (commit counts, branch names, file structures) come from internal/private repositories that are not publicly accessible. The repositories themselves remain private, though the work and patterns described here accurately represent the projects. Requests to view these private repositories are permitted under a signed mutual Non Disclosure Agreement (NDA).

---

## Project 1: Lief Company Website

### What It Is

A professional marketing website for Lief Technologies LLC, designed to attract cannabis dispensary owners seeking affordable, optimized website solutions. The site serves as the company's primary digital presence and lead generation tool.

**Live URL:** liefplatform.com

### The Challenge

The cannabis industry faces unique digital marketing challenges due to advertising restrictions on major platforms. Dispensaries need strong organic web presence but often lack technical resources or budget for custom development. Lief needed a website that:

- Clearly communicated value propositions to dispensary owners
- Generated qualified leads through contact forms
- Ranked well in search engines for industry-relevant terms
- Demonstrated the quality of work clients could expect

### My Solution

I built a high-performance, SEO-optimized website using the Astro framework with a focus on Core Web Vitals and accessibility. Key decisions included:

**Technology Stack:**

- **Astro 4.x** — Modern static-site generator with partial hydration for optimal performance
- **React** — Interactive components (forms, notifications) where needed
- **Tailwind CSS** — Utility-first styling for rapid, consistent development
- **Vercel** — Edge deployment with Incremental Static Regeneration (ISR)
- **Directus CMS** — Headless content management for form submissions

**Architecture Highlights:**

- Implemented ISR caching with 6-hour expiration to balance freshness with performance
- Built a global toast notification system using Nanostores for cross-component state
- Created Zod validation schemas for type-safe form handling
- Integrated Sentry for error monitoring and PostHog/Google Analytics for user insights

### Results & Impact

- Successfully deployed to production in May 2024
- Achieved excellent Lighthouse scores through performance optimization
- Implemented comprehensive SEO meta tags, Open Graph, and Twitter Cards
- Built accessible components meeting WCAG guidelines
- Created a scalable codebase that evolved through 7 development phases and 106 commits

---

## Project 2: Lief Template System (Dispensary Platform)

### What It Is

A sophisticated, white-label website template system designed to rapidly deploy customized websites for cannabis dispensary clients. The platform enables Lief to serve multiple dispensaries with unique branding while maintaining a shared, maintainable codebase.

### The Challenge

Creating individual websites for each dispensary client would be time-consuming and expensive. The industry needed a solution that could:

- Deploy client sites quickly with minimal customization effort
- Support multiple visual themes to differentiate client brands
- Integrate with common cannabis menu providers (Dutchie, iHeartJane)
- Scale to dozens of clients without multiplying maintenance burden
- Allow non-technical team members to update content

### My Solution

I designed and implemented "Shipyard" — a layered component architecture that separates concerns into reusable building blocks:

**The Shipyard Architecture:**

```
Widgets (Atomic)     → Buttons, Icons, Links, Cards
    ↓
Components (Molecular) → Heroes, Banners, Forms, Navigation
    ↓
Layouts (Organisms)   → Page templates, Header/Footer systems
```

This architecture enables:

- **Rapid Assembly:** New client sites are built by combining existing widgets and components
- **Consistent Quality:** All clients benefit from accessibility and performance improvements
- **Theme Flexibility:** CSS variable-based theming allows dramatic visual customization

**Technology Stack:**

- **Astro 4.x + React** — Hybrid rendering for optimal performance
- **HeadlessUI** — Accessible, unstyled React components for complex interactions
- **SwiperJS** — Touch-friendly carousels for product banners
- **Directus CMS** — Multi-tenant content management with dispensary-specific filtering
- **CSS Custom Properties** — Runtime-switchable themes supporting 6+ color schemes

**Key Technical Achievements:**

1. **Multi-Tenant Architecture:** Built tenant-aware API routes that filter content by dispensary ID, enabling a single deployment to serve multiple clients

2. **Advanced Theming System:** Created 50+ CSS variables covering colors, buttons, links, and text styles with 6 pre-built themes (Dark Green, Light Green, Modern Neutral, Deep Ocean, Mint Runtz, Orange Treat)

3. **CMS Migration:** Successfully migrated from PayloadCMS to Directus mid-project, improving developer experience and reducing infrastructure costs

4. **Import Alias System:** Designed intuitive path aliases (`@shipyard_v1/widgets/buttons/`) that make the codebase navigable and self-documenting

### Team Collaboration

This project involved coordinating with multiple contributors:

- **Developer Colleague 1** — Assisted with CMS integration and initial frontend component development
- **Developer Colleague 2** — Assisted with Feature development including page components and interactive elements
- **Marketing Team** — Design reviews and content requirements throughout

I established coding conventions, review processes, and documentation standards that enabled effective collaboration across skill levels.

### Results & Impact

- Deployed demo site demonstrating full platform capabilities
- Created 70+ reusable components organized in clear hierarchy
- Reduced estimated per-client development time from weeks to days
- Built foundation for 126 commits across 10 development phases
- Established patterns now used across the organization

---

## Technical Skills Demonstrated

### Frontend Development

| Skill            | Proficiency           | Evidence                                         |
| ---------------- | --------------------- | ------------------------------------------------ |
| **Astro**        | Advanced              | Built two production sites from scratch          |
| **React**        | Advanced              | Form handling, state management, dynamic UIs     |
| **TypeScript**   | Intermediate-Advanced | Type-safe APIs, Zod schemas, interface design    |
| **Tailwind CSS** | Advanced              | Custom theming, responsive design, CSS variables |
| **HTML/CSS**     | Expert                | Semantic markup, accessibility, animations       |

### Backend & Integration

| Skill              | Proficiency  | Evidence                                                          |
| ------------------ | ------------ | ----------------------------------------------------------------- |
| **REST APIs**      | Advanced     | Built API routes with proper error handling, HTTP method barriers |
| **Headless CMS**   | Advanced     | Directus SDK integration, PayloadCMS experience                   |
| **Authentication** | Intermediate | Static token auth, environment variable security                  |
| **Form Handling**  | Advanced     | Zod validation, sanitization, error states                        |

### DevOps & Infrastructure

| Skill          | Proficiency  | Evidence                                                 |
| -------------- | ------------ | -------------------------------------------------------- |
| **Vercel**     | Advanced     | ISR configuration, serverless functions, edge deployment |
| **Git**        | Advanced     | Branch strategies, multi-contributor workflows           |
| **CI/CD**      | Intermediate | Automated deployments, build optimization                |
| **Monitoring** | Intermediate | Sentry error tracking, analytics integration             |

### UI/UX & Accessibility

| Skill                 | Proficiency           | Evidence                                              |
| --------------------- | --------------------- | ----------------------------------------------------- |
| **Accessibility**     | Intermediate-Advanced | ARIA labels, focus states, screen reader support      |
| **Responsive Design** | Advanced              | Mobile-first/desktop-first hybrid, breakpoint systems |
| **Animation**         | Intermediate          | HeadlessUI transitions, CSS animations                |
| **SEO**               | Intermediate          | Meta tags, sitemaps, structured data                  |

---

## Key Accomplishments (Resume-Ready Bullets)

### Lief Company Website

- **Architected and deployed** a production marketing website using Astro, React, and TypeScript, achieving excellent Lighthouse performance scores
- **Implemented Incremental Static Regeneration (ISR)** with Vercel, reducing server load while maintaining content freshness through 6-hour cache cycles
- **Built type-safe form handling** using Zod validation schemas with global toast notifications via Nanostores state management
- **Integrated headless CMS** (Directus) with secure API routes, static token authentication, and proper HTTP method barriers
- **Established SEO foundations** including Open Graph tags, Twitter Cards, sitemaps, and accessibility-compliant markup

### Lief Template System

- **Designed "Shipyard" component architecture** — a 3-tier system (Widgets, Components, Layouts) enabling rapid assembly of client websites from 70+ reusable parts
- **Created advanced theming system** with 50+ CSS custom properties supporting 6 distinct color schemes switchable at runtime
- **Led CMS migration** from PayloadCMS to Directus, improving developer experience and reducing infrastructure complexity
- **Coordinated multi-contributor development** across 10 sprints, establishing coding standards and review processes for a 3-person team
- **Built multi-tenant architecture** with dispensary-specific content filtering, enabling single codebase to serve multiple clients

---

## Challenges Overcome

### Technical Challenges

**1. Directus SDK + Astro SSR Compatibility**

_Problem:_ The Directus SDK threw permission errors when imported into Astro API routes from a shared library file.

_Solution:_ Documented the issue thoroughly and implemented a workaround where each API route instantiates its own Directus client. Created inline documentation to help future developers understand the constraint.

**2. ISR + Form Submission Conflicts**

_Problem:_ After implementing ISR caching, form submissions stopped working correctly.

_Solution:_ Identified that API routes were being cached along with static pages. Configured ISR `exclude` patterns to keep form endpoints dynamic while caching content pages.

**3. Firefox Accordion Accessibility**

_Problem:_ Tab navigation prioritized accordion elements incorrectly in Firefox, disrupting keyboard accessibility.

_Solution:_ Documented the issue for future resolution and implemented HeadlessUI Disclosure components in the template system as a proven accessible alternative.

### Process Challenges

**1. Mid-Project CMS Migration**

_Challenge:_ PayloadCMS proved more complex than needed for our use case. Migrating mid-project risked delays and bugs.

_Approach:_ Created a dedicated branch for the migration, preserved the old implementation in an archive branch, and incrementally moved features over two weeks. The result was a cleaner, more maintainable integration.

**2. Balancing Speed vs. Quality**

_Challenge:_ Pressure to launch quickly while maintaining code quality for long-term maintenance.

_Approach:_ Established a clear TODO/NOTE comment convention that documents technical debt without blocking progress. Created tracking for future improvements while shipping functional features.

---

## Professional Growth

### Skills Developed During This Project

1. **Component Architecture Design** — Evolved from flat component structures to sophisticated layered systems with clear boundaries and reusability patterns

2. **TypeScript Depth** — Progressed from basic typing to advanced patterns including Zod schemas, union types, and generic interfaces

3. **Accessibility Expertise** — Deepened understanding of WCAG requirements, ARIA attributes, and keyboard navigation patterns

4. **Technical Leadership** — Gained experience coordinating contributors, establishing standards, and documenting decisions for team adoption

5. **Full-Stack Integration** — Connected frontend, CMS, authentication, and deployment into cohesive production systems

### Lessons Learned

1. **Document Decisions, Not Just Code** — Comments explaining _why_ something works a certain way are more valuable than comments explaining _what_ the code does

2. **Theming Should Be First-Class** — Retrofitting theming into an existing codebase is painful; CSS variables from day one enable flexibility

3. **Validate Early, Validate Once** — Redundant validation layers add confusion; pick one source of truth (like Zod) and use it consistently

4. **Archives > Deletions** — Keeping deprecated code in archive branches preserves context and enables rollbacks without cluttering active development

---

## Technology Summary

### Languages & Frameworks

`Astro` `React` `TypeScript` `JavaScript` `HTML5` `CSS3`

### Styling & UI

`Tailwind CSS` `HeadlessUI` `CSS Custom Properties` `SwiperJS`

### Backend & Data

`Directus CMS` `PayloadCMS` `REST APIs` `Zod` `PostgreSQL`

### DevOps & Tools

`Vercel` `Git` `GitHub` `Sentry` `PostHog` `Google Analytics`

### Concepts & Patterns

`Incremental Static Regeneration` `Headless CMS Architecture` `Multi-Tenant Systems` `Component-Driven Development` `Accessibility (WCAG)` `SEO Optimization`

---

## Portfolio Presentation

### For GitHub Profile

> **Lief Platform** — Full-stack web platform for the cannabis dispensary industry featuring a B2B marketing site and white-label template system. Built with Astro, React, TypeScript, and Directus CMS. Deployed on Vercel with ISR caching. Includes "Shipyard" — a custom 70+ component architecture enabling rapid client site deployment.

### For LinkedIn

> Led development of a multi-product web platform serving the cannabis dispensary market. Architected and deployed a production marketing website and white-label template system using modern technologies (Astro, React, TypeScript). Designed "Shipyard" component architecture with 70+ reusable parts and 6-theme CSS variable system. Coordinated 3-person development team across 10+ sprints, establishing coding standards and documentation practices.

### For Resume (Condensed)

**Lead Developer — Lief Platform** | Jan 2022 – Feb 2025 (Core Dev: Apr 2023 – Nov 2024)

- Architected and deployed 2 production web applications using Astro, React, TypeScript, and Tailwind CSS
- Designed "Shipyard" component system with 70+ reusable parts enabling rapid client site deployment
- Implemented ISR caching, headless CMS integration, and multi-tenant content filtering
- Led 3-person team, establishing coding standards and coordinating 230+ commits across both projects

---

## Interview Discussion Points

### "Tell me about a project you're proud of"

> "I led the development of Lief Platform, a web solution for cannabis dispensaries. The most interesting challenge was creating a template system that could serve multiple clients from a single codebase. I designed what we called 'Shipyard' — a layered component architecture where widgets combine into components, which combine into layouts. Combined with a CSS variable theming system I built with 50+ properties and 6 pre-built themes, we could spin up a visually distinct client site in days instead of weeks. The architecture decisions I made there are still guiding development on the platform."

### "Describe a technical challenge you overcame"

> "Mid-project, we realized our CMS choice — PayloadCMS — was more complex than we needed. Migrating to Directus while maintaining velocity was tricky. I created a dedicated branch, preserved the old implementation in an archive, and incrementally moved features over two weeks. The key was documenting everything: why we switched, what changed, and how the new system worked. The migration actually improved our developer experience because Directus was simpler to work with for our use case."

### "How do you handle working with others on code?"

> "On the Lief template system, I worked with two other developers at different stages. I established conventions early — a comment format for TODOs with priority levels, a 'NOTE' and 'README' system for documenting quirks, and an archive branch strategy for deprecated code. When new developers joined, they could get up to speed quickly because the patterns were documented inline. We even had a 'SKILLISSUE' tag for self-identified learning moments, which made code review discussions productive rather than defensive."

### "What would you do differently?"

> "I'd implement the theming system from day one on both projects. The company site uses hardcoded colors, which is fine for a single brand, but I had to retrofit everything in the template system. Starting with CSS variables would have made the code more consistent across both projects. I'd also consolidate validation earlier — we ended up with redundant validation in the form components because I added Zod after initial development. Picking one source of truth from the start would have been cleaner."

---

## Conclusion

The Lief Platform project represents 3 years of work — from early concept and research through intensive core development and production deployment. The work demonstrates proficiency across the modern web development stack — frontend frameworks, headless CMS integration, deployment infrastructure, and collaborative development practices.

The most significant contribution is the "Shipyard" architecture, which transforms client website development from a bespoke process into systematic assembly of proven components. This approach embodies the software engineering principle of solving problems once and reusing solutions — the foundation of scalable, maintainable systems.

I'm proud of both the technical outcomes and the professional growth this project enabled. The challenges pushed me to deepen TypeScript knowledge, master component architecture design, and develop leadership skills in coordinating multi-contributor development. These are capabilities I'm excited to bring to future projects and teams.

---

## Related Documentation

This portfolio review is part of a three-document set analyzing the Lief Platform:

| Document                            | Purpose                                                        |
| ----------------------------------- | -------------------------------------------------------------- |
| **reporeview-v1.md**                | Git history and repository evolution (2024 repo era)           |
| **codereview-v1.md**                | Technical code analysis, architecture, patterns, and quality   |
| **regreview-v1.md** (this document) | Human-readable portfolio summary for GitHub, LinkedIn, resumes |

For a condensed single-page summary, see **lief-case-study.md**.

---

_This document was generated from comprehensive analysis of 230+ commits, 180+ source files, and 3 years of development history (20 months core development) across the lief-c-astro and lief-lite-mono repositories._
