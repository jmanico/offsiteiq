# DESIGN.md: offsiteiq Design System

Version: 0.1 (draft, 2026-10-01)
Status: DRAFT. Derived from [REQUIREMENTS.md](REQUIREMENTS.md). Where this document and REQUIREMENTS.md disagree, REQUIREMENTS.md wins; on security, [SECURITY.md](SECURITY.md) wins. Requirement IDs (for example `FR-SRCH-08`) are cited where a design decision exists to satisfy one.

## 1. Design intent

v1 scope is defined in REQUIREMENTS.md §2. The UI must not imply that offsiteiq books anything. Use "options", "rates", and "selected", never "booked" or "confirmed".

### Personality

| Be | Not |
|---|---|
| calm | busy |
| capable | flashy |
| friendly | corporate-stiff |
| organized | dense |
| premium | luxurious |
| intelligent | "AI-themed" |

No robot imagery, glowing gradients, sparkles, or stock airline imagery as the main visual language.

## 2. Reference direction

Take broad cues from polished modern SaaS marketing sites: large confident type, light backgrounds, dark navy text, controlled accent colors, rounded cards, real product screenshots, and short sections with clear hierarchy.

Do not copy any other company's logo geometry, illustrations, page compositions, source code, or brand assets. offsiteiq must be recognizable on its own.

## 3. Brand concept

Core metaphor: **origins → routes → one destination**.

| Attribute | Expression |
|---|---|
| Coordinated | aligned cards, connected nodes |
| Distributed | multiple origin points, converging routes |
| Human | avatars, plain copy, soft geometry |
| Intelligent | explained comparisons, visible assumptions |
| Trustworthy | clear totals, source and freshness on every rate |

## 4. Logo

File: [`logo.svg`](logo.svg)

### Construction

Three colored origin nodes (blue, orchid, teal) with short curved routes converging on a navy ring with a descender. The ring reads as a destination, a meeting point, and the "q" of "iq".

- Flat colors only, no gradients.
- Must remain legible at 20px. At favicon size, drop the routes and keep the ring plus nodes.

### Wordmark

`offsite` in `brand.navy`, `iq` in `brand.teal`. Always lowercase. Inter, semibold (600–650), letter spacing about -0.02em. No italics or script.

### Variants to produce

1. Horizontal mark + wordmark (`logo.svg`)
2. Symbol only
3. Single-color navy
4. Single-color white (for dark surfaces)
5. App icon
6. Favicon

### Clear space

At least one node diameter on every side.

### Do not

- use an airplane, globe, or luggage as the logo
- add gradients, shadows, or "AI" sparkles
- recolor the nodes outside the brand palette
- stretch, rotate, or outline the mark

## 5. Color

The interface is mostly neutral. Color helps people parse information; it does not decorate.

### Brand

| Token | Hex | Use |
|---|---|---|
| `brand.navy` | `#172A46` | primary text, navigation, logo |
| `brand.blue` | `#3978F6` | primary actions, links, selection |
| `brand.teal` | `#19A88C` | within-budget, positive states, coordination |
| `brand.orchid` | `#805AD5` | itinerary and experience accents |

### Supporting

| Token | Hex | Use |
|---|---|---|
| `accent.sky` | `#DDEBFF` | blue-tinted surfaces |
| `accent.mint` | `#DDF5EE` | teal-tinted surfaces |
| `accent.lilac` | `#ECE6FA` | orchid-tinted surfaces |
| `accent.sun` | `#F8C75A` | warnings, stale data |
| `accent.coral` | `#E96A67` | over budget, conflicts, destructive actions |
| `accent.coral-tint` | `#FBE1E0` | coral-tinted surfaces (over-budget and conflict chips) |
| `accent.sun-tint` | `#FDF1D3` | sun-tinted surfaces (warning and stale chips) |
| `accent.sun-dark` | `#B7861A` | sun icons and graphics that need 3:1 contrast on light surfaces |

### Neutrals

| Token | Hex |
|---|---|
| `neutral.0` | `#FFFFFF` |
| `neutral.25` | `#FBFCFE` |
| `neutral.50` | `#F5F7FA` |
| `neutral.100` | `#E9EDF3` |
| `neutral.300` | `#C5CCD8` |
| `neutral.500` | `#737E8F` |
| `neutral.700` | `#3B4657` |
| `neutral.900` | `#172033` |

