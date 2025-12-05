# Lief Repository Review Report

**Generated:** December 3, 2025  
**Scope:** `lief-c-astro` and `lief-lite-mono` directories  
**Author:** Automated analysis of git history, code structure, and documentation  
**Version:** v1 — Anonymized for public sharing

---

## Executive Summary

This report documents the lifecycle and evolution of two interconnected Astro-based web projects within the Lief ecosystem:

| Project            | Purpose                               | Genesis     | Commits | Contributors                                                | Status             |
| ------------------ | ------------------------------------- | ----------- | ------- | ----------------------------------------------------------- | ------------------ |
| **lief-c-astro**   | Company marketing website (B2B)       | Feb 2, 2024 | 106     | 1 (bforddev7)                                               | Production         |
| **lief-lite-mono** | Dispensary template/demo system (B2C) | Feb 9, 2024 | 126     | 3 (bforddev7, Developer Colleague 1, Developer Colleague 2) | Active Development |

Both projects share a common technical foundation (Astro 4.x + React + Tailwind + Vercel) and are connected through periodic "bulk adds from AG source" synchronization, suggesting a shared private development environment.

> **Note:** This report references internal/private repositories. Repository URLs have been redacted; the repositories themselves are not publicly accessible. Requests to view these private repositories are permitted under a signed mutual Non Disclosure Agreement (NDA).

---

## Part 1: lief-c-astro — The Company Website

### Overview

- **Repository:** `[private repository]`
- **Live Site:** https://liefplatform.com
- **Purpose:** Marketing website for Lief's dispensary platform services
- **Total Commits:** 106
- **Primary Branch:** `prod/main`
- **Disk Size:** ~500MB (including node_modules)

### Chronological Development History

#### Phase 1: Genesis & Foundation (February 2024)

| Date        | Commit    | Description                                       |
| ----------- | --------- | ------------------------------------------------- |
| Feb 2, 2024 | `896e186` | **Genesis commit** — Astro project initialization |
| Feb 2, 2024 | `d8acc4f` | Core layout structure and base styling            |
| Feb 2, 2024 | `abef4e8` | Homepage hero section added                       |
| Feb 2, 2024 | `e3dc1aa` | Structure for remaining homepage content sections |

**Key Decisions:**

- Astro selected as the framework
- Tailwind CSS for styling
- Initial B2B marketing site structure established

#### Phase 2: Pre-Production Development (March–April 2024)

| Date          | Commit    | Description                                       |
| ------------- | --------- | ------------------------------------------------- |
| Mar 25, 2024  | `1aeb57c` | Major bulk update (multiple features)             |
| Mar 25, 2024  | `26ecfee` | Contact form setup, React integration added       |
| Mar 26, 2024  | `472eb3a` | Format/style corrections                          |
| Mar 27, 2024  | `9a1f469` | Phase 1 deployment prep                           |
| Mar 28, 2024  | `14db7a0` | Large feature addition                            |
| Mar 29, 2024  | `9161d6e` | FAQ accessibility improvements                    |
| Mar 30, 2024  | `199cb02` | Major production prep updates                     |
| Apr 1–2, 2024 | Multiple  | Site layout refresh, FAQ UX improvements, styling |

**Key Features Added:**

- React integration for interactive components
- Contact form (wiring pending)
- FAQ component with accessibility focus
- Perks component banner styling

#### Phase 3: Production Launch (May 2024)

| Date         | Commit    | Description                                       |
| ------------ | --------- | ------------------------------------------------- |
| May 11, 2024 | `29c49bd` | Component and style rework                        |
| May 25, 2024 | `3650a49` | **SEO components implemented**                    |
| May 26, 2024 | `ec98f13` | Link source updates, TODOs added                  |
| May 28, 2024 | `26210bf` | **Production main branch setup**                  |
| May 28, 2024 | `d75607c` | **PostHog analytics + Sentry.io integration**     |
| May 28, 2024 | `210c8b2` | Sentry configuration updates                      |
| May 29, 2024 | `b054cba` | SEO and Google rating improvements                |
| May 29, 2024 | `af76058` | **UploadThing image storage + Playform Compress** |
| May 30, 2024 | `87580c4` | **Partytown for offthread script loading**        |
| May 31, 2024 | `81acebf` | **Google Analytics implementation**               |

