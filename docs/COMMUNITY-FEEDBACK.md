# r/NoopBand — what users actually complain about

Reviewed on 2026-09-29. Input for the macOS focused-presentation work described in
[`docs/plans/2026-09-14-noop-personal-dashboard.md`](plans/2026-09-14-noop-personal-dashboard.md).

## Method and limits

reddit.com refuses every direct path available here (HTTP 403 on the JSON endpoint, a network-security
block in a headless browser, and the domain is not fetchable by the agent's fetch tool). The listings and
threads below were read through a public Reddit mirror (`safereddit.com`, a redlib instance).

Read in full: the *hot* listing (25 posts), the *top of all time* listing (25 posts), and four threads —
"Noop is too complicated…" (23 comments), "NOOP design changes" (62 comments), "Sleep performance is
fake" (17 comments), "Data comparison: noop vs. Whoop / Amazfit Helio" (2 comments). Not every post or
comment in the subreddit was read, and vote counts are a weak proxy for how widely a view is held.

## The top-line complaint is the one this work exists to fix

"Noop is too complicated and difficult to understand for regular WHOOP users" (2026-09-21, 16 up,
24 comments). The author's own summary:

> There is simply too much information, functionality, and terminology, much of which isn't clearly
> explained. […] it can be overwhelming to figure out what everything means, what is actually important,
> and what I'm supposed to do with the information.

Two clarifications the author had to add, both worth keeping:

- They are **not** asking for advanced metrics to be removed — "the default experience should be easier to
  understand, and the advanced stuff can still be there for people who want it".
- Discoverability, not absence, is the problem: the per-metric "i" explainer and the Today *Edit*
  customization both already exist, but "you sometimes have to already know what you're looking for".

The thread's own suggestions: a **toggle for advanced metrics**, and a first-run **guided walkthrough**.
One reply notes the working subset is already "Today, Trends and Sleep" — which is the three-destination
shape the plan proposes, minus Trends.

## Missing, stale and untrustworthy data is the majority experience

Roughly a third of current posts are a metric showing nothing, or showing the same thing forever:
"The app isnt showing Blood Oxygen", "Cant see my Charge and HRV", "Sleep tracking doesn't seem to update
at all", "Sleep not being tracked?", "Noop not loading sleep", "Sleep doesn't track at all", "No Data
problem", "Nothing is changing here", "Whoop 4.0 doesn't record blood oxygen".

The sharpest case is **"Sleep performance is fake"** (2026-07-12, 13 up, 17 comments): the score sat at
94/100 every day, including after a five-hour night at a festival. Replies confirm the pattern and
separate what is trustworthy from what is not:

> I have sleep score stuck on 94 but the sleep stages and duration change day to day.

> I will say sleep duration works fine. But phases are definitely not accurate.

> it really feels like you can't trust much of anything on Noop. Sleep stages, sleep score, rest score,
> stress, steps…

One user found the score only started varying after **turning off the WHOOP 5 experimental probes** — an
experimental setting silently degrading a headline number.

Separately, "Data comparison: noop vs. Whoop / Amazfit Helio" asks the question this raises: HRV and
resting HR differ materially between apps, and "which data should I trust?" (tracked as issue #1451).

## Design implications, in priority order

1. **Do not lead a screen with a composite score.** The one metric users call fake is the sleep
   percentage; the quantities they still trust are durations and timings. A sleep screen should lead with
   measured duration against estimated need, and never borrow the name of the discredited score for a
   ratio that is merely duration ÷ need.
2. **Design the empty and stale states as first-class screens**, not as a fallback. For most posters they
   *are* the screen. Each needs the specific missing basis named and the relevant action offered — which
   is what the plan's guidance contract already requires, applied to the whole screen rather than one card.
3. **Explain at the point of display.** An explainer reachable only from an "i" fails the person who does
   not know a word is a term of art. A short plain sentence under each headline number, naming the numbers
   behind it, costs one line and removes the need to hunt.
4. **A simple/advanced switch, not only a reorganised sidebar.** The community asked for it by name. It
   should govern secondary metrics on the three screens as well as the collapsed navigation group.
5. **State what a number is worth, inline.** Method, sample size and calibration state belong beside the
   value: "reference building, 3 of 14 nights", "stages approximate, on-device". This is the cheapest
   available answer to "you can't trust anything in it".
6. **Surface experimental toggles as a data-quality condition.** If an experimental probe can flatten a
   score, the screen showing that score should say the probe is on.
7. **First run is the whole first impression.** "My whoop subscription ends in a week" is a recurring
   arrival story: people install on the day their subscription lapses, with no history and no baselines,
   and decide there and then.
8. **Device identity and sync legibility.** "Noop app read device as 4.0 despite it actually being 5.0",
   "Does Noop work on Whoop 5.0", and reports that the strap sometimes needs nudging before it offloads.
   Compare with the `DeviceFamily.forRegistryModel` rule in [`CLAUDE.md`](../CLAUDE.md).
9. **Card reordering is wanted** ("Is there a way we can move cards around to customise the orders?") and
   partly exists; it is a discoverability problem more than a feature gap.

## Two warnings before building

**Copying the WHOOP interface is a risk the community itself raises.** In the "NOOP design changes"
thread, a request to "represent data like whoop original app" drew:

> It's risky to do that, the project could get shut down by lawyers

> Whoop will go after it to get it taken down if it is whoop like

This is independent of the scope rules in `CLAUDE.md`, and points the same way: take the information
hierarchy, not the branded presentation or the proprietary score names.

**The macOS UI is already being worked on upstream.** `liaolo`'s iOS redesign
([#1068](https://github.com/ryanbr/noop/pull/1068)) **merged** on 2026-08-09 — it is the version the
Reddit thread was praising, and it is in the 11.6.0 source this work is based on. Its macOS follow-up,
[#1078 "Bring macOS UI toward iOS redesign parity"](https://github.com/ryanbr/noop/pull/1078), is **still
open** (opened 2026-08-05, last updated 2026-08-30) and covers readable columns, Week-in-review gauges,
Liquid Glass paths and a `docs/MACOS_UI_CUSTOMIZATION_LEDGER.md`. Any macOS presentation change should be
checked against that branch first: the choice is to build on it, or to knowingly diverge and say why.
