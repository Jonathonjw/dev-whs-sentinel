# Sentinal WHS Design System

## CRITICAL: Every UI component must follow these rules. No exceptions.

The current UI is developer-grade — functional but visually poor. Every new page and every refactored page MUST follow this design system. If you're building UI, use the `frontend-design` or `ui-ux-pro-max` skill.

---

## Colour Palette

### Primary (Navy)
| Token | Hex | Use |
|-------|-----|-----|
| navy-950 | #0B1120 | Page background |
| navy-900 | #111827 | Card backgrounds, sidebar |
| navy-800 | #1F2937 | Borders, dividers, hover states |
| navy-700 | #374151 | Secondary borders, input backgrounds |
| navy-600 | #4B5563 | Muted text, placeholders |
| navy-500 | #6B7280 | Secondary text |
| navy-400 | #9CA3AF | Body text on dark |
| navy-300 | #D1D5DB | Primary text on dark |

### Accent Colours
| Token | Hex | Use |
|-------|-----|-----|
| green-500 | #22C55E | Primary CTA, success, active states |
| green-600 | #16A34A | CTA hover |
| blue-400 | #60A5FA | Active nav, links |
| blue-500 | #3B82F6 | Interactive elements |
| red-500 | #EF4444 | Destructive, errors, BLOCK status |
| amber-500 | #F59E0B | Warnings, WARN status |
| emerald-500 | #10B981 | PASS status |

---

## Typography

- **Headings:** `text-white font-bold` — h1: `text-2xl`, h2: `text-xl`, h3: `text-lg`
- **Body text:** `text-navy-400` on dark backgrounds, `text-navy-900` on light
- **Labels:** `text-sm font-medium text-navy-400`
- **Muted/helper:** `text-xs text-navy-500`
- **Numbers/stats:** `text-3xl font-bold text-green-500` (or appropriate accent)

---

## Card Design

ALL content must be in cards. No floating text on the page background.

```tsx
{/* Standard card */}
<div className="rounded-xl border border-navy-800 bg-navy-900 p-6">
  {/* content */}
</div>

{/* Stat card */}
<div className="rounded-xl border border-navy-800 bg-navy-900 p-6">
  <p className="text-sm font-medium text-navy-400">Label</p>
  <p className="mt-2 text-3xl font-bold text-white">42</p>
  <p className="mt-1 text-xs text-navy-500">+12% from last month</p>
</div>

{/* Action card with hover */}
<div className="rounded-xl border border-navy-800 bg-navy-900 p-6 transition-colors hover:border-navy-700 hover:bg-navy-800/50 cursor-pointer">
  {/* content */}
</div>
```

**NEVER** use white cards (`bg-white`) on the dark navy background. Everything is dark-on-dark with subtle borders.

---

## Buttons

```tsx
{/* Primary CTA */}
<button className="rounded-xl bg-green-600 px-5 py-3 text-sm font-medium text-white transition-colors hover:bg-green-500 active:bg-green-400">
  Action
</button>

{/* Secondary */}
<button className="rounded-xl border border-navy-700 bg-navy-800 px-5 py-3 text-sm font-medium text-navy-300 transition-colors hover:bg-navy-700 hover:text-white">
  Secondary
</button>

{/* Destructive */}
<button className="rounded-xl bg-red-600 px-5 py-3 text-sm font-medium text-white transition-colors hover:bg-red-500">
  Delete
</button>

{/* Ghost */}
<button className="rounded-lg px-3 py-2 text-sm text-navy-400 transition-colors hover:bg-navy-800 hover:text-white">
  Cancel
</button>
```

---

## Status Badges

