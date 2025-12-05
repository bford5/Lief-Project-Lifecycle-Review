# Lief Codebase Review Report

**Generated:** December 3, 2025  
**Scope:** `lief-c-astro` and `lief-lite-mono` codebases  
**Analysis Type:** Code quality, architecture, patterns, and implementation details  
**Version:** v1 — Anonymized for public sharing

---

## Executive Summary

This report provides a comprehensive technical analysis of two interconnected Astro-based codebases in the Lief ecosystem. While `reporeview.md` documented the git history and project evolution, this document examines the actual code implementation, patterns, quality, and architectural decisions.

> **Note:** This report references internal/private repositories. Technical details (commit counts, branch names, file paths) come from private repositories that are not publicly accessible. Requests to view these private repositories are permitted under a signed mutual Non Disclosure Agreement (NDA).

### Quick Stats

| Metric                    | lief-c-astro     | lief-lite-mono          |
| ------------------------- | ---------------- | ----------------------- |
| **Total Source Files**    | ~68              | ~112                    |
| **Primary Language**      | TypeScript/Astro | TypeScript/Astro        |
| **React Components**      | 4 (.tsx)         | 8 (.tsx/.jsx)           |
| **Astro Components**      | ~49              | ~57                     |
| **CSS Strategy**          | Hardcoded colors | CSS Variables (theming) |
| **Form Library**          | Zod + Custom     | Custom React            |
| **State Management**      | Nanostores       | None (per-component)    |
| **Accessible UI Library** | None             | HeadlessUI              |

### Key Findings

1. **Architectural Divergence**: While sharing roots, the codebases have evolved distinct patterns — `lief-c-astro` uses a flat component structure while `lief-lite-mono` implements the sophisticated "Shipyard" layered architecture.

2. **Theming Maturity**: `lief-lite-mono` has a mature CSS variable-based theming system supporting multiple color schemes; `lief-c-astro` uses hardcoded Tailwind colors.

3. **Form Handling**: Both projects have functional but incomplete form implementations with documented TODOs for refactoring.

4. **Technical Debt**: Both codebases contain deprecated files, incomplete API routes, and documented bugs awaiting resolution.

---

## Understanding Astro: Framework Context

Before diving into the architecture analysis, it's helpful to understand what Astro is and why it was chosen for these projects — especially for readers familiar with other modern frameworks.

### What Is Astro?

**Astro** is a modern web framework designed for building fast, content-focused websites. It was first released in 2021 and has rapidly grown in popularity for marketing sites, blogs, documentation, and e-commerce storefronts.

### Key Characteristics

| Feature           | How Astro Handles It                                                 |
| ----------------- | -------------------------------------------------------------------- |
| **Rendering**     | Static-first with optional server-side rendering (SSR)               |
| **JavaScript**    | Ships zero JS by default; adds only what's explicitly needed         |
| **Components**    | Native `.astro` components plus support for React, Vue, Svelte, etc. |
| **Routing**       | File-based routing (similar to Next.js)                              |
| **Data Fetching** | Happens at build time by default; can be runtime with SSR            |

### Astro vs Other Frameworks

| Aspect             | Astro                    | Next.js                 | Gatsby                   |
| ------------------ | ------------------------ | ----------------------- | ------------------------ |
| **Default Output** | Static HTML              | Server-rendered React   | Static HTML              |
| **JS Bundle Size** | Near-zero (by default)   | Full React runtime      | Full React runtime       |
| **Hydration**      | Partial ("Islands")      | Full page               | Full page                |
| **Best For**       | Content sites, marketing | Apps, dashboards        | Blogs, docs              |
| **Learning Curve** | Low (HTML-like syntax)   | Medium (React required) | Medium (React + GraphQL) |

### Why Astro for Lief?

The Lief projects are marketing and template sites — content-heavy, SEO-critical, and performance-sensitive. Astro's strengths directly address these needs:

1. **Performance**: Near-zero JavaScript means faster page loads and better Core Web Vitals scores, which directly impacts SEO rankings.

2. **Partial Hydration ("Islands")**: Interactive components (forms, menus) can use React while static content (hero sections, footers) ship as pure HTML. This is the "best of both worlds" — React's developer experience where needed, raw speed everywhere else.

3. **Incremental Static Regeneration (ISR)**: With Vercel's adapter, Astro can regenerate pages on-demand without full rebuilds — essential for CMS-driven content that changes periodically.

4. **Headless CMS Integration**: Astro's data-fetching patterns work naturally with Directus and other headless CMS systems, making content management straightforward.

### The `.astro` Component Model

Astro components look like enhanced HTML with a frontmatter section for logic:

```astro
---
// This runs at build time (or request time with SSR)
const title = "Welcome";
const items = await fetchFromCMS();
---

<section>
  <h1>{title}</h1>
  <ul>
    {items.map(item => <li>{item.name}</li>)}
  </ul>
</section>

<style>
  /* Scoped CSS — only affects this component */
  h1 { color: green; }
</style>
```

This model explains why the Lief codebases have many `.astro` files for static/structural components and fewer `.tsx` React files for interactive elements like forms and mobile navigation.

---

## Part 1: Architecture Analysis

### 1.1 Directory Structure Comparison

#### lief-c-astro (Flat Structure)

```
src/
├── components/
│   ├── Analytics/           # 3 files (GA, PostHog, Umami)
│   ├── Atoms/               # 1 file (NavLink)
│   ├── BlogComponents/      # 2 files
│   ├── Forms/               # 4 files (React components)
│   ├── Layout/              # 4 files (Header, Footer, Nav)
│   ├── Notifications/       # 1 file (Toast)
│   ├── PageComponents/      # 12 files (Hero, Perks, FAQ, etc.)
│   └── UI/                  # 14 files (Accordion, Icons, Logos)
├── layouts/                 # 1 file
├── lib/                     # 1 file (directus.ts)
├── pages/                   # 11 files + api/
├── store/                   # 1 file (toastStore.ts)
├── styles/                  # 1 file (base.css)
├── types/                   # 2 files
└── zodschema/               # 1 file
```

**Assessment**: Simple, traditional structure suitable for a single-purpose marketing site. Categories are logical but lack formal hierarchy rules.

#### lief-lite-mono (Shipyard Architecture)

