# Add order status filter, empty state, nav switching and logout confirmation modal

## Summary
Improves the Admin Dashboard's Recent Orders panel and changes the logout flow.

## Changes
**Changed behavior (existing tests may break):**
- **Logout** no longer shows a browser `alert()`. Clicking *Logout* opens a confirmation modal (`#logoutModal`) with *Cancel* and *Log out*. Confirming shows a "You have been logged out." banner, sets the username to "Guest", avatar to "G", and disables the Logout button. `Esc` closes the modal.
- **Search placeholder** changed from "Search users..." to "Search orders by ID, customer or amount..." (the search always filtered orders, so the old label was misleading).
- **Sidebar navigation** now updates the active item and the page title (`#pageTitle`) instead of doing nothing.

**New behavior:**
- **Status filter** dropdown (`#statusFilter`: All / Completed / Pending / Failed) that combines with the text search.
- **Clear** button (`#clearFilters`) resets search and status filter.
- **Results count** (`#resultsCount`) shows "Showing X of 4 orders".
- **Empty state** (`#emptyState`) shows "No orders match your search." when nothing matches.

**Testability:** added `data-testid` attributes on nav links, page title, search, status filter, clear button, results count, table, each order row (`order-row-<id>`), empty state, logout button, modal and its buttons, and logged-out banner.

## Suggested test scenarios
1. Search "Sarah" → only order #1002 visible, count "Showing 1 of 4 orders".
2. Search with no match → empty state visible, count "Showing 0 of 4 orders".
3. Status = Completed → #1001 and #1004 visible (2 of 4).
4. Status = Completed + search "Emily" → only #1004 (1 of 4).
5. Clear resets to "Showing 4 of 4 orders" and status "All statuses".
6. Click each sidebar link → it becomes active and the page title matches.
7. Logout → modal opens; Cancel closes it with user still "Admin User".
8. Logout → Log out → banner visible, username "Guest", avatar "G", Logout button disabled.
9. Logout → press Esc → modal closes.
10. Regression: no browser alert dialog appears on logout.
