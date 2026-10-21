# MediRemind — Smart Medicine Reminder

MediRemind is a web app that helps people take their medicines on time. Users add their medicines with dose times, get an alarm and browser notification when a dose is due, mark doses as taken or skipped, track remaining stock, and review adherence history.

## Features
- Secure sign-up / sign-in (email + password, or Google)
- Add medicines: name, dosage, frequency, multiple reminder times, stock, low-stock threshold, color tag, instructions
- Today dashboard grouped into Morning / Evening with a progress bar
- "Take Now" / "Skip" per dose; stock decreases automatically on "Take"
- Alarm pop-up with sound, vibration and browser notification; Snooze 5 min
- Critical (0 left) and low-stock alerts
- Medicines list with search and low-stock filter, delete
- History: current streak, 30-day taken/skipped chart, day-by-day log
- Data saved in the cloud per user — available on any device

## Technologies
- React 19 + TypeScript, TanStack Start / Router / Query
- Tailwind CSS v4
- Lovable Cloud (PostgreSQL database, authentication, row-level security)
- Zod for input validation, Sonner for toasts, Web Audio + Notification APIs for alarms

## Architecture
```text
Browser (React UI)
  ├─ Auth screen ──► Lovable Cloud Auth (hashed passwords, Google OAuth)
  ├─ Home / Medicines / History
  │     └─ TanStack Query ──► Cloud database (HTTPS)
  │                             ├─ medicines (per user, RLS)
  │                             └─ dose_logs (per user, RLS)
  └─ Alarm checker (every 5 s) ─► sound + vibration + notification
```

## Database
| Table | Key columns |
|---|---|
| medicines | id, user_id, name, dose, times[], stock, low_alert, color, note |
| dose_logs | id, user_id, medicine_id, medicine_name, dose_time, log_date, status (taken/skipped) |

Row-level security ensures each user can only read and change their own rows.

## Security
- Passwords are hashed by the authentication service (never stored in the app)
- All form inputs validated with Zod (length, type, range) and database CHECK constraints
- Parameterised queries via the client library (no SQL injection); React escapes output (no XSS)
- HTTPS on the published site

## Installation (local)
```sh
git clone <repo-url>
cd mediremind
npm install
npm run dev
```
Open http://localhost:8080.

## Future enhancements
Dark mode, AI chatbot for medicine questions, PDF/Excel export, email/SMS reminders, caregiver sharing, multi-language support, offline mode (PWA), refill ordering.

## Author
Darapaneni Vyshnavi