### Rules

- Roughly 70% white/neutral, 20% navy, 10% accent.
- Teal, sun, and coral fail 4.5:1 on white as text. Use them for fills, icons, and borders, and put text in navy or `neutral.900` on their tinted surfaces.
- No large multicolor gradient backgrounds.

## 6. Typography

Primary typeface: **Inter**.

```css
font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
```

Manrope is allowed for marketing display headings only if the extra font is worth the weight.

| Style | Size | Weight | Line height |
|---|---|---|---|
| Display | 64px | 700 | 1.05 |
| H1 | 48px | 700 | 1.10 |
| H2 | 36px | 700 | 1.15 |
| H3 | 28px | 650 | 1.20 |
| H4 | 22px | 650 | 1.25 |
| Body large | 18px | 400 | 1.60 |
| Body | 16px | 400 | 1.55 |
| Body small | 14px | 400 | 1.45 |
| Label | 13px | 600 | 1.30 |
| Caption | 12px | 500 | 1.35 |

Sentence case for headings and controls. All caps only for compact metadata labels. Use tabular figures (`font-variant-numeric: tabular-nums`) for all money and times.

## 7. Spacing

8px base grid: 4, 8, 12, 16, 24, 32, 48, 64, 96, 128.

Whitespace is a core characteristic. Do not solve hierarchy by adding borders everywhere.

## 8. Shape

### Radius

| Token | Value | Use |
|---|---|---|
| `radius.sm` | 6px | compact controls |
| `radius.md` | 10px | inputs |
| `radius.btn` | 12px | buttons |
| `radius.lg` | 16px | cards |
| `radius.xl` | 20px | large cards |
| `radius.2xl` | 28px | marketing panels |
| `radius.pill` | 999px | pills, avatars, chips |

### Borders and shadows

- Border: 1px `neutral.100`.
- Card shadow: `--oiq-shadow-card`. Floating panel shadow: `--oiq-shadow-float` (values in §39).

No dark card outlines. Use shadows sparingly.

## 9. Iconography

One rounded-outline set (Lucide). Sizes 18, 20, or 24px. Never mix filled, 3D, and outline styles.

| Category | Accent |
|---|---|
| Flights | blue |
| Hotels | teal |
| Meetings | navy |
| Restaurants | coral |
| Activities | orchid |
| Budget | teal |
| Warnings / stale data | sun |
| Conflicts / over budget | coral |

## 10. Illustration

Geometric and diagrammatic, an extension of the product UI. Preferred motifs: origin points on curved routes converging on a destination, stacked itinerary cards, calendar blocks, flight and hotel cards, simple city silhouettes.

Destination and venue photography is fine in selected places. Generic travel stock photography is not the visual system.

---

# Product UI

## 11. Roles shape every screen

What each role may see is set by SEC-AUTHZ-01 to -03 in SECURITY.md. Participant screens are designed only from the data the API returns for that role; never design a Participant view that hides Organizer data client-side.

## 12. Application shell

```text
┌──────────────────────────────────────────────────────────────┐
│ offsiteiq          Search                       Help   User  │
├───────────────┬──────────────────────────────────────────────┤
│ Trips         │                                              │
│ People        │                Main content                  │
│ Venues        │                                              │
│ Reports       │                                              │
│               │                                              │
│ Settings      │                                              │
└───────────────┴──────────────────────────────────────────────┘
```

Left nav is 240px expanded. Reports is not in v1 scope until a requirement exists (REQUIREMENTS §2.2). The current section uses `accent.sky` background with navy text. Participants see a reduced nav: My trips, My itinerary, Settings.

## 13. Organizer dashboard

Not in v1 scope until a requirement exists (REQUIREMENTS §2.2).

Answers four questions:

1. Which trips are active?
2. What needs attention (over-budget participants, itinerary conflicts, stale rates)?
3. Where does spend stand against each trip budget?
4. Who is traveling soon?

Top row: page title, date context, `Plan a trip` primary button. Then four summary cards (upcoming trips, participants, selected spend, items needing attention) and below them trip cards, budget bars, and an attention queue.

Do not show ten KPIs because the data exists.

## 14. New trip planner

The central workflow. A progressive stepper that follows the requirements:

```text
1. Trip        destination, dates          FR-TRIP-01, -02
2. Budget      trip + per-person, currency FR-TRIP-03..05
3. People      participants, origins       FR-PART-01..05
4. Flights     per participant             FR-SRCH-01, -05
5. Hotels      for the date range          FR-SRCH-02
6. Venues      meetings, food, activities  FR-VENUE-01..03
7. Itinerary   schedule + conflicts        FR-ITIN-01..03
8. Review
```

At every step the organizer can see what has been supplied, what is missing, what the system assumed, the running trip total, and the per-person cost.

### Trip inputs

- Destination uses a location search that resolves to a canonical place (city, region, country). Free text that cannot be resolved shows an inline error; never accept it silently (FR-TRIP-01).
- Date pickers block past start dates and end-before-start (FR-TRIP-02).
- Budget fields always pair an amount with a currency selector; there is no amount-only state (FR-TRIP-03, -04).
- If per-person budget × participants exceeds the trip budget, block progress with an explicit message. Show an override path only if FR-TRIP-05's override is confirmed, and require a written justification that is then shown on the review step.

### Persistent trip estimate

Desktop: sticky right panel. Mobile/tablet: collapsible bottom sheet.

```text
TRIP ESTIMATE                       USD

42 participants
Austin, TX · Oct 12–15

Flights                         $18,420.00
Hotels                          $24,800.00
──────────────────────────────────────────
Selected total                  $43,220.00
Trip budget                     $65,000.00
Remaining                       $21,780.00   ✓ Within budget

3 participants over per-person budget  →
Rates as of 14:02 local · FX rates dated Oct 1
```

- Venue costs are not in the trip total (FR-SRCH-06); show them separately, labeled "indicative".
- Converted amounts show the original currency on hover/focus and the dated FX rate used (FR-SRCH-04).

## 15. Participants and origins

Table first, map second. The table is authoritative; the map is a supporting view where origin nodes converge on the destination. No planning function is map-only.

Columns: participant, origin (airport or city only, FR-PART-05), included toggle, cheapest viable flight, per-person status, arrival.

Never show or ask for a street address. Import (CSV/HRIS) is shown as a disabled or hidden option until FR-PART-04 is resolved.

## 16. Flight options

Business travel, not bargain hunting. There is no default ranking: each option shows its agenda fit, stops, and price, and people choose (REQUIREMENTS FR-SRCH-10, FR-SRCH-12; Q29 withdrawn). Users can sort by a column and filter to nonstop. A cheaper red-eye never outranks a comparable daytime option.

```text
SFO → AUS                               $418.00 round trip
United 1572 · nonstop                   ✓ Within per-person budget
09:10 → 14:42 (3h 32m)
Source: Provider A · ref UA1572-XYZ · as of 14:02
```

Every option shows price, currency, provider, and provider reference (FR-SRCH-03), plus freshness (§31). Options that would push a participant over their per-person budget carry an "Over per-person budget by $X" chip (FR-SRCH-05). They are flagged, not hidden.

## 17. Hotel options

Cards show nightly rate, taxes and fees when the provider supplies them, total for the date range, distance to the main meeting venue, cancellation policy, and accessibility information. If taxes are unknown, say "Taxes not included" rather than implying a full price.

## 18. Venues

Meeting spaces: capacity, address, indicative cost, distance from hotel, accessibility. A capacity filter defaults to ≥ participant count (FR-VENUE-03).

Restaurants and activities: duration, capacity, distance, price per person, indoor/outdoor, accessibility. Photography is allowed here.

Do not design features that depend on rich provider markup; provider content is text only (SEC-INPUT-04).

## 19. Master itinerary

A clean agenda with a left time rail and content cards. Times are in the destination's local time zone, with the zone shown in the day header (FR-ITIN-04).

```text
TUESDAY, OCTOBER 13 · Central Time (CT)

08:00   Breakfast
        Hotel restaurant

09:00   Company meeting
        Harbor Room · Level 2

12:00   Lunch
        Catered onsite

18:00   Team dinner
        Restaurant name

⚠ 17:00 Strategy session starts before 2 participants arrive  →
```

Conflicts (overlaps, events before arrival or after departure) appear inline at the point in time where they occur and in a summary at the top (FR-ITIN-03). A version history panel shows who changed what and when (FR-ITIN-06). Export lives in the page header; formats depend on FR-ITIN-05.

