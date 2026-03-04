---
name: expo-react-native
description: >
  Complete guide for building, styling, navigating, animating, and deploying Expo / React Native apps.
  Use this skill for ANY task involving Expo, React Native, Expo Router, EAS builds, app store submission,
  native UI (NativeTabs, Stack, toolbars), animations (Reanimated), storage, media (camera/audio/video),
  3D/WebGPU, Tailwind/NativeWind setup, DOM components, CI/CD workflows, or SDK upgrades.
  Trigger whenever the user mentions: expo, react-native, eas build, expo-router, NativeTabs, Reanimated,
  expo-av migration, TestFlight, Play Store, app.json, metro.config, or any mobile app development task.
  Also trigger for: creating a new app, building a screen, fixing navigation, adding a tab bar, setting
  up storage, working with the camera, deploying to iOS or Android, or any React Native styling question.
version: 2.0.0
license: MIT
---

# Expo / React Native Skill

This skill covers the full lifecycle of Expo app development — from project structure and UI patterns
to deployment and store submission. Always consult the relevant reference file(s) before writing code.
The references contain battle-tested patterns and known pitfalls that are not obvious from docs alone.

---

## Reference Files

```
references/
  # UI & Interaction
  animations.md          Reanimated v4: entering/exiting, layout, scroll-driven, gestures, spring
  controls.md            Native iOS: Switch, Slider, SegmentedControl, DateTimePicker, Picker
  form-sheet.md          Form sheets: detents, footers, glass backgrounds, undimmed interaction
  gradients.md           CSS gradients via experimental_backgroundImage (New Arch only)
  icons.md               SF Symbols via expo-symbols, all names, animations, weights, multicolor
  media.md               Camera, audio (expo-audio), video (expo-video), saving to media library
  visual-effects.md      BlurView (expo-blur) and GlassView (expo-glass-effect, iOS 26+)
  zoom-transitions.md    Apple Zoom fluid transitions with Link.AppleZoom (iOS 18+, SDK 55+)

  # Navigation & Structure
  route-structure.md     Route conventions, dynamic routes, groups, array routes, layout files
  search.md              Search bar in headers, useSearch hook, filtering patterns, debounce
  tabs.md                NativeTabs SDK 54/55, migration from JS tabs, iOS 26 features, BottomAccessory
  toolbar-and-headers.md Stack toolbars and header buttons, menus (iOS only, SDK 55+)

  # Data & Storage
  storage.md             localStorage polyfill, full SQLite, SecureStore — when to use which
  native-data-fetching.md Fetch, React Query, SWR, auth tokens, offline support, env vars
  expo-router-loaders.md Route-level data loading for web (SDK 55+, SSR and static modes)

  # SDK Architecture
  new-architecture.md    New Architecture (default SDK 53+): JSI, Fabric, TurboModules
  react-19.md            React 19: use() hook, Context without .Provider, no more forwardRef
  react-compiler.md      React Compiler (stable SDK 54+): auto-memoization, how to enable
  native-tabs.md         SDK 55 NativeTabs migration: Trigger.Icon/Label/Badge new API

  # Deployment & Store
  testflight.md          npx testflight — one command to beta, tester strategy, tips
  ios-app-store.md       iOS credentials, submission flow, App Review, phased release
  play-store.md          Google Play service account setup, tracks, staged rollout
  app-store-metadata.md  EAS Metadata, ASO optimization, store.config.json schema
  workflows.md           EAS Workflows YAML: PR previews, production release, web deploy

  # Migration
  expo-av-to-audio.md    Migrate Audio.Sound to useAudioPlayer, Recording to useAudioRecorder
  expo-av-to-video.md    Migrate Video component to useVideoPlayer + VideoView, API mapping table

  # Advanced Topics
  webgpu-three.md        3D graphics: WebGPU + Three.js + R3F, locked versions, required lib files
  building-native-ui.md  Full app structure guide, styling philosophy, navigation patterns
  upgrading-expo.md      Step-by-step SDK upgrade, deprecated packages, metro cleanup checklist
  expo-api-routes.md     API routes (+api.ts), EAS Hosting, Cloudflare Workers runtime limits
  expo-cicd-workflows.md Writing and validating EAS workflow YAML with live schema
  expo-dev-client.md     Development client builds — when Expo Go is not enough
  expo-tailwind.md       Tailwind CSS v4 + NativeWind v5 + react-native-css complete setup
  use-dom.md             DOM components using 'use dom' for running web libraries inside native
  expo-ui-swiftui.md     @expo/ui/swift-ui — SwiftUI Views and modifiers via Expo (SDK 55)
  expo-ui-jetpack.md     @expo/ui/jetpack-compose — Jetpack Compose Views via Expo (SDK 55)

scripts/
  fetch.js               HTTP caching utility (ETags + Cache-Control) — fetch remote schemas
  validate.js            Validate EAS workflow YAML files against the official live schema
  package.json           Dependencies: ajv, ajv-formats, js-yaml for YAML validation
```

