---
title: J4V Coach Site Chatbot - Plan
type: feat
date: 2026-11-21
artifact_contract: ce-unified-plan/v1
product_contract_source: ce-plan-bootstrap
execution: code
---

# J4V Coach Site Chatbot - Plan

## Goal Capsule

- **Objective:** A visitor on the live J4V Top Coach page can ask questions in an on-page chatbot grounded in Just4Veterans content and end the conversation with a booked free 30-minute strategy session.
- **Means:** GHL Conversation AI bot embedded via the GHL chat widget (KTD1), closing to a native GHL calendar booking inside the chat (KTD2).
- **Authority hierarchy:** user instructions, then the site compliance invariants (educates, not accredited, you file your own claim, no win no fee), then this plan.
- **Stop conditions:** Definition of Done below. No booking-capable chatbot live and verified, no stop.

---

## Product Contract

### Summary

Add a GHL Conversation AI chatbot to `j4v-top-coach/index.html` that answers veteran questions from a knowledge base built by crawling selected just4veterans.com pages, speaks as the DJ Skyler coach persona, always carries the site's compliance lines, and books calls directly into a GHL calendar.

### Problem Frame

The page is one-way. Visitors make the booking decision with questions the page does not answer at that moment (eligibility, process, cost, what the call covers). Every unanswered question is a lost booking; the existing modal only serves visitors who were already convinced.

### Key Decisions

