# GuardBoost Player Safety Process

## Decision summary

GuardBoost should use a layered safety process rather than a single verification step or safety score.

The two social cohorts remain completely separate:

- Players aged 13 through 17 may interact only with players in the same minor cohort.
- Players aged 18 and older may interact only with players in the adult cohort.
- Players younger than 13 cannot use social features.

For the initial operating stage, cohort access will use a private player attestation, with guardian confirmation for players in the 13-through-17 cohort. GuardBoost will retain its provider-verification capability and data boundary, but an external age or identity provider will not be required for ordinary access. Provider verification may be activated later as an optional additional assurance layer when user volume, observed abuse, legal requirements, and the expected safety benefit justify its cost. It must not be described as proof that a player is safe.

Every player must pass the minimum access controls. Additional safety indicators show which specific safeguards have been completed. They must never imply that GuardBoost has certified a person as safe.

For the minor beta, every meetup should:

- Be approved by the linked guardian.
- Take place at a public venue during approved hours.
- Be scheduled alongside other GuardBoost pairs when possible.
- Remain observable and easy for another person to interrupt.
- Give the guardian a clear, voluntary option to attend. GuardBoost should recommend attendance for selected interactions, including a first meetup with a new player, without making it mandatory.

Introduction videos and a credit-card requirement can both contribute to safety as supporting layers. A video gives the other player and guardian something to recognize, raises the effort required to create a convincing false account, and records how the player represented themselves. A card transaction links the account to a billing instrument, creates a transaction record, raises the cost of disposable accounts, and gives GuardBoost another enforcement point. Neither measure guarantees identity, guardianship, or safe behavior, and neither can replace cohort separation, guardian approval, public meetup planning, or reporting controls. Their value comes from the additional friction and accountability they add to the larger system.

## Process at a glance

```text
1 Eligibility gate
        ↓
2 Safety onboarding
        ↓
3 Same cohort discovery and invitation
        ↓
4 Meetup safety plan
        ↓
5 Guardian event approval for minors
        ↓
6 Public venue and group window assignment
        ↓
7 Arrival and optional guardian presence check
        ↓
8 Meetup and departure confirmation
        ↓
9 Post meetup safety check
        ↓
10 Report review and intervention when needed
```

The process stops whenever a required safeguard is missing, expired, revoked, or changed.

## Safety layers

| Layer | Purpose | Required or optional | Resulting indicators |
| --- | --- | --- | --- |
| Required access controls | Confirm that the player may enter the social system | Required | Core requirements complete, with details for Cohort attested, Guardian authorization current when applicable, Safety guide complete, and Social access active |
| Additional account signals | Add familiarity, account-creation friction, accountability, or optional assurance | Optional supporting layer, or a product access requirement | Introduction video available, Guardian joined introduction, Payment account linked when appropriate, and Provider verification completed if later offered |
| Meetup-specific safeguards | Describe the protections that apply to one event | Some required and some optional | Guardian reviewed, Public venue selected, Group window scheduled, Guardian plans to attend, Players checked in, Guardian present |
| Internal platform signals | Detect patterns and support enforcement without making public accusations | Platform-managed | No public indicator; may trigger limits, suspension, or human review |

No higher layer can replace a missing lower layer. For example:

- A video contributes familiarity and friction but cannot replace a current cohort attestation.
- A payment record contributes traceability and friction but cannot replace guardian approval.
- Optional provider verification may add age or identity assurance, depending on its documented scope, but cannot replace guardian approval, meetup controls, or human safety operations.
- A group meetup cannot permit cross-cohort contact.
- Optional guardian presence adds observation but does not replace the other required controls.

## Step 1 Eligibility gate

### Goal

Prevent ineligible or cross-cohort access before a player can discover, invite, or communicate with anyone.

### Required checks