---

## Quick Reference: What to Read for Each Task

| What you are building or fixing | Reference files to read first |
|-------------------------------|-------------------------------|
| New app from scratch | `building-native-ui.md`, `route-structure.md`, `tabs.md` |
| Tab bar navigation | `tabs.md`, `route-structure.md` |
| Stack with large title and search | `toolbar-and-headers.md`, `search.md` |
| Screen with a form sheet or modal | `form-sheet.md` |
| Animations on mount and list items | `animations.md` |
| Gestures and drag interactions | `animations.md` |
| Scroll-driven header animations | `animations.md` |
| Camera screen | `media.md`, `visual-effects.md`, `icons.md` |
| Audio playback or recording | `media.md` |
| Video playback with controls | `media.md` |
| SF Symbol icons | `icons.md` |
| Blur or liquid glass effects | `visual-effects.md` |
| CSS gradient backgrounds | `gradients.md` |
| Native Switch, Slider, Picker | `controls.md` |
| Apple Zoom link transitions | `zoom-transitions.md` |
| Key-value or settings storage | `storage.md` |
| Complex queries with SQLite | `storage.md` |
| Secure token storage | `storage.md` |
| API calls and data fetching | `native-data-fetching.md` |
| React Query setup | `native-data-fetching.md` |
| Web SSR data loading | `expo-router-loaders.md` |
| API routes and server endpoints | `expo-api-routes.md` |
| Tailwind CSS in Expo | `expo-tailwind.md` |
| Web-only libraries like recharts | `use-dom.md` |
| SwiftUI native components | `expo-ui-swiftui.md` |
| Jetpack Compose native components | `expo-ui-jetpack.md` |
| 3D scenes, games, WebGPU | `webgpu-three.md` |
| Submit to TestFlight | `testflight.md` |
| Submit to App Store | `ios-app-store.md` |
| Submit to Google Play | `play-store.md` |
| App Store listing and ASO | `app-store-metadata.md` |
| CI/CD pipelines | `workflows.md`, `expo-cicd-workflows.md` |
| Upgrading Expo SDK | `upgrading-expo.md`, `new-architecture.md`, `react-19.md`, `react-compiler.md` |
| Migrating from expo-av | `expo-av-to-audio.md`, `expo-av-to-video.md` |
| SDK 55 NativeTabs update | `native-tabs.md` |
| Custom dev build needed | `expo-dev-client.md` |

---

## Non-Negotiable Rules

These rules apply to every piece of code written for Expo. Violating them causes subtle bugs,
broken dark mode, broken safe area, or App Store rejection.

### Package Choices — Always Use These

Use `expo-audio` not `expo-av` for all audio. Use `expo-video` not `expo-av` for all video.
Use `expo-image` for images — never the intrinsic `img` element. For SF Symbols in components,
use `expo-symbols` (SymbolView); in NativeTabs triggers, the `sf` prop on `NativeTabs.Trigger.Icon`
uses `expo-image` under the hood. Use `react-native-safe-area-context` not React Native's
built-in SafeAreaView. Use `expo-glass-effect` for liquid glass on iOS 26+. Use
`expo-sqlite/localStorage/install` for key-value storage — never AsyncStorage. Use
`expo-splash-screen` not `expo-app-loading`. Use `experimental_backgroundImage` for gradients —
never `expo-linear-gradient`.

Never use removed or deprecated React Native APIs: no Picker, no WebView, no AsyncStorage,
no SafeAreaView from react-native core. Never use `expo-permissions` — use each package's own
permission API instead. Never use `@expo/vector-icons` — SF Symbols always.

### Platform Detection

Use `process.env.EXPO_OS` not `Platform.OS`. This is evaluated at build time and tree-shaken
properly. `Platform.OS` still works but is less optimal for conditional platform code.

### React Hooks

Use `React.use(Context)` not `React.useContext(Context)`. This is the React 19 API available
from SDK 54+. The `use` hook can also read promises and can be called conditionally, which
simplifies components that consume multiple contexts.

### File and Import Conventions

