# Handoff: Figma design work (Horizon ONE, HorizonX Purchase, Communication Tool)

Paste this whole file into a new Claude session. It has everything needed to continue.

## Who you are working with
- The user is a UI/UX designer (the client calls her Ammara). She writes in **Roman Urdu**. Reply in Roman Urdu; anything written for the client is in **English**.
- She expects senior UX/UI quality: consistent spacing, typography, colors, alignment and working prototypes. Check every flow screen by screen and miss nothing.
- She does not want repeated permission prompts. In a cloud session she must pick **"Auto"** in the mode dropdown next to the prompt box. A `.claude/settings.json` allowing the Figma tools is committed on branch `claude/gallant-edison-gvh0ws`.
- Never use browser automation through the proxy. It was denied before.
- She once shared eSales credentials in chat. They were advised to change the password. Never ask for or reuse them.

## Tools and how to work
- Figma MCP: `mcp__Figma__use_figma` (Plugin API JS, skillNames `resource:figma-use`) and `mcp__Figma__get_screenshot`.
- Use `enableBase64Response: true` on screenshots, because downloading figma.com URLs is blocked by the proxy.
- Plugin API notes:
  - Call `await figma.setCurrentPageAsync(page)` once per call. Use `page.loadAsync()` to read another page.
  - Load fonts before text edits. Inter styles are `Regular`, `Medium`, `Semi Bold`, `Bold`. Satoshi has no Semi Bold; use `Bold`.
  - A thrown error rolls back the whole call. The return value must stay under 20 KB; the code under 50 KB.
  - `layoutPositioning='ABSOLUTE'` only works inside an auto-layout parent.
  - Prototype link shape: `{trigger:{type:'ON_CLICK'},actions:[{type:'NODE',destinationId,navigation:'NAVIGATE',transition:{type:'DISSOLVE',easing:{type:'EASE_OUT'},duration:0.2},preserveScrollPosition:false,resetVideoPosition:false}]}`. Set it with `await node.setReactionsAsync([...])`.
  - Cloning a flow-start frame duplicates the flow. Save `page.flowStartingPoints` before cloning and restore it after.
  - Remote library components (for example the Profile avatar) are read-only. Scale the instance with `rescale()` instead.

---

## 1. Horizon ONE: file `jsY9fRVRiyqIXgIKXeV97b` (main active project)
- **Pages:**
  - English `0:1`
  - German `131:33363` ("Horizon ONE — Phase 1 German")
  - Components page "Componenet"
- **Layout:** sections are frame groups. Frame names start with a number (`07.1 Order Overview — Default`). Frames are 1728x1117.
- **Phase 1 (screens 01–10, UX doc §75):** complete.
- **Phase 2 (§76):** sections 11–22 built on both pages:
  - 11 Pricing, 12 Sales Channels, 13 Fulfillment, 14 Returns, 15 Settlement, 16 Product Matching
  - 17 Master Content, 18 Customers (marked "TO CONFIRM"; in neither phase), 19 Inventory
  - 20 Reporting, 21 Settings, 22 Merchant Area
- **Status doc:** "Horizon | ONE – Phase 1 & 2 Status", https://claude.ai/code/artifact/50082a4f-6fff-4c33-a9be-a501a4bfc0eb

### Source document (ask the user to attach it again in the new chat)
- **"Horizon | ONE UX-Designkonzept für die Softwareoberfläche V1.0"**: the client's UX concept doc, written in German. "§" means section number.
- Every screen note cites it, for example "Based on: UX §31 Orders · §32".
- **§75, Phase 1:** the 10 reference screens:
  1. Horizon Dashboard
  2. Merchant Overview
  3. Merchant 360°
  4. Product Overview
  5. Product 360°
  6. Merchant Offers & Inventory
  7. Order Overview
  8. Order 360°
  9. Exception Center
  10. Merchant Dashboard
- **§76, Phase 2:**
  - Pricing (§26–27), Sales Channels (§29–30), Fulfillment (§38–39), Returns (§43–44)
  - Settlement (§45–46), Product Matching, Master Content (§21–22), Inventory (§25)
  - Reporting (§48), Settings and Roles (§58, §76)
  - Merchant Area: Products, Orders, Settlement (§52–56)
- **Customers (§47, §80)** is in neither phase. It is built and marked "TO CONFIRM".
- **Other sections used:**
  - §13: Merchant 360° tabs
  - §61: side panels
  - §63: structure of a 360° view
  - §69: search and filter
  - §77: exception states for important areas
- The original doc file is not in the repo. **The user must re-attach the PDF/DOCX** if detailed requirements are needed.

