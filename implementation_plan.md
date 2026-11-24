# Implementation Plan: Mobile Phone Support & Responsive Multi-Orientation for SmartTube

Enable SmartTube (originally designed for Android TV and Leanback) to run seamlessly on Android mobile phones in any orientation (portrait, landscape, reverse portrait/landscape) with full touch usability.

## Proposed Changes

### 1. App Manifest & Hardware Feature Flags
- File: [`smarttubetv/src/main/AndroidManifest.xml`](file:///home/obulasetty/projects/1013_smarttube_myversion/smarttubetv/src/main/AndroidManifest.xml)
  - Change `<uses-feature android:name="android.software.leanback" android:required="true" />` to `android:required="false"`.
  - Ensure touch screen (`android.hardware.touchscreen`) is enabled without blocking non-touch devices (`required="false"`).
  - Verify activity launcher filters so the app appears on mobile home screens / app launchers.

### 2. Orientation & Display Metrics Adaptation
- File: [`common/src/main/java/com/liskovsoft/smartyoutubetv2/common/misc/MotherActivity.java`](file:///home/obulasetty/projects/1013_smarttube_myversion/common/src/main/java/com/liskovsoft/smartyoutubetv2/common/misc/MotherActivity.java)
  - Fix `sCachedDisplayMetrics`: Invalidate and recompute display metrics when `onConfigurationChanged` is triggered so orientation switches recalculate bounds.
  - Adapt DPI calculation: In portrait mode, adjust density using the actual screen width rather than `Math.max(width, height)` so UI elements don't get shrunk down to unreadable TV sizes.
- File: [`smarttubetv/src/main/java/com/liskovsoft/smartyoutubetv2/tv/ui/dialogs/AppDialogActivity.java`](file:///home/obulasetty/projects/1013_smarttube_myversion/smarttubetv/src/main/java/com/liskovsoft/smartyoutubetv2/tv/ui/dialogs/AppDialogActivity.java)
  - Remove or condition `setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE)` so dialogs adapt to the current screen orientation on phones.

### 3. Responsive Card Grid & Browse Layout
- File: [`smarttubetv/src/main/java/com/liskovsoft/smartyoutubetv2/tv/ui/browse/video/GridFragmentHelper.java`](file:///home/obulasetty/projects/1013_smarttube_myversion/smarttubetv/src/main/java/com/liskovsoft/smartyoutubetv2/tv/ui/browse/video/GridFragmentHelper.java)
  - Replace static `Math.max(width, height)` with current viewport width (taking into account portrait vs landscape).
  - Clear column / card dimension caches (`sMaxColsNum`, `sCardDimensPx`) on orientation change.
  - Dynamically adjust columns (e.g. 1-2 columns for portrait phone, 3-4 for landscape phone, 4-6 for TV).
- File: [`smarttubetv/src/main/java/com/liskovsoft/smartyoutubetv2/tv/ui/browse/BrowseFragment.java`](file:///home/obulasetty/projects/1013_smarttube_myversion/smarttubetv/src/main/java/com/liskovsoft/smartyoutubetv2/tv/ui/browse/BrowseFragment.java)
  - Ensure the sidebar headers can hide or collapse when viewing content in portrait mode to avoid consuming horizontal real estate.

### 4. Touch Navigation & Usability
- Ensure cards in `VideoCardPresenter` and items in `GridItemPresenter` support immediate tap-to-open rather than requiring a focus step first.
- Maintain smooth touch scrolling in Leanback grid layouts (`VerticalGridView` / `HorizontalGridView`).
- Player overlay: Ensure transport controls, seekbar, and action buttons are easy to tap and touch targets adhere to mobile accessibility standards.

## Verification Plan

### Automated / Build Checks:
- Run Gradle check/build:
  ```bash
  JAVA_HOME=/usr/lib/jvm/java-1.17.0-openjdk-arm64 bash ./gradlew assembleStbetaDebug -x test
  ```
- Verify APK outputs and check AndroidManifest merger output.

### Manual / Device Verification:
- Install on Android phone.
- Verify:
  1. App appears in phone launcher and opens properly.
  2. Rotating device between portrait and landscape seamlessly updates UI layout without crashing.
  3. Video grid adapts (1-2 columns in portrait, multi-column in landscape).
  4. Video player works in both portrait and landscape (or auto-rotates to fullscreen landscape).
  5. Touch interactions (tapping cards, scrolling feeds, tapping player controls) work naturally.
