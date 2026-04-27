---
name: frontend-design
description: Design-aware code generation assistant with expertise in UI/UX principles, component patterns, CSS/Tailwind conventions, accessibility standards (WCAG), responsive design, and modern frontend best practices.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [frontend, ui, ux, css, tailwind, accessibility, design-systems, responsive]
    category: development
    related_skills: [github-integration, capability-evolver]
    config:
      - key: preferred_framework
        description: "Primary framework (react, vue, svelte, vanilla, astro)"
        default: "react"
        prompt: "Which frontend framework do you primarily use?"
      - key: css_approach
        description: "CSS methodology (tailwind, css-modules, styled-components, scss)"
        default: "tailwind"
        prompt: "Which CSS approach do you prefer?"
      - key: design_system
        description: "Design system or component library (material-ui, shadcn, chakra, custom)"
        default: "custom"
        prompt: "Are you using a specific design system?"
---

# Frontend Design Assistant Skill

Elevate frontend output quality with built-in design intelligence, accessibility checks, and component best practices.

## When to Use

Trigger this skill for any frontend/UI-related task:

**Component Generation:**
- "Build a [component type] component" (button, card, form, modal, table, nav, etc.)
- "Create a [feature] page layout" (dashboard, landing, settings, profile)
- "Design a [pattern] (pricing table, hero section, blog card)"

**Design Review:**
- "Review this component/page for UX issues"
- "Check WCAG accessibility compliance"
- "Does this match our design system?"

**Implementation Help:**
- "Convert this design to code" (describe or upload mockup)
- "Make this responsive/mobile-friendly"
- "Refactor this CSS to [Tailwind/CSS Modules/etc.]"
- "Add dark mode support"

**Style System:**
- "Create color palette from brand guidelines"
- "Define typography scale for this project"
- "Set up spacing/sizing system"

## Quick Reference

| Task | Command Example | Output |
|------|-----------------|--------|
| Component | `/frontend component pricing-table 3-tier` | React + Tailwind code |
| Page Layout | `/frontend layout dashboard analytics` | Full page structure |
| Accessibility Audit | `/frontend a11y audit path/to/component` | WCAG checklist + fixes |
| Responsive Fix | `/frontend responsive navbar mobile` | Mobile-optimized version |
| Style Guide | `/frontend design-system colors typography` | CSS variables/tokens |
| Refactor | `/frontend refactor css-to-tailwind file.css` | Converted code |

## Procedure

### Phase 1: Requirements Gathering (30 seconds - 2 minutes)

1. **Understand Context**
   
   Before generating any code, clarify:
   
   **Technical Stack:**
   - Framework: React/Vue/Svelte/Astro/Vanilla JS?
   - Styling: Tailwind CSS / CSS Modules / Styled Components / SCSS?
   - Component Library: Material UI / Shadcn / Chakra / Headless UI / Custom?
   - Build Tool: Vite / Next.js / Webpack / Remix?
   
   **Design Constraints:**
   - Design system available? (tokens, components, guidelines)
   - Brand colors/fonts defined?
   - Must support dark mode? RTL languages? i18n?
   - Target browsers/devices? (mobile-first? desktop app?)
   
   **Functional Requirements:**
   - What should the component/page do?
   - User interactions needed? (click, hover, focus, drag, etc.)
   - States to handle? (loading, empty, error, success, disabled)
   - Accessible? (keyboard navigation, screen reader, contrast)
   
   **Content & Data:**
   - Dynamic content? (props, API data, i18n strings)
   - Content density expected? (minimal, medium, dense)
   - Media involved? (images, icons, video, avatars)

