# 501MTB V2 — Secure administrator authentication

This setup applies only to the V2 demo. Do not change the production V1 site.

## Target

- Administrator identity: `toamasina501mtb@gmail.com`
- Sign-in: Google OAuth through Supabase Auth.
- Second factor: TOTP authenticator app (for example, Google Authenticator or Aegis).
- Authorization: only the explicitly approved administrator account may access ride check-in and editing features.
- Data storage: Supabase database with Row Level Security (RLS); the browser must never be trusted to enforce admin-only access by itself.
- Export: CSV/XLSX generated from authorized database data. GitHub Pages cannot securely write directly into the Excel workbook in the repository.

## Important status

This file is a setup plan, not proof that authentication is active. A Supabase project must be created/configured and its public project URL and publishable/anon key must be added to the V2 frontend. No password, service-role key, OAuth client secret, or other private secret belongs in this repository.

## Setup checklist

1. Create a Supabase project for the V2 check-in app.
2. In Supabase Authentication, enable Google as a provider and configure the Google OAuth client in Google Cloud.
3. Add the exact V2 site URL and local development URL (if used) to Supabase's allowed redirect URLs. Use the site's actual deployed URL; do not guess the callback URL.
4. Restrict allowed users. Create or verify the administrator identity for `toamasina501mtb@gmail.com`. Do not allow arbitrary signed-in Google users to become administrators.
5. Enable Supabase Authenticator Assurance Level 2 (AAL2) by requiring TOTP MFA for admin actions. Enrol the admin's authenticator app and store recovery codes safely offline.
6. Create the rides and ride-participation tables with Row Level Security enabled. Add policies so only the authorized admin can insert, update, or delete records. Do not rely only on hiding buttons or checking an email in JavaScript.
7. Add only the Supabase project URL and publishable/anon key to the frontend configuration. Keep the service-role key and OAuth client secret out of the browser and Git history.
8. Test: signed-out visitor, unrelated Google account, approved admin without MFA, approved admin with MFA, and attempted direct database writes.
9. Add export and workbook synchronization only after the secured database and ride data model are tested.

## Ride distance rules

- Group distance = sum of each distinct ride's official distance once, regardless of participant count.
- Individual distance = sum of official distances for the rides that individual attended.
- Prevent duplicate participation records for the same rider and ride.

## Pending manual configuration

- [ ] Supabase project created
- [ ] Google OAuth provider configured
- [ ] Redirect URLs configured for the deployed V2 site
- [ ] Administrator account restricted to `toamasina501mtb@gmail.com`
- [ ] TOTP MFA enrolled and tested
- [ ] Database schema and RLS policies applied and tested
- [ ] Export and workbook sync tested