Always use kebab-case for file names: `comment-card.tsx` not `CommentCard.tsx`. Always put
import statements at the top of the file. Configure `tsconfig.json` with path aliases and prefer
aliases over relative imports when refactoring. Never use special characters in file names.

---

## Project Structure Rules

Routes belong in the `app` directory only. Never co-locate components, types, utilities, or hooks
inside `app` — this is an anti-pattern in Expo Router. Components go in `components/`, utilities
in `utils/`, hooks in `hooks/`. Every file inside `app` should export a default React component.

The app must always have a route matching `/`. If using group routes, ensure one group resolves
to the index. Always use `_layout.tsx` files to define navigators — never configure navigation
inline in a screen. Routes can never be named `(foo).tsx` — use `(foo)/index.tsx` instead.

Remove old route files when restructuring navigation. Stale routes cause hard-to-debug 404 errors.
The `+not-found.tsx` file at the root of `app/` handles unmatched routes.

### Standard Directory Layout

A production app should look roughly like this:

```
app/
  _layout.tsx              Root layout — NativeTabs or Stack
  (home)/
    _layout.tsx            Stack navigator for home tab
    index.tsx              Home screen
    i/[id].tsx             Detail screen
  (settings)/
    _layout.tsx            Stack navigator for settings tab
    index.tsx              Settings screen
  (home,settings)/
    info.tsx               Shared screen accessible from both tabs
  +not-found.tsx           404 handler
components/
  theme.tsx
  list-item.tsx
hooks/
  use-search.ts
utils/
  storage.ts
```

---

## Styling Philosophy

Follow Apple Human Interface Guidelines for all visual decisions.

### Layout

Prefer flex gap over margin between siblings. Prefer padding over margin where possible.
Use flexbox for all layout — never the Dimensions API for layout calculations. Use
`useWindowDimensions` when you need screen size — never `Dimensions.get()`. This ensures
the app responds correctly when the window resizes on iPad or foldable devices.

Always account for safe area. The best approach is `contentInsetAdjustmentBehavior="automatic"`
on ScrollView, FlatList, and SectionList — this is smarter than SafeAreaView and handles
navigation bar insets automatically. Ensure both top and bottom safe area insets are accounted
for. When a screen belongs to a Stack, its first child should almost always be a ScrollView
with `contentInsetAdjustmentBehavior="automatic"` set.

When padding a ScrollView, always use `contentContainerStyle` for padding and gap — not the
`style` prop on ScrollView itself. Using `style` causes content to clip at the edges during
scrolling because the scroll view's hit area does not grow with it.

### Styles

Use inline styles not `StyleSheet.create` unless the style object is reused across many renders.
Use the CSS `boxShadow` style prop — never legacy React Native `shadow*` props or `elevation`.
The `inset` shadow variant is supported. Use `{ borderCurve: 'continuous' }` for rounded corners
on cards and surfaces — use `borderRadius: 9999` only for true capsule or pill shapes.

Never use CSS class names or Tailwind utility classes unless the full NativeWind v5 + react-native-css
setup from `expo-tailwind.md` has been completed. Without that setup, `className` silently does nothing.

Add entering and exiting animations for state changes — never let things pop in or out abruptly.
Use `LinearTransition` from Reanimated when list items are added or removed.

### Text

Add the `selectable` prop to every Text element displaying data the user might want to copy:
error messages, IDs, addresses, URLs, dollar amounts. Use `{ fontVariant: ['tabular-nums'] }` on
counter text so digits align as numbers change. Format large numbers as 1.4M or 38k rather
than showing raw digit strings.

### Navigation Titles

Always use a navigation stack title via `Stack.Screen options={{ title }}` — never render a
custom Text element at the top of a screen as a fake title. The system title handles large
title collapse, Dynamic Type accessibility sizing, and VoiceOver navigation correctly.

---

## Behavior Patterns

### Haptics

Use `expo-haptics` conditionally on iOS (guard with `process.env.EXPO_OS === 'ios'`) to add
tactile feedback to custom interactions. For controls that have built-in haptics — Switch and
DateTimePicker — do not add extra haptic calls on top. Camera shutter, custom action buttons,
and gesture completions benefit most from haptics.

### Scroll Position

The first child of a Stack screen should almost always be a ScrollView. This ensures the
large title collapses correctly on iOS, scroll-to-top on tab tap works, and keyboard avoidance
is handled automatically. If the ScrollView must be wrapped in a View for layout reasons,
add `collapsable={false}` to the wrapper View to preserve the scroll connection.