2. **Establish Design Principles**
   
   Based on project type, apply appropriate principles:
   
   **For SaaS/Dashboards:**
   - Clarity over decoration
   - Consistent spacing grid (4px or 8px base unit)
   - Clear visual hierarchy (headings, actions, metadata)
   - Efficient information density
   - Status indication (colors, icons, badges)
   
   **For Marketing/Landing Pages:**
   - Visual impact and whitespace
   - Clear call-to-action hierarchy
   - Trust signals (social proof, testimonials)
   - Fast loading, smooth animations
   - Mobile-optimized (often mobile-first traffic)
   
   **For Forms/Data Entry:**
   - Progressive disclosure (don't overwhelm)
   - Clear validation feedback (inline, not just on submit)
   - Logical grouping and flow
   - Keyboard efficiency (tab order, shortcuts)
   - Error prevention over error correction

### Phase 2: Design & Architecture (2-5 minutes)

3. **Component Structure Planning**
   
   Define atomic structure before writing code:
   
   ```
   Example: Pricing Table Component
   
   PricingTable (container)
   ├── PricingHeader (title, subtitle, optional badge)
   ├── PricingPlans (grid/flex container)
   │   ├── PricingPlan (individual tier) [×3]
   │   │   ├── PlanHeader (name, price, period, popular badge)
   │   │   ├── PlanFeatures (list of features with check/x icons)
   │   │   └── PlanCTA (button, disabled state for current plan)
   │   └── [repeat for each tier]
   └── PricingFooter (FAQ link, contact CTA, trust elements)
   ```

4. **Responsive Strategy**
   
   Define breakpoints and behavior:
   
   ```css
   /* Mobile-first approach */
   
   /* Base (< 640px): Single column, stacked layout */
   /* sm (≥ 640px): Minor adjustments */
   /* md (≥ 768px): Two columns start appearing */
   /* lg (≥ 1024px): Full desktop layout */
   /* xl (≥ 1280px): Max-width container, centered */
   
   Specific decisions:
   - Navigation: Hamburger menu → horizontal nav
   - Sidebar: Off-canvas drawer → fixed sidebar → always visible
   - Grid: 1 col → 2 col → 3 col → 4 col
   - Tables: Horizontal scroll → simplified cards → full table
   - Typography: Scale down headings, increase line-height on mobile
   ```

5. **Accessibility Architecture**
   
   Plan accessibility from the start:
   
   ```markdown
   ## A11y Checklist for [Component Name]
   
   ### Semantic HTML
   - [ ] Uses semantic elements (nav, main, aside, button, not div)
   - [ ] Proper heading hierarchy (h1 → h2 → h3, no skipping)
   - [ ] Landmarks correctly applied (header, nav, main, footer)
   
   ### Keyboard Navigation
   - [ ] All interactive elements reachable via Tab
   - [ ] Logical tab order (visual = DOM order)
   - [ ] Focus states clearly visible (outline, not just color change)
   - [ ] Escape closes modals/dropdowns
   - [ ] Enter/Space activates buttons, links, checkboxes
   - [ ] Arrow keys navigate within widgets (tabs, menus, lists)
   
   ### Screen Reader Support
   - [ ] Alt text for meaningful images (decorative images alt="")
   - [ ] ARIA labels for icon-only buttons
   - [ ] ARIA live regions for dynamic content updates
   - [ ] Role attributes where semantics unclear
   - [ ] aria-expanded, aria-selected for stateful widgets
   - [ ] aria-describedby for additional context
   
   ### Visual Accessibility
   - [ ] Color contrast ratio ≥ 4.5:1 (normal text), ≥ 3:1 (large text)
   - [ ] Information not conveyed by color alone (use icons/text too)
   - [ ] Focus indicator visible (not outline: 0 unless custom replacement)
   - [ ] Text resizable up to 200% without loss of function
   - [ ] No content flashes > 3 times per second (seizure safety)
   
   ### Motion & Interaction
   - [ ] Respects prefers-reduced-motion media query
   - [ ] Adequate click/touch target size (min 44×44 CSS pixels)
   - [ ] No hover-only interactions (must work on touch)
   - [ ] Loading states communicated (spinner + "Loading..." text)
   ```

### Phase 3: Code Generation (5-15 minutes)

6. **Generate Component Code**
   
   Apply consistent patterns:
   
   **React + Tailwind Example (Pricing Table):**
   ```tsx
   import { cn } from '@/lib/utils'
   
   interface PricingTier {
     name: string
     price: string
     period: string
     description: string
     features: { text: boolean; included: boolean }[]
     cta: string
     highlighted?: boolean
     current?: boolean
   }
   
   interface PricingTableProps {
     tiers: PricingTier[]
     onSelect?: (tierName: string) => void
     className?: string
   }
   
   export function PricingTable({ 
     tiers, 
     onSelect, 
     className 
   }: PricingTableProps) {
     return (
       <section 
         className={cn('w-full max-w-6xl mx-auto', className)}
         aria-label="Pricing plans"
       >
         <div className="grid gap-8 md:grid-cols-3">
           {tiers.map((tier) => (
             <article
               key={tier.name}
               className={cn(
                 'relative rounded-2xl border-2 p-8 flex flex-col',
                 tier.highlighted 
                   ? 'border-primary shadow-xl scale-105' 
                   : 'border-border',
                 tier.current && 'opacity-75'
               )}
               aria-labelledby={`plan-${tier.name}`}
             >
               {tier.highlighted && (
                 <span className="absolute -top-3 left-1/2 -translate-x-1/2 bg-primary text-primary-foreground px-3 py-1 rounded-full text-sm font-medium">
                   Most Popular
                 </span>
               )}
               
               <header id={`plan-${tier.name}`} className="mb-6">
                 <h3 className="text-xl font-semibold">{tier.name}</h3>
                 <div className="mt-4 flex items-baseline gap-1">
                   <span className="text-4xl font-bold tracking-tight">
                     {tier.price}
                   </span>
                   <span className="text-muted-foreground">
                     /{tier.period}
                   </span>
                 </div>
                 <p className="mt-2 text-sm text-muted-foreground">
                   {tier.description}
                 </p>
               </header>
               
               <ul className="flex-1 space-y-3 mb-8" role="list">
                 {tier.features.map((feature, idx) => (
                   <li 
                     key={idx} 
                     className="flex items-start gap-3"
                     aria-label={`${feature.included ? 'Included' : 'Not included'}: ${feature.text}`}
                   >
                     <span 
                       className={cn(
                         'mt-0.5 flex-shrink-0',
                         feature.included ? 'text-primary' : 'text-muted-foreground/50'
                       )}
                       aria-hidden="true"
                     >
                       {feature.included ? '✓' : '✗'}
                     </span>
                     <span className={cn(
                       'text-sm',
                       !feature.included && 'text-muted-foreground line-through'
                     )}>
                       {feature.text}
                     </span>
                   </li>
                 ))}
               </ul>
               
               <button
                 onClick={() => !tier.current && onSelect?.(tier.name)}
                 disabled={tier.current}
                 className={cn(
                   'w-full py-3 px-4 rounded-lg font-medium transition-colors',
                   tier.highlighted
                     ? 'bg-primary text-primary-foreground hover:bg-primary/90'
                     : 'bg-secondary text-secondary-foreground hover:bg-secondary/80',
                   tier.current && 'cursor-not-allowed opacity-50'
                 )}
                 aria-describedby={
                   tier.current ? `${tier.name}-current-note` : undefined
                 }
               >
                 {tier.current ? 'Current Plan' : tier.cta}
               </button>
               
               {tier.current && (
                 <p 
                   id={`${tier.name}-current-note`} 
                   className="sr-only"
                 >
                   You are currently on this plan
                 </p>
               )}
             </article>
           ))}
         </div>
       </section>
     )
   }
   ```
   
   **Key Quality Indicators in Generated Code:**
   - ✅ TypeScript interfaces for props
   - ✅ Composable utility (cn() for class merging)
   - ✅ Semantic HTML (article, header, button, not div)
   - ✅ ARIA attributes (aria-label, aria-labelledby, aria-describedby)
   - ✅ Screen-reader-only text (.sr-only) for visual-only info
   - ✅ Disabled state handling
   - ✅ Responsive classes (md: breakpoint prefix)
   - ✅ Consistent spacing scale (using Tailwind's spacing system)
   - ✅ Accessible color usage (text-muted-foreground for secondary info)

7. **Generate Supporting Styles/Tokens**
   
   If creating design system pieces:
   
   ```css
   /* Design Tokens - CSS Custom Properties */
   :root {
     /* Colors */
     --color-primary: 222.2 47.4% 11.2%;
     --color-primary-foreground: 210 40% 98%;
     --color-secondary: 210 40% 96.1%;
     --color-secondary-foreground: 222.2 47.4% 11.2%;
     --color-accent: 210 40% 96.1%;
     --color-destructive: 0 84.2% 60.2%;
     --color-border: 214.3 31.8% 91.4%;
     --color-ring: 222.2 84% 4.9%;
     --color-background: 0 0% 100%;
     --color-foreground: 222.2 47.4% 11.2%;
     
     /* Typography */
     --font-sans: 'Inter', system-ui, sans-serif;
     --font-mono: 'JetBrains Mono', monospace;
     
     /* Spacing (4px base unit) */
     --space-1: 0.25rem;  /* 4px */
     --space-2: 0.5rem;   /* 8px */
     --space-3: 0.75rem;  /* 12px */
     --space-4: 1rem;     /* 16px */
     --space-6: 1.5rem;   /* 24px */
     --space-8: 2rem;     /* 32px */
     
     /* Border Radius */
     --radius-sm: calc(var(--space-1) - 2px);
     --radius-md: calc(var(--space-2) - 2px);
     --radius-lg: var(--space-2);
     --radius-xl: var(--space-3);
   }
   
   .dark {
     --color-primary: 210 40% 98%;
     --color-primary-foreground: 222.2 47.4% 11.2%;
     /* ... inverted lightness values for dark mode */
   }
   ```

### Phase 4: Review & Optimization (2-5 minutes)

8. **Self-Quality Audit**
   
   Before presenting code to user, verify:
   
   **Code Quality:**
   - [ ] No unnecessary wrapper divs (use fragments <> where possible)
   - [ ] Props interface is complete and well-documented
   - [ ] No magic numbers (use spacing scale, token variables)
   - [ ] Consistent naming convention (componentCase for components)
   - [ ] No inline styles (use Tailwind classes or CSS modules)
   - [ ] Event handlers properly typed
   
   **Design Quality:**
   - [ ] Visual hierarchy clear (size, weight, color, spacing)
   - [ ] Alignment consistent (grid system, logical grouping)
   - [ ] Whitespace used effectively (not too dense, not too sparse)
   - [ ] Color palette cohesive (not random colors)
   - [ ] Interactive states defined (hover, focus, active, disabled)
   - [ ] Loading/empty/error states considered
   
   **Accessibility:**
   - [ ] Passes basic WCAG 2.1 AA checklist (see Phase 2, Step 5)
   - [ ] Tested with keyboard navigation mentally
   - [ ] Color contrast meets ratios (use contrast checker tools)
   - [ ] Screen reader output makes sense
   
   **Performance:**
   - [ ] No unnecessary re-renders (memo, useMemo where beneficial)
   - [ ] Images optimized (lazy loading, proper sizing, formats)
   - [ ] Fonts loaded efficiently (font-display: swap, subset)
   - [ ] Animations GPU-accelerated (transform, opacity only)
   - [ ] Bundle size considered (tree-shakeable, no heavy deps)

9. **Generate Improvement Suggestions**
   
   Always offer enhancements:
   
   ```markdown
   ## 💡 Enhancement Opportunities
   
   **Quick Wins (Low Effort, High Impact):**
   1. **Micro-interactions**: Add subtle hover scale/transition to cards
      - Code: `transition-transform hover:scale-[1.02]`
      - Impact: Feels more polished, professional
   
   2. **Skeleton Loading**: Replace spinner with skeleton placeholders
      - Why: Reduces perceived load time, less layout shift
      - Effort: ~15 minutes with existing shadcn/ui Skeleton component
   
   **Medium Effort:**
   3. **Animation**: Entrance animation with staggered reveal
      - Use: Framer Motion or CSS animations
      - Impact: Premium feel, guides attention
   
   4. **Internationalization**: Extract hardcoded strings to i18n keys
      - Prepare for multi-language support from start
   
   **Future Considerations:**
   5. **Component Variants**: Create variant system (size, color scheme)
      - Make reusable across different contexts
      - Use cva (class-variance-authority) pattern
   
   6. **Storybook/Documentation**: Add to component library docs
      - Showcase all states, variants, and usage examples
   ```

## Common Component Patterns

### Pattern Library Reference

**Buttons:**
```tsx
// Variant-based button with sizes and states
// Supports: primary, secondary, destructive, outline, ghost
// Sizes: sm, md, lg, icon
// States: loading, disabled
```

**Form Inputs:**
```tsx
// Label + Input + Helper Text + Error Message
// Integrated with react-hook-form or similar
// Accessible: label htmlFor, aria-describedby for errors
// Validation: inline feedback, not just on submit
```

**Data Display:**
```tsx
// Table: Sortable columns, selection, pagination, row actions
// Card: Header + content + footer pattern
// List: Virtualized for large datasets (react-virtuoso)
// Empty State: Illustration + message + action button
```

**Navigation:**
```tsx
// Breadcrumbs: Schema.org markup for SEO
// Tabs: Keyboard accessible, ARIA tabs pattern
// Sidebar: Collapsible, responsive (drawer on mobile)
// Pagination: Accessible, keyboard navigable
```

**Feedback:**
```tsx
// Toast/Notification: Positioning, auto-dismiss, stacking
// Modal/Dialog: Focus trap, escape to close, backdrop click
// Confirmation: Destructive actions require confirmation
// Progress: Determinate (percentage) and indeterminate (loading)
```

## Responsive Design Patterns

### Mobile-First Approach Examples

**Navigation Transformation:**
```
Mobile (< 768px):
├── Hamburger button (☰)
│   └── Slide-out drawer with full nav
│
Desktop (≥ 768px):
└── Horizontal nav bar with dropdowns
```

**Layout Transformations:**
```
Dashboard Grid:
Mobile:  Single column, stacked cards
Tablet:  2-column grid (main + sidebar)
Desktop: 3-column (sidebar + main + auxiliary)
```

**Table → Cards Pattern:**
```
Desktop Table:
| Name | Email | Role | Status | Actions |
|------|-------|------|--------|---------|
| ...  | ...   | ...  | ...    | ...     |

Mobile Cards:
┌─────────────────────┐
│ John Doe            │
│ john@email.com      │
│ Role: Admin         │
│ Status: ● Active    │
│ [Edit] [Delete]     │
└─────────────────────┘
```

## Integration Notes

**Synergistic Skills:**
- **GitHub Integration**: Create PRs with component code, get design reviews
- **Capability Evolver**: Learn project's design patterns and conventions over time
- **Utility Toolkit**: Generate placeholder images, lorem ipsum, color palettes

**Workflow Integration:**
```
Design Phase:
  1. Describe component or share Figma/mockup reference
  2. Frontend Design skill generates component with a11y built-in
  3. Review generated code, request adjustments

Development Phase:
  4. Integrate into project, test responsiveness
  5. Run accessibility audit (built-in checklist)
  6. Push to GitHub, get team feedback via GitHub skill

Optimization Phase:
  7. Review performance implications
  8. Add animations/polish
  9. Document in Storybook/component library
```

---

*Based on OpenClaw's highly-rated Frontend Design skill with enhanced accessibility standards, modern framework patterns, and comprehensive design system support for Hermes.*