1. The player is signed in and has a completed player profile.
2. The player has completed the current private age-cohort attestation.
3. The server-derived cohort is either `minor_13_17` or `adult_18_plus`.
4. The player has not been suspended from social features.
5. Social participation is explicitly enabled by the player.
6. A player in the minor cohort has current guardian confirmation of the cohort declaration and current guardian approval for social access.

The initial attestation process should:

- Reject social access when the declared age is younger than 13.
- Derive and store the cohort through a protected server action rather than trusting client-editable profile metadata.
- Store the attestation version, confirmation time, source, expiry or review date, and the date a minor must transition out of the minor cohort.
- Keep the attestation and transition data private.
- Prevent cohort changes through the ordinary profile editor.
- Rate-limit corrections and route suspicious or material changes to internal safety review.
- Pause minor social access when the player reaches 18 until the player completes adult consent and attestation.

GuardBoost should retain the existing provider-verification integration boundary so an external provider can be added without redesigning cohort separation. During the initial stage, provider verification is optional and must not be silently treated as a required field by eligibility, discovery, invitation, or meetup services.

### Outcomes

| Condition | Outcome |
| --- | --- |
| Attested as younger than 13 | No social access |
| Attested as 13 through 17 without guardian confirmation and approval | Guardian process only; no discovery or invitations |
| Attested as 13 through 17 with current guardian confirmation and approval | Minor social process |
| Attested as 18 or older | Adult social process |
| Attestation expired, revoked, contradicted, or placed under review | Social access stops immediately |
| Optional provider result conflicts with the attested cohort | Social access pauses for internal review; never silently move the player across cohorts |
| Player turns 18 | Existing minor access pauses until new consent and adult-cohort attestation are complete |

### Safety indicators created

- **Cohort attested** means the player completed the current private declaration and the server assigned a permitted cohort. For minors, the guardian also confirmed the declaration.
- **Guardian authorization current** means the minor has current social access approval from a linked guardian.

These indicators do not mean that the player's identity or exact age was independently verified, or that the player has been background checked or endorsed.

## Step 2 Safety onboarding

### Goal

Make sure the player and, for minors, the guardian understand the rules before social access begins.

### Minimum onboarding

1. Show a short safety video supported by the same guidance in text.
2. Explain what information other players can see.
3. Explain that exact ages, home addresses, schools, phone numbers, and private training locations must not be shared.
4. Show where the block, report, cancel, and guardian revoke actions are located.
5. Require a short knowledge check.
6. Ask the player to practice finding the block and report actions.
7. Require the guardian to complete a separate guardian version before approving a minor.

### Safety indicator created

- **Safety guide complete** means the current version of the guide and knowledge check have been completed.

The indicator expires when GuardBoost makes a material policy change that requires renewed training.

### Optional introduction layer

After onboarding, GuardBoost may offer a short introduction video. This is an optional familiarity and accountability layer, not a complete identity check or a replacement for minimum access requirements.

The video contributes to safety by:

- Giving the matched player and guardian a reference for who they expect to meet.
- Making a false or disposable profile more costly to construct at scale.
- Recording how the account holder presented themselves before the meetup.
- Giving a guardian another inconsistency to notice during event review or arrival.

These benefits remain limited because a recording can be staged, edited, generated, stolen, or made by someone other than the person who attends.

The recording prompt should request only:

- The player's chosen first name.
- Their playing style or position.
- One basketball skill or practice goal.

A suggested script is: **“Hi, I’m [chosen first name]. I play [position or style], and I’m looking to practice [skill].”**

It must not request a school, address, phone number, schedule, social handle, or other identifying information.

For minors:

- The guardian must consent before recording.
- The guardian may optionally join the recording, but their presence must not be required or treated as a higher safety level.
- The guardian may say that they know about GuardBoost and will review meetup plans.
- The video does not prove legal guardianship.

The video should be private, shown only after an invitation is accepted, protected from public links, reportable, and subject to expiry and deletion rules.

#### In-app recording experience