### Search

Prefer `headerSearchBarOptions` in Stack.Screen options over a custom search input rendered
inside the screen body. The native search bar integrates with the navigation header, handles
keyboard avoidance, and matches system apps perfectly. See `search.md` for the `useSearch`
hook that manages search state reactively.

---

## Navigation in Depth

### Stack Navigator

Import `Stack` from `expo-router/stack`. Every directory needs its own `_layout.tsx` with a
Stack. Configure `headerLargeTitle: true` for iOS large title screens such as list or home views.
Use `headerTransparent: true` combined with `headerBlurEffect` for the translucent header effect.
Set `headerBackButtonDisplayMode: "minimal"` to keep back buttons clean and space-efficient.

### NativeTabs

Always use `NativeTabs` from `expo-router/unstable-native-tabs` — never the old `Tabs` from
`expo-router`. NativeTabs uses the platform's native tab bar: liquid glass on iOS 26+,
Material 3 on Android. Tabs must be static — no dynamic addition or removal at runtime,
because this remounts the entire navigator and loses all navigation state.

In SDK 55+, icons, labels, and badges are accessed as `NativeTabs.Trigger.Icon`,
`NativeTabs.Trigger.Label`, `NativeTabs.Trigger.Badge` — not as separate named imports.
The `name` prop on `NativeTabs.Trigger` must exactly match the route name including parentheses.
Use the `role` prop for semantic tabs: `role="search"` places the search tab correctly on iOS 26.
Always place the search trigger last in the list. Use `minimizeBehavior="onScrollDown"` so the
tab bar hides as users scroll, matching iOS App Store and system app behavior.

Use `NativeTabs.BottomAccessory` for mini-player style UI above the tab bar on iOS 26+.
Two instances of the accessory render simultaneously — keep state outside the component
using props, context, or an external store. See `tabs.md` for the full API.

### Link and Context Menus

Use `Link` from `expo-router` for navigation. Include `Link.Preview` whenever navigating to
a detail screen — this gives iOS peek-and-pop preview for free. Add `Link.Menu` with
`Link.MenuAction` items for long-press context menus on list items. Destructive actions use
the `destructive` prop. Nested menus are supported. This follows iOS conventions and
significantly improves perceived quality of list-based apps.

### Modals and Sheets

Use `presentation: "formSheet"` in Stack.Screen options for bottom sheets. Use
`sheetAllowedDetents` to control available heights as fractions from 0 to 1. Use
`sheetLargestUndimmedDetentIndex` (zero-indexed) to keep content behind the sheet interactive —
useful for map-based apps where users should be able to pan the map while the sheet is open.
Use `contentStyle: { backgroundColor: "transparent" }` to get the liquid glass sheet background
on iOS 26+. See `form-sheet.md` for complete layout patterns including footers.

### Apple Zoom Transitions

For thumbnail-to-detail navigation such as photo galleries or card-to-detail views, use
`Link.AppleZoom` to wrap the source element and `Link.AppleZoomTarget` on the destination.
This produces the fluid iOS 18+ zoom transition. Only works inside a Stack navigator — not
with sheets or popovers. The source element does not need to match the tap target exactly;
only the content inside `Link.AppleZoom` participates in the transition. See `zoom-transitions.md`.

---

## SDK Version Compatibility

Understanding which SDK introduced which feature prevents runtime errors and wasted debugging.

**SDK 53+** — New Architecture is enabled by default. JSI, Fabric, TurboModules are active.
Expo Go only supports New Architecture from this SDK onwards. `resolver.unstable_enablePackageExports`
is on by default — remove it from metro.config if present. `autoprefixer` is no longer needed.
The `newArchEnabled: true` field in `app.json` is now redundant and can be removed.

**SDK 54+** — React 19 is included. Use `use(Context)` instead of `useContext`. Context providers
no longer need the `.Provider` suffix. `forwardRef` is no longer needed — pass `ref` as a regular
prop and type it in the props interface. React Compiler is stable — enable with
`"experiments": { "reactCompiler": true }` in `app.json`. `react-native-worklets` is required
for Reanimated to function and must be explicitly installed. `experimentalImportSupport` is on
by default in Metro — remove it from config if present. `EXPO_USE_FAST_RESOLVER=1` is removed.

**SDK 55+** — NativeTabs components are now namespaced under `NativeTabs.Trigger.*`. Stack toolbars
and headers via `Stack.Toolbar` and `Stack.SearchBar` are available on iOS only. Expo Router data
loaders (`useLoaderData`, `loader` exports) are available for web SSR. `BottomAccessory` is added
to NativeTabs for mini-player style UI. Safe area is handled automatically in NativeTabs — use
`disableAutomaticContentInsets` to opt out per tab.

