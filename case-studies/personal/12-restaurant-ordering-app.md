# 12 · Restaurant ordering app

**Stack:** Next.js 16, TypeScript, Tailwind 4, JSON menu data with a repository seam to Supabase · **Tests:** 37

## Context
A small restaurant wanted customers to scan a QR code, pick menu items/sizes/quantities, confirm,
and have the owner see the order — plus a back-office view for the kitchen.

## Approach
- Mobile-first customer flow; password-protected admin area.
- Menu stored as data behind a **repository interface**, so JSON can become Supabase without
  touching the UI.
- Back office: grill/stove batching, sales totals.

## Interesting engineering
- **Double-tap safety.** Orders carry an **idempotency key** (unique column), so a repeated tap or
  retry returns the existing order instead of creating a second one.
- **Loyalty/redemption races.** The member row is locked during redemption so balance and
  daily-limit checks cannot be raced by two concurrent requests; a concurrent duplicate that
  commits while waiting for the lock is returned, not rejected.
- Tests cover the stove batching, sales totals, double-submit protection, auth and the compositor.

## Honest status
Supabase and LINE integrations are written but not yet tested against the real services.
