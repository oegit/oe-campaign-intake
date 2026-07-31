# Campaign Intake

A single, self-contained form that produces a validated `intake.json` for the Video Production
agent. It is published from the agent's own source file, unmodified.

## It ships empty, and that is the point

The page carries no project in it. Load your project's `intake-config.json` in the browser —
choose the file, drop it, or paste the JSON — and the form builds itself from that config:
its languages, channels, templates, KPIs, colours and fonts.

**The config never leaves your browser.** Not "we don't send it" — the page *cannot*. Its
Content-Security-Policy declares `connect-src 'none'` and `form-action 'none'`, so there is no
fetch, no upload, no form post and no socket available to it. Open the console and try; the
browser refuses.

Nothing is remembered between visits unless you tick the box that says so, and there is a
"Forget" control on every screen that can hold one.

## Verifying that this is the file it claims to be

The page is byte-identical to the agent's `templates/intake/intake.html`. That is checkable:

```sh
curl -s <this page's URL> | shasum -a 256
```

    071a5a15a43714f79b307a172159bb3aa48af78453bd7aba479ce8c887d71fd1

Built from source commit `b6afeb6fd941d6b44d508e38a263d4ea42b7af2f`.

If those disagree, the page you are looking at is not the one that was reviewed.

## What it does not do

It does not submit, validate against a server, or spend anything. It produces a document you
download and hand to the agent, which then runs its own Layer 2 cross-checks — reviewer,
sign-off, closings, template, ticket — before any credit is spent.
