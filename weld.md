# Weld: a native macOS agent-built writing surface

## What it is

Weld is a macOS menu-bar app that watches the **system-wide focused text field** through
the `ApplicationServices` / `AXUIElement` accessibility APIs and floats a prompt toolbar
beside whatever field you are typing in. You compose a prompt, send it, and Weld pastes
the result back into that field. Separate panels configure the backing LLM and the
pre-baked prompt library.

It is a real `.app` bundle — native SwiftUI plus AppKit, not Catalyst — targeting macOS
26 Tahoe with a macOS 15 fallback. 143 tracked Swift files (160 on disk), 36 commits.

Most AI writing tools ask you to leave what you are writing and come back with text.
Weld's bet is that the right surface for "help me with this sentence" is *at* the
sentence — same field, same caret, no context switch.

## Why native, and why the Accessibility APIs

There is no other way to build this. A browser extension reaches one browser; an Electron
shell reaches its own window. The focused text field in Mail, Xcode, Slack, Notes and a
random web form is only addressable through `AXUIElement` — its value, its selected range,
its screen geometry so a panel can position against it. That choice dictates the rest:

- **The panel must not take focus.** Floating panels are `NSPanel` with
  `.nonactivatingPanel`, because the instant Weld becomes frontmost, the field it is
  helping you with loses first responder and the interaction collapses. This is enforced as
  a project convention, not left to each surface to remember.
- **Privileges must be honest.** Entitlements and Info.plist usage strings change in the
  same commit as the code needing them, so the app can never request a capability it does
  not use, or use one it did not declare.
- **TCC grants are keyed to a stable path.** The install script builds a signed Release
  into the canonical `/Applications/Weld.app` rather than running from derived data,
  because accessibility grants follow the bundle path. Getting this wrong looks like a
  permissions bug and is a build-layout bug.

## The multi-agent build process

Weld was built by directing a hierarchy of specialist agents with an orchestrator,
**Athena**, at the top — an Input → Reasoning → Action → Output engine that decomposes
intent, routes each piece to the specialist that owns it, enforces project conventions,
corrects sub-agent output, runs a verification pass, and returns one result.

| Agent | Owns |
|---|---|
| `swiftui-implementer` | Feature UI — Liquid Glass, animation, gestures, theming |
| `build-fixer` | Root-cause repair of red builds and failing tests |
| `accessibility-engineer` | The AXUIElement core — watcher, geometry, paste-back |
| `ux-auditor` | QA — drives the app, captures evidence, writes the audit |

Routing is explicit: a new visual surface goes to the implementer; a red build goes to
`build-fixer` **first, always**; the focused-text watcher and panel geometry go to the
accessibility engineer. Athena sequences chains (implement → build-fix → audit) and holds
the result until green.

**My contribution versus the agents'.** I architected the system, specified behavior,
evaluated outputs, resolved failures and owned the final product. Mine: the product thesis
and AX-first architecture; the agent roster, their boundaries and the routing rules; the
enforced conventions below; the design-token system; generating animation code from a
decoded design spec rather than hand-tuning it; and the audit protocol that decides when
work is done. The agents wrote most of the Swift. The leverage came from writing
constraints precise enough that implementation could be delegated and still land as one
coherent app.

## Craft details

The conventions Athena enforces are the app's real quality gate: AppKit / SwiftUI /
ApplicationServices only; **accessibility is first-class** — anything that paints UI
exposes `accessibilityLabel`, `accessibilityIdentifier` and `accessibilityValue` (an app
built on the accessibility APIs that is not itself accessible would be an embarrassment);
design tokens in `WeldDesignSystem`, never a hard-coded hex or point size; non-activating
panels; `async`/`await` with no completion handlers; no SPM dependencies, the project is a
pure `.xcodeproj`; and every Liquid Glass API gated with `if #available(macOS 26, *)` plus
a real macOS 15 fallback.

Two details I am specifically happy with. **The animated glyphs are generated, not
authored.** A client-approved Figma spec was decoded into a keyframe document with an HTML
replay serving as animation ground truth; the Swift keyframe file is generated from it and
marked never-hand-edit, which removes the place where design/build motion drift happens.
And **preview states are launch flags** — `--preview-toolbar`, `--show-settings`,
`--preview-notch=transcribe|compress|motion` open the app directly into a surface with
sample context. An interaction that only appears when the OS happens to focus the right
field is untestable; a flag makes it deterministic for the auditor agent and for me.

Verification produces evidence, not assertions: the auditor drives the running app and
captures screenshots and AX dumps into timestamped directories beside a dated report.

## Honest limitations

- **Zero external users.** No adoption, retention or task-completion data. What I can
  defend is the architecture and the craft, not a product outcome.
- **No published latency, accuracy or model-routing evaluation.** The audits are
  qualitative UX evidence, not a benchmark.
- **Source is private**, on self-hosted git. **Available for a live code walkthrough** —
  agent definitions, audit trail and generated-glyph pipeline included.
- **macOS 26 is the primary target.** The macOS 15 fallbacks exist and are gated, but the
  app is designed against the newer material APIs.
- **Permission-dependent by nature.** Without Accessibility and Input Monitoring grants it
  cannot function at all — inherent to the category, and a real onboarding cost.
