# Tasks: Run SmartTube on Mobile Phones (Multi-Orientation & Touch Usable)

- [x] **Phase 1: Foundation & Orientation Unlocking** <!-- id: 0 -->
    - [x] Update `AndroidManifest.xml` to mark `android.software.leanback` as `required="false"` <!-- id: 1 -->
    - [x] Remove hardcoded landscape locks in `AppDialogActivity` and any other dialogs <!-- id: 2 -->
    - [x] Update `MotherActivity` display metrics caching to refresh on configuration changes (`onConfigurationChanged`) <!-- id: 3 -->
- [x] **Phase 2: Responsive Grid & Layout for Portrait and Landscape** <!-- id: 4 -->
    - [x] Refactor `GridFragmentHelper` to calculate columns based on current orientation rather than forced `Math.max` <!-- id: 5 -->
    - [x] Adapt UI scaling and card dimensions dynamically for phone aspect ratios <!-- id: 6 -->
    - [x] Ensure sidebar / headers in `BrowseFragment` remain usable and non-obstructive in portrait mode <!-- id: 7 -->
- [x] **Phase 3: Touch Navigation & Usability Optimizations** <!-- id: 8 -->
    - [x] Enable smooth touch scrolling and direct click handling on video cards without requiring D-pad focus <!-- id: 9 -->
    - [x] Optimize player controls and seekbar touch target interaction <!-- id: 10 -->
    - [x] Support back navigation and edge-to-edge / status bar styling on mobile <!-- id: 11 -->
- [x] **Phase 4: Build & Verification** <!-- id: 12 -->
    - [x] Verify project build with Gradle (`assembleDebug` or check compilation) <!-- id: 13 -->
    - [x] Document changes in `walkthrough.md` and durable context in `AGENTS.md` <!-- id: 14 -->