```tsx
{/* Pass / Active / Complete */}
<span className="inline-flex items-center rounded-full bg-emerald-500/10 px-3 py-1 text-xs font-medium text-emerald-400">
  Active
</span>

{/* Warning / Pending */}
<span className="inline-flex items-center rounded-full bg-amber-500/10 px-3 py-1 text-xs font-medium text-amber-400">
  Pending
</span>

{/* Block / Error / Suspended */}
<span className="inline-flex items-center rounded-full bg-red-500/10 px-3 py-1 text-xs font-medium text-red-400">
  Blocked
</span>

{/* Neutral / Draft / Inactive */}
<span className="inline-flex items-center rounded-full bg-navy-700 px-3 py-1 text-xs font-medium text-navy-400">
  Draft
</span>

{/* Info / In Progress */}
<span className="inline-flex items-center rounded-full bg-blue-500/10 px-3 py-1 text-xs font-medium text-blue-400">
  In Progress
</span>
```

---

## Tables

```tsx
<div className="overflow-hidden rounded-xl border border-navy-800">
  <table className="w-full">
    <thead>
      <tr className="border-b border-navy-800 bg-navy-900">
        <th className="px-6 py-4 text-left text-xs font-semibold uppercase tracking-wider text-navy-500">
          Column
        </th>
      </tr>
    </thead>
    <tbody className="divide-y divide-navy-800">
      <tr className="bg-navy-900 transition-colors hover:bg-navy-800/50">
        <td className="px-6 py-4 text-sm text-navy-300">
          Cell content
        </td>
      </tr>
    </tbody>
  </table>
</div>
```

---

## Form Inputs

```tsx
{/* Text input */}
<input
  type="text"
  className="w-full rounded-lg border border-navy-700 bg-navy-800 px-4 py-3 text-sm text-white placeholder-navy-500 transition-colors focus:border-blue-500 focus:outline-none focus:ring-1 focus:ring-blue-500"
  placeholder="Enter value..."
/>

{/* Select */}
<select className="w-full rounded-lg border border-navy-700 bg-navy-800 px-4 py-3 text-sm text-white focus:border-blue-500 focus:outline-none focus:ring-1 focus:ring-blue-500">
  <option>Select...</option>
</select>

{/* Label */}
<label className="block text-sm font-medium text-navy-400 mb-2">
  Field Label
</label>
```

---

## Page Layout

```tsx
{/* Standard page */}
<div>
  {/* Header row */}
  <div className="mb-8 flex flex-col gap-4 sm:flex-row sm:items-center sm:justify-between">
    <div>
      <h1 className="text-2xl font-bold text-white">Page Title</h1>
      <p className="mt-1 text-sm text-navy-400">Description text</p>
    </div>
    <button className="...primary CTA...">
      Action
    </button>
  </div>

  {/* Content area — grid or cards */}
  <div className="grid gap-6 lg:grid-cols-3">
    {/* cards */}
  </div>
</div>
```

---

## Spacing & Layout Rules

- Page padding: handled by layout (`p-2 sm:p-4 md:p-8`)
- Card padding: `p-6`
- Gap between cards: `gap-6`
- Section spacing: `mb-8` between major sections
- Rounded corners: `rounded-xl` for cards/containers, `rounded-lg` for inputs/buttons
- Border: `border border-navy-800` on all cards

---

## DO NOT

- Use `bg-white` on any card or container — the app is dark mode
- Use inline styles — everything via Tailwind classes
- Use hard-coded colours — use the palette tokens above
- Build pages without cards — no loose text on page background
- Skip hover/transition states — every interactive element needs `transition-colors` and a hover state
- Use tiny tap targets — minimum 44px height for mobile buttons
- Forget mobile responsiveness — every page must work on phone width
- Mix light and dark UI patterns — everything is consistent dark navy

---

## Dashboard/Overview Pages

- Top row: 3-4 stat cards in a grid (`grid-cols-2 lg:grid-cols-4`)
- Middle: data table or content cards
- Bottom: quick action buttons
- Include data freshness indicator ("Last updated: 2 min ago")
- Show empty states with icon + message + CTA button

## Mobile Considerations

- Stack cards vertically on mobile (`grid-cols-1 sm:grid-cols-2 lg:grid-cols-3`)
- Larger tap targets on mobile (`py-3` minimum on buttons)
- Collapsible sections for dense forms
- Bottom-anchored primary CTA on completion flows