**Changelog (from `changelog.md`):**

- `[0.0.1]` May 28, 2024 — Production repo and Vercel deployment setup
- `[0.0.2]` May 28, 2024 — Legal verbiage and FAQ revisions
- `[0.0.3]` May 30, 2024 — Homepage SEO description adjusted
- `[0.0.4]` May 30, 2024 — Homepage SEO title and description updated

#### Phase 4: Accessibility & Performance (June–July 2024)

| Date         | Commit    | Description                                              |
| ------------ | --------- | -------------------------------------------------------- |
| Jun 4, 2024  | `4aa1549` | Minor cleanup                                            |
| Jun 9, 2024  | `ad97258` | Homepage glow style updates                              |
| Jun 12, 2024 | `19c8852` | **NavLink atom component + accessibility improvements**  |
| Jun 12, 2024 | `1d33368` | Firefox accessibility bug identified (accordion tabbing) |
| Jun 26, 2024 | `202cc91` | FreeTrial component added                                |
| Jul 5, 2024  | `e74e1f2` | Homepage product details revision                        |

**Notable Bug:**

- Firefox tabbing issue with AccordionItem component identified but not fully resolved

#### Phase 5: CMS Integration & ISR (August–September 2024)

| Date         | Commit    | Description                                                 |
| ------------ | --------- | ----------------------------------------------------------- |
| Aug 15, 2024 | `caafb3b` | PageSpeed Insights improvements                             |
| Aug 19, 2024 | Multiple  | Verbiage and UI corrections                                 |
| Aug 30, 2024 | `7f8dd7c` | **Directus SDK integration + forms**                        |
| Sep 3, 2024  | `0ee6051` | **Forms rebuilt with Zod validation + Toast notifications** |
| Sep 3, 2024  | `bd2020b` | ISR capability added to Astro config                        |
| Sep 3, 2024  | `8da026d` | **API route authentication via Directus static token**      |
| Sep 3, 2024  | `4830611` | ISR temporarily removed (bug), SSR fallback                 |
| Sep 3, 2024  | `f96dbbf` | ISR re-enabled after bug fix                                |
| Sep 6, 2024  | `ec70d06` | Design changes per designer review                          |

**Technical Challenge Documented:**

```
SKILLISSUE: when trying to implement isr in a prior commit I did this
incorrectly at first, i left a note for myself to go back and fix when
isr is needed
```

**ISR Configuration (from `astro.config.mjs`):**

```javascript
adapter: vercel({
  isr: {
    expiration: 60 * 60 * 6, // 6 hours
    bypassToken: '[redacted]',
    exclude: ['/api/contact', '/api/contact-get-started'],
  },
}),
```

#### Phase 6: Enterprise & Marketing Updates (September–October 2024)

| Date            | Commit    | Description                                 |
| --------------- | --------- | ------------------------------------------- |
| Sep 15, 2024    | `8e5704a` | SEO updates, styling, housekeeping          |
| Sep 22, 2024    | `7113206` | **Enterprise page added**, verbiage changes |
| Sep 23, 2024    | `53e63e0` | Privacy policy page revision                |
| Oct 21–22, 2024 | Multiple  | Bulk adds from AG source                    |

#### Phase 7: V2 Components & Blog (November 2024 — Latest)

| Date         | Commit    | Description                                                    |
| ------------ | --------- | -------------------------------------------------------------- |
| Nov 1, 2024  | `643b1c6` | **V2 components created** (Hero, Perks), marketing team review |
| Nov 1, 2024  | `9cd276d` | Small device navigation fix                                    |
| Nov 1, 2024  | `c695fe2` | Bad links corrected                                            |
| Nov 18, 2024 | `2134091` | **Blog index + 2 blog pages**, logo rotator started            |
| Nov 18, 2024 | `97dad42` | BlogCard script tag fix (current HEAD)                         |

### Current Architecture