### Screen and flow inventory: English page (the German page is identical; 89 screens each)
| # | Area | Screens | Frames |
|---|---|---|---|
| 01 | Horizon Dashboard | 3 | Default, Exception, Empty |
| 02 | Merchant Overview | 3 | Default, Exception, Empty |
| 03 | Merchant 360° | 12 | Default, Exception, Empty/Onboarding, plus tabs Products & Offers, Orders & Fulfillment, Settlement, Performance, Integrations, History, Inventory, Returns, Settings |
| 04 | Product Overview | 4 | Default, Merchant-sources popover, Empty, Exception |
| 05 | Product 360° | 18 | Default, Overview, Overview with product flow, Merchant Products, Offers, Inventory, Data Quality, Activity, Loading, Empty, Exception (sync alert), Master Content, Pricing, Variants, Channels, Orders, Returns, History |
| 06 | Merchant Offers & Inventory | 3 | Default, Exception, Empty |
| 07 | Order Overview | 4 | Default, Multi-merchant popover, Empty, Exception |
| 08 | Order 360° | 10 | Default, Routing & Allocation, Change Merchant panel, Items, Activity, Exception, Fulfillment, Shipment, Returns, Settlement |
| 09 | Exception Center | 4 | Default, Detail side panel, Bulk actions, Empty |
| 10 | Merchant Dashboard | 4 | Default, Detail panel, Exception, Empty |
| 11 | Pricing (P2) | 2 | List, Price Calculation panel |
| 12 | Sales Channels (P2) | 2 | List, Channel 360° (Zalando DE) |
| 13 | Fulfillment (P2) | 2 | Queue, Add Tracking panel |
| 14 | Returns (P2) | 2 | List, Return 360° (RET-20413) |
| 15 | Settlement (P2) | 2 | List, Settlement 360° (Apex Electronics) |
| 16 | Product Matching (P2) | 2 | Queue, Compare & Confirm panel |
| 17 | Master Content (P2) | 2 | Content sources, Select Content Source panel |
| 18 | Customers (to confirm) | 2 | List, Customer 360° (Jia Chan) |
| 19 | Inventory (P2) | 1 | List (rows open Product 360° Inventory tab) |
| 20 | Reporting (P2) | 3 | Management, Operations, Finance |
| 21 | Settings (P2) | 1 | Users & Roles (tabs: General, Users & Roles, Routing Rules, Pricing Rules, Integrations, Notifications) |
| 22 | Merchant Area (P2) | 3 | Merchant Products, Merchant Orders, Merchant Settlement |

**Totals:**
- Phase 1: 65 screens. Phase 2: 24 screens. Together: 89 screens on each page, 178 across both pages.
- **18 flows on each page:**
  - 01 Dashboard, 02 Merchants, 03 Products, 04 Orders, 05 Exception Center, 06 Merchant Area
  - 11 Pricing, 12 Sales Channels, 13 Fulfillment, 14 Returns, 15 Settlement, 16 Product Matching
  - 17 Master Content, 18 Customers, 19 Inventory, 20 Reporting, 21 Settings, 22 Merchant Area (Phase 2)
- Every screen has a "Note / Screen" below it (Why, What, Benefit, Concept §, Based on §). Every section has a "Note / Section" (click-through and open points).

### Known gaps / possible next work (Horizon ONE)
- **Phase 2 is designed at "key screen" depth: list plus one detail or panel.**
  - The 360° drawers for 12.2, 14.2, 15.2 and 18.2 have only the Overview tab designed; their other tabs have no screens.
  - Settings has only Users & Roles (other tabs not designed).
  - Inventory and Settings have no empty or exception states.
- **Phase 2 has no empty or exception states.** Phase 1 has them for every area.
- **Customers needs client confirmation** (it is in neither phase).
- **Activity (05.7) and History (05.18)** overlap in content.

### Shell components
- **Navbar:** component set `1:7861` (Variation=First and Variation=two).
  - Header height is **64px**.
  - The logo rectangle "3 1" is 192x28.5, with paddingLeft 24 so it aligns with the sidebar icons.
  - Search field is 40px high.
  - Icon buttons are 40x40 with **24px icons**.
  - The Profile instance is scaled so the avatar is 40px.
- **Shell / Page Header:** component `135:806`.
  - The Title and Breadcrumb are hidden.
  - The Sub-tabs bar is white with a 1px bottom border `#EAECF0`.
  - Title Block padding is matched to the content padding.
- **Page CTAs:** a frame named **"Page Actions"**, positioned ABSOLUTE inside the Main Content Area. It sits at the right of the Sub-tabs row, vertically centered.
  - Contents: Import/Add Product, Last updated + Export + Refresh, or Add Merchant.
  - There are 68 of them on each page.
- **Settings 21.1:** the extra tab "Tab 6 · Notifications" is absolutely positioned next to Tab 5.
- **Dashboard 01.x:** the Shell header is hidden because there are no tabs. The merchant "Dashboard header" (greeting) is hidden on 10.x and 22.x.

### Tokens
- Blue `#0A31FF`, ink `#101828`, body `#344054`, muted `#667085`, line `#EAECF0`.
- Green `#12B76A`, amber `#F59E0B`, red `#EF4444`.
- Active tab: blue text, Bold/Semi Bold, 2px blue bottom border.

