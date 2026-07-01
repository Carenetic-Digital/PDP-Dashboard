# PDP Website Dashboard — AI Site Builder Handoff

**Project:** Premier Dentist Partners (PDP) — Internal Website Tracker Dashboard  
**Delivered by:** Care Netic  
**Built with:** Astro (static site)  
**Audience:** PDP internal team — non-technical, browser-only access

---

## 1. Project Summary

Care Netic is building individual websites for each dental practice under Premier Dentist Partners. Because the Astro sites do not use a networked CMS like WordPress, PDP needs a simple internal dashboard to track the status of every practice website in one place.

This dashboard is a single Astro page. It gives PDP's team instant visibility into each practice, where it is in the build process, and easy access to the relevant links — whether that's a staging environment and QA tool during the build phase, or the live site and content management system after launch.

---

## 2. Tech Stack

- **Framework:** Astro (static site generation)
- **Data source:** A single structured data file in the project (JSON or YAML — builder's choice) that the AI Site Builder tool manages. No external CMS or database.
- **Hosting:** Builder's choice (Netlify, Vercel, Cloudflare Pages, etc.)
- **Auth:** None — publicly accessible via URL

---

## 3. Single Page: Dashboard

The entire experience is one page. There are no subpages or routing requirements.

### 3.1 Page Layout (top to bottom)

1. **Header** — Logo + page title
2. **Search & Filter bar** — Text search + status filter dropdown
3. **Practice table** — One row per practice
4. **Footer** — Care Netic attribution + last-updated date

---

## 4. Header

- Display the **Premier Dentist Partners logo** (SVG, left-aligned)
- Page title text: `"Practice Website Tracker"`
- Subtitle text: `"Managed by Care Netic"`
- Background: Dark navy (`#1A3A52`)
- Text and logo: White

---

## 5. Search & Filter Bar

Positioned directly below the header, above the table. Sticky on scroll so it stays visible when the list is long.

### Text Search
- A single text input field
- Placeholder: `"Search practices..."`
- Filters the table in real time by **practice name** (case-insensitive, partial match)

### Status Filter
- A dropdown or segmented button group with four options:
  - `All` (default)
  - `In Build`
  - `Live`
  - `On Hold`
- Selecting a status filters the table to show only practices with that status
- Search and status filter work together (both can be active at once)

---

## 6. Practice Table

### Columns

| Column | Description |
|---|---|
| **Practice Name** | Full name of the dental practice |
| **Status** | A colored badge (see §7) |
| **Live Site** | Link to the publicly live website — shown only when status is `Live` |
| **Staging URL** | Link to the staging/preview environment — shown only when status is `In Build` |
| **BugHerd** | Link to the BugHerd QA board for this project — shown only when status is `In Build` |
| **CMS** | Link to the Care Netic custom CMS for this practice — shown only when status is `Live` |

### Display rules by status

| Status | Live Site | Staging URL | BugHerd | CMS |
|---|---|---|---|---|
| `In Build` | — | ✅ Show | ✅ Show (if URL provided) | — |
| `Live` | ✅ Show | — | — | ✅ Show (if URL provided) |
| `On Hold` | Show if URL exists | Show if URL exists | — | — |

When a link is not applicable to the current status, the table cell should be empty (no placeholder text, no broken icon).

### Link display
- Each link renders as a short labeled button or pill, not a raw URL
- Labels: `"View Site"`, `"Staging"`, `"BugHerd"`, `"CMS"`
- Links open in a new tab (`target="_blank"`)

### Empty state
- If the search/filter returns no results, display a centered message: `"No practices match your search."`

### Sorting
- Default sort: alphabetical by practice name (A–Z)
- Column headers for **Practice Name** and **Status** are clickable to toggle sort ascending/descending

---

## 7. Status Badges

Each practice row displays a single status badge. The badge is a small pill/tag with a colored background.

| Status | Badge color | Badge text |
|---|---|---|
| `In Build` | Teal / `#3EB489` | `In Build` |
| `Live` | Dark navy / `#1A3A52` | `Live` |
| `On Hold` | Muted gray / `#8A9BAD` | `On Hold` |

Text on all badges: White, small, semi-bold.

---

## 8. Data Schema

Each practice is a record in the data file. All fields except `name` and `status` are optional — if a URL is not yet known, the field can be omitted or set to an empty string, and the corresponding link simply won't render.

```json
{
  "practices": [
    {
      "name": "Algonquin Dental Associates",
      "status": "live",
      "liveSiteUrl": "https://algonquindental.com",
      "stagingUrl": "",
      "bugherdUrl": "",
      "cmsUrl": "https://cms.carenetic.com/algonquin"
    },
    {
      "name": "Lakewood Family Dentistry",
      "status": "building",
      "liveSiteUrl": "",
      "stagingUrl": "https://staging.lakewoodfamilydentistry.com",
      "bugherdUrl": "https://app.bugherd.com/projects/XXXX",
      "cmsUrl": ""
    },
    {
      "name": "Northview Dental",
      "status": "on_hold",
      "liveSiteUrl": "",
      "stagingUrl": "",
      "bugherdUrl": "",
      "cmsUrl": ""
    }
  ]
}
```

**Valid status values:** `"live"`, `"building"`, `"on_hold"`

---

## 9. Brand Guidelines

### Color Palette (sourced from premierdentistpartners.com)

| Role | Hex | Usage |
|---|---|---|
| Primary Navy | `#1A3A52` | Header background, footer, Live badge, primary text on dark |
| Deep Navy | `#122A3E` | Hero/accent backgrounds if needed |
| Teal / Green | `#3EB489` | CTA buttons, In Build badge, interactive highlights |
| Teal Accent | `#2EC4A0` | Hover states, secondary highlights |
| Muted Blue-Gray | `#5A6F82` | Subtext, muted labels |
| Dark Text | `#2C3E50` | Body text on white backgrounds |
| On Hold Gray | `#8A9BAD` | On Hold badge background |
| White | `#FFFFFF` | Page background, text on dark backgrounds |

### Typography
- Use a clean sans-serif system font stack or match PDP's web font if available
- Suggested: `font-family: 'Inter', system-ui, -apple-system, sans-serif`
- Heading (page title): 24–28px, bold, white on dark
- Table header row: 12–13px, uppercase, letter-spaced, muted color
- Table body: 14–15px, regular weight
- Badge text: 11–12px, semi-bold

### Logo
- Source: `https://premierdentistpartners.com/wp-content/uploads/2022/12/Group-84508302.svg`
- Use the light/white version of the logo in the header (dark background)
- Care Netic may need to supply the logo file directly if the hosted SVG is not embeddable

---

## 10. Footer

- Left: Care Netic wordmark or small attribution — `"Built by Care Netic"`
- Right: `"Last updated: [date]"` — static, manually updated when the data file is edited
- Background: Deep Navy (`#122A3E`)
- Text: Muted Blue-Gray (`#5A6F82`)

---

## 11. Responsive Behavior

- **Desktop (≥1024px):** Full table layout as described
- **Tablet (768–1023px):** Table remains but may allow horizontal scroll if columns don't fit
- **Mobile (<768px):** Table collapses to a card-per-practice layout. Each card shows the practice name, status badge, and only the relevant links for that status.

---

## 12. Sample / Seed Data

Pre-populate the data file with the following placeholder rows so the dashboard renders something meaningful from the start. Care Netic will replace these with real practice records before delivery.

```json
{
  "practices": [
    {
      "name": "Algonquin Dental Associates",
      "status": "live",
      "liveSiteUrl": "https://example-live.com",
      "stagingUrl": "",
      "bugherdUrl": "",
      "cmsUrl": "https://cms.carenetic.com/example"
    },
    {
      "name": "Lakeshore Smile Studio",
      "status": "building",
      "liveSiteUrl": "",
      "stagingUrl": "https://staging.example-dental.com",
      "bugherdUrl": "https://app.bugherd.com/projects/00000",
      "cmsUrl": ""
    },
    {
      "name": "Northview Dental",
      "status": "building",
      "liveSiteUrl": "",
      "stagingUrl": "https://staging.northviewdental.com",
      "bugherdUrl": "",
      "cmsUrl": ""
    },
    {
      "name": "Riverpark Family Dentistry",
      "status": "on_hold",
      "liveSiteUrl": "",
      "stagingUrl": "",
      "bugherdUrl": "",
      "cmsUrl": ""
    },
    {
      "name": "West End Orthodontics",
      "status": "live",
      "liveSiteUrl": "https://example-ortho.com",
      "stagingUrl": "",
      "bugherdUrl": "",
      "cmsUrl": "https://cms.carenetic.com/westend"
    }
  ]
}
```

---

## 13. Open Items / Inputs Still Needed

Before the build is complete, Care Netic needs to supply the following:

- [ ] **PDP logo file** — white/light version of the logo as an SVG or PNG, to embed in the header (do not hotlink from their WordPress site)
- [ ] **CMS URL pattern** — confirm the base URL structure for Care Netic's custom CMS (e.g., `https://cms.carenetic.com/{practice-slug}`) so placeholder links can be validated
- [ ] **Hosting destination** — where will this dashboard be deployed? (Netlify, Vercel, Cloudflare Pages, client-hosted, etc.)
- [ ] **Dashboard URL** — what domain or subdomain will this live on? (e.g., `tracker.carenetic.com` or `sites.premierdentistpartners.com`)
- [ ] **Full practice list** — the complete list of PDP practices with their names and initial statuses, to populate the data file before delivery

---

## 14. Out of Scope

The following are explicitly **not** part of this build:

- User authentication or password protection
- Any form of editing the data from within the dashboard UI (all updates go through the AI Site Builder tool or the data file directly)
- Email notifications or status change alerts
- Integration with any external project management tool (Asana, Notion, etc.)
- Automatic status sync with BugHerd or the CMS

---

*Document prepared by Care Netic — June 2026*