```
lief-c-astro/src/
├── components/
│   ├── Analytics/          # GA, PostHog, Umami
│   ├── Atoms/              # NavLink
│   ├── BlogComponents/     # BlogCard, BlogContentSection
│   ├── Forms/              # ContactUs, GetStarted (React + Zod)
│   ├── Layout/             # Header, Footer, Navigation
│   ├── Notifications/      # ToastNotification (React)
│   ├── PageComponents/     # Hero, Perks, FAQ, Tiers (v1 & v2)
│   └── UI/                 # Accordion, Icons, Logos
├── layouts/
│   └── Layout.astro
├── lib/
│   └── directus.ts         # Directus SDK setup
├── pages/
│   ├── api/                # contact-us.ts, contact-get-started.ts
│   ├── blog/               # Blog index + static pages
│   ├── index.astro
│   ├── faq.astro
│   ├── get-started.astro
│   ├── lief-enterprise.astro
│   ├── privacy-and-terms.astro
│   └── product-details.astro
├── store/
│   └── toastStore.ts       # Nanostores for toast state
├── styles/
│   └── base.css
├── types/
│   └── directus.types.ts
└── zodschema/
    └── index.schema.ts
```

### Key Dependencies

| Package              | Version | Purpose            |
| -------------------- | ------- | ------------------ |
| astro                | ^4.15.2 | Framework          |
| @astrojs/vercel      | ^7.8.0  | Deployment adapter |
| @directus/sdk        | ^16.1.0 | CMS integration    |
| @tanstack/react-form | ^0.29.2 | Form handling      |
| zod                  | ^3.23.8 | Schema validation  |
| @sentry/astro        | ^8.5.0  | Error monitoring   |
| nanostores           | ^0.11.3 | State management   |

### Branch Structure

| Branch            | Purpose                   |
| ----------------- | ------------------------- |
| `prod/main`       | Production (current HEAD) |
| `main`            | Development main          |
| `staging/refresh` | Staging environment       |

---

## Part 2: lief-lite-mono — The Dispensary Template

### Overview

- **Primary Repository:** `[private repository]`
- **Additional Remotes:** Multiple private deployment targets (demo, production, template)
- **Purpose:** Template system for dispensary client websites
- **Total Commits:** 126
- **Current Branch:** `testing/shipyard`
- **Disk Size:** ~666MB (including node_modules)

### Development Philosophy

From the `README.md`:

> **LIEF KISS Philosophy:**
>
> - **L**ightweight
> - **I**ntegrated
> - **E**xtensible
> - **F**lexible
> - **K**eep **I**t **S**imple **S**traightforward

> Official onboarding material: https://grugbrain.dev/

### Chronological Development History

#### Phase 1: Monorepo Genesis (February 2024)

| Date            | Commit     | Description                                                          |
| --------------- | ---------- | -------------------------------------------------------------------- |
| Feb 9, 2024     | `d0b58633` | **Genesis commit** — Monorepo with frontend + PayloadCMS placeholder |
| Feb 9, 2024     | `7b933716` | Typo fix                                                             |
| Feb 16, 2024    | `1900d627` | PayloadCMS scaffold                                                  |
| Feb 17, 2024    | `61f86cd5` | **Careers collection in PayloadCMS** (Developer Colleague 1)         |
| Feb 17, 2024    | `f82b1c75` | PayloadCMS careers integrated into Astro                             |
| Feb 17, 2024    | `652f1ee5` | **React support added to Astro**                                     |
| Feb 17, 2024    | `9e6b25bb` | Rich text content render support (Developer Colleague 1)             |
| Feb 18, 2024    | `8e823f9a` | PostgreSQL support for PayloadCMS                                    |
| Feb 19–20, 2024 | Multiple   | Testing branch, styling, gitignore fixes                             |
| Feb 26, 2024    | `cac54a1c` | Frontend components added batch-1 (Developer Colleague 1)            |
| Feb 26, 2024    | `6077d454` | **Newsletter component + Payload support**                           |

**Initial Architecture:**

- Monorepo structure: `lief-lite-fe/` (Astro frontend) + PayloadCMS backend
- PostgreSQL database for CMS
- Careers page as first CMS-driven content

#### Phase 2: Styling & Component Development (March 2024)

