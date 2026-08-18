# Case studies — Charbel Malo

Senior AI Design Engineer / AI Product Prototyper specializing in human–AI interaction, agentic systems, cognitive interfaces, and spatial creative tools.

I design and build AI-native products that turn opaque model behavior, agent state and large semantic datasets into understandable human experiences.

---

## The three studies

| Study | One line | Source availability |
|---|---|---|
| [Mind / Iris](mind-iris.md) | A local-first personal context and agent-harness system combining MCP, scored retrieval, hierarchical memory, RAG and multiple inspection interfaces | Private (self-hosted git) — available for walkthrough |
| [Weld](weld.md) | A native macOS menu-bar writing surface that watches the system-wide focused text field via the Accessibility APIs, built by directing a multi-agent cluster | Private (self-hosted git) — available for walkthrough |
| [Magmaflow](magmaflow.md) | An AI-native spatial creative environment with depth-aware layers, shaders, particles, 3D assets and agent-readable project state | Private fork; **the live app is the artifact** |

Each study includes an explicit "Honest limitations" section. Where a claim is not
measured, it says so.

---

## Public artifacts you can open right now

| Artifact | What it is | Link |
|---|---|---|
| **Magmaflow (live)** | The spatial creative environment, running | <https://design.charbelmalo.online> |
| **nebulai** | Mechanistic-interpretability visualizer — tokens / SAE features / MLP neurons → UMAP → HDBSCAN → LLM auto-naming → WebGPU map. Renders 50,000+ feature nodes at 60 FPS via WGSL compute shaders. | <https://github.com/charbelmalo/nebulai> |
| **cox-protocol-package** | npm CLI + JSON-Schema spec for agent-readable UI/design contracts. 84% adoption across 35+ engineers and designers at VML Middle East; 40% reduction in visual-regression bugs. | [repo](https://github.com/charbelmalo/cox-protocol-package) · [npm](https://www.npmjs.com/package/cox-protocol-package) |
| **PrismaticShadersPack** | 9 effects authored three times over — WebGL2 GLSL, WGSL and Metal MSL — across 28 files. | <https://github.com/charbelmalo/PrismaticShadersPack> |
| **Semantha** | SAE-diffing client for comparing sparse-autoencoder features between models (~2,838 LOC). | <https://huggingface.co/spaces/charbel-malo/Semantha> |

Related background: [The Human–Agent Experience Layer](https://charbelmalo.online) —
steerable interfaces, deterministic boundaries and trust protocols for enterprise AI
adoption. Its headline finding: human-in-the-loop steering is **4× more effective at
decision checkpoints during agent execution** than post-hoc review of finished output,
and schema-constrained tool boundaries eliminated **80% of regressions**.

---

## What is *not* claimed anywhere in this repo

- External users or adoption numbers for Mind, Weld or Magmaflow. They have one user.
- Production scale beyond internal tooling at VML Middle East.
- Published research papers.

The metrics that are cited (cox-protocol adoption, nebulai frame budget, the 4×
steering result, the 80% regression reduction) come from work with real users or real
measurement. The rest is described as what it is: built, running, unmeasured.

---

## Contact

- **Email** — <charbel.work@outlook.com>
- **LinkedIn** — <https://linkedin.com/in/charbel-malo-b96857206>
- **Portfolio** — <https://charbelmalo.online>
- **GitHub** — <https://github.com/charbelmalo>
- **Hugging Face** — <https://huggingface.co/charbel-malo>