- **GHL Conversation AI as the bot platform.** (session-settled: user-directed — chosen over a custom-built simulated bot: matches the agency stack, native KB crawler and calendar booking, no new infrastructure to own.) Governs R1, R6, R8.
- **Native GHL calendar booking inside the chat.** (session-settled: user-directed — chosen over handing off to the existing simulated modal: makes the booking real; the modal stays as the page-level "Book a call" path.) Governs R6, R7.
- **The bot may use real Just4Veterans service facts under the DJ Skyler persona.** (session-settled: user-directed — chosen over keeping the fictional wall: the KB's service and process content is the coach's real value proposition and carries no personal data.) Governs R2, R4, R10.
- **The booking modal becomes a labeled demo preview.** (session-settled: user-directed via doc review 2026-11-21 — chosen over repointing the modal to the real calendar, and over leaving the fake confirmation in place: removes the live misrepresentation while keeping the page's CTA structure.) Governs R7, U5.

### Requirements

**Bot behavior**

- R1. The bot answers questions about VA disability claims, the coaching process, services, and pricing model from its knowledge base, in the DJ Skyler coach persona (retired Navy Command Master Chief, coaches one-on-one).
- R2. The bot states the compliance lines when discussing representation or outcomes: he educates and you decide, you file your own claim, he is not an accredited representative and is not the VA, no win no fee.
- R3. The bot never gives legal, medical, or claims-advice verdicts (no "you will get rated X%"), and defers those to the call.
- R4. The bot stays in persona; it never invents credentials, testimonials, or outcomes beyond the knowledge base.
- R4b. The bot attributes services, process, and outcomes to Just4Veterans or the coaching program, not personally to DJ Skyler, except where the KB explicitly supports personal delivery; identity questions are answered from the KB without embellishment.
- R5. When the visitor shows booking intent (asked about the call, eligibility answered, or explicitly asks to book), the bot offers the booking flow rather than more information.

**Booking**

- R6. The bot can present available slots and complete a booking into a GHL calendar for the free 30-minute strategy session, creating the GHL contact and appointment.
- R6a. The public booking surface carries GHL's native spam protection (reCAPTCHA/bot filter), enabled on the strategy-session calendar.
- R6b. The booking flow surfaces a privacy notice naming Just4Veterans' privacy policy as the data-controller notice before collecting name, email, or phone in the chat.
- R7. The existing booking modal and CTAs stay in place but are demoted to a labeled demo preview: the fake "You are on the books" confirmation is removed and the modal states that no appointment is created. The chatbot is the real booking path; the modal is not.

**Site integration**

- R8. The chat widget loads on the live page via the GHL embed snippet, renders correctly at desktop and 375px mobile widths, and does not regress the hero sequence, chapters, or modal.
- R9. The page ships through the established deploy path (local smoke test, SFTP byte-verified upload, live browser verification).

**Knowledge base**

- R10. The KB is built from just4veterans.com service and education pages: home, about, va-claims-consulting, va-disability-rating-calculator (text content), j4v-foundation, disclaimer. (URLs are resolved at crawl time in U2: as of 2026-11-21, /j4v-foundation 301-redirects to home and is dropped from the crawl if still redirecting.)
- R11. The KB excludes the coach roster (roughly 119 profile pages with real people's names), privacy policy, terms, and contact pages.

### Actors

- A1. Veteran visitor — asks questions, may book. Reached only through the public page.
- A2. Chatbot (DJ Skyler persona) — answers from KB, carries compliance, drives to booking. Operated by GHL Conversation AI.
- A3. GHL platform — hosts bot, KB, widget, calendar; creates contact and appointment on booking.
- A4. Site operator (user) — owns GHL agency account, deploys page changes, verifies.

### Key Flows

- F1. Question to booking
  - **Trigger:** A1 opens the widget and asks a claims or service question.
  - **Steps:** A2 answers from KB with compliance framing where relevant; A1 signals readiness; A2 offers the call; A1 picks a slot; A3 creates contact and appointment; A2 confirms with details.
  - **Outcome:** Booking exists in the GHL calendar; A1 saw a confirmation.
- F2. Deflection
  - **Trigger:** A1 asks for legal or medical advice, a rating prediction, or something off-topic.
  - **Steps:** A2 declines in persona, gives the safe general answer from KB, steers back to the call.
  - **Outcome:** No advice given; conversation continues or ends politely.
- F3. Quiet visitor
  - **Trigger:** A1 reads without asking.
  - **Steps:** None. No proactive pop-up interrupting the cinematic scroll (non-goal, see Scope Boundaries).
  - **Outcome:** Page behaves exactly as today.

### Success Criteria

- The scripted test matrix in the Verification Contract passes on the live URL, including booking end-to-end into the GHL calendar.
- Three consecutive red-team runs produce compliant answers (no invented facts, no advice, compliance lines present when due).
- The page at 375px and desktop shows no layout regression with the widget present.
- Chatbot-sourced appointments in the GHL calendar are counted over a defined post-launch window and compared against the modal-only baseline period, so the plan records whether the bot changed the booking rate.

### Scope Boundaries

- Non-goals: wiring the existing booking modal to a real booking backend (it becomes a labeled demo preview per R7, not a real booking path); proactive chat pop-ups; embedding the bot on any other page or site; per-user or per-contact data lookups from the KB (vector search cannot do exact row matching; there is no per-user data in this KB anyway).
- Deferred to Follow-Up Work: wiring booked leads into a GHL nurture workflow; handing the bot to the client's own sub-account when the engagement is signed; Spanish-language answers.

### Open Questions

- Q1 (blocking for U1): Which GHL location hosts the bot and calendar — the agency demo location or a new J4V-dedicated location. Assumed: a new J4V-dedicated location under the agency account, so leads are clean.
- Q2 (check during U1): Whether the agency plan includes the Conversation AI add-on or it must be enabled per location.

### Sources

- `https://just4veterans.com/` content and sitemaps (page inventory: 9 pages excluding the ~119 coach profiles and post archive; R10 names 6 in-scope pages, R11 excludes privacy policy, terms, and contact).
- Live GHL MCP hub check: token and location not configured on this machine (`token_configured: false`, `location_configured: false`) — U1 resolves this.
- Institutional learning: ghl-conversation-ai-kb skill (KB trigger precedence, prompt-save verification, vector-vs-exact-match limits).
- Deploy invariants: static-site-deploy skill (byte-level live verification).

---

## Planning Contract

### Key Technical Decisions

- KTD1. **GHL Conversation AI bot in the agency account.** (session-settled: user-directed — chosen over a custom simulated bot: native KB crawler, calendar booking, and widget; nothing new to host.) Owns the bot mechanism for R1, R8.
- KTD2. **Native calendar booking inside the chat.** (session-settled: user-directed — chosen over modal handoff: real booking is the goal.) Owns R6.
- KTD3. **KB via GHL web crawler, scoped to 6 pages.** The crawler handles public websites; the roster and legal pages are excluded to keep real names and boilerplate out of answers. Rationale: a scoped crawl beats importing the whole site and hoping the prompt avoids roster answers. Owns R10, R11.
- KTD4. **Compliance is prompt engineering plus test matrix, not a content filter.** The bot prompt carries the verbatim compliance lines and refusal rules; the red-team matrix in U6 is the enforcement. Known pitfall from the institutional learning: KB trigger must not be "active for all queries" if workflows are added later, and the saved prompt must be verified by reload after editing. Owns R2, R3.
- KTD5. **Widget embed is the GHL-generated snippet before the closing body tag, loaded as-is.** GHL's snippet is async by default; no custom lazy-loading wrapper until a measured problem exists. Owns R8.

### Assumptions

- The GHL agency account can create a dedicated location and enable Conversation AI (Open Question Q2).
- AJ (the client) approves the bot speaking real J4V service facts under the DJ Skyler persona (settled direction from this session; confirm at handoff).
- The just4veterans.com pages in scope are crawlable (public, no login).

### Risks & Dependencies

| Risk / Dependency | Mitigation |
|---|---|
| GHL Conversation AI add-on not on the plan (cost/availability) | U1 verifies before any build; fallback is to defer to client sub-account |
| Bot answers beyond the KB (hallucinated rating promises) | KTD4 prompt rules + U6 red-team; compliance lines mandatory |
| Widget conflicts with the fixed-hero layout or mobile viewport | U5 verifies at 375px and desktop before declaring done |
| GHL credentials not on this machine | U1 prerequisite; user authorizes the GHL connection once |
| J4V site content changes make KB stale | Re-crawl after client content updates; note in Operational Notes |

### Sequencing

U1 (access) unblocks U2-U4 (GHL-side config in any order, calendar before bot). U5 (embed + deploy) after U4. U6 (test matrix) last and gates done.

---

## Implementation Units

### U1. GHL access and J4V location

- **Goal:** Working, authorized GHL access with a dedicated J4V location ready for bot configuration.
- **Requirements:** advances all R-IDs (prerequisite).
- **Dependencies:** none.
- **Files:** none in repo (GHL-side configuration only).
- **Approach:**
  1. Authorize GHL (MCP hub token + location, or the ghl-operator hub CLI per its skill).
  2. Create/select the J4V-dedicated location under the agency account (Open Question Q1).
  3. Verify the Conversation AI feature is available on the location (Open Question Q2); record the outcome in the session.
  4. Verify the Conversation AI bot exposes a calendar/booking action that can present available slots and create the contact and appointment in chat; if it does not, record the fallback shape (workflow-assembled booking, or handoff to the U3 public booking link) before U2-U4 proceed.
- **Test expectation:** none -- pure account/location configuration; the proof is that U2's crawler step can run against this location.
- **Verification:** GHL calls authenticated against the location; a probe read (e.g. location details) returns live data.

### U2. Knowledge base from just4veterans.com

- **Goal:** A searchable KB in the J4V location containing only the in-scope J4V content.
- **Requirements:** R10, R11.
- **Dependencies:** U1.
- **Files:** none in repo.
- **Approach:**
  1. Resolve each R10 URL over HTTP: confirm it returns its own page content rather than redirecting to another scoped page; drop any that redirect and record the final scoped list (as of 2026-11-21, /j4v-foundation redirects to home).
  2. Add a website KB source crawling the resolved scoped URLs from R10.
  3. Run the crawl; confirm ingestion per source (GHL shows indexed sources).
  4. Verify exclusion of roster/legal content by querying the KB for a coach roster fact and confirming no result.
- **Patterns to follow:** the ghl-conversation-ai-kb institutional learning: website crawling works; do not rely on this KB for exact-match lookups later.
- **Test scenarios:**
  - Happy path: KB search for "what services do you offer" returns the four services listed on the va-claims-consulting page.
  - Edge case: KB search for a coach roster name returns nothing from the roster (content absent).
  - Integration: each resolved scoped URL appears as an indexed source in GHL.
- **Verification:** the two probes above return expected results against the live KB.

### U3. Strategy-session calendar

- **Goal:** A GHL calendar "Free 30-minute strategy session" with availability, connected to the location, emitting booking notifications.
- **Requirements:** R6, R6a.
- **Dependencies:** U1.
- **Files:** none in repo.
- **Approach:**
  1. Create the calendar: 30-minute slots, coach availability windows, assignment to the coach user.
  2. Enable notifications (email/SMS per agency default) to the coach on new bookings.
  3. Take the calendar's public booking link as a fallback URL (consumed by the U4 booking-failure rule).
  4. Enable GHL's native spam protection (reCAPTCHA/bot filter) on the calendar.
- **Test scenarios:**
  - Happy path: a test booking through the calendar's own link creates a contact and appointment.
  - Edge case: booking outside availability is rejected by the calendar.
  - Integration: the booked test contact appears in the location's contacts with the appointment attached.
- **Verification:** the test booking exists in the calendar and contacts; notifications delivered.

### U4. Bot configuration

- **Goal:** The Conversation AI bot answers as DJ Skyler from the KB, carries compliance, and closes to the U3 calendar.
- **Requirements:** R1, R2, R3, R4, R4b, R5, R6, R6a, R6b.
- **Dependencies:** U2, U3.
- **Files:** none in repo.
- **Approach:**
  1. Set the bot prompt: persona (retired Navy Command Master Chief coach), goal (book the free 30-minute strategy session), the verbatim compliance lines from R2, refusal rules from R3, the attribution rule from R4b (services and outcomes attributed to Just4Veterans or the coaching program, not personally to Skyler), and the booking behavior from R5 (offer slots when intent shows; confirm with details after booking), plus a booking-failure rule: if booking fails (no available slot, or appointment creation error), apologize in persona and offer the U3 calendar's public booking link.
  2. Attach the U2 KB and restrict triggering so general questions route to the KB.
  3. Configure the booking action to the U3 calendar, with the R6b privacy notice (Just4Veterans' privacy policy named as data controller) shown before collecting name, email, or phone.
  4. Save, reload the page, and re-read the prompt to confirm the save took (institutional learning: the agent page can show a stale prompt).
- **Patterns to follow:** ghl-conversation-ai-kb learning on KB trigger precedence and prompt-save verification.
- **Test scenarios:**
  - Happy path: "what is the coaching process" answered from KB in persona, ending with a soft booking offer.
  - Happy path: "I want to book" leads to slot selection and a confirmed appointment in the calendar.
  - Edge case: "will I get 100%?" gets a refusal plus the compliance framing, no prediction.
  - Error path: off-topic question ("what stocks should I buy") gets a polite in-persona deflection back to the call.
  - Integration: booking made through the bot creates the same contact and appointment shape as the U3 direct booking.
- **Verification:** each scenario passes in the GHL bot test panel; a real browser conversation on the live page repeats the happy paths.

### U5. Widget embed and deploy

- **Goal:** The GHL chat widget loads on the live J4V Top Coach page on desktop and mobile without regression.
- **Requirements:** R7, R8, R9.
- **Dependencies:** U4.
- **Files:** `j4v-top-coach/index.html`.
- **Approach:**
  1. De-simulate the booking modal per R7: remove the fake "You are on the books" stamped confirmation, label the modal as a demo preview that creates no appointment, and keep the flow otherwise intact.
  2. Paste the GHL widget embed snippet immediately before the closing body tag in `j4v-top-coach/index.html`.
  3. Serve locally and smoke test: page loads, widget renders, modal (demo preview) and hero unaffected.
  4. Deploy per the static-site-deploy skill (SFTP byte-verify, nginx untouched).
  5. Verify live at desktop and 375px: widget visible and styled; hero sequence, chapters, and modal still work.
- **Patterns to follow:** static-site-deploy skill (read back every remote step; byte-level proof).
- **Test scenarios:**
  - Happy path: live page loads with the widget; opening it starts a bot conversation.
  - Integration: the existing modal flow (5 steps to the demo-preview end state per R7, no fake confirmation) still passes end-to-end on the deployed page.
  - Edge case: with the launcher and an open chat window present, the widget does not cover the CTA or modal content at 375px or desktop, and the modal's flow stays fully visible and operable.
- **Verification:** live URL shows the working widget; desktop and mobile screenshots reviewed; modal regression passes.

### U6. Conversion and compliance test matrix

- **Goal:** Repeated, scripted proof that the bot converts booking intent and never breaks compliance.
- **Requirements:** R1-R6 (acceptance).
- **Dependencies:** U5.
- **Files:** none in repo (test runs recorded in session; failures filed as fixes to U2-U5).
- **Approach:**
  1. Run the scripted matrix on the live page: FAQ answers (3), booking end-to-end (2, including a mobile-width run), booking failure recovery (1: force or simulate a failure and assert the bot offers the U3 public booking link), deflections (3: rating prediction, legal advice, off-topic), compliance probes (2, checking the compliance lines appear when representation is discussed).
  2. Repeat the full matrix three times; non-deterministic drift between runs is a bug signal per the institutional learning.
  3. Fix and re-run until three consecutive clean passes.
- **Test scenarios:** the matrix rows above, each naming input, expected behavior, and expected outcome.
- **Verification:** three consecutive clean passes logged; any single failure resets the count.

---

## Verification Contract

| Gate | Proof |
|---|---|
| Deploy | Every changed file byte-matched on the server; live URL serves the widget page |
| Widget live | Opens and converses on desktop and 375px at the live URL |
| Booking | Bot-made appointment exists in the GHL calendar with contact attached |
| Compliance | Three consecutive red-team passes: no advice, no invented facts, compliance lines present when due |
| No regression | Existing modal flow passes on the deployed page ending in the R7 demo-preview state (no fake confirmation), with the widget launcher and an open chat window present at desktop and 375px; hero/chapter screenshots unchanged |

## Definition of Done

- U1-U6 all verified per the Verification Contract.
- No launch-blocking Open Question remains (Q1 resolved at U1; Q2 verified or explicitly deferred with user consent).
- The live page at `https://tenaci-tech.com/portfolio/j4v-top-coach/` carries the working chatbot.
- Changes to `j4v-top-coach/index.html` committed and pushed to the templates repo.
- No abandoned-experiment code or snippets left in the page beyond the single widget embed.