| Date         | Commit     | Description                                         |
| ------------ | ---------- | --------------------------------------------------- |
| Mar 13, 2024 | `0214e05b` | Temp staging setup                                  |
| Mar 13, 2024 | Multiple   | Careers index template carried over                 |
| Mar 21, 2024 | `b3b2c11c` | Styling updates, placeholder images                 |
| Mar 22, 2024 | `099b26ff` | Privacy/terms placeholder pages                     |
| Mar 25, 2024 | `d803623e` | Modal structure setup                               |
| Mar 26, 2024 | `178ef2cc` | UX restyling                                        |
| Mar 27, 2024 | `21c46226` | **Staging/prod branch established**                 |
| Mar 27, 2024 | `8bdbfa35` | API refactor, mobile nav, careers collection update |
| Mar 28, 2024 | `3dbe0911` | **Age verification modal started**                  |
| Mar 28, 2024 | `7e3d67e0` | Modal components prep for Block 5                   |

#### Phase 3: Block 5 Work (April 2024)

| Date        | Commit     | Description                               |
| ----------- | ---------- | ----------------------------------------- |
| Apr 2, 2024 | `790970bd` | **Block 5 tasks** (Developer Colleague 1) |

**Note:** "Block" references appear to be sprint/milestone identifiers in the development workflow.

#### Phase 4: Theme System & Architecture (May 2024)

| Date         | Commit     | Description                            |
| ------------ | ---------- | -------------------------------------- |
| May 3, 2024  | `e40a803a` | **Theme rework started**               |
| May 3, 2024  | `9fe9d141` | Merged updates for theme testing       |
| May 13, 2024 | `b7984091` | New primary branch established         |
| May 13, 2024 | `394b4103` | **SEO components (GA, Umami) added**   |
| May 21, 2024 | `4b21f0d9` | Hero2 component responsiveness         |
| May 21, 2024 | `1dc26f2a` | AboutV2 widget added                   |
| May 23, 2024 | `b2995778` | Newsletter component fix + DOMPurify   |
| May 28, 2024 | `a49080f7` | **Sitemap integration**                |
| May 30, 2024 | `42789249` | Bulk add from AG source for production |

#### Phase 5: Component Atomization & SwiperJS (June 2024)

| Date         | Commit     | Description                                    |
| ------------ | ---------- | ---------------------------------------------- |
| Jun 7, 2024  | `199b456b` | Prettier/ESLint setup, lodash debounce         |
| Jun 7, 2024  | `e6753ca4` | Monorepo prettier issues documented            |
| Jun 8, 2024  | `6b22fc18` | **NavLink component + SwiperJS investigation** |
| Jun 8, 2024  | `47a42554` | **IconSocial component**                       |
| Jun 9, 2024  | `514941d7` | Bulk revisions                                 |
| Jun 17, 2024 | `915fa14b` | **Components atomized for reusability**        |
| Jun 17, 2024 | `42f694be` | **Headless UI components** (mobile menu, FAQ)  |
| Jun 18, 2024 | `6379103c` | **SwiperJS integrated into Hero components**   |

**Technical Note (from commit):**

> NOTE: current swiper style mixing is not ideal, am looking into
> alternatives to better leverage tailwind

#### Phase 6: Directus Migration (July 2024)

| Date         | Commit     | Description                               |
| ------------ | ---------- | ----------------------------------------- |
| Jul 1, 2024  | `55e0f219` | **Directus SDK added** — GET/POST working |
| Jul 1, 2024  | `0809b764` | Directus careers integration continued    |
| Jul 12, 2024 | `60117f11` | Core Directus setup for blog/careers      |
| Jul 12, 2024 | `42e1671a` | Production demo deployment                |
| Jul 12, 2024 | `39c6b81d` | Archive branch created                    |
| Jul 17, 2024 | `f8b0c259` | Minor demo changes                        |

**Migration Note:**

- PayloadCMS archived to `dev-archive-WITHPAYLOADCMS-july-2024` branch
- Directus becomes primary CMS going forward

#### Phase 7: Demo Site & Improvements (August 2024)

