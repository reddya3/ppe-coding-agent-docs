# TSPCM-5811 - Detailed Test Cases

## Scope
This document expands TS-001 through TS-016 into executable manual QA test cases for the My Evolve Past Due modal behavior.

## Environment And Base Test Data

- Evolve URL: https://evolvetest.elsevier.com/
- PS URL: https://evolvetest.elsevier.com/tsp
- Admin account: Aut.superuser@guerrillamail.com / Test@12345
- Student account: john.doe@example.com / Password123
- Payment window model:
- Add/drop date: configured per cohort/installment fixture
- Late-payment window close: add/drop + 21 days

## Test Case Summary

| TC ID | Title | Type | Priority | Scenario Ref | Automated? |
|---|---|---|---|---|---|
| TC-001 | Show Past Due modal on My Evolve for eligible unpaid student | Positive | P0 | TS-001 | Candidate |
| TC-002 | Ensure My Evolve Past Due modal does not appear in LMS flow for this story | Negative | P0 | TS-002 | Manual |
| TC-003 | Verify immediate trigger right after add/drop boundary | Boundary | P0 | TS-003 | Manual |
| TC-004 | Verify modal reappears on each visit after dismiss while unpaid | Positive | P0 | TS-004 | Candidate |
| TC-005 | Suppress modal when student is excluded from billing | Negative | P0 | TS-005 | Candidate |
| TC-006 | Show modal for re-added eligible unpaid student before close window | Integration | P0 | TS-006 | Manual |
| TC-007 | Do not show modal when re-added after close window | Negative | P0 | TS-007 | Manual |
| TC-008 | Validate exact modal copy and CTA label | Positive | P0 | TS-008 | Candidate |
| TC-009 | Route CTA to installment-specific payment flow | Integration | P0 | TS-009 | Candidate |
| TC-010 | Enforce one-modal rule for multiple past-due installments | Boundary | P0 | TS-010 | Manual |
| TC-011 | Verify CTA ties to urgent installment in one-modal context | Integration | P1 | TS-011 | Manual |
| TC-012 | Confirm modal excludes dollar amounts and tax values | Negative | P1 | TS-012 | Candidate |
| TC-013 | Validate warning styling, icon, and keyboard accessibility | Non-Functional | P1 | TS-013 | Manual |
| TC-014 | Stop modal display after 21-day late window closure | Boundary | P1 | TS-014 | Manual |
| TC-015 | Remove modal after successful payment and restore access | Integration | P2 | TS-015 | Candidate |
| TC-016 | Verify long institution/cohort names wrap without clipping | Non-Functional | P2 | TS-016 | Manual |

## TC-001: Show Past Due modal on My Evolve for eligible unpaid student

- Scenario: TS-001
- Type: Positive
- Priority: P0
- Module: My Evolve Notifications - Past Due
- Preconditions:
1. Student has installment past add/drop date.
2. Student unpaid and eligible for billing.
3. Student has My Evolve access.
- Test Data:
- Account: john.doe@example.com / Password123
- Cohort: COHORT-PASTDUE-ELIGIBLE

| # | Step | Expected Result |
|---|---|---|
| 1 | Log into https://evolvetest.elsevier.com/ as student. | Login succeeds and My Evolve entry is available. |
| 2 | Navigate to My Evolve page. | Red past-due banner is visible in page context. |
| 3 | Observe modal on load. | Past Due modal appears with warning style and CTA. |
| 4 | Capture screenshot evidence. | Evidence shows modal over red banner context. |
- Postconditions: Student remains unpaid.

## TC-002: Ensure My Evolve Past Due modal does not appear in LMS flow for this story

- Scenario: TS-002
- Type: Negative
- Priority: P0
- Module: Context Gating
- Preconditions:
1. Student past due and unpaid.
2. LMS launch URL configured for the same student.
- Test Data:
- Account: john.doe@example.com / Password123
- Flow: LMS launch to payment request page

| # | Step | Expected Result |
|---|---|---|
| 1 | Launch course from LMS and authenticate. | User reaches payment request page flow. |
| 2 | Observe for My Evolve-specific modal behavior. | My Evolve Past Due modal from this story is not shown in LMS context. |
| 3 | Capture screenshot. | Evidence confirms absence of My Evolve modal. |
- Postconditions: None.

## TC-003: Verify immediate trigger right after add/drop boundary

- Scenario: TS-003
- Type: Boundary
- Priority: P0
- Module: Timing Trigger
- Preconditions:
1. Add/drop threshold can be controlled or simulated.
2. Student unpaid and eligible.
- Test Data:
- Add/drop timestamp fixture: AD-BOUNDARY-001

