# TSPCM-5811 - Story Details

## Story Information

- Story ID: TSPCM-5811
- Title: Student Late Payment Notification - Past Due Modal - MyEvolve Page
- Issue Type: Story
- Status: Review
- Story Points: 8
- Priority: 3-Medium
- Parent Epic: TSPCM-4871 - Student My Evolve - Upcoming and Late Payment Notifications

## Description

As a Program Solutions student whose installment is past due after my add/drop date, I want a dismissible Past Due modal on My Evolve so that I understand my access has been revoked and I can pay to regain access.

## Acceptance Criteria

```text
1) Show the Past Due modal on My Evolve only (do not show this modal to LMS users).
2) Start the modal immediately after add/drop date passes for the applicable installment.
3) Keep showing the modal on every visit while student is unpaid and eligible for billing, until payment is completed or late-payment window closes 21 days after add/drop.
4) Re-added cohort behavior: if student is re-added after add/drop, before payment window close date, still unpaid, and included for billing, show modal on every visit until paid or window closes.
5) Modal is dismissible and reappears until payment completion (one prompt per visit).
6) Exact wording:
   - Headline: "Your Program Solutions installment [number] payment for [account/school name] is past due."
   - Body: "Access to the associated products has been revoked. Complete your payment to regain access. For additional assistance, please contact Student VIP Support at 1-800-764-0131."
   - Primary action: "Pay and regain access"
7) Style: red warning/danger modal with warning icon, accessible contrast, prominent CTA button.
8) Do not show dollar amounts in modal; amounts/tax are shown in payment flow.
9) CTA navigates to installment-specific payment flow used by payment emails.
10) One-modal rule: if multiple installments are past due, show one modal only (most urgent by earliest add/drop date) and tie CTA to that installment payment flow.
11) Banner relation: Red Past Due banner remains visible behind modal on My Evolve.
```

## Linked Issues (Current Story)

- No linked Jira issues found on TSPCM-5811.
- EDQAENG linked issues on current story: None

## Comments (Current Story)

- Shandy Pittman (2026-02-05): Added AC update for re-added cohort conditions.
- Arnab Sen (2026-02-26): Requested design confirmation with implementation screenshot.
- Paola Sotelo (2026-02-26): Suggested ellipsis handling for long cohort/school names.
- Vivek Saxena (2026-02-27): Confirmed no ellipsis planned; wrapping preferred for context and accessibility.
- Paola Sotelo (2026-02-27): Reiterated concern over long multi-line banners.
- Vivek Saxena (2026-02-27): Kept current behavior; ellipsis can be future enhancement.
- Paola Sotelo (2026-02-27): Proposed max-length style compromise (for visual quality).
- Vivek Saxena (2026-02-27): Reconfirmed no ellipsis currently, full text visibility favored.

## Epic Context

- Epic ID: TSPCM-4871
- Epic Title: Student My Evolve - Upcoming and Late Payment Notifications
- Epic Status: Review
- Epic Child Issues Retrieved: 13
- EDQAENG Linked Issues Found Across Epic Children: 0

### Relevant Epic Narrative (Summary)

- Notification progression across payment lifecycle: blue (open), yellow (due soon), yellow imminent-risk modal, red past-due banner/modal.
- Past due behavior includes revoked access and payment-enabled recovery during a 21-day late-payment window.
- CTA routing must be installment-specific and aligned with payment email links.
- One-modal-at-a-time logic with urgency ordering is required when multiple obligations qualify.
- No dollar amounts in banners/modals; amounts remain in payment flow.

## Linked Stories Under Parent Epic And Their Linked Issues

| Story Key | Type | Status | Summary | Linked EDQAENG Issues | Comment Count |
|---|---|---|---|---|---:|
| TSPCM-5769 | Task | Done | [Design] Students Upcoming & Late Payment Notifications | - | 0 |
| TSPCM-5805 | Story | Review | Student Upcoming Payment Notification - Blue Banner - Initial Awareness | - | 0 |
| TSPCM-5806 | Story | Review | Student Upcoming Payment Notifications - Yellow Banner - Due Soon - Week Prior to Add Drop | - | 4 |
| TSPCM-5807 | Story | Review | Student Upcoming Payment Notifications - Yellow Banner - Imminent Risk - 2 Days Prior to Add Drop | - | 2 |
| TSPCM-5808 | Story | Review | Student Late Payment Notification - Red Banner - Past Due | - | 1 |
| TSPCM-5809 | Story | Review | Student Upcoming Payment Notifications - Imminent Risk Modal - 2 Days Prior to Add Drop | - | 1 |
| TSPCM-5810 | Story | Refinement | Student Late Payment Notification - Past Due Modal - LMS | - | 0 |
| TSPCM-5811 | Story | Review | Student Late Payment Notification - Past Due Modal - MyEvolve Page | - | 8 |
| TSPCM-5820 | Story | Done | Complete Payment Link on Student Payment Notifications | - | 0 |
| TSPCM-5821 | Story | Draft | Report - How Many Students Use Notifications and Modals to Pay | - | 0 |
| TSPCM-5842 | Story | Draft | Clear and Create New Notifications if Add/Drop Date Changed | - | 0 |
| TSPCM-5853 | Story | Review | Student Upcoming Payment Notifications - Multiple Payment Obligations - Contact Support Banner | - | 0 |
| TSPCM-5860 | Story | To Do | Report - Students With Multiple Open Payment Windows | - | 0 |

## Comments In Linked Stories (Epic Children)

- TSPCM-5806: Design-review thread on banner color treatment and version consistency.
- TSPCM-5807: Design-review comments aligned to yellow imminent-risk banner styling.
- TSPCM-5808: Implementation confirmation request for red past-due banner.
- TSPCM-5809: Implementation confirmation request for imminent-risk modal.
- TSPCM-5811: Main discussion on re-added conditions and long-title wrapping vs ellipsis behavior.
- Remaining epic children: no comments or comments unrelated to this modal behavior.

## Notes And Risks For QA Planning

- Clarify whether long school/cohort names must wrap indefinitely or should be capped/ellipsized (comment thread indicates current decision is wrapping).
- Confirm exact trigger time boundary for "immediately after add/drop" (timezone and scheduler granularity).
- Confirm expected behavior at exactly day 21 close boundary (inclusive/exclusive timestamp).