The first release should record the introduction inside GuardBoost instead of accepting a video selected from the device. This gives GuardBoost a predictable format, lowers accidental disclosure, and provides practical control over duration and file size.

The recording flow should:

1. Explain what the video is for, who can see it, and what must not be shared.
2. Request camera and microphone permission only after the player chooses to record.
3. Show a live preview, the suggested script, and a three-second countdown.
4. Display a visible elapsed-time counter while recording.
5. Allow the player to stop early and automatically stop the recorder at 15 seconds.
6. Require at least 5 seconds, recommend 10 through 12 seconds, and never require the player to fill the full 15 seconds.
7. Let the player privately review the result, record again, cancel, or submit it for review.
8. Stop all camera and microphone tracks when recording or previewing ends.

The initial technical limits should be:

- 480p target resolution.
- A controlled recording bitrate intended to produce a file of roughly 2 to 3 MB.
- A 5 MB hard file-size limit.
- MP4 or WebM, selected according to the format supported by the player's browser.
- One current introduction video per player.

There should be no ordinary camera-roll or file-upload control in the first release. GuardBoost may add an accessible fallback later, but it must enforce the same duration, size, privacy, consent, and review rules.

#### Upload and duration enforcement

The browser's recording timer is a user-experience control, not a complete security boundary. A modified client could bypass it. GuardBoost should therefore enforce the upload through several independent controls:

- The client automatically stops capture at 15 seconds.
- A server action confirms that the signed-in player is allowed to submit a video.
- The server issues a short-lived upload authorization for a random object path in a private Storage bucket.
- The bucket accepts only the approved media types and rejects files above 5 MB.
- The submission records the expected duration, media type, byte size, owner, consent version, and object path.
- The internal reviewer rejects malformed, manipulated, or overlong recordings.

For the first release, in-app-only capture, the Storage file limit, and internal review provide reasonable duration enforcement. Strict server-side validation of the media duration should be added later with a controlled media-inspection process before GuardBoost accepts file uploads or operates at larger scale.

#### Private storage, review, and playback

New videos should enter a private `pending_review` state. Before approval, only the player and authorized internal safety reviewers may access them. Approval creates the **Introduction video available** evidence; rejection must not create a negative public indicator.

After approval:

- The video is available only to the player, authorized safety reviewers, and the counterpart in an accepted same-cohort invitation.
- Playback uses a short-lived signed URL rather than a public object URL.
- The player interface does not autoplay the video or preload the entire file.
- Replacing the video returns the new recording to review; the prior approved version remains governed by the replacement and deletion policy.
- Suspension, revocation, expiry, or a safety intervention removes playback access immediately.
- Rejected uploads are deleted promptly unless temporary preservation is required for a safety investigation.
- Approved introductions expire after six months and must be recorded and reviewed again.

External YouTube, Instagram, or TikTok links should not be used for minor introduction videos. They can expose account names, comments, followers, unrelated content, and location clues outside GuardBoost's access controls.

#### Cost controls

Short in-app recordings keep infrastructure use predictable. GuardBoost should record at the controlled resolution and bitrate, retain only one current approved video plus any narrowly required review evidence, avoid autoplay, and load the video only when an authorized person presses play. Human review time is expected to become a larger operating cost than file storage, so the moderation queue should measure submission volume, average review time, replacement rate, and unresolved age of pending reviews.

### Optional safety indicators created

- **Introduction video available** means a private introduction video is available to the matched player.
- **Guardian joined introduction** means a guardian participated in the minor's private introduction.

## Step 3 Same cohort discovery and invitation

### Goal

Let eligible players find compatible practice partners without revealing unnecessary personal information.

### Discovery rules

