# Student Scholarship Monitoring and Academic Compliance System

A browser-based academic project built with HTML, CSS, JavaScript ES modules, and Supabase. It supports student academic-summary submissions, staff verification, scholarship compliance evaluation, renewal decisions, and an audit history.

## Project files

- `index.html` — login page
- `student.html` — submit and view academic summaries
- `staff.html` — staff/faculty review and decision workspace
- `style.css` — responsive styling
- `js/` — Supabase connection and page logic
- `supabase/schema.sql` — database tables, policies, and workflow functions
- `supabase/demo_accounts.sql` — creates application profiles for the two demo users after you create their Auth accounts

## Step 1 — Open the project

1. Extract the ZIP file.
2. Open the `ScholarshipMonitoringSystem` folder in VS Code.
3. Install the **Live Server** extension if needed.
4. Right-click `index.html` and choose **Open with Live Server**. Do not open the HTML file by double-clicking it.

## Step 2 — Connect Supabase

1. Sign in at https://supabase.com/ and open your project, or create a new one.
2. Open **Project Settings → API** (or **Connect**) and find the **Project URL** and **publishable key**. Older projects may show an `anon` public key.
3. Open `js/supabase.js`.
4. Check that `SUPABASE_URL` and `SUPABASE_ANON_KEY` belong to your project. A publishable key or legacy `anon` public key is supported. Replace them if they do not.
5. Use only the publishable/anon public key in browser code. Never put a `service_role` or secret key in this project or commit it to GitHub.

The app validates the Supabase URL and public-key format when it loads. If the page says Supabase is not connected, verify the URL and key and save the file.

## Step 3 — Create the database

1. In Supabase, open **SQL Editor → New query**.
2. Open `supabase/schema.sql` in VS Code and copy the entire file.
3. Paste it into the SQL Editor and click **Run**.
4. Confirm that it completes without an error. It creates the tables, sample scholarship policies, security policies, and staff workflow functions.

Run the schema before creating demo profiles. If you already ran an earlier version, run the updated schema again to install the correction/resubmission function.

## Step 4 — Create the two demo login accounts

In Supabase, open **Authentication → Users → Add user** (the exact label may vary). Create both accounts and enable **Auto Confirm User** if that option is shown.

| Role | Email | Password |
|---|---|---|
| Student | `student@example.com` | `student` |
| Staff | `staff@example.com` | `staff123` |

These are demo credentials. Do not use them for real accounts or public production systems. Creating a row in SQL does not create an Auth password; the users must exist in Authentication first.

## Step 5 — Assign their application roles

1. In VS Code, open `supabase/demo_accounts.sql`.
2. Copy the whole file.
3. In Supabase SQL Editor, create a new query, paste it, and click **Run**.
4. The final query should show `student@example.com` with role `student` and `staff@example.com` with role `staff`.

The script also assigns the student to the Merit Scholar program. It can be run again if you need to reset these demo profile details. Supported staff roles are `staff`, `faculty`, and `admin`; do not assign one of these roles to a student account.

## Step 6 — Test the workflow

1. Open the Live Server page and sign in as `student@example.com`.
2. On **My Submissions**, enter an academic year, semester, GWA, units, failed/incomplete/dropped counts, and a document reference. Submit it.
3. Sign out. Sign in as `staff@example.com`.
4. Open **Staff Workspace** and click **Review** on the new submission.
5. Choose **Verify evidence**, then save.
6. Review it again and choose **Evaluate compliance**.
7. If it is Compliant, choose **Approve renewal**. If it has a deficiency, choose **Hold renewal** and provide a reason.
8. Sign back in as the student to check the updated status.
9. If staff returns a submission for correction, sign in as the student and submit the corrected values for that same year and semester. The existing returned record is updated and sent back for verification.

## Scholarship rules in the demo

- **Merit Scholar:** GWA at most 1.75, at least 18 units, and no failed, incomplete, or dropped subjects.
- **Community Scholar:** GWA at most 2.25, at least 15 units, no failed or incomplete grades, and at most one dropped subject.
- Demo submission deadline: October 30, 2026 at 5:00 PM Philippine time.

These are sample rules for a school project. Confirm the actual requirements with your institution before using real records.

## Common problems

- **Login fails:** confirm the user exists in Supabase Authentication and the password is correct.
- **Profile setup required:** run `supabase/demo_accounts.sql` after creating both Auth users.
- **No scholarship assigned:** run the demo account SQL again and confirm `Merit Scholar` exists in `scholarship_programs`.
- **Supabase connection error:** check the URL/key in `js/supabase.js`, your internet connection, and that the Supabase project is active.
- **Permission denied:** check that the user profile has the correct role and that you ran the complete `supabase/schema.sql`.
- **Module or import error:** use Live Server rather than a `file://` URL.
- **Do not turn off Row Level Security (RLS) to fix errors.** Fix the profile or policy instead.

## Publish to GitHub Pages (optional)

1. Create a GitHub repository and upload the contents of this folder.
2. In the repository, open **Settings → Pages** and publish from the `main` branch and root folder.
3. In Supabase **Authentication → URL Configuration**, add your local Live Server URL and GitHub Pages URL to the allowed redirect URLs if required.
4. Test the deployed app using the demo accounts.

Anyone can view frontend source code in a public repository. The public Supabase key is intended for browser use, but database access must remain protected by RLS. Do not put secrets or real student information in the repository.

## Current scope

The app stores a document reference rather than uploading files. It does not connect to the Registrar, process payments, send SMS, or support appeals. Use fictional data for demonstrations and test all permissions before any real deployment.
