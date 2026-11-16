# Changelog

All notable changes to SignuDoc are recorded here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions follow
[Semantic Versioning](https://semver.org/). Until 1.0, a minor release may
change settings or the database in ways that need a migration step; each
release says when it does.

## [0.2.1] - 2026-11-16

A smoother first deploy. No database change: this release needs no
migration step.

### Deploying

- The Deploy button adds Resend first, then Neon and Blob, so Resend's
  sending domain has longer to be ready before the first deployment starts.
- The README and the self-hosting guide say what to do when Vercel stops the
  first deployment with "One or more integration resources failed to
  provision": wait, refresh and redeploy.
- New [guide for AI agents](docs/deploy-with-an-agent.md) that deploy
  SignuDoc for someone through their browser.
- The README's button now reads "Deploy to Vercel", with "or copy this to
  your LLM" under it, linking to that guide.

## [0.2.0] - 2026-11-15

Installs can now find and take new releases without using git. No database
change: this release needs no migration step.

### Keeping an install up to date

- **Check for updates** in Settings, SignuDoc (administrators only): shows
  the version the site runs, asks GitHub for the latest release, and shows
  what is new in it.
- **Update SignuDoc workflow** (`.github/workflows/update-signudoc.yml`) in
  each copy: runs every Monday or by hand, merges the new release with the
  copy on a `signudoc-update/<version>` branch and opens a pull request, so
  Vercel builds a preview first. Changes you made to the code are kept; where
  you and SignuDoc changed the same lines it stops and lists the files. It
  never force-pushes.
- Optional settings for the workflow: a `SIGNUDOC_UPDATE_TOKEN` secret so it
  can update workflow files too, `SIGNUDOC_AUTO_MERGE=1` to merge clean
  updates straight into production, and `SIGNUDOC_PRODUCTION_BRANCH` when
  Vercel deploys from a branch other than the default one.
- `docs/self-hosting.md` explains the workflow and a by-hand update for a
  Deploy-button copy, which shares no history with this project.

### Upgrading from 0.1.0

A copy made from 0.1.0 does not have the update workflow yet. Add
`.github/workflows/update-signudoc.yml` to it once (Settings in SignuDoc
links to the right page, and `docs/self-hosting.md` has the steps), then run
it. If it asks which version you deployed, use `v0.1.0`.

### Other changes

- The Code of Conduct file is removed.

### Not yet verified

- The update workflow is tested in CI against stand-in repositories. A real
  run in a Deploy-button copy has not been done yet.
- Everything listed under 0.1.0 as not yet verified still is.

## [0.1.0] - 2026-11-14

The first open-source release. SignuDoc is an e-signature platform that an
organization deploys to its own Vercel account and runs on free plans.

### Deploy and run it yourself

- One-click **Deploy to Vercel** button that provisions Neon Postgres, Vercel
  Blob and Resend from the Vercel Marketplace, all on their free plans.
  Vercel keeps its free Hobby plan for personal, non-commercial use, so a
  company belongs on Vercel Pro ($20 a month, whatever its size).
- Setup wizard on first visit: creates the first administrator, asks for the
  install's own support email, and fills in the email sending domain (with a
  `sign.` subdomain by default).
- Daily Vercel cron for reminders, deadlines and retention, rate limits kept
  in Postgres, and optional Vercel BotID (`BOTID=1`).
- Runs locally with no outside services for development and tests.
- Copyright Nyllon Software, released under the license in `LICENSE`.

### Sending and signing

- Soft Studio interface for the dashboard, field placement, envelopes and the
  signing page, with first-page thumbnails and a phone layout.
- Signers sign without an account. A signer's extra check is one choice: an
  emailed code or an access code.
- Text, date, number, checkbox, dropdown and radio fields; signing order;
  CC recipients; deadlines with expiry; optional reminders.
- Templates for documents sent often ("Save as template", "Use template").
- Branding with your own name, logo and color, and an administrators panel.

### Records you can rely on

- Electronic-records consent disclosure and a permitted-use list, both
  versioned, to meet US ESIGN/UETA and EU eIDAS expectations for simple
  electronic signatures.
- Hash-chained audit trail and a completion certificate on every envelope.
- Finished documents sealed with a PAdES signature (an install's own key, or
  your certificate through `SEAL_P12_BASE64`), with an RFC 3161 timestamp when
  `SEAL_TIMESTAMP_URL` is set.
- PAdES B-LT and B-LTA: revocation data and a document timestamp are added
  after the seal so it stays checkable for years.
- Retention settings with a warning before anything is deleted, and signer
  erasure requests.

### Regulated use

- **Part 11 mode** for FDA 21 CFR Part 11 records: signers sign from a
  verified account and enter their password at signing, each signature
  carries its meaning, and settings changes go to their own audit chain with
  CSV export. A validation pack in `docs/part11/` gives an install a start on
  its own validation.
- **Signer advanced and qualified signatures (AdES/QES)**: a signer can sign in
  their own name through eID Easy or any Cloud Signature Consortium (CSC v2)
  provider, with the result embedded in the PDF.

### Not yet verified

These parts are built and tested against stand-ins in CI, but have not been
checked against the real services:

- A full Deploy-button pass on a clean Vercel account, step by step against the
  README. A first run surfaced Vercel's "Needs Attention" badge on Neon
  variables, which `docs/self-hosting.md` now explains.
- Live eID Easy and CSC providers for signer AdES/QES.
- A real timestamp authority and certificate authority for the seal and for
  B-LT/B-LTA.
- Part 11 mode and the validation pack are a starting point: each
  organization still has to validate its own install.

SignuDoc is software, not legal advice. See the README's legal disclaimer.

[0.2.1]: https://github.com/NYLLON-SOFTWARE/SignuDoc/releases/tag/v0.2.1
[0.2.0]: https://github.com/NYLLON-SOFTWARE/SignuDoc/releases/tag/v0.2.0
[0.1.0]: https://github.com/NYLLON-SOFTWARE/SignuDoc/releases/tag/v0.1.0