1. Discovery is off by default.
2. The service returns only players in the requester's current server-controlled cohort.
3. Cross-cohort discovery and exact-handle invitations are rejected at the database or service boundary.
4. Block relationships apply in both directions.
5. Discovery shows only the chosen social name, handle, broad area, skill band, training interests, and applicable safety indicators.
6. Discovery does not show exact age, birth date, email, phone number, school, home address, exact venue, guardian identity, or verification-provider information.
7. The minor beta remains invite-only or limited to a trusted circle.

Within the 13 through 17 cohort, GuardBoost should also consider close-age matching. The product could use private bands such as 13 through 15 and 16 through 17, derived from the current attestation and guardian confirmation, without displaying exact age.

### Invitation rules

Invitations use structured fields for:

- Proposed date and time.
- Duration.
- Broad public area.
- Practice focus.
- A short restricted note.

The invitation must not include contact details, home addresses, ride requests, or unrestricted free-form coordination.

Open direct messaging should not be part of the initial release.

## Step 4 Meetup safety plan

### Goal

Turn an accepted invitation into a specific event that can be evaluated and approved.

### Required plan fields

- Date and start time.
- Duration.
- Public venue selected from an approved catalog.
- Venue operating or reliably busy hours.
- The expected participants.
- Whether the guardian voluntarily plans to attend.
- Whether the pair will join a shared GuardBoost venue window.
- Arrival and departure confirmation method.

Private homes, isolated locations, schools that identify a minor, and unapproved venues are not allowed.

GuardBoost should not allow players to arrange transportation, pickups, or home drop-offs through the app.

### Safety indicator created

- **Public venue selected** means the event uses an approved public venue during an approved time.

It does not mean that the venue or GuardBoost is supervising the event.

## Step 5 Guardian event approval for minors

### Goal

Require the guardian to approve the specific meetup, not only general social access.

### Guardian review

The guardian sees:

- The other player's display name and minor-cohort status.
- Applicable player safety indicators.
- The public venue and time.
- The practice duration.
- Whether either guardian currently plans to attend.
- Whether other GuardBoost pairs are expected.
- Any later change or cancellation.

The guardian can approve, decline, or revoke approval for the event.

The event must return to guardian review if any material element changes, including:

- The counterpart.
- The venue.
- The date or time.
- The group-window status.

A change to optional guardian attendance updates the event indicator and notifies the participants, but it does not by itself invalidate an otherwise approved meetup.

### Optional in-person participation

Guardian attendance remains optional and voluntary. A guardian can approve a meetup without attending, and GuardBoost should not reduce access, lower a player's standing, or imply negligence when a guardian declines.

GuardBoost should nevertheless make attendance easy and recommend it when it would add useful observation or confidence, particularly:

- For the first meetup with a new player.
- When either family has not used GuardBoost social meetups before.
- When no introduction video has been shared.
- At an unfamiliar venue.
- After a material plan change or a previous concern.
- Whenever the player or guardian would simply feel more comfortable with an adult present.

The guardian chooses whether to attend each interaction. The recommendation should be clear but must not use pressure, default opt-in, or a warning that suggests the meetup is unsafe without attendance.

### Safety indicator created

- **Guardian plans to attend** means the guardian has voluntarily indicated that they expect to attend this specific event.

This is an event indicator. It must not remain on the player's profile as a permanent claim. When no guardian plans to attend, the indicator is simply absent; GuardBoost should not replace it with a warning or negative mark.

## Step 6 Public venue and group window assignment

### Goal

Reduce isolation by scheduling several compatible pairs in the same public place and time window.

### Group-window process

1. GuardBoost offers a small catalog of approved public courts or gyms.
2. Each venue has approved days and time windows.
3. Several same-cohort pairs may choose the same venue window.
4. The app tells a pair that other GuardBoost players are expected without revealing the other players' identities or exact schedules.
5. Guardians are invited to attend and can indicate their plans voluntarily.
6. When guardians choose to attend, GuardBoost may coordinate overlapping attendance so the activity remains more observable without making adult presence a condition of participation.

### Safety indicator created

