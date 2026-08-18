# Magmaflow: an AI-native spatial creative environment

**Live: <https://design.charbelmalo.online>** — open it. The running app is the artifact.

> An AI-native spatial creative environment with depth-aware layers, shaders, particles,
> 3D assets and agent-readable project state.

## What it is

Magmaflow is a design environment where the canvas is spatial rather than flat and the
document is a first-class surface for agents as well as a pen. It is a ~3,700-file
TypeScript/Vue monorepo rendering through WebGPU with a WebGL2 fallback, shipping as a
browser app and a Tauri v2 desktop build, with an MCP sidecar exposing 200+ design tools
to agents.

It is built on a fork of **OpenPencil**, an MIT-licensed open-source design editor
supplying the substrate: Skia/CanvasKit renderer, Yoga WASM layout, the `.fig` Kiwi
codec, scene graph, base tool registry. What is mine is the spatial camera model,
the agent surface, the publish pipeline and the evaluation lens on top.

**Where this sits relative to existing canvas tools.** Magmaflow extends the canvas
paradigm rather than arguing against it. Every design tool worth using converged on the
same primitives — frames, auto layout, components, variables — and Magmaflow keeps them,
reading and writing `.fig` natively. What it adds is a third axis, an agent API over the
document, and a publish path, on the bet that the next thing a canvas absorbs is depth,
motion and a collaborator that reads the file as well as draws in it.

## The spatial architecture

The canvas is 3D-native: layers carry an authored depth and one shared perspective camera
projects every plane. Three decisions make that survivable.

**Depth is authored, never inferred.** An earlier version derived depth from stacking
order, which turned every flat document into a staircase the moment the camera engaged.
Now every layer starts at `z = 0` and only an explicit number moves it, accumulating as a
signed sum down the tree — so a negative offset holds one child back while its siblings
ride their group forward.

**Occlusion is layer order; Z is depth, and they may disagree.** The paint walk visits
children in order and never sorts, so paint order — and with it hit-testing priority —
stays exactly the stacking order the layer panel shows. Animated depth arrives through a
per-frame overlay that the scene walk, projection and hit-testing all read from, so draw
and geometry cannot disagree.

**The 2D path is byte-identical when the camera is flat**, pinned by a command-stream
golden test rather than promised in a comment. Every 3D path is dead at the defaults —
what makes it shippable into existing documents rather than a separate mode.

On top of that: shader and noise fills, particles, importable 3D scene bundles, Gaussian
splats, and publish tiers that cool a document into a standalone page.

## The agent surface

Tools are defined once, framework-agnostically, and surfaced through three hosts — the
in-app AI chat, the MCP server (stdio and HTTP), and the CLI's `eval` path — so an agent
and a human operate the same document through the same operations. Coding agents run in
the chat panel over ACP against that server.

The permission model is the part I would defend hardest. **Autopilot Mode auto-answers
only tools inside Magmaflow's own MCP namespace.** The agent's shell, file and network
tools always prompt, and Autopilot answers with the narrowest allow, so nothing persists
into the user's agent config. The observation behind it: the dangerous permission
prompt is not the one you deny, it is the one you approve forty times until approving is
reflex. Scoping auto-answer to design operations confines that reflex to what cannot hurt
you.

There is also a **design-judge lens** — a rubric engine shared by a CLI and an in-editor
inspector, deliberately *not* an agent tool: that would bury a design verdict under
conversation turns. Deterministic hard gates run *before* any model call, since rejection
is free and inference is not. And the rubric only discriminates because of its anchors:
loosen the grading instruction and every criterion returns ~0.9 — identical to a rubric
that works. Adding one now costs a measurement, not just a diff.

## What is demonstrated, and what is not

**Demonstrated, by opening the link:** a working WebGPU/WebGL2 spatial canvas; native
`.fig` read/write; the depth camera and its 2D-parity guarantee; the MCP surface with 200+
tools; agent-driven document construction; shader and particle fills; 3D scene bundles and
splats; the desktop build and publish pipeline; and a quality gate covering architecture
rules, type-aware lint, unit tests and Playwright E2E/visual-regression suites.

**Not evidenced.** No public adoption, no measured task completion against another tool,
no user study — publishing those numbers would mean inventing them.

## Honest limitations

- **One user, no adoption data.** Every usability claim is an n-of-1 dogfooding claim.
- **It is a fork.** Mine is the spatial camera model, the agent and permission surface, the
  judge lens, publish tiers, 3D and shader work, and the design system — not the base
  editor.
- **Source is private** (self-hosted). The live deployment is the public artifact; a
  walkthrough is available.
- **Collaboration is presence-only.** Cursors and awareness are shipped; document *content*
  sync is not implemented.
- **Some capabilities cannot survive publishing, and say so.** A cooled page has no camera,
  so camera-dependent motion cools to rest with a disclosed diagnostic rather than dropping
  silently — disclosed degradation is still degradation.
- **Pen hardware validation is incomplete.** The pen grammar and input pipeline are real;
  validation on real tablet hardware is open.
