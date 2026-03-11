# DS Visual Consistency Plan (macOS + iOS)

## Goal
Create one shared visual system for both apps, with `DragonShield/Core/DesignSystem` as the source of truth for color, spacing, typography, and core component styles.

## Baseline (2026-02-26)
- DS token usage in macOS views: `64`
- DS token usage in iOS views: `0`
- Direct `.font(.system...)` in macOS views: `495`
- Direct `.font(.system...)` in iOS views: `15`
- Numeric `.padding(...)` in macOS views: `766`
- Numeric `.padding(...)` in iOS views: `81`
- Numeric `.cornerRadius(...)` in macOS views: `47`
- Numeric `.cornerRadius(...)` in iOS views: `11`
- Parallel style layers currently in use: `DSColor/DSLayout/DSTypography`, `Theme`, `Color+Palette`, local per-view constants

## Priority Rules
1. No big-bang redesign.
2. Migrate by high-traffic views first.
3. Keep behavioral changes and visual changes separate when possible.
4. Add guardrails before broad migration to prevent new hardcoded values.

## Phase 1: Stabilize Source of Truth
1. Expand `DSColor` to be explicitly cross-platform (`#if os(macOS)` / `#if os(iOS)` where needed).
2. Move shared spacing/radius/dimensions to `DSLayout` and include table/dashboard tokens currently living elsewhere.
3. Finalize typography scale in `DSTypography` for:
   - Title/section/body/meta
   - Numeric/monospaced financial values
   - iOS dynamic type compatibility
4. Define a small token mapping table from legacy names (`Theme.*`, `Color.*` extension values) to DS tokens.

Deliverable:
- Design token spec in code with explicit token names and migration map.

## Phase 2: Compatibility Layer (Low-Risk Bridge)
1. Keep `Theme` and key `Color` extension entries as shims that forward to DS tokens.
2. Mark shim APIs as deprecated with migration comments.
3. Replace global button styles (`PrimaryButtonStyle`, `SecondaryButtonStyle`) to internally use DS tokens, then phase into `DSButtonStyle`.

Deliverable:
- Existing screens render the same, but tokens flow through DS.

## Phase 3: Migrate iOS First (Current Biggest Gap)
1. Wave iOS-A:
   - `DragonShield iOS/Views/Dashboard/IOSDashboardView.swift`
   - `DragonShield iOS/Views/Reports/AssetManagementReportView.swift`
   - `DragonShield iOS/Views/Settings/IOSSettingsView.swift`
2. Wave iOS-B:
   - `DragonShield iOS/Views/Todos/TodoBoardView.swift`
   - Remaining iOS view files with hardcoded spacing/typography
3. Convert direct numeric values to DS tokens first, then adjust visual polish.

Deliverable:
- iOS views adopt DS colors/spacing/typography with no functional regressions.

## Phase 4: Migrate macOS High-Churn Screens
1. Wave macOS-A:
   - `DragonShield/Views/PortfolioView.swift`
   - `DragonShield/Views/AccountsView.swift`
   - `DragonShield/Views/CurrenciesView.swift`
2. Wave macOS-B:
   - `DragonShield/Views/PositionsView.swift`
   - `DragonShield/Views/InstitutionsView.swift`
   - `DragonShield/Views/TransactionHistoryView.swift`
3. Normalize table styling through DS tokens:
   - Header backgrounds
   - Row spacing/padding
   - Font ladders
   - Badge colors and border strengths

Deliverable:
- Core macOS CRUD/reporting screens aligned to same token system as iOS.

## Phase 5: Guardrails and CI Enforcement
1. Add lint script (for example `scripts/ci/design_tokens_guard.sh`) to flag new hardcoded patterns:
   - `.font(.system(size:`
   - `.padding(<number>)`
   - `.cornerRadius(<number>)`
   - `Color(red:`
2. Keep an allowlist for approved exceptions.
3. Add a PR checklist item: "No new non-token visual constants."

Deliverable:
- Drift prevention in daily development.

## Phase 6: QA and Rollout
1. Validate both light/dark modes for macOS and iOS.
2. Run a view matrix for core screens and smallest/largest font settings.
3. Perform targeted regression checks for:
   - Truncation in tables and segmented controls
   - Tap/click target sizes
   - Contrast on status colors
4. Release in small batches (one wave per PR), not one giant migration PR.

Deliverable:
- Controlled rollout with manageable review scope and low regression risk.

## Definition of Done
1. `Theme` and legacy palette usage reduced to compatibility-only or removed.
2. New UI code uses DS tokens by default.
3. iOS + macOS core views share the same token vocabulary.
4. CI prevents re-introduction of hardcoded visual constants.
5. Visual QA pass is complete for both platforms in light/dark mode.

## Execution Order (Recommended)
1. Phase 1 + Phase 2
2. Phase 3 (iOS)
3. Phase 4 (macOS)
4. Phase 5 + Phase 6

## First Implementation PR Scope
1. Expand DS tokens and add compatibility shims.
2. Migrate `IOSDashboardView` + `IOSSettingsView`.
3. Add initial token-guard lint script in warning mode (non-blocking).