```
src/components/v1_Lief_Shipyard/
├── 1_Lief_Components/           # Page-level complex components
│   ├── CP_Components/           # Content Page components
│   │   ├── CP_CareerContent/
│   │   ├── CP_FeaturedContent/
│   │   ├── CP_Heros/
│   │   └── CP_SEO/
│   ├── HP_Components/           # Homepage components
│   │   ├── HP_Banners/
│   │   ├── HP_FeaturedContent/
│   │   ├── HP_Heros/
│   │   ├── HP_MENU_PROVIDER/
│   │   └── HP_SEO/
│   └── WS_Components/           # Website-wide components
│       ├── WS_Cookies/
│       ├── WS_Forms/
│       ├── WS_Modals/
│       └── WS_Toast/
├── 2_Lief_Widgets/              # Simple reusable elements
│   ├── Accordions/
│   ├── Buttons/
│   ├── Cards/
│   ├── Carousels/
│   ├── Icons/
│   ├── Links/
│   └── Tabs/
├── 3_Lief_Layouts/              # Layout templates
│   └── Layout_Standard/
├── 98_General_Helpers/          # Utility components
└── 99_Lief_DEMO/                # Demo-specific utilities
```

**Assessment**: Sophisticated, scalable architecture designed for multi-tenant template system. Clear separation between Widgets (atomic), Components (molecular), and Layouts (organisms). Numbered directories enforce import order awareness.

### 1.2 Import Alias Systems

#### lief-c-astro (Simple Aliases)

```json
// tsconfig.json
{
	"paths": {
		"@/*": ["src/*"],
		"@components/*": ["src/components/*"],
		"@ui/*": ["src/components/UI/*"],
		"@layoutcmp/*": ["src/components/Layout/*"],
		"@layouts/*": ["src/layouts/*"]
	}
}
```

**Usage Example:**

```typescript
import Header from '@layoutcmp/Header.astro';
import NavLink from '@/components/Atoms/NavLink.astro';
import { contactUsSchema } from '@/zodschema/index.schema';
```

#### lief-lite-mono (Shipyard Aliases)

```json
// tsconfig.json (truncated)
{
	"paths": {
		"@shipyard_v1/*": ["src/components/v1_Lief_Shipyard/*"],
		"@shipyard_v1/components/*": [
			"src/components/v1_Lief_Shipyard/1_Lief_Components/*"
		],
		"@shipyard_v1/components/hp/*": [
			"src/components/v1_Lief_Shipyard/1_Lief_Components/HP_Components/*"
		],
		"@shipyard_v1/components/cp/*": [
			"src/components/v1_Lief_Shipyard/1_Lief_Components/CP_Components/*"
		],
		"@shipyard_v1/components/ws/*": [
			"src/components/v1_Lief_Shipyard/1_Lief_Components/WS_Components/*"
		],
		"@shipyard_v1/widgets/*": [
			"src/components/v1_Lief_Shipyard/2_Lief_Widgets/*"
		],
		"@shipyard_v1/layouts/*": [
			"src/components/v1_Lief_Shipyard/3_Lief_Layouts/*"
		],
		"@shipyard_v1/demo/*": ["src/components/v1_Lief_Shipyard/99_Lief_DEMO/*"]
	}
}
```

**Usage Example:**

```typescript
import Header from '@shipyard_v1/layouts/Layout_Standard/Header.astro';
import HPHero3 from '@shipyard_v1/components/hp/HP_Heros/HP_Hero_3.astro';
import Button from '@shipyard_v1/widgets/buttons/Button.astro';
import MobileNavLink from '@shipyard_v1/widgets/links/MobileNavLink';
```

**Documented Limitation** (from tsconfig.json comments):

> "looks like astro can only take up to 3 alias paths before the /_ wildcard ; if 4 or more alias paths are defined before the /_ wildcard the import alias will not work."

---

## Part 2: Code Quality Assessment

### 2.1 TypeScript Usage and Type Safety

#### Type Definitions

**lief-c-astro** - Explicit type definitions:

```typescript
// src/types/directus.types.ts
export type ContactUsSubmissionType = {
	dispensaryName: string;
	dispensaryEmail: string;
	dispensaryPhone: string;
	dispensaryContactName?: string;
	dispensaryMessage?: string;
};

export type GetStartedSubmissionType = {
	dispensaryName: string;
	dispensaryEmail: string;
	dispensaryPhone: string;
	dispensaryContactName?: string;
	dispensaryInterest: 'core' | 'essentials' | 'custom' | '';
	dispensaryMessage?: string;
	dispensaryState?: string;
	dispensaryCurrentSite?: string;
	dispensaryMenuProvider: 'dutchie' | 'iheartjane' | 'other' | 'none';
};
```

**Quality**: Good use of union types for constrained values. Optional properties correctly marked.

**lief-lite-mono** - Interface-based typing:

```typescript
// src/interfaces/career.ts
// (Inferred from usage patterns)

// Component Props (inline)
interface Props {
	title: string;
	description: string;
}
```

**Quality**: More ad-hoc typing, relies on inline interface definitions in component files.

#### Type Safety Issues Identified

1. **`any` Type Usage**:

```typescript
// lief-c-astro/src/components/Forms/ContactUs.tsx
const QuickFormFieldWrapper = ({ children }: any) => {
  return <div className="...">{children}</div>;
};
```

**Recommendation**: Replace with `React.PropsWithChildren<{}>` or explicit `{ children: React.ReactNode }`.

2. **Type Assertions**:

```typescript
// lief-c-astro/src/pages/api/contact-us.ts
const dataToSend: ContactUsSubmissionType = {
	dispensaryName: data.get('dispensaryName') as string,
	// ...
};
```

**Issue**: `as string` assertions bypass null checking. `FormData.get()` returns `FormDataEntryValue | null`.

### 2.2 Form Handling Patterns

#### lief-c-astro: Zod + Custom Validation

```typescript
// src/zodschema/index.schema.ts
export const contactUsSchema = z.object({
	dispensaryName: z.string().min(1),
	dispensaryEmail: z
		.string()
		.email()
		.regex(
			/^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/,
			'Invalid email format'
		),
	dispensaryPhone: z
		.string()
		.transform((val) => val.replace(/\D/g, ''))
		.refine((val) => val.length >= 10 && val.length <= 15, {
			message:
				'Phone number must have between 10 and 15 digits after removing non-numeric characters.',
		}),
	dispensaryContactName: z.string().optional(),
	dispensaryMessage: z.string().max(1500).optional(),
});
```

