# Brain Arena — rebuilt package

This rebuild includes a static site, 30 questions each for English, Mathematics, Biology, Chemistry and Physics, explanations, payment form with receipt upload, admin review UI, 14-day approval records, and Supabase Edge Functions for admin and student email notifications.

## Before real payments: required setup
A static ZIP alone cannot safely approve transactions or send emails. Do these steps once in the existing Supabase project:

1. In `app.js`, replace `PASTE_YOUR_SUPABASE_PUBLISHABLE_KEY_HERE` with your Supabase **publishable** key. Never put a service-role/secret key in the website.
2. In Supabase SQL Editor, run all of `supabase/setup.sql`.
3. In Supabase Authentication → Users, create an admin user with email `akinyanmijoshua531@gmail.com` and a strong password.
4. Deploy `supabase/functions/notify-payment` and `supabase/functions/admin-review` to your Supabase project.
5. Set Edge Function secrets: `SUPABASE_URL=https://wgticiijleeurkhoztlf.supabase.co`, `SUPABASE_ANON_KEY` (your publishable/anon key), `SUPABASE_SERVICE_ROLE_KEY` (secret kept server-side only), `ADMIN_EMAIL=akinyanmijoshua531@gmail.com`, `RESEND_API_KEY` (from Resend), and `EMAIL_FROM` (sender verified with Resend).
6. Upload the **contents** of this folder to Netlify so `index.html` is at the site root.

## What it does
- Start practising → choose subject; if no active local premium period, payment page opens when a subject is selected.
- Shows OPay account 9021669942, account name Joshua Jesuliho Akinyanmi, and call/WhatsApp support +2347042320940.
- Payment form requires name, email, reference, date/time and receipt (image/PDF, max 5 MB). Success/error text displays on the page, including actual Supabase errors.
- Admin signs in with Supabase Auth, views pending requests and private receipt links, then approves/rejects. Approval writes a 14-day record and sends the student an email if Resend is configured.
- Quiz has 30 shuffled questions per subject with explanations and local result history; quiz results can also be stored in Supabase.

## Important limitation to finish before production
The current quiz lock reads a local browser access record, so it is not yet a tamper-proof premium gate or cross-device login system. The admin approval function writes the correct 14-day access record, but the student access-check/login flow needs to be connected to that record for secure production access. The email alert is not live until the Edge Functions and Resend secrets are configured. Test with a non-payment test reference first.
