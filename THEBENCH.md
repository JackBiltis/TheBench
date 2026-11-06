# TheBench: plan

Written 2026-11-06 for Jack Biltis from his brief of the same day. "TheBench" is the working name
(§12). Audience: Jack, deciding whether and how to build it, and later whoever builds it.

## 1. The idea, in Jack's words

A project "for lawyers and judges to rate each other (along with lawyers rating lawyers and judges
rating judges). They will pass a validation to ensure that they are really a lawyer or judge but
the ratings and comments will be anonymous unless they choose otherwise. The public will be able to
see consolidated ratings and reviews. The site will also prepare surveys for other lawyers/judges to
participate in the ratings system. All parties will also be able to rate individual decisions and
comment on the merits."

## 2. What exists and what is different

| Today | What it is | Its gap |
|---|---|---|
| Avvo, Justia, Martindale-Hubbell | Lawyer ratings; Justia and Martindale take peer reviews, Avvo mostly client reviews | Lawyers only; peer reviews are signed or opaque; no judges, no decisions |
| The Robing Room | Anonymous public ratings of federal and state judges | Anyone can post, so it is unverified and often ignored; judges only |
| Bar association judicial performance surveys (Arizona's JPR, Colorado's, many county bars) | Verified lawyer surveys of judges, published before retention elections | Once every few years, one jurisdiction, dense PDFs, no decisions, no lawyer ratings |
| Bench IQ, Pre/Dicta, Lex Machina | Analytics on how judges rule, sold to firms | Data, not opinion; expensive; one-way |

TheBench is the verified, anonymous, two-way version: only confirmed lawyers and judges rate; the
rater's identity is held but not shown; the public sees the consolidated picture; and a specific
decision can be rated on its merits, so a judge's score can be traced to rulings rather than to
mood. The verification is the product. Without it this is The Robing Room again; with it, ratings
carry the weight of a bar survey, continuously.

## 3. Who uses it

| Person | What they do |
|---|---|
| Lawyer (verified) | Rates judges they have appeared before and lawyers they have faced or worked with; rates and comments on decisions; answers surveys; reads peers' and judges' views before an appearance or a hire |
| Judge (verified) | Rates lawyers who appear before them and other judges (trial judges on appellate panels and vice versa); rates decisions; answers surveys |
| Public (anyone) | Reads consolidated ratings, review summaries and decision ratings; no posting |
| The rated (lawyer or judge) | Claims their profile, posts one response per review, corrects facts, opts into showing a reviewer's name when the reviewer allowed it |

## 4. Scope

### 4.1 Who can be rated

- **Lawyers**: anyone with an active or recent license in a US state bar. Profiles seeded from
  public bar directories (name, bar number, admission date, status, firm), never from scraping
  behind a login.
- **Judges**: federal (Article III, magistrate, bankruptcy) and state (trial and appellate),
  seeded from court directories and the Federal Judicial Center's biographical database.
- **Decisions**: a published opinion or order, identified by citation or a link to CourtListener
  (free, open case law from the Free Law Project), with the judge or panel attached.

### 4.2 Verification

Verification is one-time per account, and the proof is kept separate from everything the account
posts (§7). Methods, in order of preference:

1. **Bar email**: a message to the email on file with the state bar (where the bar publishes one).
2. **Bar-record match plus a document**: bar number, name and admission date matched against the
   public directory, plus a photo of the bar card or a court-filed document with the lawyer's name,
   checked by a person at first and by a model later.
3. **Judges**: a message to the court's official email domain (uscourts.gov, a state judiciary
   domain), or a letter to chambers with a code.
4. **Fallback**: a short live video call with staff.

Each verification is re-checked yearly against the bar's status (suspended or disbarred accounts
lose posting rights; their old ratings stay but are flagged in the aggregate).

### 4.3 Ratings

Rating criteria are fixed per role so results aggregate (the criteria list lives in one table,
`Criterion`, and nothing else defines it). First draft, borrowed from established judicial
performance surveys so results are comparable with them:

| Rating a judge | Rating a lawyer | Rating a decision |
|---|---|---|
| Legal knowledge; impartiality; temperament and courtesy; preparation; timeliness of rulings; clarity of orders; management of the courtroom; settlement skill (trial judges) | Legal skill; preparation; candor with the court; civility; responsiveness; writing; advocacy; ethics | Reasoning sound; result right; writing clear; precedent handled fairly; practical impact |

Each criterion is 1 to 5, with a written comment (optional, prompted). A rater must state the
basis: appeared before the judge, opposed the lawyer, co-counsel, supervised, read the decision.
Judges rating lawyers may do so only for lawyers who appeared before them. Self-rating, rating your
own firm, and rating a judge on a case you are currently in front of are blocked (the account's own
stated conflicts and bar firm data catch the obvious cases; the rest is honor plus reporting).

### 4.4 Anonymity

Default: the rater is shown only as "verified lawyer, civil litigation, 15+ years" (role, practice
area band, experience band, and only when the band holds at least ten people, so no one is unique).
A rater may sign any individual review. Identities are never shown to the rated person, to staff
outside the verification team, or in exports. §7 has the design.

### 4.5 Consolidated public view

The public sees, per lawyer or judge: the number of ratings, the per-criterion averages with the
sample size, the distribution, the trend over time, excerpts of comments chosen by the model as
representative (not the best or worst), and the rated person's response. A profile with fewer than
**five** ratings shows no score, only "too few ratings yet", so one grudge cannot define a page.
Decision pages show the same for a decision, with a link to the full text on CourtListener.

### 4.6 Surveys

The site writes and runs surveys so the ratings reach beyond people who show up on their own:

- **Scheduled**: before a retention or contested judicial election, every verified lawyer in that
  jurisdiction gets a survey on the judges on the ballot; results publish as a report, with the
  methodology, on a fixed date.
- **Triggered**: when a judge or lawyer reaches a milestone (new appointment, 100 appearances
  logged by raters), their counterparties are asked.
- **Requested**: a bar association, a court, or a firm can commission a survey of its own members
  or panel; results go to them and, if they choose, publish.

Surveys are the same criteria as ratings, so answers feed the same aggregates.

### 4.7 Responses, disputes and takedowns

- The rated person gets one public response per review, and a private "report" button:
  factual error, wrong person, conflict, retaliation, harassment. Reports go to a review queue.
- A review is removed only for: the rater not being who they said, a conflict rule broken, personal
  information, threats, or a court order. "I disagree" is answered by the response, not removal.
- A written policy explains all of this in plain language and is applied the same way to every
  profile, with a public log of removals (counts, not content).

## 5. Why it is legally workable, and where it is not

- **Section 230.** User reviews are third-party content; the platform is not the publisher
  (the Avvo and Yelp cases went this way; the one loss, *The Dirty*, was a site that solicited and
  egged on the offensive content, which is the behavior to avoid). What the site writes itself
  (survey reports, model-written summaries, the "representative excerpts" selection) is its own
  speech and has to be fair, accurate and opinion-labeled.
- **Anonymity and subpoenas.** A rated judge or lawyer can sue a "John Doe" and subpoena the
  identity. Courts apply a balancing test (*Dendrite*, *Cahill*) and usually protect opinion. The
  site's position: identities are released only on a court order after the rater has been notified
  and had a chance to oppose, and the architecture keeps the key in a place staff cannot casually
  read (§7). Say this in the policy so raters know the limit of "anonymous".
- **Judges rating lawyers.** Codes of judicial conduct restrict judges' public comment on pending
  matters and on lawyers in ways that could look like favoritism. A judge posting an anonymous
  rating of a lawyer is new ground; some state judicial ethics committees will advise against it.
  Design answer: judges rate only after a matter is closed, their ratings are aggregated with
  lawyers' in the public view (no "judges say" line until the sample is large), and the site asks
  for advisory opinions from two or three state judicial ethics bodies before judges can post in
  those states.