**Implementation in Form Component:**

```typescript
// src/components/Forms/ContactUs.tsx
const submit = async (e: FormEvent<HTMLFormElement>) => {
	// Manual validation function
	const validateForm = (): boolean => {
		const newErrors: FormErrors = {};
		if (!contactFormData.dispensaryName.trim()) {
			newErrors.dispensaryName = 'Business name is required';
		}
		// ... more manual validation
	};

	if (validateForm()) {
		// Zod validation as secondary layer
		dataToSend = contactUsSchema.parse(dataToCLEAN);
	}
};
```

**Issue Documented in Code:**

```typescript
// TODO: review and refactor the validateForm() + zod setup, probably dont need it,
// need to setup for built in zod error handling instead.
```

**Assessment**: Redundant dual-validation system. Manual `validateForm()` duplicates Zod schema rules. Should consolidate to Zod-only with proper error handling.

#### lief-lite-mono: Custom React Validation

```javascript
// src/components/v1_Lief_Shipyard/.../WS_Form_Newsletter_1.jsx
const handleSubmit = async (e) => {
	e.preventDefault();
	const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
	const emailAddress = emailRef.current.value?.trim();
	const isValidEmail = !!emailAddress && emailRegex.test(emailAddress);

	if (!isValidEmail) {
		setErrorStatus(true);
		window.alert('Please enter a valid email address');
		return;
	}
	const sanitizedEmail = DOMPurify.sanitize(emailAddress);
	// ... submit logic
};
```

**Assessment**: Simple inline validation. Uses `window.alert()` for errors (not ideal UX). No Zod integration. DOMPurify sanitization is good security practice.

### 2.3 API Route Implementation

#### lief-c-astro: Complete API Pattern

```typescript
// src/pages/api/contact.ts
import { createItem, createDirectus, rest, staticToken } from '@directus/sdk';
import type { APIRoute } from 'astro';

export const POST: APIRoute = async ({ request }) => {
	const directus = createDirectus(import.meta.env.PUBLIC_DIRECTUS_URL)
		.with(staticToken(import.meta.env.DIRECTUS_STATIC_TOKEN))
		.with(rest());

	const data = await request.json();
	if (!data) {
		return new Response(
			JSON.stringify({ message: 'Missing required fields' }),
			{ status: 400 }
		);
	}

	const cleanData: ContactUsSubmissionType = contactUsSchema.parse(data);

	try {
		await directus.request(
			createItem('lief_business_contact_form_submissions', cleanData)
		);
	} catch (error) {
		console.error('Form submission error:', error);
		return new Response(JSON.stringify({ message: 'Error submitting form' }), {
			status: 500,
		});
	}

	return new Response(JSON.stringify({ message: 'Success!' }), { status: 200 });
};

// HTTP Method Barriers
export const GET: APIRoute = async () => {
	return new Response(JSON.stringify({ message: 'Invalid method' }), {
		status: 405,
	});
};
// ... PUT, DELETE, PATCH barriers
```

**Quality Indicators:**

- ✅ Proper error handling with try/catch
- ✅ HTTP method barriers (405 responses)
- ✅ Zod schema validation
- ✅ Static token authentication
- ⚠️ Console logging in production code

#### lief-lite-mono: Stub API Routes

```typescript
// src/pages/api/contact-newsletter.ts
// TODO: setup directus SDK here to POST newsletter form data ;; barrier all other METHODS

// src/pages/api/Data-careers.ts
// TODO: setup directus SDK here to GET careers data ;; barrier all other METHODS
```

**Assessment**: API routes are placeholder stubs. Form submissions happen directly in React components (see Newsletter form) which bypasses SSR API layer.

### 2.4 State Management

#### lief-c-astro: Nanostores

```typescript
// src/store/toastStore.ts
import { atom } from 'nanostores';

type ToastState = {
	isVisible: boolean;
	message: string;
	type: 'success' | 'error' | 'info' | 'warning';
};

export const toastState = atom<ToastState>({
	isVisible: false,
	message: '',
	type: 'info',
});

export function showToast(message: string, type: ToastState['type'] = 'info') {
	toastState.set({ isVisible: true, message, type });
}

export function hideToast() {
	toastState.set({ ...toastState.get(), isVisible: false });
}
```

**Usage:**

```typescript
// In ContactUs.tsx
import { showToast } from '../../store/toastStore';

if (res.ok) {
	showToast('Form submitted successfully!', 'success');
}
```

**Assessment**: Clean, minimal state management for cross-component communication. Well-typed with proper action functions.

#### lief-lite-mono: Component-Local State

```javascript
// Newsletter form uses local React state
const [isLoading, setIsLoading] = React.useState(false);
const [submitStatus, setSubmitStatus] = React.useState(false);
const [errorStatus, setErrorStatus] = React.useState(false);
```

**Assessment**: No global state management. Each component manages its own state. This is simpler but limits cross-component communication patterns.

---

## Part 3: UI/UX Implementation

### 3.1 Tailwind CSS Theming Strategies

#### lief-c-astro: Hardcoded Colors

```javascript
// tailwind.config.mjs
export default {
	theme: {
		extend: {
			colors: {
				'lief-darkgreen-main': '#111311',
				'lief-hdrBorderColor': '#102626',
				'lief-lightgreen-cta': '#8DD8B7',
				'lief-green-cta': '#0FDB83',
				'lief-softgreen': '#1F403A',
				'lief-text-primary': '#8DD8B7',
				'lief-text-secondary': '#102626',
				'lief-text-white': '#f1f1f1',
				'lief-careerMain': '#04191A',
			},
		},
	},
};
```

**Usage:**

```html
<body class="bg-lief-darkgreen-main text-lief-text-white"></body>
```

**Assessment**: Simple but inflexible. No theme switching capability. Colors are tied to brand identity.

#### lief-lite-mono: CSS Variables (Advanced Theming)