- **Group window scheduled** means at least two GuardBoost pairs are expected in the same public venue window.

It does not mean that every attendee is known, screened, or supervised by GuardBoost.

## Step 7 Arrival and optional guardian presence check

### Goal

Confirm that the expected players and venue match the approved plan, and record optional guardian presence when a guardian chose to attend.

### Check-in sequence

1. Each player confirms arrival in the app without sharing continuous GPS.
2. A guardian who voluntarily planned to attend may confirm arrival.
3. The app confirms that the expected counterpart checked in.
4. The app reminds players not to proceed if the person or venue differs from the approved plan.
5. Guardians receive a notification if a minor pair does not check in within the expected window.
6. If planned guardian attendance changes, the event is updated and the participants are notified. The guardian may leave the existing event approval in place or revoke it; the product must not presume cancellation merely because attendance was optional.

### Stop conditions

The meetup does not begin when:

- An unexpected adult or participant appears.
- The counterpart wants to move to another venue.
- The venue is closed, empty, or no longer observable.
- Anyone asks to keep a change secret or move communication to a private channel.
- A required approval or indicator has expired or been revoked.
- The player or guardian is no longer comfortable proceeding.

### Safety indicators created

- **Players checked in** means the expected participants confirmed arrival.
- **Guardian present** means a guardian who voluntarily chose to attend confirmed physical presence for the event.

Both indicators expire when the meetup ends.

## Step 8 Meetup and departure confirmation

### During the meetup

- A voluntarily attending guardian should remain close enough to observe and interrupt the interaction while respecting the players' reasonable independence.
- The meetup stays in the approved public area.
- Players do not leave together in a vehicle arranged through GuardBoost.
- A player or guardian can cancel, block, or report at any time.

### Departure

1. Each player confirms departure.
2. A guardian who attended may confirm departure.
3. The meetup closes automatically after both sides complete checkout or after a defined timeout.
4. Event-specific indicators are removed from active status.

## Step 9 Post meetup safety check

### Goal

Identify concerns that may not have been reported during the meetup.

GuardBoost asks each participant privately:

1. Did the meetup take place?
2. Did the expected player attend?
3. If a guardian planned to attend, did that guardian arrive?
4. Did the venue and time match the plan?
5. Did you feel safe?
6. Do you want to meet this player again, block them, or report a concern?

Responses remain private. GuardBoost should not publish player safety ratings or popularity scores.

Repeated no-shows, venue changes, attempts to move off-platform, unwanted recontact, and reports should be reviewed as patterns.

## Step 10 Report review and intervention

### Goal

Respond consistently when a player or guardian raises a concern.

| Severity | Examples | Immediate product action | Operating response |
| --- | --- | --- | --- |
| Urgent | Immediate danger, missing minor, credible threat, suspected abuse | Show emergency guidance, restrict implicated accounts, preserve necessary records | Immediate escalation under the applicable safeguarding and legal process |
| High | Sexual message, coercion, stalking, repeated off-platform contact | Suspend social access, block contact, preserve relevant evidence | Priority human review and guardian contact when appropriate |
| Standard | Harassment, unsafe venue change, no-show, repeated rule violation | Block or restrict contact and cancel affected meetups | Review within a published response target |
| Low | Confusing profile, outdated indicator, minor conduct issue | Correct the state or remove the indicator | Track recurrence and improve guidance |

Before expanding the minor cohort, GuardBoost needs:

- A named safety reviewer.
- Published internal response targets.
- An after-hours process for urgent reports.
- Evidence-retention rules.
- A documented guardian-contact process.
- Market-specific guidance for mandatory or external reporting.

## Safety indicator rules

GuardBoost should call these **safety indicators**, not badges, trust levels, or safety scores.

Every indicator must:

- Describe one factual safeguard.
- Explain what evidence supports it.
- Explain what it does not prove.
- Expire automatically with its evidence.
- Disappear immediately when revoked.
- Use neutral styling rather than a green shield or ranked score.
- Avoid implying that safety can be purchased or that a paid player is inherently safer.
- Be tappable so players and guardians can read its definition.