| # | Step | Expected Result |
|---|---|---|
| 1 | Open My Evolve 1 minute before configured add/drop threshold. | Past Due modal is not shown yet. |
| 2 | Refresh at threshold crossing time. | Past Due modal appears as soon as threshold is crossed. |
| 3 | Repeat with second account in another timezone fixture. | Trigger behavior is consistent with business timezone rule. |
- Postconditions: Note exact observed trigger timestamp.
- Notes: Requires clarification on timezone and scheduler granularity for pass/fail strictness.

## TC-004: Verify modal reappears on each visit after dismiss while unpaid

- Scenario: TS-004
- Type: Positive
- Priority: P0
- Module: Dismiss/Revisit Lifecycle
- Preconditions:
1. Student is eligible past-due unpaid.
2. Modal currently displayed.

| # | Step | Expected Result |
|---|---|---|
| 1 | Dismiss modal using close action. | Modal closes for current visit. |
| 2 | Navigate away from My Evolve and return. | Modal appears again. |
| 3 | Log out, log in, and revisit My Evolve. | Modal appears once per visit while unpaid. |
- Postconditions: Student remains unpaid.

## TC-005: Suppress modal when student is excluded from billing

- Scenario: TS-005
- Type: Negative
- Priority: P0
- Module: Eligibility Gating
- Preconditions:
1. Student has past-due installment.
2. Billing exclusion flag set.

| # | Step | Expected Result |
|---|---|---|
| 1 | Log in as excluded student. | Login succeeds. |
| 2 | Open My Evolve. | No Past Due modal is displayed. |
| 3 | Validate API/state payload for exclusion. | Eligibility reflects excluded status. |
- Postconditions: None.

## TC-006: Show modal for re-added eligible unpaid student before close window

- Scenario: TS-006
- Type: Integration
- Priority: P0
- Module: Re-added Cohort Logic
- Preconditions:
1. Student previously removed and re-added.
2. Re-add occurred after add/drop and before day-21 close.
3. Student unpaid and billing-eligible.

| # | Step | Expected Result |
|---|---|---|
| 1 | Re-add student via cohort admin flow with qualifying timing. | Student returns to cohort roster. |
| 2 | Log in as student and open My Evolve. | Past Due modal appears. |
| 3 | Dismiss and revisit page. | Modal reappears once per visit until paid or close date. |
- Postconditions: None.

## TC-007: Do not show modal when re-added after close window

- Scenario: TS-007
- Type: Negative
- Priority: P0
- Module: Re-added Cohort Logic
- Preconditions:
1. Student re-added after day-21 close.
2. Student still unpaid.

| # | Step | Expected Result |
|---|---|---|
| 1 | Log in as re-added-after-close student. | Login succeeds. |
| 2 | Open My Evolve. | No Past Due modal is shown. |
| 3 | Verify status text/state. | Payment window closed behavior is active. |
- Postconditions: None.

## TC-008: Validate exact modal copy and CTA label

- Scenario: TS-008
- Type: Positive
- Priority: P0
- Module: Content Fidelity
- Preconditions:
1. Past Due modal is displayed.

| # | Step | Expected Result |
|---|---|---|
| 1 | Read modal headline text. | Matches exact required sentence with installment number and account/school name placeholders resolved. |
| 2 | Read modal body text. | Includes revoked-access message and Student VIP Support phone 1-800-764-0131 exactly. |
| 3 | Read primary CTA label. | CTA label equals "Pay and regain access". |
- Postconditions: None.

## TC-009: Route CTA to installment-specific payment flow

- Scenario: TS-009
- Type: Integration
- Priority: P0
- Module: Payment Routing
- Preconditions:
1. Student has identified past-due installment.
2. Payment email link route pattern is known for comparison.

| # | Step | Expected Result |
|---|---|---|
| 1 | Open Past Due modal. | Modal is visible with CTA. |
| 2 | Click "Pay and regain access". | User is redirected to installment-specific payment flow. |
| 3 | Validate route/query parameters against expected installment ID. | Destination matches same installment routing logic used by email flow. |
- Postconditions: Payment not required for this case.

## TC-010: Enforce one-modal rule for multiple past-due installments

- Scenario: TS-010
- Type: Boundary
- Priority: P0
- Module: Multi-Obligation Prioritization
- Preconditions:
1. Student has at least 2 past-due installments.
2. Each installment has different add/drop date.