```javascript
// tailwind.config.mjs
colors: {
  't-main': 'var(--t-main)',
  't-primary': 'var(--t-primary)',
  't-secondary': 'var(--t-secondary)',
  't-text-main': 'var(--t-text-main)',
  't-btn-primary-bg': 'var(--t-btn-primary-bg)',
  't-btn-primary-text': 'var(--t-btn-primary-text)',
  // ... 30+ theme variables
}
```

**CSS Variable Definitions:**

```css
/* src/styles/base.css */
:root {
	/* DARK THEME - Green */
	--t-main: #04191a;
	--t-primary: #0fdb83;
	--t-secondary: #1f403a;
	--t-text-main: #f1f1f1;
	--t-btn-primary-bg: #0fdb83;
	--t-btn-primary-text: #102626;
	/* ... */
}

/* LIGHT THEME - Green (commented out, ready to swap)
  --t-main: #eaeceb;
  --t-primary: #0fdb83;
  ...
*/

/* Multiple theme variations available:
   - Dark Green (active)
   - Light Green
   - Modern Neutral
   - Deep Ocean
   - Mint Runtz
   - Orange Treat
*/
```

**Special Pattern for Opacity Modifiers:**

```css
/* Colleague notation for Tailwind opacity support */
--t-secondary-JD: 31 64 58; /* RGB values without rgb() wrapper */
```

```javascript
// tailwind.config.mjs
't-secondary-jd': 'rgb(var(--t-secondary-JD))',
// Enables: bg-t-secondary-jd/50 for 50% opacity
```

**Assessment**: Production-ready theming system with 6 pre-built themes. Allows runtime theme switching. Well-documented with contributor attribution.

### 3.2 Accessibility Patterns

#### Consistent Focus State Pattern

Both codebases use a shared accessibility style constant:

```typescript
// Used throughout both codebases
const ACCESSIBILITY_STYLE = `focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-500`;
```

**Sometimes with transition overrides:**

```typescript
const defaultStyle = `focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-lief-lightgreen-cta focus-visible:transition-none focus-visible:duration-100`;
```

#### ARIA Label Implementation

**Good Examples:**

```html
<!-- lief-lite-mono Navigation -->
<a href="/#menu" aria-label="navigation link to the product menu" class="..."
	>Our Products</a
>

<!-- lief-c-astro Blog Card -->
<button class="share-btn" aria-label="Share" data-url="{url}"></button>
```

**Missing ARIA Examples:**

```html
<!-- lief-c-astro Accordion -->
<button
	id="accordion-header-1"
	aria-expanded="false"
	aria-controls="accordion-panel-1"
	onclick="toggleAccordionItem()"
>
	<!-- Missing aria-label --></button
>
```

#### Screen Reader Content

```html
<!-- Skip link pattern in lief-lite-mono -->
<a href="/#menu" class="sr-only" aria-label="skip link to the product menu">
	Skip to Menu
</a>

<!-- Hidden navigation for screen readers -->
<ul class="sr-only" tabindex="-1">
	<li><a href="/product-details" tabindex="-1">Product Details</a></li>
</ul>
```

#### Known Accessibility Bug

```typescript
// lief-c-astro/src/components/PageComponents/FAQ.astro
// TODO: ON FIREFOX when tabbing through the site the accordion items are
// being prioritized and overriding other elements. Chrome/etc is fine and
// functions correctly, look into this and try to fix
```

### 3.3 Responsive Design Approaches

#### Breakpoint Usage

Both codebases use standard Tailwind breakpoints (`sm`, `md`, `lg`, `xl`, `2xl`):

```html
<!-- lief-c-astro Hero -->
<h2
	class="text-[1.55rem] sm:text-[1.875rem] md:text-[2.25rem] lg:text-[2.865rem]"
>
	<!-- lief-lite-mono Layout -->
	<header
		class="flex justify-between lg:justify-center max-lg:py-2 lg:py-6"
	></header
></h2>
```

#### Mobile-First vs Desktop-First

**lief-c-astro** tends toward desktop-first with `max-*` overrides:

```html
<NavLink href="/product-details" class="hidden md:inline-block" />
<NavLink href="/product-details" class="inline-block md:hidden" />
```

**lief-lite-mono** uses a mix with HeadlessUI for complex mobile patterns:

```typescript
// MobileNavigation.tsx
<MenuItems className="... max-h-[599px] min-h-[469px] w-full ...">
  <Transition
    enter="duration-[250ms] ease-out"
    enterFrom="scale-100 opacity-0 -translate-y-10"
    enterTo="scale-100 opacity-100 translate-y-0"
  >
```

#### Performance Verification Note

Performance testing and optimization were conducted throughout development and tracked internally. While detailed Lighthouse scores and TTFB measurements are not included in this report, the codebases were regularly evaluated against Core Web Vitals benchmarks during development. Key performance optimizations implemented include:

- ISR caching with 6-hour expiration
- Playform Compress for image optimization
- Partytown for offloading analytics scripts to web workers
- Astro's partial hydration to minimize JavaScript bundle size

---

## Part 4: CMS Integration Patterns

### 4.1 Directus SDK Usage

#### lief-c-astro Pattern

```typescript
// src/lib/directus.ts
import { createDirectus, rest, readItems, createItem } from '@directus/sdk';

const directus = createDirectus(import.meta.env.DIRECTUS_URL).with(rest());

export default directus;

// Documented SSR Issue:
// README: for whatever reason Astro and Directus don't cooperate well when
// you try and use an exported func from /lib/directus.ts in the /api/...
// directory files it throws a "dont have permission to access" error.
```

**Workaround (API routes recreate client):**

```typescript
// src/pages/api/contact.ts
const directus = createDirectus(import.meta.env.PUBLIC_DIRECTUS_URL)
	.with(staticToken(import.meta.env.DIRECTUS_STATIC_TOKEN))
	.with(rest());
```

#### lief-lite-mono Pattern