The interface should display the layers separately. It must not combine them into one score, rank, percentage, or claim that a player is safe.

### 1 Required access controls

These are gates rather than achievements. A player cannot enter discovery or send invitations while a required control is missing, expired, revoked, or suspended.

On a discovery or matched-player card, GuardBoost may collapse them into one neutral indicator:

- **Core requirements complete** means all requirements for this player's cohort are current.

Tapping the indicator shows the underlying controls:

| Indicator | Meaning | Does not mean |
| --- | --- | --- |
| Cohort attested | A current private declaration produced a server-controlled minor or adult cohort; a guardian confirmed the declaration for a minor | Exact age or identity was independently verified, exact age is public, or the person is safe |
| Guardian authorization current | A minor has current guardian approval for social access | The guardian approved this meetup or will attend |
| Safety guide complete | The current guide and knowledge check were completed | The player will always follow the rules |
| Social access active | The account is opted in and is not currently suspended | No concern has ever been reported |

Because every discoverable player must satisfy these controls, they should not confer higher status or compete for prominence.

### 2 Additional account signals

These optional signals contribute familiarity, friction, or traceability. They are not identity-assurance levels.

| Indicator | Meaning | Does not mean |
| --- | --- | --- |
| Introduction video available | A private short introduction is available to the matched player | Identity verified, background checked, or guaranteed to match the attendee |
| Guardian joined introduction | A guardian participated in the minor's private introduction | Legal guardianship independently proven or guardian attendance promised |
| Payment account linked | The account completed the required card transaction or payment-verification step | The payer is the player, the payer is the guardian, identity was verified, or the player is safe |
| Provider verification completed | An optional external provider completed the specifically described age or identity check | The person is safe, background checked, guaranteed to attend, or exempt from any other safeguard |

Provider verification should initially remain a private assurance and risk signal or appear only after matching. If GuardBoost displays it, the explanation must identify exactly what the selected provider checked and when it expires. GuardBoost must not use a generic **Verified** badge or allow the signal to create a higher public safety rank.

Payment should be handled according to its product role:

- If payment is required for every social participant, it is an underlying access and enforcement layer rather than a distinguishing public indicator.
- If payment is optional, GuardBoost should initially keep it as a private risk signal or reveal it only after matching. A public payment indicator could wrongly imply that paid players are safer.
- If it is displayed, **Payment account linked** must appear as a neutral supporting signal with an explanation of what it does and does not establish.

### 3 Meetup-specific safeguards

These indicators describe the current event. They must be recalculated whenever the participant, venue, time, group window, approval, or optional attendance plan changes.

| Indicator | Meaning | Does not mean |
| --- | --- | --- |
| Guardian reviewed | The linked guardian approved this minor's specific meetup plan | The guardian will attend or independently vetted the other player |
| Public venue selected | The event uses an approved public venue and time | The venue or GuardBoost supervises the meetup |
| Group window scheduled | Several pairs are expected in the same public venue window | Every attendee has been screened or will arrive as planned |
| Guardian plans to attend | A guardian voluntarily indicated that they expect to attend this minor event | Attendance is required or the guardian is currently present |
| Players checked in | The expected players confirmed arrival | Their identity was newly verified in person |
| Guardian present | A voluntarily attending guardian confirmed physical presence | Guardian attendance is mandatory or GuardBoost provides supervision |

When no guardian plans to attend, the optional indicator is simply absent. GuardBoost must not show a warning, incomplete state, or negative mark.

### 4 Internal platform signals

Some safety layers should influence GuardBoost's risk controls without becoming public indicators:

- Payment failures, reversals, or unusual payment-account changes.
- Repeated account creation or linked-account patterns.
- Unusual invitation, area-change, or recontact velocity.
- Repeated declines, blocks, cancellations, or no-shows.
- Attempts to exchange prohibited contact or location information.
- Reports under review and enforcement history.

These signals may trigger rate limits, additional review, temporary restrictions, or suspension. GuardBoost must not expose them as public accusation, reputation, or popularity data.

### Recommended display

A matched player or guardian could see:

```text
Safety layers

Required access
✓ Core requirements complete

Additional account signals
✓ Introduction video available
✓ Guardian joined introduction

This meetup
✓ Guardian reviewed
✓ Public venue selected
✓ Group window scheduled
○ Guardian plans to attend
```

Every indicator should open a short explanation answering:

1. What does this mean?
2. What evidence created it?
3. What does it not prove?
4. When was it last confirmed?
5. When does it expire?

### Indicator visibility

| Surface | What to show |
| --- | --- |
| Discovery card | Core requirements complete; avoid a long badge row |
| Matched-player or invitation view | Core requirements plus applicable additional account signals |
| Meetup plan | Event-specific safeguards and any optional guardian-attendance plan |
| Guardian view | Full meanings, evidence dates, expiry, changes, and approval controls |
| Operator view | Full evidence state, private risk signals, reports, and enforcement history subject to role-based access |

### Do not use

- Bronze, silver, gold, or other safety levels.
- A numerical score or percentage.
- Stars, rankings, or streaks.
- **Trusted player**, **Safe player**, **Verified safe**, or **GuardBoost approved**.
- A **No reports** indicator or public report count.
- Red or incomplete styling when an optional signal is absent.
- A single composite badge that hides which layers are actually present.

## Assessment of the proposed measures

| Proposed measure | Recommendation | Place in the process |
| --- | --- | --- |
| 10-to-15-second in-app introduction video | Optional supporting safety layer that adds familiarity, evidence, and account-creation friction; use a 15-second hard stop, private storage, and internal review | After safety onboarding and before an accepted pair meets |
| Introduction video with guardian | Recommended supporting safety layer for minors that adds guardian participation and another consistency check | After guardian consent and before event approval |
| Guardian participates in person | Recommended voluntary supporting layer for selected interactions, especially a first meetup with a new player, an unfamiliar venue, or whenever either family wants added observation | Guardian event review, optional attendance choice, and optional check-in |
| Credit-card paywall | Supporting safety and anti-abuse layer that adds economic friction, a billing-account link, a transaction record, and another enforcement point; it does not prove identity or guardianship | Social-access gate after the core eligibility and guardian checks, never instead of them |
| Several pairs at the same place and time | Strong default for minors | Group-window assignment |
| Prominent safety guide and video | Required baseline | Before social access and again before the first meetup |

## Additional no-cost or low-cost safeguards

- Keep the minor beta invite-only or restricted to known families.
- Prefer close-age matching inside the 13 through 17 cohort.
- Use a curated public venue catalog rather than player-entered addresses.
- Restrict minor meetups to daylight or normal staffed hours.
- Copy all minor coordination and changes to the linked guardian account.
- Offer a simple voluntary guardian-attendance choice and recommend it contextually without preselecting it or penalizing a decline.
- Prevent transportation and pickup coordination.
- Use manual check-in and checkout instead of continuous location tracking.
- Require renewed guardian approval after material event changes.
- Rate-limit invitations, area changes, repeated declines, and recontact attempts.
- Automatically expire cohort attestations, optional provider verification, guardian approval, videos, invitations, venue confirmations, and event indicators.
- Ask players to practice finding block and report controls during onboarding.
- Use a private post-meetup safety check instead of public player ratings.
- Suspend new social activity while a serious report is under review.
- Review repeated behavior across events, not only individual reports.
- If social access requires payment, use the payment account as one risk and enforcement signal while retaining the same cohort, guardian-approval, venue, group-scheduling, and reporting requirements.

## Recommended rollout

