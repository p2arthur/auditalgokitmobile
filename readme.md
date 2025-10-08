# AlgoKit Lora — Mobile Responsiveness Audit (v0.1)

## 1. Scope and Goals

### Scope
- Layout shell (header, topbar, sidebar, main content, footer)
- Navigation (primary, secondary, search)
- Data display (tables, grids, description lists)
- Forms and inputs
- Overlays (modals, popovers, drawers, dropdowns)
- Typography, spacing, touch targets, and interaction feedback
- Tailwind v4 readiness and migration implications

### Goals
- Ensure the application is fully usable and visually consistent across mobile, tablet, and desktop devices.  
- Remove visual overflow, cramped elements, and poor spacing across small screens.  
- Unify responsive spacing and typography across all breakpoints.  
- Introduce a mobile-first design approach compatible with Tailwind v4’s container queries and theming features.  

### Recommended Breakpoints
- **Mobile:** <640px  
- **Tablet:** 640–1024px  
- **Desktop:** >1024px  
- **XL Desktop:** >1280px (optional, for wide tables or development tools)

### Global Spacing Defaults
- **Mobile:** padding 2  
- **Tablet+:** padding 4  
This ensures content remains readable without excessive margins.

---

## 2. High-Priority Findings

### 1. Header/Nabber Cramping
On smaller screens, elements like the logo, search bar, network selector, and wallet connection button become too cramped or overflow.  
**Recommendation:** Introduce a collapsible navigation drawer triggered by a hamburger menu. Move search, settings, theme selector, and navigation links into this drawer for mobile users.  

### 2. Sidebar Space Usage
The sidebar remains visible on all screen sizes, taking excessive space on mobile.  
**Recommendation:** Hide the sidebar on mobile and tablet, keeping it for desktop only. Use the new collapsible menu as the mobile alternative.  

### 3. Hidden Search on Mobile
The search functionality disappears entirely on smaller viewports.  
**Recommendation:** Relocate search into the collapsible navigation drawer. Provide an accessible icon button in the header to trigger the drawer and focus the search input automatically.  

### 4. Unresponsive Tables
Data tables overflow horizontally with no visual indicators.  
**Recommendation:** Implement mobile-friendly display patterns—either stacked “card” layouts or scrollable tables with visual cues. Consider allowing column visibility toggles by breakpoint.  

### 5. Grid Layouts
Multi-column grids do not adjust gracefully on mobile, causing visual imbalance.  
**Recommendation:** Reduce to a single-column layout on smaller screens and adjust spacing.  

### 6. Description Lists
Two-column layouts become cramped, squeezing text into unreadable widths.  
**Recommendation:** Stack label-value pairs vertically on mobile and use smaller typography.  

### 7. Touch Targets and Interaction
Buttons and form inputs may not meet accessibility standards for touch.  
**Recommendation:** Ensure a minimum target size of 44x44px. Increase padding for mobile elements.  

### 8. Container Padding
Fixed large paddings are used throughout the layout.  
**Recommendation:** Use responsive padding utilities (e.g., smaller padding on mobile) to maximize available space.  

### 9. Typography Scaling
Heading sizes and text spacing optimized for desktop appear too large on mobile.  
**Recommendation:** Reduce font sizes and increase line heights for better readability on smaller screens.  

### 10. Missing Touch Feedback
Many interactive elements lack tactile feedback for touch interactions.  
**Recommendation:** Introduce visual/tactile states (e.g., opacity changes or highlights) for touch interactions.  

### 11. Modal and Overlay Testing
Popovers, dropdowns, and modals have not been fully tested on mobile devices.  
**Recommendation:** Test and redesign modals to adapt as full-width sheets on smaller screens.  

### 12. Scroll Indicators
Horizontal scroll areas lack cues to indicate more content exists offscreen.  
**Recommendation:** Add gradient or shadow-based scroll indicators to guide user navigation.  

### 13. Column Visibility Controls
All table columns render by default, overwhelming small screens.  
**Recommendation:** Introduce conditional rendering for columns based on screen size or user selection.  

### 14. Vertical Spacing
Large gaps optimized for desktop make mobile layouts excessively tall.  
**Recommendation:** Adjust gap utilities per breakpoint to maintain balance and reduce unnecessary scroll length.  

---

## 3. Recommended Information Architecture (Mobile-First)

### Mobile and Tablet Navigation
- Replace the persistent sidebar with a collapsible drawer accessed via a hamburger menu.  
- Move search, navigation links, theme selector, and settings into the drawer.  
- Keep the header minimal with a compact logo, screen title, and essential icons (wallet and network).  

### Desktop Navigation
- Maintain the sidebar with its current layout and navigation hierarchy.  
- Keep main content centered and padded appropriately.  