## 20. Participant itinerary

Each participant sees their own flights and hotel plus shared events, derived from the same trip data as the master itinerary (FR-ITIN-02). Mobile is the primary surface for this view. Include an "Add to calendar" or export action when FR-ITIN-05 is settled.

Because v1 does not book, show selected options with their provider reference, not confirmation numbers.

## 21. Budget UX

Two budget concepts stay distinct and always labeled:

- **Trip budget:** maximum total for the trip.
- **Per-person budget:** maximum attributable to one participant.

Visualize with horizontal bars, not gauges:

```text
Trip budget
████████████████░░░░  $43,220 / $65,000   ✓ Within budget
```

Over-budget states use coral fill plus the words "Over by $X". The UI never shows floating-point artifacts, always shows two decimals in detailed views (FR-TRIP-06), and shows the same total for a trip in every view.

Category budgets (airfare, meals, and so on) are not in v1 requirements. Do not design for them yet.

## 22. Overrides and exceptions

The only v1 approval-like object is the budget override in FR-TRIP-05. Treat it as an explicit, recorded item:

```text
BUDGET OVERRIDE

Per-person budget × participants   $67,200.00
Trip budget                        $65,000.00
Difference                         +$2,200.00

Justification (required)
[ Two participants must fly from Singapore.       ]

Recorded by Jane Doe · Oct 1, 14:05
```

Trip states for v1: **Draft**, **Ready for review**, **Finalized**, **Archived**. No booking states.

---

# Marketing site

## 23. Homepage

The marketing site (§23 to §25) is not in v1 scope until a requirement exists (REQUIREMENTS §2.2).

Navigation: logo, Product, How it works, Security, Pricing, Sign in, `[Plan a company trip]`.

Hero:

```text
Company offsites without
the coordination chaos.

Bring everyone's home city, your budgets, flights, hotels,
and meeting spaces into one plan and one itinerary.

[Plan your first trip]   [See how it works]
```

Hero visual: an original illustration of origin cities converging on a single itinerary, built from the logo's node-and-route language.

Proof bar under the hero: "Plan the whole offsite in one place: People · Flights · Hotels · Venues · Itinerary". Replace with real customer logos or verified metrics only when they exist. Never invent customer counts or metrics.

## 24. Three pillars

| Pillar | Accent | Copy |
|---|---|---|
| Get everyone there | blue | Search travel from each participant's real home city. |
| Stay on budget | teal | Check every option against trip and per-person budgets. |
| One itinerary | orchid | Turn flights, hotels, and venues into one shared schedule. |

## 25. Product story sections

Alternate text with real product screenshots: plan the trip, coordinate distributed participants, compare real options, control the budget, build the itinerary, keep every participant informed.

A Security section is worth having: SSO via your identity provider, role-based access, minimal personal data. Make only claims SECURITY.md §3 guarantees.

---

# Components

## 26. Buttons

| Variant | Style |
|---|---|
| Primary | blue fill, white text, 12px radius, min height 44px |
| Secondary | white fill, `neutral.100` border, navy text |
| Tertiary | text or ghost |
| Destructive | coral, only for truly destructive actions (never for closing a dialog) |

## 27. Inputs

Height 44–48px. States: default, hover, focus, populated, disabled, error, read-only. Labels stay visible; placeholders are never labels.

Focus: `--oiq-focus` ring and a `brand.blue` border.

Money inputs are a paired amount + currency control.

## 28. Cards

Primary grouping mechanism. Generous padding, subtle border, optional soft shadow, clear title, one primary action. No nesting deeper than one level.

## 29. Tables

Used for participants, flight options, hotels, budgets, and itinerary history. Support sorting, filtering, column visibility, pagination or virtualization, keyboard navigation, and selection. Row height 48–56px. Use real `<table>` markup, not div grids.

## 30. Status chips

`Within budget` · `Over per-person budget` · `Over trip budget` · `Nonstop` · `Conflict` · `Stale rate` · `Draft` · `Finalized`

Every chip has text and color, and an icon where status is critical.

## 31. Data freshness

Every provider-derived number carries its source and fetch time (FR-SRCH-08). When a rate passes the staleness window (value `OPEN`), show a sun-colored "Stale rate" chip and a refresh action. Do not silently refresh numbers the organizer is looking at.