---

## Deprecated Packages — Always Replace

| Deprecated | Replacement |
|---|---|
| `expo-av` audio | `expo-audio` with `useAudioPlayer` and `useAudioRecorder` |
| `expo-av` video | `expo-video` with `useVideoPlayer` and `VideoView` |
| `expo-permissions` | Per-package permission APIs such as `useCameraPermissions` |
| `@expo/vector-icons` | `expo-symbols` (SymbolView) or `NativeTabs.Trigger.Icon` |
| `AsyncStorage` | `expo-sqlite/localStorage/install` polyfill |
| `expo-app-loading` | `expo-splash-screen` |
| `expo-linear-gradient` | `experimental_backgroundImage` CSS gradient on View |
| `react-native SafeAreaView` | `react-native-safe-area-context` SafeAreaView |
| `react-native Picker` | `@react-native-picker/picker` community package |
| `expo-constants` if implicit | Remove from `package.json` — it is a transitive dep |
| `@babel/core` if implicit | Remove from `package.json` — it is a transitive dep |
| `babel-preset-expo` if implicit | Remove from `package.json` — it is a transitive dep |

---

## Expo Go vs Custom Builds

Always start with Expo Go. Run `npx expo start`, scan the QR code, test thoroughly. Most
Expo apps — including those using camera, location, notifications, Reanimated, and gesture
handler — work fine in Expo Go without any custom native code.

Only create a custom build when you specifically need one of these:
- Local Expo modules with custom native code in the `modules/` directory
- Apple targets such as widgets, app clips, or extensions via `@bacons/apple-targets`
- Third-party native modules that are not bundled in Expo Go
- Custom native configuration that cannot be expressed in `app.json`

Creating custom builds adds complexity, requires Xcode or Android Studio, and significantly
slows iteration. The question is not "should I use a dev client?" but "do I have a specific
reason I cannot use Expo Go?" See `expo-dev-client.md` for the build and distribution workflow.

---

## Animations Quick Reference

Use Reanimated v4. Never use React Native's built-in Animated API for any new code.

Add `entering` and `exiting` props to `Animated.View` for mount and unmount transitions.
Common presets: `FadeIn`, `FadeInDown`, `SlideInUp`, `ZoomIn` for entering; matching `*Out`
variants for exiting. Customize with `.duration(300)`, `.delay(100)`, `.springify()`, and
`.damping(15)`. Chain modifiers: `FadeInDown.duration(400).delay(200).springify()`.

Use `layout={LinearTransition}` on containers so items animate smoothly when siblings are
added or removed from a list. Use `useScrollViewOffset` with `useAnimatedStyle` and
`interpolate` for scroll-driven header parallax or opacity effects. Use `useSharedValue` with
`withSpring` or `withTiming` for imperative animations triggered by events.

Combine with `Gesture.Pan()` from react-native-gesture-handler for draggable elements.
The `.onUpdate` handler modifies shared values and `.onEnd` uses `withSpring(0)` to snap back.

PlatformColors cannot be passed to Reanimated animated style values — use static colors.

For staggered list entry animations, pass `entering={FadeInUp.delay(index * 50)}` per item.
Keep animations under 300ms for a responsive feel. Avoid animating layout properties such as
width and height — use `transform: [{ scaleX }, { scaleY }]` to keep animations on the UI thread.

---

## Storage Decision Guide

Use the `localStorage` polyfill (`expo-sqlite/localStorage/install`) for simple key-value data:
settings, preferences, small config objects, theme choices, recently viewed IDs. It is
synchronous, requires no async/await, and is appropriate for anything under a few kilobytes.

Use full `expo-sqlite` when you need complex queries, relational data, large datasets, or
any benefit from SQL. Open with `SQLite.openDatabaseAsync`, use `execAsync` for DDL schema
creation, `runAsync` for writes with parameters, and `getAllAsync` for queries.

Use `expo-secure-store` for anything sensitive: auth tokens, passwords, API keys, user
credentials. Never store tokens in localStorage, SQLite, or AsyncStorage. The secure store
uses the platform keychain and keystore and is not accessible without device unlock.

For reactive UI that updates when storage changes, use a subscription pattern with
`useSyncExternalStore`. The `storage.md` reference includes a ready-to-use implementation.

---

## Media Guidelines