```typescript
// src/lib/directus.ts
import {
	createDirectus,
	rest,
	readItems,
	createItem,
	staticToken,
} from '@directus/sdk';

const directus = createDirectus(import.meta.env.PUBLIC_DIRECTUS_URL)
	.with(staticToken(import.meta.env.DIRECTUS_STATIC_TOKEN))
	.with(rest());

export default directus;

// Pre-built queries
export const getBlogPosts = await directus.request(
	readItems('blog_data', {
		fields: [
			'status',
			'blog_slug',
			'blog_title',
			'blog_content',
			'blog_to_dispensary_id',
		],
		filter: {
			blog_to_dispensary_id: { _eq: import.meta.env.DIRECTUS_DISPO_ID },
			status: { _eq: 'published' },
		},
	})
);

export const getCareers = await directus.request(
	readItems('career_data', {
		fields: ['*'],
		filter: {
			career_to_dispensary_id: { _eq: import.meta.env.DIRECTUS_DISPO_ID },
			status: { _eq: 'published' },
		},
	})
);
```

**Direct Component Usage (bypassing API):**

```javascript
// WS_Form_Newsletter_1.jsx
const directus2 = createDirectus(import.meta.env.PUBLIC_DIRECTUS_URL).with(
	rest()
);

await directus2.request(
	createItem('newsletter_submission', {
		newsletter_email: sanitizedEmail,
		newsletter_submission_to_dispensary_id: import.meta.env
			.PUBLIC_DIRECTUS_DISPO_ID,
	})
);
```

### 4.2 ISR Configuration

Both projects use identical ISR patterns:

```javascript
// astro.config.mjs (both repos)
adapter: vercel({
  isr: {
    expiration: 60 * 60 * 6, // 6 hours cache
    bypassToken: '[redacted — 32+ character token]',
    exclude: ['/api/contact', '/api/contact-get-started'],
  },
}),
```

**Key Requirement Documented:**

```javascript
// IMPORTANT: this has to be at least 32 characters long otherwise it throws a build error
```

### 4.3 Environment Variable Patterns

| Variable                   | lief-c-astro | lief-lite-mono | Notes                 |
| -------------------------- | ------------ | -------------- | --------------------- |
| `DIRECTUS_URL`             | ✅           | ❌             | Server-side only      |
| `PUBLIC_DIRECTUS_URL`      | ✅           | ✅             | Client-accessible     |
| `DIRECTUS_STATIC_TOKEN`    | ✅           | ✅             | Authentication        |
| `DIRECTUS_DISPO_ID`        | ✅           | ✅             | Tenant identification |
| `PUBLIC_DIRECTUS_DISPO_ID` | ❌           | ✅             | Client-side tenant ID |
| `SENTRY_AUTH_TOKEN`        | ✅           | ✅             | Error monitoring      |

**Security Note**: `PUBLIC_*` prefixed variables are exposed to client-side code. Both repos expose the Directus URL publicly, which is acceptable for read operations but requires proper Directus permissions configuration.

---

## Part 5: Component Analysis

### 5.1 Reusable Component Patterns

#### Button Component (lief-lite-mono)

**Well-designed variant system:**

```typescript
// src/components/v1_Lief_Shipyard/2_Lief_Widgets/Buttons/Button.astro
interface Props {
  href: string;
  linkText: string;
  class?: string;
  linkType: 'primary' | 'secondary' | 'outline' | 'career' | 'share';
  ariaLabel: string;
  target?: string;
}

// Conditional rendering based on linkType
{linkType === 'primary' && (
  <a
    href={href}
    aria-label={ariaLabel}
    class={`${styleOverrides} ${ACCESSIBILITY_STYLE} text-t-btn-primary-text rounded-md border-2 border-t-primary bg-t-primary transition-[color,background-color] duration-[300ms] hover:bg-t-main hover:text-t-tertiary`}>
    {linkText}
  </a>
)}

{linkType === 'outline' && (
  <a
    href={href}
    class={`${styleOverrides} ${ACCESSIBILITY_STYLE} hover:text-t-btn-primary-text ... hover:bg-t-primary`}>
    {linkText}
  </a>
)}
```

**Assessment**: Clean variant pattern using conditional rendering. Theme-aware colors. Consistent accessibility treatment.

#### NavLink Component Comparison

**lief-c-astro (Simple):**

```typescript
// src/components/Atoms/NavLink.astro
interface Props {
	href: string;
	linkText: string;
	ariaLabel: string;
	linkType: 'header' | 'footer';
	class?: string;
}
```

**lief-lite-mono (Extended):**

```typescript
// src/components/v1_Lief_Shipyard/2_Lief_Widgets/Links/NavLink.astro
interface Props {
	href: string;
	linkText: string;
	class?: string;
	linkType: 'desktop' | 'mobile' | 'footer' | '';
	ariaLabel: string;
	target?: string;
}
```

**Assessment**: lief-lite-mono has more granular variant control with explicit mobile/desktop differentiation.

### 5.2 Accordion Implementations

#### lief-c-astro: Custom CSS/JS Implementation

```typescript
// src/components/UI/AccordionItem.astro
<li class="accordion__item">
  <h3 class="text-lief-text-white transition-colors duration-300 hover:text-lief-green-cta">
    <button
      id="accordion-header-1"
      class="accordion__header"
      aria-expanded="false"
      aria-controls="accordion-panel-1"
      onclick="toggleAccordionItem()">
      <span>{header}</span>
      <svg class="header__toggle-indicator">...</svg>
    </button>
  </h3>
  <div id="accordion-panel-1" class="accordion__panel">
    <div class="panel__inner">
      <slot />
    </div>
  </div>
</li>

<style is:global>
  .accordion__panel {
    visibility: hidden;
    height: 0;
    transition: height 0.3s ease-in-out, visibility 0s 0.3s;
  }
  .is-active .accordion__panel {
    visibility: visible;
    height: auto;
  }
</style>
```

**Controller Script (in FAQ.astro):**

```javascript
document.addEventListener('astro:page-load', () => {
  const faqs = document.querySelectorAll('[data-faq-index]');
  faqs.forEach((faq) => {
    faq.querySelector('button')!.addEventListener('click', () => {
      // Toggle logic with maxHeight calculations
      (answer as HTMLElement).style.maxHeight = (answer as HTMLElement).scrollHeight + 'px';
    });
  });
});
```

**Assessment**: Manual DOM manipulation with global styles. Functional but requires careful coordination between component and controller.

#### lief-lite-mono: HeadlessUI Implementation