| Date         | Commit     | Description                                   |
| ------------ | ---------- | --------------------------------------------- |
| Aug 18, 2024 | `1d0bdbfb` | Bulk improvements for demo branch             |
| Aug 18, 2024 | `741652b4` | Career page style helper for Directus content |
| Aug 18, 2024 | `01ab625e` | Accessibility improvements for demo           |

#### Phase 8: ISR & Design Work (September 2024)

| Date            | Commit     | Description                                 |
| --------------- | ---------- | ------------------------------------------- |
| Sep 3, 2024     | `2e0ac0ad` | Bulk revisions and optimizations            |
| Sep 4, 2024     | `e0df5e08` | **ISR integration + Directus static token** |
| Sep 4, 2024     | `20fb1629` | Astro upgrade for deployment                |
| Sep 7, 2024     | `9267b167` | **Design revisions — Bulk from Block 1**    |
| Sep 10, 2024    | `3ae3bcb0` | Blog page header shift fix                  |
| Sep 14, 2024    | `e0f32977` | **95% migration to new theme structure**    |
| Sep 15, 2024    | `9144caec` | Theme variable assignment bulk changes      |
| Sep 16, 2024    | `6483c239` | Homepage banner swiper, theme revisions     |
| Sep 18, 2024    | `ac55cb08` | **Mobile nav rebuilt to match mockup**      |
| Sep 18, 2024    | `01199323` | Mobile nav finalized                        |
| Sep 19, 2024    | `12f02f06` | FeaturedContent card per mockup             |
| Sep 25–26, 2024 | Multiple   | Bulk adds from AG source                    |

#### Phase 9: Block 4 — Developer Colleague 2 Collaboration (October 2024)

| Date         | Commit     | Author                | Description                          |
| ------------ | ---------- | --------------------- | ------------------------------------ |
| Oct 5, 2024  | `acdf6652` | Developer Colleague 2 | General project notes, README        |
| Oct 5, 2024  | `bced95dc` | Developer Colleague 2 | Component-specific notes             |
| Oct 16, 2024 | `691b12fc` | bforddev7             | Bulk add from AG source              |
| Oct 21, 2024 | `210a44ac` | bforddev7             | Block 4 starting location            |
| Oct 24, 2024 | `af7480d3` | bforddev7             | **Shipyard genesis commit**          |
| Oct 24, 2024 | `214f12bc` | bforddev7             | Shipyard file/folder setup           |
| Oct 24, 2024 | `6b29124b` | bforddev7             | Homepage Shipyard migration          |
| Oct 25, 2024 | `cebf80d2` | bforddev7             | **Core Shipyard migration complete** |
| Oct 25, 2024 | `7bce73bb` | bforddev7             | Final Shipyard migration steps       |
| Oct 26, 2024 | `7f16af31` | Developer Colleague 2 | npm install changes                  |
| Oct 26, 2024 | `0d34aa20` | Developer Colleague 2 | careers page navigation added        |
| Oct 26, 2024 | `fa27218e` | bforddev7             | Hero3 made reusable                  |
| Oct 26, 2024 | `81557957` | Developer Colleague 2 | Components for page build            |
| Oct 26, 2024 | `8f6fa884` | Developer Colleague 2 | Careers page progress                |
| Oct 27, 2024 | `6c442af7` | bforddev7             | Slider added                         |
| Oct 27, 2024 | `a55a45bc` | Developer Colleague 2 | Theme color variables for text       |

#### Phase 10: Developer Colleague 2 Block 4 Completion (November 2024)

| Date         | Commit     | Author                | Description                          |
| ------------ | ---------- | --------------------- | ------------------------------------ |
| Nov 1, 2024  | `1847cec8` | Developer Colleague 2 | Swiper carousel styling              |
| Nov 2, 2024  | `fe20b062` | bforddev7             | Theme color with opacity modifier    |
| Nov 2, 2024  | `93168c53` | Developer Colleague 2 | Swiper section + career components   |
| Nov 2, 2024  | `dbee42ce` | Developer Colleague 2 | New atoms added                      |
| Nov 2, 2024  | `c1101d4f` | Developer Colleague 2 | Accordion progress                   |
| Nov 2, 2024  | `12f0f199` | Developer Colleague 2 | Accordion rounded borders            |
| Nov 2, 2024  | `5740fc59` | Developer Colleague 2 | Double border fix                    |
| Nov 2, 2024  | `775019e7` | Developer Colleague 2 | Responsive layout styling            |
| Nov 2, 2024  | `5cf747e2` | Developer Colleague 2 | **Block 4 final comments**           |
| Nov 4, 2024  | `095120fa` | bforddev7             | **Bento box component**, dev toolbar |
| Nov 19, 2024 | `dd0f4c0e` | bforddev7             | Bulk add, colleague work merged      |
| Nov 26, 2024 | `6023f587` | bforddev7             | **Block 7 prep** (current HEAD)      |