For camera: eagerly request camera permission when the screen loads — do not wait for the
user to tap something. Lazily request media library permission only when the user actually
tries to save to their library. Mirror the camera for front-facing shots using the `mirror`
prop on CameraView, matching social app conventions. Hide navigation headers on full-screen
camera screens. Use liquid glass buttons via GlassView for camera controls. Use SF Symbols:
`arrow.triangle.2.circlepath` for flip, `photo` for gallery, `bolt` for flash.

For audio: `useAudioPlayer(source)` replaces `Audio.Sound.createAsync`. The player loads
immediately and cleans up automatically on component unmount — no explicit unload needed.
Time is measured in seconds not milliseconds, matching web audio standards. After playback
completes the player stays positioned at the end — call `seekTo(0)` then `play()` to replay.

For video: `useVideoPlayer(source)` creates the player, `VideoView` renders it. They are
deliberately decoupled so one player can power multiple views, or a player can preload before
a VideoView mounts. Use `useEvent` and `useEventListener` from `expo` for playback state
changes — not callback props on VideoView. Use `player.replace(newSource)` to swap videos
without recreating the player. On Android, never assign `player.currentTime` in the
`useVideoPlayer` setup callback — set it after the component mounts instead.

Never install both `expo-av` and `expo-video` simultaneously on Android — this causes VideoView
to render a black screen. Uninstall expo-av entirely before installing expo-video.

---

## Data Fetching Principles

Prefer `fetch` with explicit error checking over axios. Always check `response.ok` before
calling `.json()` — a non-2xx status code does not throw automatically and will silently
return an error body if you skip the check.

For apps with complex data requirements, use TanStack Query (React Query). Set up
`QueryClientProvider` in the root layout with a `QueryClient` configured with sensible
defaults: `staleTime` of 5 minutes, `retry` of 2. Use `useQuery` for reads and `useMutation`
for writes. Invalidate related queries after successful mutations. React Query handles
request deduplication, background refetch, automatic retry with backoff, and loading states.

Use `EXPO_PUBLIC_` prefix for environment variables needed in the client bundle — API base
URLs, public keys, feature flags. Never put secrets in `EXPO_PUBLIC_` variables — they are
embedded in the built app and visible to anyone who inspects it. Server-only secrets go in
non-prefixed env vars used only inside API routes. Restart the Metro dev server after
changing `.env` files — they are inlined at build time, not runtime.

For offline support, combine `NetInfo` from `@react-native-community/netinfo` with React
Query's `onlineManager.setEventListener` to automatically pause and resume queries when
network connectivity changes. React Query will queue failed mutations and retry them when
the connection is restored.

---

## API Routes and Backend

API routes live in `app/api/` with the `+api.ts` suffix. Export named functions for each
HTTP method: `GET`, `POST`, `PUT`, `DELETE`. They follow the web `Request` and `Response` API.
Dynamic routes work the same as page routes: `users/[id]+api.ts` receives `id` as a second
parameter.

These routes run on Cloudflare Workers via EAS Hosting. Key runtime limitations: no Node.js
filesystem (`fs` module is unavailable), no persistent connections, 30-second CPU timeout for
compute-heavy operations. Use Web Crypto instead of Node crypto. Use standard `fetch` for
outbound HTTP. Use cloud databases — Turso, Supabase, PlanetScale, or Neon — for persistence
since the local filesystem is unavailable. For CORS on web clients, export an `OPTIONS`
handler that returns the appropriate access control headers.

Never expose API keys or secrets in client code. Always validate and sanitize user input —
especially dynamic route params and query strings before using them in queries. Use proper
HTTP status codes. Handle all errors with try/catch and return structured JSON error responses.

---

## Deployment Workflow

The standard path is: develop locally → TestFlight internal → TestFlight external beta →
App Store or Play Store production. Never skip TestFlight. Beta App Review is faster and more
lenient than full App Store Review. Internal testers get builds immediately with no review.
External testers need one Beta App Review then get subsequent builds immediately.

For iOS: run `npx testflight` to build and submit to TestFlight in one command. Set
`EXPO_APPLE_ID` and `EXPO_APPLE_TEAM_ID` environment variables to skip interactive prompts.
For App Store production, use `eas build -p ios --profile production --submit`. Credentials
are managed via `eas credentials`. Use an App Store Connect API key for CI/CD to avoid 2FA
prompts on every submission — configure it in `eas.json` under `submit.production.ios`.
Common rejection reason: missing `usesNonExemptEncryption` declaration — add it to `app.json`
under `expo.ios.config`. See `ios-app-store.md` for the complete checklist.