```typescript
// src/components/v1_Lief_Shipyard/2_Lief_Widgets/Accordions/Accordion_1.tsx
import { Disclosure, DisclosureButton, DisclosurePanel } from '@headlessui/react';
import { ChevronDownIcon } from '@heroicons/react/20/solid';

export default function Accordion({ data }: FAQData) {
  return (
    <div className="...">
      {data?.map(({ title, content, setDefaultOpen }: FAQItem) => (
        <Disclosure as="div" key={`${title}-accordion`} defaultOpen={setDefaultOpen}>
          {({ open }) => (
            <>
              <DisclosureButton className={`... ${open ? 'border-b-[1px]' : ''}`}>
                <span>{title}</span>
                <ChevronDownIcon className="group-data-[open]:rotate-180" />
              </DisclosureButton>
              <DisclosurePanel className="...">
                {content}
              </DisclosurePanel>
            </>
          )}
        </Disclosure>
      ))}
    </div>
  );
}
```

**Assessment**: Leverages battle-tested HeadlessUI for accessibility and state management. Data-driven with `defaultOpen` support. Cleaner separation of concerns.

### 5.3 Form Component Analysis

#### ContactUs Form (lief-c-astro)

**Strengths:**

```typescript
// Good state organization
const [contactFormData, setContactFormData] = useState<FormData>({...});
const [formErrors, setFormErrors] = useState<FormErrors>({});
const [isSubmitting, setIsSubmitting] = useState(false);
const [submitError, setSubmitError] = useState<string | null>(null);
const [submitSuccess, setSubmitSuccess] = useState(false);

// Proper controlled inputs
<input
  type="text"
  id="dispensaryName"
  name="dispensaryName"
  required
  value={contactFormData.dispensaryName}
  onChange={handleChange}
/>
```

#### Newsletter Form (lief-lite-mono)

**Strengths:**

```javascript
// XSS protection
const sanitizedEmail = DOMPurify.sanitize(emailAddress);
```

### 5.4 Navigation Patterns

#### Desktop Navigation (lief-c-astro)

```typescript
// src/components/Layout/Navigation.astro
<nav id="navMenu" class="flex flex-row gap-4 lg:gap-8">
  <NavLink href="/product-details" class="hidden md:inline-block" linkText="Product Details" />
  <NavLink href="/product-details" class="inline-block md:hidden" linkText="Products" />
  <!-- Duplicate links with different text for mobile/desktop -->
</nav>
```

**Assessment**: Uses show/hide classes for responsive variants. Simple but creates duplicate markup.

#### Mobile Navigation (lief-lite-mono)

```typescript
// src/components/v1_Lief_Shipyard/3_Lief_Layouts/Layout_Standard/MobileNavigation.tsx
import { Menu, MenuButton, MenuItem, MenuItems, Transition } from '@headlessui/react';

<Menu>
  <MenuButton className="my-auto">
    {({ active }) => (
      <div className={`${active ? 'bg-t-secondary' : ''} lg:hidden`}>
        <svg>/* hamburger icon */</svg>
      </div>
    )}
  </MenuButton>
  <Transition
    enter="duration-[250ms] ease-out"
    enterFrom="opacity-0 -translate-y-10"
    enterTo="opacity-100 translate-y-0"
  >
    <MenuItems className="...">
      <MenuItem>
        {({ close }) => (
          <MobileNavLink href="/#menu" onClick={close} />
        )}
      </MenuItem>
    </MenuItems>
  </Transition>
</Menu>
```

**Assessment**: Sophisticated animated mobile menu with HeadlessUI. Proper close handler propagation. Smooth enter/leave transitions.

---

## Part 6: Code Patterns and Conventions

### 6.1 Styling Conventions

#### Inline Style Variables

Both codebases use style variables for repeated patterns:

```typescript
// Common pattern
const nw1Active = `min-w-0 flex-auto rounded-md border-0 bg-white/5 px-3.5 py-2 ...`;
const submitBtnStyle = `bg-t-primary flex-none rounded-md border-2 ...`;
```

#### Class String Composition

```typescript
// Template literal composition
class={`${styleOverrides} ${ACCESSIBILITY_STYLE} text-t-btn-primary-text`}

// Conditional classes
className={`${active ? 'bg-t-secondary' : ''} cursor-pointer`}
```

#### "BREAK" Debug Pattern

Both codebases use a unique debugging convention:

```html
<!-- Intentional class breaking for debugging -->
<body class="max-w-[100px]BREAK bg-lief-darkgreen-main">
	<main class="px-4BREAK min-h-screen">
		<div class="hiddenBREAK sticky bottom-10"></div></main
></body>
```

**Purpose**: Appending "BREAK" to class names invalidates them without removing them, useful for debugging layout issues.

### 6.2 Naming Conventions

#### Component Files

| Convention   | lief-c-astro           | lief-lite-mono     |
| ------------ | ---------------------- | ------------------ |
| **Astro**    | PascalCase.astro       | PascalCase.astro   |
| **React**    | PascalCase.tsx         | PascalCase.tsx/jsx |
| **Versions** | ComponentName_v2.astro | (numbered folders) |

#### CSS Variables

| Pattern        | Example                              |
| -------------- | ------------------------------------ |
| Theme prefix   | `--t-`                               |
| Component type | `--t-btn-`, `--t-text-`, `--t-link-` |
| State          | `--t-btn-primary-hover`              |

#### Import Alias Prefixes

| Prefix                     | Meaning                   |
| -------------------------- | ------------------------- |
| `@/`                       | src root                  |
| `@layouts/`                | Layout components         |
| `@shipyard_v1/`            | Shipyard component system |
| `@shipyard_v1/widgets/`    | Atomic elements           |
| `@shipyard_v1/components/` | Complex components        |

### 6.3 Comment and TODO Patterns

#### TODO Format

```typescript
// TODO: HIGH PRIORITY -> description
// TODO: MID PRIORITY -> description
// TODO: LOW PRIORITY -> description
// TODO: description (no priority)
```

#### Note Types

```typescript
// NOTE: explanation of why something is done a certain way
// README: important context for future developers
// IMPORTANT: critical information
// SKILLISSUE: self-identified learning moment/mistake
```

**Examples from codebase:**

