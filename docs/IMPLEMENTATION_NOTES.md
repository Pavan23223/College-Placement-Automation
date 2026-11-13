# Implementation notes and review checklist

## Export inventory

| Export | Nodes | Configured trigger |
|---|---:|---|
| AI Placement Assistant | 34 | Coordinator-facing n8n Form Trigger |
| Registration Status Sync | 10 | Every 15 minutes |
| Deadline Reminders | 14 | Daily at 9 AM |
| Company Registration Reports | 12 | Daily at 6 PM |

The four uploaded exports were marked inactive (`active: false`) at export time.

## Eligibility rules visible in the company intake workflow

- Branch must match the configured eligible branches, or the company can specify `ALL`.
- Student CGPA must meet the minimum CGPA.
- Backlogs must be at or below the configured maximum.
- Students with statuses such as placed, opted out, debarred, blocked, or not interested are excluded.
- Graduation year is checked only if a graduation-year column is detected in the master sheet.
- Placement status is checked only if a placement-status column is detected.
- Duplicate USNs and invalid/missing email addresses are excluded.

## Coordinator approval and communication

The company intake workflow includes an approval step before company resource creation and student announcements. It also checks email history and logs sent announcements to reduce duplicate messages.

## Reminder behavior

The reminder workflow runs daily at 9 AM. It reads active drives, closes drives whose deadlines have passed, selects records with `Pending` status, checks the reminder log, and sends a reminder only when a same-day reminder key has not already been recorded.

## Reporting behavior

The reporting workflow runs daily at 6 PM. It computes eligible, registered, declined, pending, and needs-review counts, a confirmation rate, and department-wise confirmed/total figures. It records report snapshots and sends a summary email.

## Review before demonstration or deployment

- [ ] Reconnect all Google and OAuth credentials in your own n8n account.
- [ ] Replace sanitized placeholders with your own resource IDs.
- [ ] Confirm the master student sheet column names match the workflow's expected fields.
- [ ] Confirm the Forms API OAuth scope and access permissions.
- [ ] Review the registration-status sync node named `Match Responses To Students`: its supplied code currently only adds a `myNewField` property. Confirm the intended downstream matching node is wired and tested.
- [ ] Verify all Data Table filters and report status conditions with test records.
- [ ] Test approval, rejection, duplicate-drive, no-eligible-student, registered, declined, pending, expired-deadline, and report scenarios.
- [ ] Ensure reminder and report schedules are disabled until testing is complete.
- [ ] Use synthetic student data for screenshots and demos.
- [ ] Confirm that the reporting workflow's source status labels align with the statuses written by the sync workflow.

## Privacy

The repository copies have credentials and webhook IDs removed and account-specific values replaced with placeholders. Still inspect the exports and screenshots before publishing. Never upload production student data or secrets.