For Android: set up a Google Play Console service account, download the JSON key, configure
its path in `eas.json` under `submit.production.android.serviceAccountKeyPath`. Use the
`internal` track first, then promote through `alpha`, `beta`, `production`. Use staged rollout
with `releaseStatus: "inProgress"` and a `rollout` percentage for major updates. Use
`autoIncrement: true` in `eas.json` so version codes are managed automatically and you never
hit "version code already used" errors. See `play-store.md` for setup details.

For web: `npx expo export -p web` then `eas deploy --prod` for production, `eas deploy` for
preview branches. PR previews can be automated via EAS Workflows — see `workflows.md`.

For App Store metadata, use `eas metadata:push` with a `store.config.json` file. This manages
titles, descriptions, keywords, categories, age ratings, and review information from version
control. Use all 100 available keyword characters — every character adds discoverability.
Keywords in the title field carry 10x more ranking weight than the keyword field. Do not
duplicate keywords across title, subtitle, and the keyword field — Apple counts each word once
globally. See `app-store-metadata.md` for the full ASO optimization guide.

---

## EAS Workflows and CI/CD

EAS Workflows live in `.eas/workflows/*.yml`. They support triggers on push, pull_request,
schedule, and manual `workflow_dispatch`. Job types are: `build`, `submit`, `update` (OTA),
`deploy` (web), and `run` (custom shell commands). Jobs can express dependencies using `needs`.

The schema evolves as new features are added — always fetch the live schema before authoring
YAML and validate before committing. Use the scripts in `references/scripts/` for this.
The `validate.js` script fetches the live schema with caching via `fetch.js` (which uses ETags
and Cache-Control headers for efficient revalidation) and runs AJV validation. Install
script dependencies first with `npm install` in the `references/scripts/` directory.

Use `workflow_dispatch` for manual production releases that need human approval. Use PR preview
deploys for web and OTA update branches for native previews. Use tag-based triggers (`v*`) for
versioned production releases. Store secrets in EAS Secrets, not in workflow YAML files.

---

## SDK Upgrade Checklist

When upgrading Expo SDK, follow this order strictly to avoid dependency version conflicts.

First run `npx expo install expo@latest` then `npx expo install --fix` to align all package
versions. Run `npx expo-doctor` to identify remaining issues. Clear caches by deleting
`node_modules` and `.expo`, running `watchman watch-del-all`, and reinstalling. Check the
release notes at `https://expo.dev/changelog` for breaking changes specific to the target version.
Run `npx expo prebuild --clean` only if native directories need regeneration. For bare workflow:
update CocoaPods with `pod install --repo-update` and Gradle with `./gradlew clean`.

During housekeeping: delete `sdkVersion` from `app.json` — Expo manages it. Remove implicit
packages from `package.json` that are transitive deps: `@babel/core`, `babel-preset-expo`,
`expo-constants`. Delete `babel.config.js` if it only contains `babel-preset-expo`. Delete
`metro.config.js` if it only contains Expo defaults. Remove `autoprefixer` — not needed SDK 53+.
Review `expo.install.exclude` entries in `package.json` — exclusions added as temporary workarounds
may no longer be needed. Check the `patches/` directory for outdated patches to remove.

Enable React Compiler in SDK 54+ by adding `"experiments": { "reactCompiler": true }` to
`app.json`. It automatically memoizes components and eliminates most `useMemo`, `useCallback`,
and `React.memo` usage. It is safe to enable on existing code — if it cannot optimize a
component it skips it silently without breaking anything.

---

## WebGPU and 3D Graphics

WebGPU requires a custom build — it does not work in Expo Go. Use these exact locked package
versions to avoid type errors and runtime incompatibilities: `react-native-wgpu@^0.4.1`,
`three@0.172.0`, `@react-three/fiber@^9.4.0`, `wgpu-matrix@^3.0.2`, `@types/three@0.172.0`.
Install with `--legacy-peer-deps` due to canary peer dependency conflicts. Configure Metro to
resolve `three` imports as the WebGPU build, not the default WebGL build.

Two required lib files must be created manually — `make-webgpu-renderer.ts` and
`fiber-canvas.tsx` — as they are project scaffolding, not published packages. These wrap
the native canvas, initialize the WebGPU renderer asynchronously, and integrate with
React Three Fiber's root reconciler. Extend the Three.js namespace with every component
class you use before mounting the scene, or you will get "not part of the THREE namespace"
runtime errors. Wrap 3D scenes in `React.lazy` for code-splitting and faster initial load.
See `webgpu-three.md` for the complete setup including orbit controls and particle systems.