```typescript
// NOTE: when using this in a react file, the import.meta.env.DIRECTUS_URL
// will NOT be read correctly, need to use a PUBLIC_ prepended version

// README: when migrating to SSR and using the /api/ folder these custom
// requests, nor the createDirectus WILL NOT work correctly

// SKILLISSUE: when trying to implement isr in a prior commit I did this
// incorrectly at first
```

---

## Part 7: Technical Debt Inventory

### 7.1 Deprecated Code

#### lief-c-astro

**Deprecated API Route:**

```typescript
// src/pages/api/contact-us.ts
/*
   TODO: THIS FILE IS DEPRECATED AND NEEDS TO BE CLEANED UP
*/
export const POST: APIRoute = async ({ request }) => {
	// Mock implementation
	console.log('mock api call starts');
	await new Promise((resolve) => setTimeout(resolve, 1500));
	const response = { ok: true };
	console.log('mock api call ends');
};
```

**Legacy Form Component:**

```
src/components/Forms/GetStarted_LEGACY.tsx
```

**Commented Components:**

```typescript
// import HomepageHero from '@/components/PageComponents/HomepageHero.astro';
// import HomepagePerks from '@/components/PageComponents/HomepagePerks.astro';
// V2 versions now in use
```

### 7.2 Incomplete Implementations

#### lief-lite-mono API Routes

```typescript
// src/pages/api/contact-newsletter.ts
// TODO: setup directus SDK here to POST newsletter form data

// src/pages/api/Data-careers.ts
// TODO: setup directus SDK here to GET careers data

// src/pages/api/Data-blog.ts
// (Presumed similar stub)
```

#### Form Error Handling

```typescript
// lief-c-astro ContactUs.tsx - Bug
{Array.isArray(formErrors) && formErrors.map((error, i) => (
  // formErrors is an object, never enters this block
```

#### Newsletter Disabled

```javascript
// lief-lite-mono Newsletter - Intentionally disabled
<input disabled readOnly />
<button disabled>Subscribe</button>
<p>temporarily disabled for demo purposes.</p>
```

### 7.3 Tracked Items

| Item                          | Location               | Status                      |
| ----------------------------- | ---------------------- | --------------------------- |
| Browser-specific tab behavior | lief-c-astro FAQ       | Non-critical, investigating |
| Form error consolidation      | lief-c-astro ContactUs | Refactor planned            |
| Directus SSR import pattern   | Both repos             | Workaround in place         |
| Browser cache behavior        | lief-lite-mono         | Documented                  |

**Firefox Bug Details:**

```typescript
// TODO: ON FIREFOX when tabbing through the site the accordion items are
// being prioritized and overriding other elements. Chrome/etc is fine
```

**Chrome Cache Bug Details:**

```typescript
// NOTE: on local there's a caching issue with redirect prop passed to
// defineConfig. chrome browser is dumb and force caches all 301 redirects
```

---

## Part 8: Security Considerations

### 8.1 Input Sanitization

#### DOMPurify Usage (lief-lite-mono)

```javascript
import DOMPurify from 'dompurify';

const sanitizedEmail = DOMPurify.sanitize(emailAddress);
```

**Assessment**: Good XSS protection for user input before storing. Only used in newsletter form.

#### Zod Validation (lief-c-astro)

```typescript
const cleanData: ContactUsSubmissionType = contactUsSchema.parse(data);
```

**Assessment**: Schema validation provides data shape guarantees but doesn't sanitize HTML/XSS.

### 8.2 API Authentication

#### Static Token Pattern

```typescript
const directus = createDirectus(import.meta.env.PUBLIC_DIRECTUS_URL)
	.with(staticToken(import.meta.env.DIRECTUS_STATIC_TOKEN))
	.with(rest());
```

**Security Notes from Code:**

```typescript
// NOTE: directus is using static tokens set as an env var per deployment
// docs: https://docs.directus.io/guides/sdk/authentication.html#create-a-directus-client-with-a-token

// TODO: HIGH PRIORITY -> update the createDirectus func so that with() uses
// authorization and leverages the static key generated in the directus-instance
// on the INTERNAL-ONLY-COMPANY-ACCOUNT
```

### 8.3 Environment Variable Exposure

**Client-Exposed Variables:**

```javascript
// Accessible in browser JavaScript
import.meta.env.PUBLIC_DIRECTUS_URL;
import.meta.env.PUBLIC_DIRECTUS_DISPO_ID;
```

**Server-Only Variables:**

```javascript
// Only accessible in server-side code (API routes, SSR)
import.meta.env.DIRECTUS_STATIC_TOKEN;
import.meta.env.DIRECTUS_URL;
import.meta.env.SENTRY_AUTH_TOKEN;
```

**Recommendation**: Ensure Directus permissions are properly configured to restrict operations available via public URL.

### 8.4 HTTP Method Barriers

```typescript
// Good practice in lief-c-astro
export const GET: APIRoute = async () => {
	return new Response(JSON.stringify({ message: 'Invalid method' }), {
		status: 405,
	});
};
export const PUT: APIRoute = async () => {
	/* 405 */
};
export const DELETE: APIRoute = async () => {
	/* 405 */
};
export const PATCH: APIRoute = async () => {
	/* 405 */
};
```

**Assessment**: Proper HTTP method restriction prevents unintended operations.

---

## Part 9: Recommendations

### 9.1 High Priority

1. **Complete API Routes (lief-lite-mono)**

   - Implement `contact-newsletter.ts` with proper Directus integration
   - Implement `Data-careers.ts` and `Data-blog.ts` for SSR data fetching
   - Add HTTP method barriers to all routes

2. **Fix Form Error Display (lief-c-astro)**

   ```typescript
   // Current (broken)
   {Array.isArray(formErrors) && formErrors.map(...)}

   // Fix
   {Object.entries(formErrors).map(([field, error]) => (
     error && <p key={field} className="text-red-600">{error}</p>
   ))}
   ```

3. **Consolidate Validation (lief-c-astro)**

   - Remove manual `validateForm()` function
   - Use Zod's built-in error handling:

   ```typescript
   const result = contactUsSchema.safeParse(dataToCLEAN);
   if (!result.success) {
   	setFormErrors(result.error.flatten().fieldErrors);
   	return;
   }
   ```

### 9.2 Medium Priority