- **Lawyer advertising rules.** A lawyer's profile with ratings is not the lawyer's advertisement,
  but a lawyer who claims a profile and quotes the rating elsewhere is subject to their state's
  rules on "ratings" (several states require the rating's methodology to be disclosed). The site
  publishes its methodology for that reason.
- **Retaliation.** The fear that keeps lawyers quiet on The Robing Room is that a judge figures out
  who wrote it. The practice-area and experience bands (§4.4) and the five-rating floor exist for
  this. In small jurisdictions the floor will mean many judges show no score for a long time;
  that is the right trade.
- **State bar scraping.** Bar directories are public but many have terms that forbid bulk copying;
  seed from the ones with open data or APIs, ask the rest, and let lawyers claim and complete their
  own profiles.
- **Verification data** (bar cards, court filings, video) is sensitive; it is deleted once the
  account is verified, keeping only "verified on date by method".

## 6. Revenue

Not from the rated and not from placement. Options, in the order to try:

1. **Commissioned surveys and reports** for bar associations, courts and judicial nominating
   commissions, which pay consultants for this today.
2. **Pro subscriptions for firms**: detailed judge pages (trend, decision ratings, how peers in a
   practice area rate), export, alerts on a judge's docket, with the public view staying free.
3. **Claimed-profile tools** for lawyers (response drafting, rating badge with the methodology
   link), modestly priced, never affecting the score.
4. No ads in the first years; a legal-services ad on a rating page looks like the thing the site is
   trying not to be.

## 7. Architecture and the anonymity design

| Layer | Choice |
|---|---|
| Runtime, hosting | Node 24+, TypeScript, Postgres, Fly.io with Cloudflare in front, same as the other WetSpark web products |
| Model | Claude for comment moderation, summary excerpts, document checks in verification, survey report drafting; every model output is labeled and the raw data is always one click away |
| Case law | CourtListener API (Free Law Project) for decisions, judges and citations |
| Sign-in | Email magic link, Google, Apple; password sign-in with Show Password |

**The anonymity split.** Two databases, two Fly apps, two sets of credentials:

- **Identity store**: `Person` (name, bar number, verification method and date, contact) and a
  one-way `RaterKey` derived from the person with a secret pepper held only by that app.
- **Ratings store**: `Rating`, `Comment`, `SurveyAnswer`, keyed by `RaterKey`, never by `PersonId`.
  Bands (practice area, experience) are copied into the ratings store at verification time as
  coarse values, not joined live.

The public site and staff tools read only the ratings store. Nothing in it can be joined back to a
person without the identity app's pepper, and that app has no endpoint that returns a name for a
`RaterKey`; the join exists only as an offline procedure run by Jack (or counsel) on a court order.
Logs are scrubbed of IPs after seven days. This is what lets the site say "anonymous" honestly.

## 8. Data model

Follows `SQL.md`: proper case, one capital per word, meaningful keys, each fact once.

Identity store:

| Table | Key | Holds |
|---|---|---|
| `Person` | `PersonId` | Sign-in, Role (lawyer / judge), Name, contact |
| `BarRecord` | `State, BarNumber` | PersonId, AdmissionDate, Status, StatusCheckedOn (refreshed yearly) |
| `Verification` | `PersonId, VerifiedOn` | Method, VerifiedBy; evidence deleted after approval |
| `Conflict` | `PersonId, Sequence` | Declared firms, co-counsel, pending matters, used to block rating |

Ratings store:

| Table | Key | Holds |
|---|---|---|
| `Subject` | `SubjectId` | A rateable lawyer or judge: public name, role, court or firm, State, BarNumber or court id, ClaimedOn |
| `Decision` | `DecisionId` | Citation, CourtListener id, Court, DecidedOn, judges on it (`DecisionJudge`) |
| `Criterion` | `CriterionCode` | The fixed criteria per role (§4.3); the only definition |
| `Rating` | `RaterKey, SubjectId` (or `DecisionId`) | Basis, SubmittedOn, Signed (bool), band values, Status (live / removed, with reason) |
| `RatingScore` | rating key + `CriterionCode` | Score 1–5 |
| `Comment` | rating key | Text, model moderation result |
| `Response` | `SubjectId, RatingKey` | The rated person's reply |
| `Survey` | `SurveyId` | Scope (jurisdiction, subjects), OpensOn, ClosesOn, PublishesOn, Commissioner |
| `SurveyInvite` | `SurveyId, RaterKey` | SentOn, AnsweredOn |
| `Report` | `ReportId` | RatingKey, Reason, FiledBy (subject or rater), Outcome |
| `SubjectSummary` | `SubjectId` | A **cache** of the public aggregates, rebuilt by one job after each change |

## 9. Phases

| Phase | Delivers | Done when |
|---|---|---|
| **0. Plan** (this document) | Folder, repo, plan, name check, the two legal questions below | Jack decides to go on |
| **1. One jurisdiction, lawyers rate judges** | Arizona (open bar directory, a published JPR program to compare with); verified lawyer accounts; judge profiles for Maricopa Superior Court, the Court of Appeals and the Arizona Supreme Court; ratings with the anonymity split; public consolidated pages with the five-rating floor; responses and reports | 200 verified Arizona lawyers and the first judges with scores |
| **2. Lawyers rate lawyers; decisions** | Lawyer profiles and peer ratings; decision pages from CourtListener; conflict rules; claimed profiles | Peer ratings live in Arizona |
| **3. Judges rate; surveys** | Judge accounts after ethics advisory opinions; scheduled retention-election surveys; commissioned surveys as the first revenue | A published survey report used by someone outside the site |
| **4. More states** | State by state, each with its bar data source and ethics check; firm subscriptions | |

## 10. Risks

- **Adoption is the hard part**, not code. Lawyers rate judges when they believe it is safe and
  matters; the five-rating floor and the bands buy safety, and a comparison with the official JPR
  results buys credibility. Seeding the first 200 lawyers in Arizona is a personal-network task.
- **A judge sues.** Likely at some point. Section 230 plus the removal policy plus the identity
  split is the defense; counsel reviews the policy before launch and the site carries media
  liability insurance.
- **Judicial ethics opinions go against judges posting.** Then phase 3 is lawyers-only surveys and
  judges read but do not rate; the product still stands.
- **Gaming**: a firm rating its own people up or an opponent down. Verification plus one rating per
  rater per subject per year, conflict declarations, anomaly checks on clustered ratings, and
  the public sample sizes.
- **Brand.** Like WokeAMeter, a site that judges judges colors anything else under the same name,
  and a site that lawyers and judges trust has to look independent of a venture studio's product
  line. See §11.

## 11. Identity and WetSpark

**Its own entity at launch** (Jack, 2026-11-06), not a WetSpark trade name, for two reasons: the
defamation and subpoena exposure should not sit on the company that owns the other products, and
independence is part of the product's credibility. Until then, a WetSpark lab project, private on
the site (listed in the portfolio with status `lab`, `identity: 'entity'`, public: false), the same
treatment as DocQuote and Private Court. Counsel and media-liability insurance before public launch.

## 12. The name

"TheBench" is the working name. Checked 2026-11-06:

- thebench.com, thebench.co, thebench.law, thebench.ai, the-bench.com and ratethebench.com are
  registered; thebenchrating.com is free.
- "Bench IQ" is a judicial-analytics company; "Judge-y" is a small judge-rating app; "The Bench" is
  also a common name for legal recruiters and bar newsletters.
- No USPTO search yet; do it before launch.

Keep the working name for the folder; pick the launch name with the entity name in phase 3.

## 13. Decisions (Jack, 2026-11-06)

1. Phase 1 scope: **open; Jack wants the options discussed** before choosing (see the WetSpark session of 2026-11-06).
2. Own entity at launch; WetSpark incubates it until then.
3. The repo is public.
4. WetSpark portfolio: `lab`, private (off the site).

## 14. Branches and pull requests

| Repo | Branch | Pull request | State |
|---|---|---|---|
| [JackBiltis/TheBench](https://github.com/JackBiltis/TheBench) (public) | main | none (Jack's own repo: commits go straight to main) | plan only |
