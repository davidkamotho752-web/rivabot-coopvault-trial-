# CoopVault v13.2 — Stage 2 (trial)
Browser-only trial. **Use test data only.** Data lives in the browser it is entered in.

## Open it
Upload `index.html`, `sw.js`, `manifest.webmanifest`, `icon.svg`, `.nojekyll` to the GitHub repo (replace the old files). The first screen is a setup wizard: the app starts empty.

## What Stage 1 has
- Empty start; first-run setup creates the Administrator. No hard-coded members, loans or sample data.
- Seven windows: Member, Manager/Accountant, Credit, Management, Education, Supervisory, Administrator.
- Login: PIN (PBKDF2 hashed) + second step (real TOTP, RFC 6238). Trial mode shows the code on screen; switch it off for real use.
- Lockout after 5 wrong tries, idle logout, step-up code for sensitive actions.
- Privacy by role (masked vs full), Supervisory read-only, Administrator masked with recorded temporary access.
- Footprints: append-only SHA-256 chained audit trail; members see who opened or changed their data; any change notifies the member.
- Membership applications and admission (Management), General Meeting confirmation, activation codes.
- Loan Policy and By-laws as clause library and editable rules (changes need a resolution reference).
- Feedback loop to the Administrator, notices from Education, encrypted backup/restore.

## Added in Stage 2
- Loans: normal, school fees, emergency. Live policy check with clause numbers; 5 guarantors (accept, decline, pledge less, replace, remind); Manager check, Credit (2 of 3), Executive (3 of 4), ratification list, payout, repayments with receipts, arrears steps (1, 2, 3 months), guarantees released on clearing.
- Returns, appeals, questions and ratings: every request ends with an answer.
- Groups (3 to 10): proposal, member agreement, Management ratification, group loan lines (several liability), joint payout, every member signs off as received.
- Member to Supervisory complaints: two-way, confidential, officers named are excluded.
- Administrator: fake test-data generator (made at run time, editable, erasable) and a test clock. No data is hard-coded.