### The Lief Shipyard System

Introduced in October 2024, Shipyard is a component organization architecture:

```
src/components/v1_Lief_Shipyard/
├── 1_Lief_Components/          # Complex page-level components
│   ├── CP_Components/          # Content Page components
│   │   ├── CP_CareerContent/
│   │   ├── CP_FeaturedContent/
│   │   ├── CP_Heros/
│   │   └── CP_SEO/
│   ├── HP_Components/          # Homepage components
│   │   ├── HP_Banners/
│   │   ├── HP_FeaturedContent/
│   │   ├── HP_Heros/
│   │   ├── HP_MENU_PROVIDER/
│   │   └── HP_SEO/
│   └── WS_Components/          # Website-wide components
│       ├── WS_Cookies/
│       ├── WS_Forms/
│       ├── WS_Modals/
│       └── WS_Toast/
├── 2_Lief_Widgets/             # Simple reusable elements
│   ├── Accordions/
│   ├── Buttons/
│   ├── Cards/
│   ├── Carousels/
│   ├── Icons/
│   ├── Links/
│   └── Tabs/
├── 3_Lief_Layouts/             # Layout templates
│   └── Layout_Standard/
│       ├── Analytics_Helpers/
│       ├── Footer.astro
│       ├── Header.astro
│       ├── MobileNavigation.tsx
│       └── Navigation.astro
├── 98_General_Helpers/         # Utility components
└── 99_Lief_DEMO/               # Demo-specific utilities
    ├── DemoMenuBtn.tsx
    └── DevToolbar.tsx
```

**Shipyard Philosophy (from README):**

> - **Widget(s)** are SIMPLE reusable pieces of code used to build components
> - **Component(s)** are complex reusable pieces of code used to build pages
> - **Layout(s)** are complex reusable pieces of code for overall website layout

### Import Alias System

From `tsconfig.json`:

```json
{
	"paths": {
		"@shipyard_v1/*": ["src/components/v1_Lief_Shipyard/*"],
		"@shipyard_v1/components/*": [
			"src/components/v1_Lief_Shipyard/1_Lief_Components/*"
		],
		"@shipyard_v1/components/hp/*": [
			"src/components/v1_Lief_Shipyard/1_Lief_Components/HP_Components/*"
		],
		"@shipyard_v1/widgets/*": [
			"src/components/v1_Lief_Shipyard/2_Lief_Widgets/*"
		],
		"@shipyard_v1/layouts/*": [
			"src/components/v1_Lief_Shipyard/3_Lief_Layouts/*"
		]
	}
}
```

### Current Architecture

```
lief-lite-mono/
├── lief-lite-fe/               # Astro frontend
│   ├── src/
│   │   ├── api.ts
│   │   ├── assets/
│   │   ├── components/
│   │   │   ├── filler/
│   │   │   └── v1_Lief_Shipyard/   # Shipyard system
│   │   ├── interfaces/
│   │   ├── layouts/
│   │   ├── lib/
│   │   │   └── directus.ts
│   │   ├── pages/
│   │   │   ├── api/            # Server routes
│   │   │   ├── blog/
│   │   │   ├── careers/
│   │   │   ├── careers-alt/    # Alternate careers page
│   │   │   └── privacy-and-terms/
│   │   ├── styles/
│   │   └── types/
│   └── dist/                   # Build output
└── README.md
```

### Key Dependencies