### Stage 0 Private family beta

Required before use:

- Server-enforced cohort separation based on private attestation, with guardian confirmation for minors.
- Current guardian approval for minors.
- Trusted-circle access.
- Public venues.
- A voluntary guardian-attendance option with clear recommendations for selected interactions.
- Safety guide.
- Block and report controls.
- A named report reviewer.

### Stage 1 Grouped minor venue windows

Add:

- Curated venue windows.
- Event-specific guardian approval.
- Optional guardian-attending and grouped-meetup indicators.
- Arrival and departure confirmation.
- Event updates and notifications when previously planned voluntary attendance changes, without automatic cancellation.

### Stage 2 Limited local minor beta

Proceed only after:

- The cohort-attestation, guardian-confirmation, transition-at-18, correction, and internal-review controls are operating as designed.
- Incident-response targets are being met.
- Privacy and safeguarding review is complete.
- Market-specific legal review is complete.
- Product data shows that group and guardian controls are functioning as intended.

### Stage 3 Broader social release

Proceed only when GuardBoost has evidence that the controls work at the intended scale and can support ongoing human review.

At this stage, GuardBoost should evaluate whether optional provider verification is justified. Activation should require a documented decision covering user volume, abuse and incident patterns, market-specific legal requirements, provider scope, privacy and retention, accessibility, failure handling, and total cost. Provider verification may remain optional unless law, demonstrated risk, or a separately approved product policy requires otherwise.

## Decisions required before implementation

1. Define the interactions for which GuardBoost should recommend voluntary guardian attendance, such as a first meetup with a new player, while keeping attendance optional in every case.
2. Choose between close-age preferences inside one minor cohort and explicit 13 through 15 and 16 through 17 matching bands.
3. Decide whether introduction videos remain optional after the pilot.
4. Define the approved venue catalog and hours.
5. Decide whether a guardian may voluntarily designate another trusted adult to attend and how that choice is communicated without making attendance mandatory.
6. Approve the safety-indicator names, definitions, visibility, and expiry behavior.
7. Define the measurable conditions under which optional provider verification should be introduced, what it would check, and whether the result remains private or appears after matching.
8. Name the safety reviewer and approve the response and external-reporting process.

## Sources and basis

- [U.S. Center for SafeSport Minor Athlete Abuse Prevention Policies](https://uscenterforsafesport.org/training-and-education/minor-athlete-abuse-%20prevention-policies/)
- [U.S. Center for SafeSport 2025 MAAPP Manual](https://maapp.uscenterforsafesport.org/wp-content/uploads/2024/12/2025-MAAPP-Manual-Final.pdf)
- [Sport Integrity Australia Safeguarding Children and Young People Policy](https://www.sportintegrity.gov.au/what-we-do/national-integrity-framework/safeguarding-children-and-young-people-policy-overview)
- [Sport Integrity Australia Digital Communication](https://www.sportintegrity.gov.au/dos-and-donts/digital-communication)
- [Sport Integrity Australia Supervision](https://www.sportintegrity.gov.au/dos-and-donts/supervision)
- [Australian eSafety Commissioner Safety by Design](https://www.esafety.gov.au/industry/safety-by-design)
- [United Kingdom Information Commissioner's Office Age Appropriate Design Code](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/childrens-information/childrens-code-guidance-and-resources/age-appropriate-design-a-code-of-practice-for-online-services/)
- [United Kingdom Information Commissioner's Office Nudge Techniques](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/childrens-information/childrens-code-guidance-and-resources/age-appropriate-design-a-code-of-practice-for-online-services/13-nudge-techniques/)
- [U.S. Federal Trade Commission COPPA Frequently Asked Questions](https://www.ftc.gov/business-guidance/resources/complying-coppa-frequently-asked-questions)

These sources provide useful safeguarding and privacy principles. GuardBoost still needs jurisdiction-specific legal and safeguarding review before expanding access for minors.
