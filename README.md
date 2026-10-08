# Civic-Chain: Tax Increment Financing (TIF) Demo

Deployed at: https://tax-increment-financing.netlify.app/

How TIF works: freeze assessed value at designation = base; growth above it = 
captured assessed value (CAV); the mill rate on the CAV = increment revenue,
routed to a district fund instead of the general fund.

A town-agnostic, interactive demonstration of how a municipal **Tax Increment
Financing** district can be run as a transparent, independently-verifiable public
ledger, including the labor and pension conditions a community can attach to the
deal, and the ability for residents and trust funds to check the math themselves.

> **This is a demonstration.** Every figure is illustrative sample data. The
> three municipality profiles are archetypes, not real towns, and nothing here is
> legal, financial, or investment advice. The on-chain anchoring is modeled for
> the demo (see [Verification](#6-verification-model)).

This repo is the **deployable static bundle** (a pre-built static export, no
build step, no server). The product source lives in the main CivicChain repo
under `app/tif/`.

---

## Contents

1. [What this demonstrates](#1-what-this-demonstrates)
2. [TIF in one page (the mechanics)](#2-tif-in-one-page-the-mechanics)
3. [The demo, tab by tab](#3-the-demo-tab-by-tab)
4. [The labor & pension module](#4-the-labor--pension-module-the-research-centerpiece)
5. [How the numbers are derived](#5-how-the-numbers-are-derived)
6. [Verification model](#6-verification-model)
7. [Research findings & grounding](#7-research-findings--grounding)
8. [Deploy & regenerate](#8-deploy--regenerate)
9. [Sources](#9-sources)

---

## 1. What this demonstrates

TIF is one of the most common, and most criticized, economic-development tools
in the United States. The criticism is almost always about **opacity**: residents
can't see how much public tax revenue was diverted, who received it, what was
built, or whether the promises attached to the deal were kept.

This demo reframes a TIF district as a **public, tamper-evident ledger**:

- Every captured tax dollar and where it went.
- Every developer agreement and bond serviced from the increment.
- Every public project the increment funded.
- The **labor standards** attached to the deal (union labor, prevailing wage,
  local hire, apprenticeship) and **proof the pension/health contributions were
  actually paid**, quarter by quarter.
- A one-click **tamper check**: alter a published figure and watch the
  independently-recomputed fingerprint stop matching the anchored record.

It is deliberately **town-agnostic**. A profile selector swaps between three
archetypes so the same product reads for any municipality:

| Profile | Stands in for |
| --- | --- |
| Riverfront mill town | A post-industrial waterfront converting mills to mixed use |
| Suburban growth township | A highway-interchange logistics park funding town-center housing |
| Small-town main street | A single compact downtown redevelopment district |

---

## 2. TIF in one page (the mechanics)

When a municipality designates a TIF district, it **freezes** the assessed
property value inside the boundary at that moment. That frozen number is the
**base value**.

As private development raises assessed value, the amount **above the base** is the
**captured assessed value** (the "increment"). The municipality applies its
property-tax rate (the **mill rate**, expressed as dollars per $1,000 of value) to
that increment. The resulting **increment revenue** is routed into a district fund
instead of flowing to the general fund.

```
captured increment   = current assessed value − frozen base value
increment revenue    = captured increment × (mill rate ÷ 1000) × capture rate
```

That district fund is used to:

- **Reimburse developers** through a **Credit Enhancement Agreement (CEA)**, the
  instrument that hands back a negotiated share of the increment the project
  itself created.
- **Service TIF bonds** issued to pay for public infrastructure up front.
- **Pay for public improvements**: roads, utilities, parking, streetscape,
  public realm.

The **capture rate** is the policy share of the increment routed into the fund
(vs. shared back to the general fund). At the end of the district's **term**
(commonly 20–30 years), the district closes and the **full, grown value returns to
the normal tax rolls**, the public payoff for the deferral.

**A Maine-specific wrinkle worth knowing:** sheltering the captured value also
lowers the town's *state-reported valuation*, which protects its position in
state revenue sharing, county tax apportionment, and school-subsidy formulas.
This is a real reason Maine municipalities use TIF beyond the direct reimbursement.

---

## 3. The demo, tab by tab

A profile selector (top right) and a district selector (Districts tab) drive the
entire view.

- **Overview**: the selected district's base-vs-captured value composition, an
  assessed-value sparkline since designation, mill rate / capture rate / term
  stats, and a **live projection slider**: set an assumed annual growth rate and
  see projected increment revenue across the remaining term.
- **Districts**: a selectable table of every district on the ledger (captured
  value, annual revenue, **debt-coverage ratio**, anchor status).
- **Funding**: the project **capital stack** (developer equity, pension-fund
  co-investment, CEA, bonds, grants) and, where present, a **pension co-investor**
  card. Below it: the outstanding obligations (CEAs/bonds/grants), coverage ratio,
  and increment-funded public projects.
- **Labor & benefits**: the centerpiece; see [section 4](#4-the-labor--pension-module-the-research-centerpiece).
- **Verification**: the anchored fingerprint, ledger tx/block, and an interactive
  **tamper check**.

---

## 4. The labor & pension module (the research centerpiece)

This module exists because of a real policy conversation: a selectboard discussing
tying **pension funds and union-only labor** into a TIF-backed project. That
phrase maps onto **two distinct, legitimate mechanisms**, and the demo models
**both as one connected loop.**

### Reading A: labor standards attached to the incentive

A municipality can condition the CEA on a **Project Labor Agreement (PLA)**, a
pre-hire collective-bargaining agreement that uses **union signatory contractors
only**. Those contractors pay **prevailing wage *plus* a fringe-benefit rate**,
and that fringe is **employer contributions into bona-fide pension, health, and
training (apprenticeship) funds**, i.e. union **multiemployer (Taft-Hartley)
pension funds**.

So "pension funds tied into the project" means the TIF-subsidized jobsite is
**routing real benefit dollars into construction workers' pension funds**, and
the incentive can be **clawed back** if the standard isn't met.

The demo models this per district:

- PLA / prevailing-wage flags, **local-hire** and **apprenticeship** targets.
- Quarterly **certified payrolls** showing total vs. union hours, local-hire %,
  apprentice %, gross wages, and the **pension / health / training contributions**
  remitted, with the named trust fund.
- A **holdback / clawback**: a payroll that misses the local-hire or apprentice
  target triggers a configurable holdback of that quarter's CEA payment until a
  compliant, anchored payroll is filed. (The "Town Center Housing" district has a
  deliberately non-compliant quarter so you can see the holdback fire.)

### Reading B: a pension fund as a co-investor

Large public and union pension systems invest in real estate and infrastructure
as **Economically Targeted Investments (ETIs)** under a **Responsible Contractor
Policy (RCP)**, a hiring preference for contractors who pay fair wage, **employer-
paid health and pension**, and run apprenticeships (in practice, union labor).

The demo puts a **pension fund inside the capital stack** on the Funding tab, with
its commitment, vintage, target return, and RCP flag, so you can see the fund
*financing* the project, not just receiving benefit contributions from it.

### The loop

Put together, the two readings close a loop the module makes explicit:

> **Pension capital funds the build → the RCP/PLA requires union labor → the union
> jobsite remits wages and benefits back into the pension/health/training funds →
> every step is anchored and independently verifiable.**

---

## 5. How the numbers are derived

All figures are computed from a small set of inputs with pure functions
(`tif-data.ts` in the source repo), so the data stays internally consistent:

- **Captured increment** = `max(0, currentValue − baseValue)`.
- **Annual increment revenue** = `increment × (millRate / 1000) × captureRate`.
- **Coverage ratio** = `annual increment revenue ÷ annual debt service` (above
  1.0× means the district services its obligations from its own increment).
- **CEA reimbursement cap** = 65% of the increment for economic-development
  districts, **up to 100% for affordable-housing** districts (modeled on Bangor's
  policy, below).
- **Quarterly payroll** is generated from a per-hour wage + fringe package
  (`wage`, `pension`, `health`, `training`) × hours, so wages and contributions
  always reconcile.
- **Holdback** = `missed quarters × quarterly CEA payment × holdback %`.
- **Projection** compounds current assessed value forward at the slider's growth
  rate to the end of term and recomputes increment revenue each year.

---

## 6. Verification model

Each district publishes a **fingerprint** computed over the fields a resident
could independently re-derive from the public record (base value, current value,
mill rate, capture rate, designation year, term). The demo recomputes that
fingerprint live; the **tamper check** alters a figure and shows the recomputed
fingerprint diverging from the anchored one. The same anchoring is applied to the
quarterly certified payrolls, so the trust funds can confirm every pension dollar
was actually remitted.

> **Honesty note for engineering review:** in this standalone demo the fingerprint
> is a fast deterministic hash (FNV-1a) computed in the browser, and ledger
> tx/block identifiers are illustrative. It demonstrates the *verification UX and
> data model*: anchor a canonical record, let anyone recompute and compare, not
> a live blockchain write. The production CivicChain platform is where real
> anchoring lives.

---

## 7. Research findings & grounding

The model is grounded in real Maine statute and a real municipal policy, so the
mechanics hold up to scrutiny.

**Bangor, Maine TIF policy** (City of Bangor Community & Economic Development):

- Captured Assessed Value → TIF Revenues; worked example uses **75% capture at an
  $18.55 mill rate**.
- **Credit Enhancement Agreements**: maximum **average 65%** reimbursement over the
  term, **up to 100% for affordable housing**; term negotiated to match the
  developer's private financing. Worked example: *$70,000 new taxes × 65% × 10
  years = $455,000* reimbursed.
- **1% annual administrative fee** to the city (minimum $250), deducted before
  remittance.
- **Eligibility**: minimum **$1,000,000** new real-property investment **or 6
  affordable-housing units**; a **job-creation plan** (number *and* quality of
  jobs) is required; scored against **13 public-benefit criteria** including
  "significant long-term employment," "public benefits for other workers," and
  "supports local contractors and suppliers / job training / internships."
- **Labor clause**: an applicant must not have "engaged in illegal or unfair labor
  and employment practices", the statutory hook a community uses to attach labor
  standards.

**Maine statute:**

- Municipal TIF: **30-A M.R.S. §5221–5235**; affordable-housing TIF: **§5245+**
  (higher capture allowed).
- **Employment TIF (ETIF)** is a *separate* state program that reimburses a share
  of **state income-tax withholding** for net-new jobs, distinct from the
  property-tax TIF modeled here (a natural future district type).
- **Prevailing wage**, **26 M.R.S. Chapter 15**: an hourly wage *plus* a fringe-
  benefit rate, where the fringe is paid as employer contributions into bona-fide
  pension/health plans (or cash equivalent).

**Pension + labor mechanisms:**

- **Project Labor Agreements (PLAs)**: pre-hire CBAs that set union wages and
  benefits for a project.
- **Responsible Contractor Policies**: adopted by the largest public pension
  funds (e.g., NY State Common Retirement Fund, CalPERS) to prefer contractors
  paying fair wage + employer-paid health + pension + training.
- **Economically Targeted Investments**: pension allocations (e.g., NYC's ~2%)
  directed to local/workforce housing; union housing investment trusts channel
  pension capital into union-built affordable housing.

---

## 8. Deploy & regenerate

**Deploy (static, no build):**

- **Netlify**: publish directory `.` (repo root), no build command. `netlify.toml`
  is already set. Or drag the repo folder onto the Netlify "Sites" page.
- **Any static host** (GitHub Pages, Cloudflare Pages, S3, nginx), serve the repo
  root at the domain root. Asset paths are absolute (`/_next/...`, `/brand/...`),
  so the bundle must sit at the root, not a subpath.
- `index.html` is the exported demo served at `/`; it is fully client-rendered, so
  these files are all that's needed.

**Regenerate after a source change** (source lives in the main CivicChain repo at
`app/tif/`, Node ≥ 20):

```bash
# in the main repo
npm run build
cp out/tif.html  <this-repo>/index.html
cp out/404.html  <this-repo>/404.html
rm -rf <this-repo>/_next && cp -r out/_next <this-repo>/_next
cp out/brand/civic-chain-logo.svg out/brand/civic-chain-social-preview.webp \
   out/brand/favico.png  <this-repo>/brand/
```

**Stack:** Next.js (App Router) static export, React, Tailwind CSS v4,
CivicChain's shared design system. No runtime backend.

---

## 9. Sources

- [City of Bangor: Tax Increment Financing & Credit Enhancement Agreement Policy (PDF)](https://www.bangormaine.gov/DocumentCenter/View/3044/CED---Tax-increment-Financing-TIF-Policy-PDF)
- [City of Bangor: TIF Project Application (PDF)](https://www.bangormaine.gov/DocumentCenter/View/3060/Housing---Tax-Increment-Financing-Application-PDF)
- [Tax Increment Financing in Maine: Michael Walker, Maine Law Review](https://digitalcommons.mainelaw.maine.edu/cgi/viewcontent.cgi?article=1455&context=mlr)
- [Tax increment financing (Maine): Wikipedia](https://en.wikipedia.org/wiki/Tax_increment_financing_(Maine))
- [Employment Tax Increment Financing: Maine DECD](https://www.maine.gov/decd/business-development/tax-incentives-credit/employment-tax-incentive-financing)
- [Maine Prevailing Wage (26 M.R.S. Ch. 15): Maine DOL](https://www.maine.gov/labor/labor_stats/publications/wagerateconst/prevailingwage/index.shtml)
- [Project Labor Agreements: AFL-CIO](https://aflcio.org/what-unions-do/empower-workers/project-labor-agreements)
- [Pension Fund Responsible Contractor Policy: AFL-CIO](https://aflcio.org/about/leadership/statements/pension-fund-responsible-contractor-policy)
- [Responsible Contractor Policy: NY State Comptroller (PDF)](https://osc.state.ny.us/pension/responsible-contractor-policy.pdf)
- [Economically Targeted Investments: NYC Comptroller](https://comptroller.nyc.gov/services/financial-matters/pension/responsible-investing/economically-targeted-investments/)