| Package           | Version  | Purpose               |
| ----------------- | -------- | --------------------- |
| astro             | ^4.15.2  | Framework             |
| @astrojs/vercel   | ^7.8.0   | Deployment            |
| @directus/sdk     | ^16.1.0  | CMS                   |
| @headlessui/react | ^2.0.4   | Accessible components |
| swiper            | ^11.1.4  | Carousel              |
| @sentry/astro     | ^8.7.0   | Error monitoring      |
| lodash            | ^4.17.21 | Utilities (debounce)  |
| dompurify         | ^3.1.4   | XSS protection        |

### Branch Structure

| Branch                                 | Purpose                              |
| -------------------------------------- | ------------------------------------ |
| `testing/shipyard`                     | Current active development (HEAD)    |
| `prod/main`                            | Production main                      |
| `prod/demo`                            | Demo site production                 |
| `staging/main`                         | Staging                              |
| `dev-colleague/block4`                 | Developer Colleague 2's Block 4 work |
| `dev-archive-WITHPAYLOADCMS-july-2024` | PayloadCMS archive                   |
| `testing/add-directus`                 | Directus migration                   |
| `feat/payload-cms-integration`         | Original PayloadCMS work             |

---

## Part 3: Technical Architecture Comparison

### Shared Technology Stack

| Layer              | Technology        | Version    |
| ------------------ | ----------------- | ---------- |
| Framework          | Astro             | 4.15.2     |
| UI Library         | React             | 18.2.0     |
| Styling            | Tailwind CSS      | 3.4.1      |
| CMS                | Directus          | SDK 16.1.0 |
| Hosting            | Vercel            | Serverless |
| Error Tracking     | Sentry            | ~8.x       |
| Build Optimization | Playform Compress | 0.0.10     |
| Script Loading     | Partytown         | 2.1.2      |

### Architectural Differences

| Aspect                  | lief-c-astro        | lief-lite-mono          |
| ----------------------- | ------------------- | ----------------------- |
| **Purpose**             | Single company site | Multi-tenant template   |
| **Component Structure** | Flat hierarchy      | Shipyard layered system |
| **Form Handling**       | Zod + TanStack Form | Custom React components |
| **State Management**    | Nanostores          | None (per-component)    |
| **Carousel**            | None                | SwiperJS                |
| **Accessible UI**       | Custom              | HeadlessUI              |
| **CMS History**         | Directus only       | PayloadCMS → Directus   |
| **ISR Caching**         | 6 hours             | 6 hours                 |

### Directus Integration Pattern

Both projects share a similar Directus SDK pattern:

```typescript
// lief-c-astro/src/lib/directus.ts
const directus = createDirectus(import.meta.env.DIRECTUS_URL).with(rest());

// lief-lite-mono/lief-lite-fe/src/lib/directus.ts
const directus = createDirectus(import.meta.env.PUBLIC_DIRECTUS_URL)
	.with(staticToken(import.meta.env.DIRECTUS_STATIC_TOKEN))
	.with(rest());
```

**Documented Issue (both repos):**

> README: when migrating to SSR and using the /api/ folder these custom
> requests, nor the createDirectus WILL NOT work correctly, need to
> migrate them over individually to proper routes.

---

## Part 4: Relationship Between Projects

### "AG Source" Synchronization

Throughout the git history, both projects receive periodic "bulk add from AG source" commits, suggesting:

1. A private development environment ("AG") where primary development occurs
2. These two repositories are downstream deployments
3. Code is periodically synchronized from the upstream source

**Example commits:**

- `23917a2` (lief-c-astro): "chore: bulk add from AG source" (Oct 22, 2024)
- `dd0f4c0e` (lief-lite-mono): "chore: bulk add from AG source, merged in colleague work" (Nov 19, 2024)

### Code Sharing Patterns

1. **Directus SDK integration** — Nearly identical patterns
2. **ISR configuration** — Same bypass token structure
3. **Analytics setup** — Google Analytics, PostHog, Sentry
4. **Styling approach** — Tailwind with similar base configurations

### Business Relationship

| lief-c-astro                 | lief-lite-mono                    |
| ---------------------------- | --------------------------------- |
| Markets to dispensary owners | Delivered to dispensary customers |
| B2B focus                    | B2C focus                         |
| Single deployment            | Template for multiple deployments |
| Company branding             | White-label ready                 |

---

## Part 5: Contributor Analysis