### Spacing and Padding
- Default container padding should scale with breakpoints:
  - `p-2` on mobile  
  - `p-4` on tablet and above  

### Typography
- Use smaller heading scales and increased line height on mobile.  
- Maintain clear visual hierarchy through consistent font sizing and weight.  

---

## 4. Component Audit Summary

| Area | Current Issue | Recommended Fix | Effort | Priority |
|------|----------------|------------------|---------|-----------|
| Topbar | Elements overlap on small screens | Introduce collapsible navigation drawer | Medium | P0 |
| Sidebar | Visible on mobile | Hide on mobile; use drawer instead | Small | P0 |
| Search | Hidden on small screens | Relocate into drawer | Small | P0 |
| Tables | Overflow horizontally | Implement card layouts or scroll indicators | Large | P0 |
| Grids | Multi-column cramped | Collapse to single column | Small | P1 |
| Description Lists | Two-column too tight | Stack vertically | Small | P1 |
| Forms | Small touch areas | Increase target sizes and spacing | Medium | P1 |
| Containers | Padding too large | Use responsive padding | Small | P1 |
| Typography | Oversized on mobile | Adjust font scaling | Small | P2 |
| Modals/Popovers | Not optimized | Use full-width mobile sheets | Medium | P2 |
| Scroll Indicators | Missing | Add shadows or gradients | Small | P2 |
| Column Visibility | All columns shown | Add breakpoint-based column controls | Medium | P2 |
| Vertical Spacing | Excessive | Reduce gaps per breakpoint | Extra Small | P2 |

---

## 5. Implementation Order (Gradual Plan)

1. **Shell and Layout (P0):** Build the responsive header, collapsible drawer, and hide sidebar on mobile.  
2. **Search Functionality (P0):** Relocate search and ensure accessibility and focus behavior.  
3. **Data Tables (P0):** Redesign for mobile readability and scrolling behavior.  
4. **Spacing and Typography (P1):** Standardize padding and font scaling.  
5. **Forms and Touch Targets (P1):** Enhance usability for touch devices.  
6. **Grid and Description Lists (P1):** Refactor layouts for smaller screens.  
7. **Overlays (P2):** Redesign modals, drawers, and popovers for mobile.  
8. **Column Visibility and Scroll Indicators (P2):** Add visual cues and column toggles.

This order allows for iterative rollout while maintaining stability and test coverage.

---

## 6. Tailwind v4 Migration Context

Upgrading from **Tailwind 3.4.1** to **Tailwind 4.0** offers major speed and DX improvements:
- Up to 5× faster build performance.  
- Automatic content detection (no manual file paths).  
- Native CSS variable theming.  
- Support for container queries, 3D transforms, and modern color functions.  
- Adoption of newer CSS features such as cascade layers and `@property`.  

However, migration requires reviewing configuration and plugin compatibility.  
Projects depending on older browsers or legacy utilities must test and adjust accordingly.

### Migration Phase
1. **Upgrade and Toolchain**
   - Run: `npx @tailwindcss/upgrade@latest`
   - Install: `@tailwindcss/postcss` and `@tailwindcss/cli`
2. **Config Review**
   - Update theme tokens to use CSS variables.
3. **Audit Utilities**
   - Replace deprecated utilities and confirm plugin compatibility.
4. **Validation**
   - Perform visual regression testing across mobile, tablet, and desktop.

---

## 7. Screenshot Placeholders

Use these areas to insert annotated images showing specific layout issues:

- **Screenshot A – Navbar on Mobile:**  
  _Insert image here. Describe element overlap and cramped layout._

- **Screenshot B – Sidebar Overflow:**  
  _Insert image here. Note how sidebar pushes content offscreen._

- **Screenshot C – Table Overflow:**  
  _Insert image here. Highlight missing scroll cues and readability issues._

---

## 8. Acceptance Criteria

- Inventory of all components requiring mobile optimization.  
- Specific solutions documented for each responsive issue.  
- Recommended breakpoint strategy and spacing guidelines.  
- Estimated effort and prioritization for implementation.  
- Implementation order for gradual rollout.  
- Tailwind v4 migration guidance and testing plan.  

---

## 9. Next Steps

1. Validate the audit findings with screenshots and confirm affected components.  
2. Approve proposed navigation restructuring and layout strategy.  
3. Begin development of the new responsive layout (drawer, search relocation).  
4. Schedule Tailwind v4 migration after core layout fixes are stable.  
5. Establish QA sessions for testing responsiveness on major mobile devices.

---

**Prepared for:** AlgoKit Lora Team  
**Purpose:** Research and planning document — no code changes during this phase.
