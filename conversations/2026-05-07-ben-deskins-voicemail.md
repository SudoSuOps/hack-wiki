---
type: conversation
slug: 2026-05-07-ben-deskins-voicemail
name: "Ben Deskins · voicemail · dial follow-up to 2026-05-06 email"
ts: 2026-05-07T13:00:00Z
channel: voicemail
direction: inbound
broker_slug: brokers/secure-net-lease/ben-deskins
lead_slug: leads/2026-05-06-ben-deskins-3-deals
relationships:
  - rel: BETWEEN
    target: leads/2026-05-06-ben-deskins-3-deals
  - rel: FROM_BROKER
    target: brokers/secure-net-lease/ben-deskins
tags: [voicemail, dial-follow-up, inbound, first-touch-arc]
---

# Ben Deskins · voicemail · 2026-05-07

## Channel

Inbound voicemail · machine speech-to-text transcribed (errors preserved verbatim).

## Verbatim transcription

> "Hey Donovan, Ben Deskin, hope you're doing well. Wanted to give you a quick call. I thought you took a look at a Starbucks deal in Ingleside, corporate cricket, NSA, that were currently, um, selling order to feed out a buyer in mind for that. Um, not to pick an email with some of the deals that were, um, focused on right now and looking to move, um, feel free to uh, give me a call back when you can. Would be happy to have that conversation with you."

## Decoded

Speech-to-text errors:
- "Ben Deskin" → Ben Deskins
- "corporate cricket" → corporate credit
- "NSA" → NNN
- "selling order to feed out a buyer" → selling, looking to find a buyer

Cleaned:

> Hey Donovan, Ben Deskins, hope you're doing well. Wanted to give you a quick call. Wondered if you'd taken a look at the Starbucks deal in Ingleside — corporate credit, NNN — that we're currently selling, looking to find a buyer for. Wanted to ping you with another email of some deals we're focused on right now and looking to move. Feel free to give me a call back when you can. Would be happy to have that conversation with you.

## Doctrine read · the dialer's playbook in the wild

- **Email yesterday → dial today** = email + dial = MAGIC G (Grind) · multi-channel touch
- **Anchor on Ingleside** (highest cap of 3 · 6.35) = classic broker move · lead with the most-likely-yes deal to qualify the buyer
- **Soft tease** (more deals coming) = keeps the relationship warm
- **Soft close** ("call me back when you can") = low pressure · no demand
- This is exactly the [Harvey playbook](../doctrine/q1-harvey-canonical-scene.md) running from the OTHER side of the desk · we are the target prospect on Ben's 300/day list

## Our response

[Email reply sent 2026-05-07 15:18 UTC](2026-05-07-our-email-reply-to-ben.md) · gold pair #1 sealed.

Call-back script ready · prepared by `/leads` skill · stored at `/data1/leads/secure-net-lease/ben-deskins-callback-script-001.md`.
