# Trace Desk Requirement Analysis

## Feature Analyzed

Password Reset Functionality

## Purpose

This document demonstrates how Trace Desk transforms an unstructured business requirement into structured Business Analyst and QA deliverables.

## Analysis Workflow

Trace Desk performs:

1. Requirement gap analysis
2. Stakeholder clarification question generation
3. User story creation
4. Acceptance criteria generation
5. QA test scenario generation
6. Requirement traceability review

## Source Requirement

Customers should be able to reset their password if they forget it.

When a customer clicks "Forgot password" on the login screen, the system should send them a link by email that lets them set a new password.

For security, the link should stop working after some time. The new password needs to meet our normal password rules.

We also want to make sure customers can't spam the reset request over and over.

-----------------------------------------------------------------------------------------------------------

## Generated Analysis

Trace Desk — Requirement Analysis

Source Requirement: Password Reset


Customers should be able to reset their password if they forget it.

When a customer clicks "Forgot password" on the login screen, the system should send them a link by email that lets them set a new password. For security, the link should stop working after some time. The new password needs to meet our normal password rules.

We also want to make sure customers can't spam the reset request over and over.


1. Requirement Gaps Identified

#Gap Why it matters1No defined link expiration window ("after some time")Directly affects security posture and UX; can't be built or tested without a number2"Normal password rules" not defined in this documentImplementers/testers need the actual rule set (length, character classes, reuse policy)3No definition of "spam" / rate limiting thresholdsNeeded to size throttling logic (max requests per time window, lockout behavior)4No behavior specified for unregistered/unknown email addressesSecurity-sensitive: revealing whether an email exists is an account-enumeration risk5No mention of what happens to active sessions after a resetCommon security expectation is to invalidate existing sessions/tokens6No mention of user notification after a successful resetBest practice: notify the account email so the real owner is alerted to unauthorized resets7No specification of single-use vs. multi-use reset linkAffects whether a link can be replayed8No mention of accessibility/localization for the email or reset flowAffects design and testability across locales

2. Clarification Questions

CQ-01 — Link Expiration Window
What is the exact expiration time for the password reset link (e.g., 15 minutes, 1 hour, 24 hours)? This is needed to implement and test the expiration logic precisely.

CQ-02 — Password Rule Specification
Where is the canonical definition of "normal password rules" (minimum length, required character types, disallowed reuse, etc.)? The reset flow's validation depends on this.

CQ-03 — Rate Limiting / Anti-Spam Threshold
What request volume or pattern counts as "spamming" the reset feature, and what should happen when the threshold is hit (temporary block, CAPTCHA, silent throttling)?

CQ-04 — Unknown Email Handling
When someone requests a reset for an email that isn't in the system, should the response message be identical to a successful request (to prevent account enumeration), or should it differ?

CQ-05 — Session Invalidation
Should a successful password reset log the user out of all other active sessions/devices, or only require re-authentication on next use?


3. User Stories

US-01 — Request a password reset link
As a customer who has forgotten their password, I want to request a reset link from the login screen, so that I can regain access to my account without contacting support.

US-02 — Set a new password via the reset link
As a customer with a valid reset link, I want to set a new password that meets the site's password rules, so that I can securely log back in.

US-03 — Be protected from reset-link abuse
As a customer, I want the password reset feature to reject excessive or automated reset requests, so that my account and inbox aren't targeted by abuse.

(Flag: US-02 and US-03 both rest on details not yet confirmed — see CQ-02 and CQ-03 — but the story-level intent is clear enough to draft now.)


4. Acceptance Criteria (Gherkin)

AC-01 — Reset link is sent on request (linked to US-01)

Given a customer is on the login screen
When they click "Forgot password" and submit a registered email address
Then the system sends an email containing a unique password reset link

AC-02 — Reset link expires after the defined window (linked to US-02) — ⚠ needs review, depends on CQ-01

Given a customer has received a password reset link
When they attempt to use the link after the expiration window has passed
Then the system rejects the link and prompts them to request a new one

AC-03 — New password must meet password policy (linked to US-02) — ⚠ needs review, depends on CQ-02

Given a customer is on the "set new password" screen with a valid link
When they submit a new password that does not meet the password rules
Then the system rejects the submission and displays the specific rule(s) not met

AC-04 — Excessive reset requests are throttled (linked to US-03) — ⚠ needs review, depends on CQ-03

Given a customer (or automated actor) requests password resets for the same account repeatedly
When the request count exceeds the defined threshold within the defined time window
Then the system stops sending further reset emails and returns a generic confirmation message without revealing whether the block occurred


5. QA Test Scenarios

TC-01 — Happy path: successful reset

Preconditions: Customer has a registered account with a known email address
Steps: 1) Click "Forgot password" 2) Enter registered email 3) Submit
       4) Open the received email 5) Click the reset link
       6) Enter a new password meeting policy 7) Submit
Expected result: Password is updated; customer can log in with the new password;
                 reset link can no longer be reused

TC-02 — Expired link rejected — ⚠ needs review, depends on CQ-01

Preconditions: Customer has requested a reset link
Steps: 1) Wait until after the link's expiration window
       2) Click the expired link
Expected result: System displays an "expired link" message and offers to
                 send a new reset request; no password change occurs

TC-03 — Reset requested for unregistered email — ⚠ needs review, depends on CQ-04

Preconditions: Email address is not associated with any account
Steps: 1) Click "Forgot password" 2) Enter the unregistered email 3) Submit
Expected result: System shows the same generic confirmation message as a
                 successful request, and no email is actually sent

TC-04 — Repeated reset requests are throttled — ⚠ needs review, depends on CQ-03

Preconditions: Customer or script sends multiple reset requests for the
               same account in rapid succession
Steps: 1) Submit a reset request N+1 times within the defined time window
          (N = the not-yet-defined threshold)
Expected result: Requests beyond the threshold do not trigger new emails;
                 system responds without revealing that throttling occurred


Summary

3 user stories, 4 acceptance criteria, 5 clarification questions, 4 test scenarios drafted.
5 items flagged "needs review" (AC-02, AC-03, AC-04, TC-02, TC-04) — all trace back to the three biggest open gaps: link expiration time, password rule specifics, and rate-limit thresholds. Getting answers to CQ-01, CQ-02, and CQ-03 would resolve most of the ambiguity in one pass.
Two additional risks worth raising with the stakeholder even though they didn't generate formal clarification questions above:
account-enumeration protection (CQ-04) and session invalidation after reset (gap #5) — both are common security expectations that the requirement is silent on.

## Requirement Traceability Summary

| Requirement Area | User Story | Acceptance Criteria | Test Coverage |
|---|---|---|---|
| Password reset request | US-01 Request reset link | AC-01 Reset link sent | TC-01 Successful reset |
| Password creation | US-02 Set new password | AC-03 Password validation | TC-01 Successful reset |
| Link expiration | US-02 Set new password | AC-02 Expiration handling | TC-02 Expired link |
| Abuse prevention | US-03 Prevent reset abuse | AC-04 Rate limiting | TC-04 Throttling |
| Unknown accounts | Security Requirement | CQ-04 Review needed | TC-03 Unknown email |