## Honest limits
Anyone with browser developer tools can alter local data; the chain detects casual edits only. Real protection arrives with the server (Stage 6). Documents are small files (under 200 KB) kept in the browser. Stage 3 (payroll and bank file loop, accountant books, other windows' document vaults, staff bus), Stage 4 (meetings) and Stage 5 (public AI guide, USSD simulator) are not built yet. Tested in Chromium only.

## v13.3 — navigation, assistant, feedback loop
- Browser Back/Forward work across all windows (loan, group, member and complaint pages included). After log-out, Back shows no private screen.
- 🤖 Ask: an assistant on every screen (also for visitors). English and basic Swahili. Answers come from the Loan Policy and By-laws settings and from how the app works; for members it works out their own borrowing limit on the device. It cannot approve or change anything. Unanswered questions go to the Administrator.
- 💬 Feedback on every screen: records the screen and build version, goes to the Administrator's inbox, and the Administrator replies and sets a status (seen, working, fixed, not planned). The sender is notified and confirms “fixed” or reopens it.
- Badges on tabs and a “What do you want to do?” panel on every Home screen.
- System page: a 12-point data-consistency check across receipts, loans, guarantees, groups and the audit chain.

## Deploy and test (free)
**Option A, on your laptop (safest):** in this folder run `python3 -m http.server 8000`, then open http://localhost:8000 in Chrome. Data stays in that browser only.
**Option B, GitHub Pages:** upload every file here (including the hidden `.nojekyll`) to the repo, then Settings → Pages → Deploy from branch → main / root. Free Pages needs a public repo; that is fine because the code holds no secrets and no data. Do not enter real member data and share the link with nobody until the Supabase version.
**Phone:** open the Pages link in Chrome, menu → Add to Home screen (installs as “CoopVault”). Phone data is separate from the laptop's.
**First run:** the setup screen asks for your society name and creates the Administrator. Leave “Fill the app with editable sample people and activity” ticked: you get sample people for every role, loans at every stage, complaints, a group, an application, expenses and data requests, all made at run time and all editable. The login page then shows “Try a window” buttons (no PIN) and every screen has a Window switcher. Turn that off in Rules before real use. Normal PIN login still works with the test PIN shown in System.
**Report problems:** 💬 Feedback on any screen; read them in the Administrator's Feedback inbox.

## v13.4 — Stage 3, drop 1: data files, books, vaults
- **Data files (Manager):** request a payroll, bank or M-Pesa file, upload it when it arrives by email (PDF, Excel .xlsx, CSV, photo; under 400 KB). Payroll files are matched to members by payroll number, differences are flagged, unknown/repeated numbers are never posted, and posting puts loan instalments first then savings, with receipts. Posting twice is blocked.
- **Books (Manager writes; Management and Supervisory read-only):** cash book, trial balance, income and expenditure, balance sheet, expenses with payment vouchers (above the limit in Rules an Executive officer approves), bank/M-Pesa/cash reconciliation, month closing (closed months reject back-dated entries; an Executive officer can reopen with a reason), monthly statement (due within 10 days of month end).
- **Document vaults:** Manager, Credit, Management, Education and Supervisory each have a vault; the Supervisory Committee can open all and every opening by someone else is logged.
- System page: 17 consistency checks (5 new for the books).
- Not yet: M-Pesa auto-matching (parked at your request), the staff bus, opening balances, year-end close and dividends. The bus comes in drop 2.

## v13.4b — see every window
Sample society and “Try a window” (trial only). The app has no hard-coded society name; you type it at setup. Only the Loan Policy and By-laws are built into the code.

## v13.5 — documents routed by role, training, attachments
- **Members → My documents:** send ID copy, payslip, nominee form or proof of payment to the Accountant or the Credit Committee. The receiving office verifies or rejects with a reason; the member is told. Loan documents still go on the loan page (Accountant verifies, Credit and Supervisory can read).
- **Applicants** can attach an ID copy and payslip to the application; the Accountant, Credit and Executive can open them, and on admission they move to the new member.
- **Management, Credit, Education → Vault → “Send to the Accountant for filing”:** receipts, vouchers, invoices, resolutions. The Accountant marks them filed with a reference and the sender is told.
- **Education → Vault → “Publish to all members”:** members are notified and find it under **Training**.
- **Complaints:** a member (and the Supervisory Committee in replies) can attach a document; only the member and Supervisory can open it, and not an officer the complaint names.
- **Supervisory:** “Every document” list, read-only, every opening logged.
- **Screenshots:** 💬 Feedback accepts a screenshot file and, where the browser allows it, “Capture this screen”. Images are shrunk to about 200 KB and go to the Administrator only. Until there is a shared server, testers use “Download my feedback” and send the file to the developer, who adds it under Feedback → “Bring in feedback from testers”.
- Group constitution upload removed. Files stay under 400 KB (200 KB for loan documents) during the browser trial.

## v13.6 — Staff bus (Accountant)
- New **Bus** window: Manager writes; Management and Supervisory read-only; other roles do not see it.
- Daily log (trips, odometer checks), bookings with partial payments, money received (M-Pesa code, bank transfer, cash, cheque) with numbered receipts posting to Books, expenses with vouchers (above the limit they wait for an Executive officer), garage records, driver details, expiry alerts (insurance, inspection, licence, driver licence, PSV, service due), monthly summary.
- Attachments (slips, receipts) open for Manager, Executive officers and Supervisory only.
- Test tools: "Add sample bus records". Consistency checks now 21.

## v13.7 — Page one shows every window
- First page lists all seven windows (Member, Executive, Credit, Education, Supervisory, Manager/Accountant, System Administrator). In trial mode a tap opens that window; with one-tap off it goes to Log in, naming the chosen window.
- Back, Forward and Home buttons at the top of every page; "All windows" when logged in.