4. **Add DOMPurify to lief-c-astro**

   - Currently no XSS sanitization on form inputs
   - Add sanitization before Directus submission

5. **Replace window.alert() (lief-lite-mono)**

   - Implement toast notification system similar to lief-c-astro
   - Consider porting nanostores + ToastNotification pattern

6. **Firefox Accordion Fix**

   - Investigate `tabindex` management in AccordionItem
   - Consider migrating to HeadlessUI Disclosure (already used in lief-lite-mono)

7. **Remove Deprecated Files**
   - Delete `contact-us.ts` (deprecated API route)
   - Delete or refactor `GetStarted_LEGACY.tsx`

### 9.3 Low Priority

8. **Standardize Type Definitions**

   - Replace `any` types with proper typing:

   ```typescript
   // Instead of
   const QuickFormFieldWrapper = ({ children }: any) => ...

   // Use
   const QuickFormFieldWrapper: React.FC<{ children: React.ReactNode }> = ({ children }) => ...
   ```

9. **Extract Shared Code**

   - Consider creating a shared package for:
     - Accessibility style constants
     - Directus client configuration
     - Common type definitions

10. **Document Theme Variables**
    - Create documentation for the CSS variable theming system
    - Include usage examples for each theme variant

### 9.4 Architecture Recommendations

11. **Consider Shipyard for lief-c-astro**

    - The Shipyard architecture in lief-lite-mono is more scalable
    - Could benefit the company site as it grows

12. **Standardize State Management**
    - Either adopt nanostores in lief-lite-mono
    - Or remove it from lief-c-astro if not needed
    - Current inconsistency adds cognitive load

---

## Appendix A: File Inventory

### lief-c-astro Source Files

| Directory                 | File Count | File Types  |
| ------------------------- | ---------- | ----------- |
| components/Analytics      | 3          | .astro      |
| components/Atoms          | 1          | .astro      |
| components/BlogComponents | 2          | .astro      |
| components/Forms          | 4          | .tsx        |
| components/Layout         | 4          | .astro      |
| components/Notifications  | 1          | .tsx        |
| components/PageComponents | 12         | .astro      |
| components/UI             | 14         | .astro      |
| layouts                   | 1          | .astro      |
| lib                       | 1          | .ts         |
| pages                     | 11         | .astro, .ts |
| pages/api                 | 3          | .ts         |
| store                     | 1          | .ts         |
| styles                    | 1          | .css        |
| types                     | 2          | .ts, .d.ts  |
| zodschema                 | 1          | .ts         |

### lief-lite-mono Source Files

| Directory             | File Count | File Types         |
| --------------------- | ---------- | ------------------ |
| Shipyard/1_Components | ~29        | .astro, .jsx       |
| Shipyard/2_Widgets    | ~31        | .astro, .tsx, .jsx |
| Shipyard/3_Layouts    | ~8         | .astro, .tsx       |
| Shipyard/98_Helpers   | 1          | .astro             |
| Shipyard/99_DEMO      | 3          | .tsx               |
| interfaces            | 1          | .ts                |
| layouts               | 1          | .astro             |
| lib                   | 1          | .ts                |
| pages                 | ~12        | .astro, .ts        |
| pages/api             | 5          | .ts                |
| styles                | 3          | .css               |
| types                 | 3          | .ts, .js           |

---

## Appendix B: Dependency Comparison

### Shared Dependencies

| Package            | lief-c-astro | lief-lite-mono |
| ------------------ | ------------ | -------------- |
| astro              | ^4.15.2      | ^4.15.2        |
| @astrojs/react     | ^3.6.2       | ^3.6.2         |
| @astrojs/tailwind  | ^5.1.0       | ^5.1.0         |
| @astrojs/vercel    | ^7.8.0       | ^7.8.0         |
| @directus/sdk      | ^16.1.0      | ^16.1.0        |
| @playform/compress | ^0.0.10      | ^0.0.10        |
| @sentry/astro      | ^8.5.0       | ^8.7.0         |
| dompurify          | ^3.1.6       | ^3.1.4         |
| react              | ^18.2.0      | ^18.2.0        |
| tailwindcss        | ^3.4.1       | ^3.4.1         |
| typescript         | ^5.3.3       | ^5.3.3         |

### Unique to lief-c-astro

| Package                    | Purpose                 |
| -------------------------- | ----------------------- |
| @tanstack/react-form       | Form handling (unused?) |
| @tanstack/zod-form-adapter | Form validation         |
| nanostores                 | State management        |
| @nanostores/react          | React bindings          |
| zod                        | Schema validation       |
| astro-icon                 | Icon handling           |

### Unique to lief-lite-mono

| Package                     | Purpose               |
| --------------------------- | --------------------- |
| @headlessui/react           | Accessible components |
| @heroicons/react            | Icon library          |
| swiper                      | Carousel              |
| lodash                      | Utilities (debounce)  |
| clsx                        | Class composition     |
| @fontsource-variable/inter  | Font loading          |
| @fontsource-variable/nunito | Font loading          |

---

## Appendix C: Code Metrics

### Lines of Code (Approximate)

| Metric                | lief-c-astro | lief-lite-mono |
| --------------------- | ------------ | -------------- |
| TypeScript/JavaScript | ~2,500       | ~4,000         |
| Astro Components      | ~3,000       | ~5,500         |
| CSS                   | ~150         | ~450           |
| **Total**             | ~5,650       | ~9,950         |

### Complexity Indicators

| Indicator          | lief-c-astro | lief-lite-mono |
| ------------------ | ------------ | -------------- |
| Import aliases     | 5            | 15+            |
| Theme variables    | 0            | 50+            |
| Component variants | ~15          | ~40            |
| API routes         | 3            | 5 (stubs)      |
| External libraries | 12           | 15             |

---

## Related Documentation

This report is part of a three-document set analyzing the Lief Platform:

| Document                             | Purpose                                                        |
| ------------------------------------ | -------------------------------------------------------------- |
| **reporeview-v1.md**                 | Git history and repository evolution                           |
| **codereview-v1.md** (this document) | Technical code analysis, architecture, patterns, and quality   |
| **regreview-v1.md**                  | Human-readable portfolio summary for GitHub, LinkedIn, resumes |

---

_End of Report_
