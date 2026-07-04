# Flory Freedom — Codex Task Brief
**File to edit: `index.html` (single-file React 18 PWA, ~2150 lines)**
**Repo: `C:\Users\micha\flory-freedom\`**
**Live app: https://florymj.github.io/flory-freedom/**

Read the full redesign proposal first:
`C:\Users\micha\OneDrive\Documents\Epic Charter School App\docs\flory-freedom-redesign.md`

## Status

All P0 and P1 tasks below are **COMPLETE** as of commit `feat: redesign P0-P1 implementation`.
P1-C (MC memoization) was done in an earlier commit `fix: school-year presets, correct defaults, TRS COLA note, MC memoization`.

The verification checklist at the bottom lists what to confirm in the browser after each deploy.

---

## P0 Tasks — DONE

### [x] P0-A: Fix typo in component name
`AduitChecklist` → `AduChecklist` (component definition + usage site).

### [x] P0-B: Fix DEF default portfolio rounding
`currentPortfolio` is now `1248950` (= investmentValue + cashValue + otherValue).

### [x] P0-C: Spouse income toggle + lower slider minimum
- `DEF.includeWifeIncome = true`
- `incomeForYear` respects `s.includeWifeIncome !== false`
- `buildEngine` MAGI calc respects `includeWifeIncome`
- IncomeTab "Income Layers" panel has a SwitchLine toggle above the slider; slider `min` lowered to `0`

### [x] P0-D: TRS COLA slider
- `DEF.trsCola = 0.00`
- `incomeForYear` applies `Math.pow(1 + (s.trsCola || 0), trsYearsActive)` from age 65
- IncomeTab TRS panel has a RangeControl (0–3%, step 0.5%) with the COLA callout updated to match

---

## P1 Tasks — DONE

### [x] P1-A: Reduce tab bar from 7 to 5 tabs
- `PlanTab` = IncomeTab + BudgetTab
- `SettingsTab` = ADUTab + GuideTab
- TABS array updated; CSS `repeat(7)` → `repeat(5)`; tab font-size 9px → 10px
- App render updated to 5 conditionals

### [x] P1-B: Capital gains distributions to MAGI (ACA fix)
- `DEF.capitalGainsDist = 8000`
- MAGI in `buildEngine` includes `+ (s.capitalGainsDist || 0)`
- IncomeTab ACA panel has a "Capital gains distributions (est.)" slider

### [x] P1-C: Memoize Risk tab scenario calculations
- `riskComputations = React.useMemo(fn, [e])` wraps all 6 scenarios + 3 stress MCs
- Done in earlier commit

### [x] P1-D: TRS and SS milestone lines on FanChart
- FanChart accepts `yearsToTrs` and `yearsToSs` props
- Dashed cyan "TRS" line and dashed gold "SS" line drawn at correct year indices
- Midpoint age label added to x-axis
- CommandTab FanChart callsite passes `e.yearsToTrs` and `e.yearsToSs`

### [x] P1-E: 3-KPI topbar strip
- `.header-metric` is now a 3-column grid with `.hm-item / .hm-label / .hm-value` classes
- Shows: Portfolio at retirement (green), Plan success % (color-coded), Family draw rate (gold/orange)

### [x] P1-F: Switch body font to DM Sans
- Google Fonts `<link>` tags added before `</head>`
- `font-family` updated to `'DM Sans', ui-sans-serif, ...`

### [x] P1-G: Portfolio import lock indicator
- `DEF.portfolioSourceLocked = false`
- CSV import sets `portfolioSourceLocked: true`
- Portfolio Summary panel shows a callout + "Sync to holdings total" button when drift > $5,000

---

## Verification checklist

- [ ] App loads in browser without JS errors (open browser console)
- [ ] Tab bar shows exactly 5 tabs: Home, Plan, Portfolio, Risk, Settings
- [ ] Plan tab shows both Income and Budget content
- [ ] Settings tab shows both ADU and Guide content
- [ ] Income floor toggle zeros out income when switched off; draw rate rises accordingly
- [ ] TRS COLA slider appears in Plan > TRS section; changing it moves TRS annual figure
- [ ] Capital gains distributions slider appears in Plan > ACA section; changing it moves MAGI
- [ ] Risk tab renders noticeably faster than before (scenarios are memoized)
- [ ] FanChart shows cyan "TRS" and gold "SS" vertical dashed lines at correct ages
- [ ] Topbar shows 3 KPIs: projected portfolio, plan success %, family draw rate
- [ ] Font is DM Sans (inspect element in browser)
- [ ] Importing a CSV then adjusting "Portfolio today" slider shows the sync warning
- [ ] `git diff HEAD~1 index.html` shows no unintended deletions

## Notes for future Codex runs

- The entire app is in `index.html`. No separate JS or CSS files; no build system.
- React 18 via CDN: `h = React.createElement`. No JSX. All components use `h(...)` syntax.
- State is saved to `localStorage` under key `flory-freedom-v1`. `normalizeState` handles defaults for missing fields — new DEF fields are picked up automatically on first load.
- Do not add a build system, bundler, or node_modules. Keep it single-file.
- Do not change the existing color palette variables or the dark theme.
- After implementing, run: `git add index.html CODEX.md && git commit -m "feat: <description>"`