---

## DOM Components

DOM components (files with `'use dom'` at the top) run in a WebView on iOS and Android and
as regular React on web. Use them when you need a web-only library: charts (recharts,
chart.js), syntax highlighters, rich text editors, or any library that depends on DOM APIs.

Each DOM component must be in its own dedicated file with a single default export. Props must
be serializable — strings, numbers, booleans, arrays, plain objects. Async functions passed
as props become bridge calls from web content into native code. Router hooks that require
synchronous access to native routing state — `useLocalSearchParams`, `usePathname`,
`useSegments` — do not work inside DOM components. Read these values in the native parent
component and pass them as serializable props. See `use-dom.md` for CSS isolation rules and
full examples.

---

## Tailwind Setup Requirements

Tailwind CSS via NativeWind v5 requires a specific setup that differs substantially from
NativeWind v4. There is no `babel.config.js` for Tailwind — configuration is CSS-first.
You need `@tailwindcss/postcss`, `react-native-css`, `nativewind@5`, and `tailwindcss@^4`.
Metro config uses `withNativewind` with `inlineVariables: false` and
`globalClassNamePolyfill: false`. Every component that uses `className` must be wrapped with
`useCssElement` from `react-native-css` — you cannot use `className` on plain React Native
components. Create wrapper components for View, Text, ScrollView, Pressable, Image, and
any animated components you need. Theme variables use `@theme` in CSS, not `tailwind.config.js`.
Platform media queries like `@media ios` and `@media android` apply platform-specific CSS
variables. See `expo-tailwind.md` for the complete setup including Apple semantic colors.

---

## Common Pitfalls

Using `className` without the full NativeWind setup — it silently does nothing and no error
is thrown, making it very hard to debug.

Using `Dimensions.get()` instead of `useWindowDimensions` — breaks when the window resizes
on iPad split screen or foldable devices.

Putting padding on a ScrollView's `style` prop instead of `contentContainerStyle` — content
clips at the edges during scrolling because the scroll hit area does not grow with the style.

Using `Platform.OS` instead of `process.env.EXPO_OS` — misses build-time tree-shaking and
includes dead platform code in the bundle.

Keeping `newArchEnabled: true` in `app.json` for SDK 53+ — it is now the default and the
field is redundant but harmless if left in.

Installing both `expo-av` and `expo-video` simultaneously on Android — VideoView shows a black
screen. Uninstall expo-av entirely before installing expo-video.

Setting `player.currentTime` in the `useVideoPlayer` setup callback on Android — it does not
take effect. Set it after the component mounts in a useEffect instead.

Calling `useLoaderData()` directly in the default export of a route — it suspends during client
navigation and requires a Suspense boundary above the calling component. Push the
`useLoaderData` call into a child component and wrap the screen's default export in Suspense.

Not calling `player.seekTo(0)` before calling `player.play()` when replaying completed audio —
the player stays at the end of the track and calling `play()` alone does nothing.

Using `Link.AppleZoom` with sheets or popovers — zoom transitions only work inside a Stack
navigator. They do not work with modal presentation.

Animating `width` and `height` style properties with Reanimated — these trigger layout
recalculation on every frame. Use `transform: [{ scaleX }, { scaleY }]` instead to keep
animations entirely on the UI thread.

Using NativeTabs with dynamic tab counts or conditionally hiding tabs after initial render —
tabs must be static because changes remount the navigator and lose navigation state. Use the
`hidden` prop only during the initial render based on startup state, not as a toggle.

Not providing a `+not-found.tsx` file — users who navigate to a nonexistent route get a
blank screen with no way to recover.

---

## Validating EAS Workflow YAML

Always validate EAS workflow YAML before committing. The schema evolves as new job types,
trigger options, and expression contexts are added.

Install the validation script dependencies first by running `npm install` inside
`references/scripts/`. Then run `node references/scripts/validate.js path/to/workflow.yml`
to validate against the live schema. The script caches schema responses for 15 minutes using
ETags so repeated runs are fast. To fetch the raw schema for reference, run
`node references/scripts/fetch.js https://api.expo.dev/v2/workflows/schema`.

Do not rely on memorized job type names or parameter names — always confirm against the live
schema. The `expo-cicd-workflows.md` reference covers all job types, trigger types, expression
syntax, `${{ }}` context variables, and the `needs` dependency system in full detail.
