### UX Best Practices — HCI Interaction Principles

These 8 principles are the backbone of usable interface design. Every SaaS design you create must address all of them — they're as important as the visual specs above.

**1. Consistency — Standardize everything the user learns once.**
- If a filter panel works a certain way on the dashboard (position, controls, apply/clear behavior), it must work identically on every other page that has filters.
- Standardize interaction patterns across the app: how tables sort, how modals open/close, how forms validate, how search behaves, how lists paginate. Document these as reusable patterns, not one-off implementations.
- Icons should mean the same thing everywhere: a gear icon always means settings, a pencil always means edit, a trash can always means delete. Never repurpose icons.
- Action placement consistency: primary actions go top-right or bottom-right. Destructive actions are always visually distinct (red, separated, requiring confirmation).
- Use the design token system (Section 9) to enforce visual consistency automatically — same colors, spacing, and typography everywhere without manual checking.

**2. Shortcuts — Accelerate experienced users without confusing new ones.**
- Always include a command palette (⌘K / Ctrl+K) for keyboard-driven navigation. This is the single most impactful power-user feature in SaaS. Design it with search across pages, actions, and recent items.
- Implement keyboard shortcuts for frequent actions: `N` for new item, `E` for edit, `/` to focus search, `Esc` to close modals/panels, arrow keys for list navigation.
- Show shortcut hints in tooltips and menu items (e.g., "New Document  ⌘N") so users discover them organically.
- Design bulk action patterns: select multiple items with checkboxes + shift-click range selection, then apply actions to all selected.
- Support URL-based deep linking so users can bookmark and share specific views, filters, or items.

**3. Feedback — The user should always know what happened, what's happening, and what will happen.**
- **Immediate feedback (< 100ms):** Button press states, toggle switches, checkbox ticks — these must respond instantly with visual state changes.
- **Short-wait feedback (100ms–2s):** Show a subtle spinner or pulse animation. Disable the trigger to prevent double-submission. Example: "Saving..." on a button after click.
- **Long-wait feedback (> 2s):** Show a progress bar or step indicator with estimated time. Example: "Analyzing repository... Step 2 of 4 — Mapping dependencies."
- **Completion feedback:** Toast notifications for background operations ("Document generated successfully"), inline success states for form submissions (green checkmark replacing the submit button), or redirect to the result.
- **Inline validation:** Validate form fields on blur (not on every keystroke — that's annoying). Show validation state immediately: green checkmark for valid, red message for invalid, with specific guidance ("Password must be at least 8 characters", not "Invalid input").
- **Optimistic UI:** For actions that almost always succeed (toggling a setting, starring an item), update the UI immediately and roll back silently if the action fails. Don't make the user wait for a server round-trip for trivial actions.

**4. Dialogue — Communicate clearly at every decision point.**
- **Task completion messages:** After significant actions, confirm what happened with specific language: "Architecture document generated for payment-service" — not just "Success!" Tell the user what completed and offer the logical next action ("View Document" / "Generate Another").
- **Confirmation dialogs:** Before destructive or irreversible actions, show a dialog that names exactly what will happen: "Delete 'payment-service Architecture Overview'? This cannot be undone." Include the item name — never just "Are you sure?"
- **Empty states with guidance:** When a page has no data yet, don't show a blank void. Show an illustration, a brief explanation of what will appear here, and a clear CTA: "No documents yet. Connect a repository to generate your first technical design document."
- **Toast notifications:** Use for non-blocking feedback. Position consistently (top-right or bottom-right). Auto-dismiss after 4–6 seconds. Include a dismiss button. Color-code by severity: info (blue), success (green), warning (yellow), error (red). Keep text under 10 words.
- **Microcopy quality:** Every label, placeholder, tooltip, and error message should be written in plain language. Avoid technical jargon in user-facing text. Placeholder text should be example values, not instructions (show "jane@company.com" not "Enter your email address").

**5. Error handling — Prevent errors first, then help users recover fast.**
- **Prevention over recovery:** Use input constraints (date pickers instead of free text, dropdowns instead of open fields), disable invalid actions rather than showing errors after the fact, and show format hints before the user types.
- **Inline field validation:** Validate as the user progresses (on blur). Don't wait until form submission to reveal 5 errors at once — catch them one at a time, in context.
- **Error messages that help:** Every error message should include: what went wrong (specific), why it happened (if useful), and how to fix it (actionable). Example: "Could not connect to GitHub. Check that your OAuth token has repo access and try again." — not "Connection failed."
- **Error pages:** Design custom 404, 403, and 500 pages. Include the app's navigation (so the user isn't stranded), a human-readable explanation, and a link to go back or report the issue.
- **Graceful degradation:** If a component fails to load (a chart, an API call), show a contained error state within that component — don't break the entire page. Include a retry button.
- **Form recovery:** If a page refresh or navigation happens during form entry, preserve the user's input. Use auto-save drafts for long forms and document editors.

**6. Reversal — Every action should feel safe to try.**
- **Undo for destructive actions:** Instead of a confirmation dialog, consider the undo pattern: perform the action immediately, show a toast with "Item deleted — Undo" for 6–8 seconds, then finalize. This is faster and less disruptive than modal confirmations for non-catastrophic deletions.
- **Edit/Cancel flows:** Every edit mode should have a clear cancel path that restores the previous state. Show both "Save" and "Cancel" buttons. If the user has unsaved changes and tries to navigate away, show a warning: "You have unsaved changes. Discard?"
- **Version history:** For document-heavy SaaS (like the design doc tool), show version history with the ability to view, compare, and restore previous versions.
- **Draft auto-save:** Save work-in-progress automatically at regular intervals. Show a subtle "Last saved 2 minutes ago" indicator so the user knows their work is safe.
- **Soft delete:** When items are deleted, move them to a trash/archive for 30 days before permanent deletion. Show "Moved to trash — Undo | View Trash" in the confirmation toast.

**7. Control — Let users drive; the interface should adapt to them.**
- **View switching:** For any list/collection, offer multiple view options: list view (compact, scannable), grid/card view (visual, spacious), and table view (data-dense, sortable). Remember the user's preference.
- **Customizable layouts:** Let users resize panels (drag sidebar width), collapse/expand sections, and reorder dashboard widgets where appropriate.
- **User preferences:** Design a settings page that includes: theme (light/dark/system), notification preferences, default views, and display density (comfortable/compact).
- **Bulk operations:** Allow users to select multiple items and act on them in batch: delete, export, tag, move, assign. Show a count of selected items and available actions in a floating action bar.
- **Saved views and filters:** Let users save filter combinations as named views ("My open high-priority tickets") and switch between them quickly.

**8. Memory load — Don't make users remember; help them recognize.**
- **Search everywhere:** Implement global search (⌘K) that searches across all entity types: documents, repos, settings, team members. Show results grouped by type with recent searches at the top.
- **Breadcrumbs:** Show breadcrumbs on every page below the top-level navigation so users always know their location: "Dashboard > Documents > payment-service Architecture Overview".
- **Recent items and favorites:** Show recently accessed items and allow users to star/favorite frequently used items. Put these in the sidebar or command palette for quick access.
- **Filters and faceted search:** For any list longer than 20 items, provide filters. Use faceted search (checkboxes by category) rather than a single search box. Show active filter count as a badge.
- **Contextual help:** Use info tooltips (ⓘ) next to complex fields or settings. For first-time features, use inline callouts or coach marks — not a 10-step tutorial.
- **Sensible defaults:** Pre-fill every form field you can from context (current user, most common choice, last used value). The user should only need to change what's different, not re-enter what's obvious.

