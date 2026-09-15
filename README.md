# Mérouane Karki — Portfolio

A single-page portfolio built with React and TypeScript. It combines a glass-style profile hub with filterable projects, skill dialogs, a career timeline, and reading progress navigation.

The interface uses an ice-blue palette, translucent panels, and a landscape background. Content is defined in the application source, with local assets for the portrait and downloadable CV.

**First release: 0.1.0**

## Implementation highlights

- **Frame-scheduled scroll updates.** The [profile hub](./app/glass-hub.tsx) uses a passive scroll listener and [`requestAnimationFrame`](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestAnimationFrame) to group reading-progress and active-section updates. A [`ResizeObserver`](https://developer.mozilla.org/en-US/docs/Web/API/ResizeObserver) recalculates progress when the document’s size changes. Listeners, observers, and pending frames are cleaned up on unmount.

- **One-time section reveals.** The [content component](./app/portfolio-content.tsx) uses [`IntersectionObserver`](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) to reveal sections at an 8% visibility threshold, then stops observing them. Sections remain visible until JavaScript enables the reveal styles.

- **Glass effects in CSS.** The [stylesheet](./app/globals.css) combines translucent gradients, inset shadows, and [`backdrop-filter`](https://developer.mozilla.org/en-US/docs/Web/CSS/backdrop-filter) to create depth over the background image.

- **Reduced-motion support.** JavaScript skips scroll reveals when reduced motion is requested. CSS also disables animations, transitions, hover movement, and smooth scrolling through [`prefers-reduced-motion`](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion).

- **Native scrolling behavior.** Anchor navigation uses CSS [`scroll-behavior`](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-behavior). Dialog content uses [`overscroll-behavior`](https://developer.mozilla.org/en-US/docs/Web/CSS/overscroll-behavior) to contain scrolling within the panel.

- **Shared data for cards and dialogs.** The [project and skill definitions](./app/portfolio-content.tsx) drive both summaries and detail views. React state controls filters and selected items. Filter buttons expose their pressed state, and a live region announces the result count.

## Libraries and tooling

The [package manifest](./package.json) records the dependencies and their pinned versions.

| Technology | Role in this project |
| :--- | :--- |
| [React](https://react.dev/) and [TypeScript](https://www.typescriptlang.org/) | Components, local interaction state, and typed project selections. |
| [Vinext](https://github.com/cloudflare/vinext) and [Vite](https://vite.dev/) | Next.js-style application conventions on a Vite build pipeline. The project pins Vinext `1.0.0-beta.5`. |
| [Cloudflare Vite plugin](https://developers.cloudflare.com/workers/vite-plugin/) and [Wrangler](https://developers.cloudflare.com/workers/wrangler/) | Workers runtime integration, configured in [vite.config.ts](./vite.config.ts). |
| [Base UI](https://base-ui.com/react/components/dialog) and [shadcn/ui](https://ui.shadcn.com/docs) | Locally maintained wrappers for dialogs, tabs, progress indicators, and other UI primitives. |
| [Tailwind CSS 4](https://tailwindcss.com/docs/theme) and [tw-animate-css](https://github.com/Wombosvideo/tw-animate-css) | Theme tokens, utility classes, and component transition utilities alongside custom CSS. |
| [Class Variance Authority](https://cva.style/docs) | Typed style variants for shared components, including the [tabs](./components/ui/tabs.tsx). |
| [clsx](https://github.com/lukeed/clsx) and [tailwind-merge](https://github.com/dcastil/tailwind-merge) | Conditional class composition and Tailwind conflict resolution through the [shared utility](./lib/utils.ts). |
| [Lucide React](https://lucide.dev/guide/react) | SVG icons for navigation, cards, and controls. |
| [Oxlint](https://oxc.rs/docs/guide/usage/linter.html) and [Oxfmt](https://oxc.rs/docs/guide/usage/formatter.html) | Linting and formatting. |

The [UI directory](./components/ui/) contains a broader component collection than the current page uses. The main interface imports the dialog, tabs, and progress wrappers.

## Typography

The [stylesheet](./app/globals.css) uses a system font stack: `-apple-system`, `BlinkMacSystemFont`, `Segoe UI`, and `sans-serif`.

This selects the platform’s available interface font, including [Apple’s system fonts](https://developer.apple.com/fonts/) and [Segoe UI](https://learn.microsoft.com/en-us/typography/font-list/segoe-ui). No external font files are downloaded.

## Project structure

Source and configuration layout:

```text
/
  app/
  components/
    ui/
  hooks/
  lib/
  public/
  components.json
  next.config.ts
  package.json
  pnpm-lock.yaml
  pnpm-workspace.yaml
  tsconfig.json
  vite.config.ts
```

- [app/](./app/) — Page composition, document metadata, portfolio content, interaction logic, and global styling.
- [components/ui/](./components/ui/) — Shared UI primitives and their local styling wrappers.
- [hooks/](./hooks/) — Reusable hooks, including viewport detection for shared components.
- [lib/](./lib/) — Shared utilities for composing component classes.
- [public/](./public/) — Landscape and portrait images, the SVG favicon, and the downloadable PDF CV. Assets sit directly in this directory.
