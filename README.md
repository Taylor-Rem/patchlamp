# Patchlamp

**A website you change by texting it.** The owner of a small business texts
a number. Patch, the operator, opens the site's files, makes the change,
checks that it is live, and texts back, usually in a minute. "Undo that"
reverts it. Anything Patch should not handle alone, such as a domain, a store
or a login, goes to me.

Live at [patchlamp.com](https://patchlamp.com), for home-service businesses
in Utah, on monthly plans ([current prices](https://patchlamp.com/pricing)).
Built and run by one person, me, working with Claude. Operating since
September 2026.

**Why this repository has no code.** Patchlamp is a running business. The
site, the relay and the toolbelt are private because they are the product.
This page is the part a reader can evaluate: what it is, how it is built, how
big it is, and the rules it is built under. Every number here was measured by
a script on the date shown.

## How it works

```
 owner ──SMS / Telegram / Discord──▶ the relay ──▶ Claude Code, inside that
                                       │           one client's folder, with
                                       │           that folder's permissions
                                       ▼
                                   the toolbelt: images, sites, databases,
                                   printables, documents, payments, posting
                                       │
                                       ▼
                           the client's site, live on Cloudflare Pages
                           (and patchlamp.com: sign-up, account, chat)
```

Three parts.

- **The site**, patchlamp.com: Laravel, deployed on Laravel Cloud. The pages,
  the sign-up wizard, the account page, the chat bubble.
- **The relay**: Python, run by systemd on my own machine. It picks up a
  message, works out who sent it and which project they may touch, runs
  Claude Code in that project under that project's permissions, and replies
  on the same channel. Every transport pulls: it polls Twilio, long-polls
  Telegram and opens an outbound connection to Discord. No webhook, no open
  port, no tunnel, no public URL on the machine that runs the agent.
- **The toolbelt**: small command-line tools Claude calls for the work a
  text can ask for. Stock photos and generated images, new sites from
  templates, a site's own database, printable QR cards and menus, reading
  the documents people send, invoices on the owner's own Stripe, a photo and
  a line posted to Facebook, Instagram, X and Google at once.

**A client is a directory.** One business, one folder, one git history, one
permission file. A sender is mapped to a role, the projects they may use and
a daily limit. Inside a client's project Claude may edit that folder, run
git on its own repos, call the toolbelt and fetch the site's own live URL.
Nothing outside.

## The rules it is built under

- **Every claim on a page is literally true of the code that runs.** A
  claims file maps each sentence about behaviour to the line of code that
  makes it true. A sentence with no row does not ship.
- **Prices live in one file.** A test fails if a page stops showing what
  the constants say, or if a retracted claim comes back.
- **Nothing activates itself.** A payment opens an account; I approve it.
- **Spending limits are enforced by the tools, not the prompt.** Image and
  video budgets come from environment variables the tool itself checks, so a
  chat cannot talk its way past them.
- **Credentials never reach the model.** The toolbelt reads its keys from a
  key file and never prints them, so Claude can use a service without seeing
  the credential.
- **Visits are counted without a cookie**, and the privacy page describes
  exactly that count.
- **Tests pass before a change is done**, style is checked, both run in CI,
  and a push to `main` is a production deploy.

## Stack and integrations

PHP 8.3 and Laravel 13 with Blade and Pest. Python 3.13 and systemd. Claude
Code with a per-project manual and permission file. Integrations: the Claude
API, Twilio, Telegram, Discord, Stripe Checkout and Connect, Google Places and
Google Business Profile, Meta Graph for Facebook and Instagram, the X API,
Resend for email, Cloudflare Pages and D1, Laravel Cloud, QuickBooks Online.

## By the numbers

Measured 2026-10-12 over the working copies, vendored directories excluded.
Test files are the files under `tests/` named `*Test.php` or `test_*.py`.

| part | commits | first to last commit | code | test files |
|---|---|---|---|---|
| the site | 263 | 2026-09-15 to 2026-10-12 | 30,630 lines PHP, 3,982 Blade | 49 |
| the relay | 195 | 2026-09-12 to 2026-10-12 | 63,338 lines Python | 55 |
| the toolbelt | 342 | 2026-09-14 to 2026-10-12 | 36,472 lines Python, 3,259 JS, 3,188 HTML | 77 |

Eight hundred commits and about 130,000 lines of application code and tests
in four weeks, by one person directing Claude. I own the architecture, the
review and the verification; the tests are what let a change be done.

## About me

I am Taylor Remund, a full-stack engineer in American Fork, Utah. Patchlamp
is the most ambitious thing I have built. More at
[taylorremund.com](https://taylorremund.com) and the
[résumé page](https://taylorremund.com/resume/).
