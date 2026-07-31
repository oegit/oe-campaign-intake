# Campaign Intake

A single, self-contained form that produces a validated `intake.json` for the Video Production
agent. It is built from the agent's own source file.

## What this page carries

It is built for **Open English** and opens straight on that project's
form — no file to load, no chooser. What travelled here is an **allowlist**, decided once in the
agent's `scripts/sanitize-intake-config.js`: languages (code, label, market and reviewer *status*),
voice roles and providers, templates, KPIs, durations and ratios, brand colours and fonts, and the
budget ceiling.

What deliberately did **not** travel:

- `deliveryDriveDefault` — published empty. a Drive folder id — process-inbox.js injects the real value from Layer 2 at ingestion
- `_note`
- `brand.fontsHref`
- `_brand_note`
- `_channels_note`
- `_languages_note`
- `voices[].name`
- `voices[].verified`
- `_voices_note`
- `music.alternatives`
- `_kpis_note`
- `_sceneCostEstimate_note`
- `_templates_note`
- `brandCharacters`
- `_brandCharacters_note`
- `_delivery_note`

Reviewer names, reviewer emails, voice ids, Drive folders and measured spend are not on this page.
That is not a promise about intent — the build refuses to write itself if any of them appears.

**Where your answers go.** Pressing **Start Project** sends the intake — and only the intake —
to one submission endpoint, which files it for the person who runs the agent. That destination is
the *only* one this page can reach: its Content-Security-Policy names those two origins in
`connect-src` and nothing else, and keeps `form-action`, `base-uri`, `object-src` and
`default-src` at `'none'`. Nothing is sent until you press the button, and if the send fails the
**Download intake.json** button comes back with the reason — a filled form is never lost.

The page carries a submission token. **It is not a secret** — it is in the source of a public page,
so anyone looking at this can read it. It exists to stop crawlers and stray posts. What protects the
inbox is that the endpoint only writes, validates what it is given, caps the size, and returns no
data to anyone.

Nothing is remembered between visits unless you tick the box that says so, and there is a
"Forget" control on every screen that can hold one.

## Verifying that this is the file it claims to be

This page is **reproducible**: the agent's `templates/intake/intake.html` with one literal
replaced by the allowlisted config above. Rebuild it and the hashes must match.

```sh
curl -s <this page's URL> | shasum -a 256
node scripts/publish-intake.js --project open-english --endpoint <url> --token <token> --out <dir>
```

    d726cfb02a21f3182c498f05c22b32a2fdff70da5e2ed925387b2de27dad342f

Built from source commit `afc49c973f37c6d8dd9c79b92dd83768d25d95b6`.

If those disagree, the page you are looking at is not the one that was reviewed.

## What it does not do

It does not validate against a server or spend anything. It produces a document the agent then
runs its own Layer 2 cross-checks over — reviewer, sign-off, closings, template, ticket — before
any credit is spent.