### lief-c-astro

| Contributor | Commits    | Role              |
| ----------- | ---------- | ----------------- |
| bforddev7   | 106 (100%) | Primary developer |

### lief-lite-mono

| Contributor           | Commits   | Period            | Focus                              |
| --------------------- | --------- | ----------------- | ---------------------------------- |
| bforddev7             | 107 (85%) | Feb 2024–Nov 2024 | Lead development                   |
| Developer Colleague 2 | 15 (12%)  | Oct–Nov 2024      | Feature development, UI components |
| Developer Colleague 1 | 4 (3%)    | Feb–Apr 2024      | CMS integration, project advising  |

---

## Part 6: Timeline Visualization

```
2024
│
├─ Feb ──┬── lief-c-astro genesis (Feb 2)
│        └── lief-lite-mono genesis (Feb 9)
│             └── PayloadCMS integration (Developer Colleague 1)
│
├─ Mar ──┬── Both: Pre-production development
│        └── Modal structures, mobile nav
│
├─ Apr ───── lief-lite-mono: Block 5 work
│
├─ May ──┬── lief-c-astro: PRODUCTION LAUNCH (May 28)
│        │   └── Analytics, SEO, Sentry
│        └── lief-lite-mono: Theme system started
│
├─ Jun ──┬── lief-c-astro: Accessibility focus
│        └── lief-lite-mono: SwiperJS, atomization
│
├─ Jul ───── lief-lite-mono: Directus migration
│             └── PayloadCMS archived
│
├─ Aug ──┬── lief-c-astro: Directus integration
│        └── lief-lite-mono: Demo improvements
│
├─ Sep ──┬── Both: ISR implementation
│        └── Both: Design reviews
│
├─ Oct ──┬── lief-lite-mono: SHIPYARD ARCHITECTURE (Oct 24)
│        └── Block 4 begins (Developer Colleague 2)
│
└─ Nov ──┬── lief-c-astro: V2 components, blog
         ├── lief-lite-mono: Block 4 complete
         └── Block 7 prep (Nov 26)
```

---

## Part 7: Outstanding Items & TODOs

### lief-c-astro

From code comments:

1. Build "join the waitlist" form for homepage
2. Change FAQ to SEO-optimized About section
3. Update UploadThing pathname restriction

### lief-lite-mono

From code comments:

1. Review API routes for ISR exclusion
2. Newsletter submission query refactor
3. General contact form Directus integration
4. Import alias cascading issue investigation

### Shared Technical Debt

1. **Directus SSR quirk** — SDK doesn't work in `/api/` routes when imported from `/lib/`
2. **Firefox accessibility** — Accordion tabbing issue
3. **Prettier/ESLint in monorepo** — Configuration challenges documented

---

## Appendix A: Remote Repository Map

### lief-c-astro

```
repo → [private repository]
```

### lief-lite-mono

```
repo       → [private repository]
demorepo   → [private repository]
prodrepo   → [private repository]
production → [private repository]
```

---

## Appendix B: Commit Conventions

From `lief-c-astro/README.md`:

| Prefix     | Purpose                     |
| ---------- | --------------------------- |
| `feat`     | New feature for the user    |
| `fix`      | Bug fix for the user        |
| `docs`     | Documentation changes       |
| `style`    | Formatting (no code change) |
| `refactor` | Refactoring production code |
| `test`     | Adding/refactoring tests    |
| `chore`    | Build/tooling changes       |

**Additional observed prefixes:**

- `improv` — Improvements
- `update` — Updates
- `misc` — Miscellaneous
- `BULK` — Large synchronized changes
- `SKILLISSUE` — Self-identified learning moments
- `BUG` — Bug documentation

---

## Related Documentation

This report is part of a three-document set analyzing the Lief Platform:

| Document                             | Purpose                                                        |
| ------------------------------------ | -------------------------------------------------------------- |
| **reporeview-v1.md** (this document) | Git history and repository evolution                           |
| **codereview-v1.md**                 | Technical code analysis, architecture, patterns, and quality   |
| **regreview-v1.md**                  | Human-readable portfolio summary for GitHub, LinkedIn, resumes |

---

_End of Report_