### Prototype state (audited clean on 12 Nov: 0 dead links, no cross-flow wrong links)
- **Lists open their detail screen:**

  | List | Opens |
  |---|---|
  | 02 | 03.1 |
  | 04 | 05.1 |
  | 07 | 08.1 |
  | 11.1 | 11.2 |
  | 12.1 | 12.2 |
  | 13.1 | 13.2 |
  | 14.1 | 14.2 |
  | 15.1 | 15.2 |
  | 16.1 | 16.2 |
  | 18.1 | 18.2 |
  | 19.1 rows | 05.5 |

  Every row is clickable.
- **Drawers and side panels:** Backdrop and Close return to their own list.
- **360° drawer tabs:** in 12.2, 14.2, 15.2 and 18.2 the tabs have no links (only Overview is designed). They previously pointed wrongly to Order 360.
- **Product 360° tabs (05.x, 17.x):** one consistent set of 13 tabs, each linking to its screen:
  - Overview 05.11, Master Content 05.12, Variants 05.14, Merchant Products 05.3, Offers 05.4, Inventory 05.5, Pricing 05.13
  - Channels 05.15, Orders 05.16, Returns 05.17, Data Quality 05.6, Activity 05.7, History 05.18
  - German labels: Übersicht, Master-Content, Varianten, Händlerprodukte, Angebote, Bestand, Preise, Kanäle, Bestellungen, Retouren, Datenqualität, Aktivität, Verlauf.
- **Backgrounds:** behind the 05.x, 17.x and 08.x drawers is a clone of the current list screen, named "Background Overview Screen".
- **Reporting:** the 20.x tabs Management, Operations and Finance link to each other.
- **Flows:** 18 flows on each page, starting at 01.1, 02.1, 04.1, 07.1, 09.1, 10.1, 11.1 through 22.1. German flow names are in German.

### German page
- Fully translated: number format `12.481`, `249,00 €`, "vor 2 Std.".
- Product, merchant and person names, IDs and emails stay in English.
- Layer and frame names stay in English.

### Open items to mention to the user
- Activity (05.7) and History (05.18) overlap in content; the client may want only one.

---

## 2. HorizonX Purchase Platform: file `uNusjz5cOO52mcHnMQpfk5` (PAUSED by the user)
- **Work on page "Concept Screens – EN" `65:5335` only.** The page "Concept Screens" `0:1` is the original.
- **Client email (deadline was Wed 11 Nov, noon; Europcar presentation Friday):**
  1. Add a **Europcar car rental** example: images, duration, price, conditions, Europcar logo. It shows that HorizonX supports bookable services.
  2. Add **2–3 tyre offers** with supplier and price comparison. Use europcar.de and reifen.auto.check24.de as inspiration only.
  3. Do a full **UI consistency review**: spacing, type, colors, buttons, cards, layout.
  4. Provide a **full-screen presentation link** for the 4 journeys, with no Figma UI (`hide-ui=1`, starting point per flow).
- **Already exists:** a previous Claude added Tyres (5 screens, section `76:11587`) and Europcar (4 screens, section `77:11026`), plus flows "EN Demo 5/6".
- **Done in this session:**
  - Frames that overflowed 1024px now hug their content (min 1024) and have a sticky header.
  - The header logo image was restored on 24 header instances.
  - Colors were normalized (purple to brand blue `#0A31FF`, unified greys and borders).
  - The sidebar active state is consistent per journey.
- **Not done yet:**
  - Polish the Europcar and Tyres screens (Airbnb/Pinterest-quality image cards). The sidebar shows "Home" on the search screens.
  - Replace the Europcar logo placeholder `77:11027`.
  - The rest of the spacing and type review. A German word "Aktion" appears on the EN page.
  - Verify the presentation link and flows.
- The user said "stop" on this file. **Ask before continuing.**

## 3. Communication Tool (Visticle): file `zCEigbJArmI0UANlxMoT5Q`
- **Pages:** Information Architecture, Structure, Components, UI Screens English, UI Screens German.
- **Status:** done. This covers the sidebar merge, Resolved tab, AI tone/suggestions, Delivery Note CTA and PDF popup, notes, Stage 1 doc review and German translation.
- **Source docs:** two eSales "Stage 1" PDFs that the user shared earlier. They are not in the repo; the user must re-attach them if needed. The Stage 1 doc review is published at https://claude.ai/code/artifact/46d5baed-3a61-41ca-9b7e-ca04668c92f1.
- The user asked about an **"Article Management"** feature for Visticle. There is no record of it in this session. Ask for the file or link.

## Repo
- `/home/user/Claude-Code`, branch `claude/gallant-edison-gvh0ws`.
- The repo only holds `.claude/settings.json` and this handoff. All design work lives in Figma.
