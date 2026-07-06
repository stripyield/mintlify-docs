# Docs update: launch design, boost mechanics, corrections

Not a docs page (not in docs.json navigation); delete after reading or keep in repo root.

## New pages
- launch-phases.mdx (added to Start Here group in docs.json)

## Updated pages
- docs.json: launch-phases added to navigation
- welcome.mdx: Launch Phases added to Where to go next
- lockless-boost.mdx: STRIP-exposure clock, alignment table, accrual ramp-and-carry (1x to 5x, carried forward), fail-safe claims, third-party contract warning
- supply-dynamics.mdx: 900k/day, lambda 0.001 confirmed; emission table; ceiling-vs-realized section
- strip-token.mdx: allocation table (90% emissions, 1% angels 12m cliff + 24m linear, 9% treasury), treasury does not stake into stSTRIP
- pt-strip-liquidity.mdx: wrapper-only liquidity (Direct LP section removed)
- system-architecture.mdx: wrapper-only liquidity, timelocked emission split
- fees-and-allocations.mdx: venue split policy, no hard caps, 24h timelock on split changes
- trust-assumptions.mdx: rate-limit table replaced by timelock, boost computation honesty section
- risks.mdx: oracle liveness (withdrawals included), emergency pro-rata withdrawals, HWM reset, market/incentive risk, Accrual Period risk
- principal-tokens.mdx: the depositor's trade (breakeven) section
- faq.mdx: Accrual Period, APR before price, boost counts/resets, why 5x, cross-chain, timelocked split
- guides/deposit-and-withdraw.mdx: PT unstaking boost-neutral, oracle note
- guides/stake-principal-tokens.mdx: boost from STRIP exposure, unstaking boost-neutral
- guides/provide-liquidity.mdx: wrapper-only, sWLP unstake exposure note
- guides/claim-rewards.mdx: Accrual Period info box, fail-safe 1x baseline

## Not changed (still current)
- why-strip-exists.mdx, how-strip-works.mdx, yield-routing.mdx, st-strip.mdx,
  transparency.mdx, contract-addresses.mdx, audits-and-security.mdx, guides/stake-strip.mdx

## Before committing
- Diff current risks.mdx and faq.mdx tails against these versions (source snippets may have been truncated)
- supply-dynamics.mdx references /images/schedule.png; confirm the image exists

## Dev follow-ups (code must match docs)
- initializeEmissions with E0 = 900,000/day, lambda = 0.001 (update README + deploy script from 1.35M/0.0015)
- Remove maxPoolAllocBps / maxDeltaBps / minUpdateInterval; gate setAllocPoints behind onlyTimelock
- Keep contract boost range at 0.05-1.0 (do NOT upgrade MAX_BOOST to 20e18); 1x-20x is presentation layer
- Keeper: accrual epochs ramp 1x to 5x on time staked; whitelist floors as max(ramp, floor); carry-forward at claims-enable with the keep >= 50% of cumulative claimed STRIP rule; exclude unclaimed rewards from exposure (explicit spec line)
- Set claimsEnabledTimestamp at deployment (launch + 30 days)
- Do not register the sWLP pool in the EmissionsController until claims-enable