## 32. Empty states

Say exactly what to do.

> No participants have been added to this trip yet. Add them from the employee roster.
>
> `[Add participants]`

## 33. Loading

Skeletons for page and card loads. Long multi-provider searches show real progress:

```text
Searching travel options…

✓ Participant origins loaded
✓ 2 of 3 flight providers checked
• Checking hotel availability
```

No indefinite spinner for work that may take many seconds.

## 34. Errors

Preserve the user's work and say: what failed, what is still valid, whether retry is safe, and what to do next.

> Hotel availability could not be refreshed. Your selections are unchanged. Retry, or continue with the rates retrieved at 14:02.

Error content limits: SECURITY.md §5.7. Do not show internal identifiers.

---

# Responsive

## 35. Breakpoints

`sm 640` · `md 768` · `lg 1024` · `xl 1280` · `2xl 1536`

- **Desktop:** primary planning environment for organizers.
- **Tablet:** review, itinerary edits, participant lookup.
- **Mobile:** participant-first. Itinerary, flight and hotel details, event schedule, directions. Do not reproduce every planning table on mobile.

# Accessibility

## 36. Standard

Target **WCAG 2.2 AA**: 4.5:1 text contrast, 3:1 for large text and key graphics, full keyboard access, visible focus, semantic headings, correct labels, accessible dialogs, skip link, reduced motion support, minimum target size, and no color-only status.

# Motion

## 37. Animation

Motion communicates state: 120–180ms for controls, 180–240ms for cards and panels, an optional route-drawing effect in marketing illustrations. Honor `prefers-reduced-motion: reduce`. No parallax or decorative entrance animations.

# Content

## 38. Voice

Concise, calm, specific, operational, transparent.

| Prefer | Avoid |
|---|---|
| 3 participants exceed the $600 per-person budget. | Oops! A few folks are over budget! |
| Hotel rates were retrieved 18 minutes ago. | We found amazing hotel deals! |
| Selected | Booked |

# Tokens

## 39. CSS variables

```css
:root {
  --oiq-navy: #172A46;
  --oiq-blue: #3978F6;
  --oiq-teal: #19A88C;
  --oiq-orchid: #805AD5;

  --oiq-sky: #DDEBFF;
  --oiq-mint: #DDF5EE;
  --oiq-lilac: #ECE6FA;
  --oiq-sun: #F8C75A;
  --oiq-coral: #E96A67;
  --oiq-coral-tint: #FBE1E0;
  --oiq-sun-tint: #FDF1D3;
  --oiq-sun-dark: #B7861A;

  --oiq-neutral-0: #FFFFFF;
  --oiq-neutral-25: #FBFCFE;
  --oiq-neutral-50: #F5F7FA;
  --oiq-neutral-100: #E9EDF3;
  --oiq-neutral-300: #C5CCD8;
  --oiq-neutral-500: #737E8F;
  --oiq-neutral-700: #3B4657;
  --oiq-neutral-900: #172033;

  --oiq-radius-sm: 6px;
  --oiq-radius-md: 10px;
  --oiq-radius-btn: 12px;
  --oiq-radius-lg: 16px;
  --oiq-radius-xl: 20px;
  --oiq-radius-2xl: 28px;
  --oiq-radius-pill: 999px;

  --oiq-shadow-card: 0 1px 2px rgba(23, 32, 51, 0.04), 0 8px 24px rgba(23, 32, 51, 0.06);
  --oiq-shadow-float: 0 12px 36px rgba(23, 32, 51, 0.12);
  --oiq-focus: 0 0 0 3px rgba(57, 120, 246, 0.22);
}
```

# Implementation rules

## 40. Constraints for engineering

1. Build reusable primitives before page-specific variants.
2. Use tokens, never hard-coded colors.
3. Semantic HTML first.
4. Keep forms keyboard accessible and preserve form state across steps.
5. Prefer progressive disclosure over giant forms.

# First screens

## 41. Build order

1. Sign-in (OIDC redirect, no password fields)
2. Organizer dashboard
3. New trip: trip and budget steps
4. Participants and origins
5. Flight options
6. Hotel options
7. Venues
8. Master itinerary with conflicts
9. Review and budget override
10. Participant itinerary (mobile)
11. Marketing homepage