| # | Step | Expected Result |
|---|---|---|
| 1 | Log in and open My Evolve. | Only one modal is shown. |
| 2 | Compare shown installment against urgency ordering. | Modal corresponds to most urgent installment (earliest add/drop date). |
| 3 | Capture evidence with installment metadata. | Proof supports one-modal + urgency rule. |
- Postconditions: None.

## TC-011: Verify CTA ties to urgent installment in one-modal context

- Scenario: TS-011
- Type: Integration
- Priority: P1
- Module: Multi-Obligation Routing
- Preconditions:
1. TC-010 setup with multiple past-due installments.
2. One modal displayed for urgent installment.

| # | Step | Expected Result |
|---|---|---|
| 1 | Click modal CTA from one-modal display. | Payment flow opens. |
| 2 | Inspect installment identifier in URL/payload. | Identifier maps to urgent installment, not any secondary installment. |
| 3 | Back out and re-open My Evolve. | One-modal behavior remains consistent. |
- Postconditions: None.

## TC-012: Confirm modal excludes dollar amounts and tax values

- Scenario: TS-012
- Type: Negative
- Priority: P1
- Module: Content Guardrails
- Preconditions:
1. Past Due modal displayed for any eligible user.

| # | Step | Expected Result |
|---|---|---|
| 1 | Inspect headline/body/footer/modal CTA area. | No dollar amount is shown. |
| 2 | Inspect tooltips or hidden labels if any. | No tax/amount values are exposed in modal. |
| 3 | Proceed to payment flow. | Amounts and tax appear only in payment flow, not modal. |
- Postconditions: None.

## TC-013: Validate warning styling, icon, and keyboard accessibility

- Scenario: TS-013
- Type: Non-Functional
- Priority: P1
- Module: Accessibility and UI
- Preconditions:
1. Modal is visible.

| # | Step | Expected Result |
|---|---|---|
| 1 | Verify modal theme and warning icon. | Red warning style and icon are visible and prominent. |
| 2 | Tab through close and CTA controls. | Focus order is logical and visible. |
| 3 | Run contrast checks on text/background elements. | Contrast meets accessibility expectations for warning modal. |
- Postconditions: Accessibility observations logged.

## TC-014: Stop modal display after 21-day late window closure

- Scenario: TS-014
- Type: Boundary
- Priority: P1
- Module: Late Window Lifecycle
- Preconditions:
1. Student was previously receiving Past Due modal.
2. System date/time advanced beyond day-21 close threshold.

| # | Step | Expected Result |
|---|---|---|
| 1 | Open My Evolve after late-window close. | Past Due modal is not displayed. |
| 2 | Re-login and revisit My Evolve. | Modal remains suppressed after close. |
| 3 | Confirm status in data model/logs. | Payment-window-closed state is reflected. |
- Postconditions: None.

## TC-015: Remove modal after successful payment and restore access

- Scenario: TS-015
- Type: Integration
- Priority: P2
- Module: Payment Completion Outcome
- Preconditions:
1. Student currently sees Past Due modal.
2. Valid payment method is available in test env.

| # | Step | Expected Result |
|---|---|---|
| 1 | Click CTA and complete payment successfully. | Payment confirmation is returned. |
| 2 | Return to My Evolve. | Past Due modal is no longer shown for paid installment. |
| 3 | Access previously revoked content. | Access is restored as expected. |
- Postconditions: Installment marked paid.

## TC-016: Verify long institution/cohort names wrap without clipping

- Scenario: TS-016
- Type: Non-Functional
- Priority: P2
- Module: Content Layout
- Preconditions:
1. Installment references long account/school name string.

| # | Step | Expected Result |
|---|---|---|
| 1 | Display modal with long school/cohort name fixture (>70 chars). | Headline renders full context with wrapping. |
| 2 | Inspect for overflow/cutoff across desktop viewport widths. | No clipping/overlap occurs. |
| 3 | Verify CTA and close controls remain usable. | Actions stay visible and operable. |
- Postconditions: None.

## Execution Notes

- Recommended order: TC-001, TC-008, TC-009, TC-004, TC-010, TC-011, TC-012, TC-013, TC-006, TC-007, TC-014, TC-015, TC-002, TC-005, TC-016, TC-003.
- High-priority blockers: inability to seed past-due/re-add timing scenarios; unavailable installment-specific route diagnostics.
- Evidence checklist per TC: screenshot, account/cohort fixture ID, timestamp/timezone, and observed route or UI state.

## Clarifications Needed

1. For "immediately after add/drop," what exact timezone and scheduler cadence define the pass/fail boundary?
2. Should LMS past-due modal behavior be treated as out of scope for this story only, with separate validation handled in TSPCM-5810?
