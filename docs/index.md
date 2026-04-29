---
hide:
  - navigation
---

# Collaborative State Machines (CSM)

**A Unified Programming Model for the Computing Continuum**

CSM is a framework for developing applications that operate seamlessly across cloud, edge, and IoT
environments. Unlike component-centric models that often lead to fragmented architectures, CSM allows you to
model an entire distributed system as a single, coherent entity. By managing coordination and state at a
global level, the model simplifies orchestration and maintains architectural integrity across your
infrastructure.

---

<div style="display: flex; align-items: center; gap: 30px; margin-bottom: 40px;">
  <img src="assets/icons/noun-networking-5588829.svg" style="height: 110px; width: auto;" />
  <div>
    <strong>Hierarchical System Design</strong><br>
    CSM uses hierarchical nesting to manage complex application logic. By representing the application as a
    unified tree rather than isolated black boxes, the entire system topology remains visible. This enables
    the runtime to perform system-wide optimizations that are impossible in traditional architectures.
  </div>
</div>

<div style="display: flex; align-items: center; gap: 30px; margin-bottom: 40px;">
  <img src="assets/icons/noun-data-8097426.svg" style="height: 110px; width: auto;" />
  <div>
    <strong>Native Data Scoping</strong><br>
    Data management is integrated into the model through explicit scoping rules. CSM categorizes variables as
    persistent (global), transient (local to a machine), or static (local to a state). Aligning code structure
    with memory placement reduces network synchronization and improves data efficiency.
  </div>
</div>

<div style="display: flex; align-items: center; gap: 30px; margin-bottom: 40px;">
  <img src="assets/icons/noun-computation-7788823.svg" style="height: 110px; width: auto;" />
  <div>
    <strong>Decoupled Logic and Execution</strong><br>
    Maintain a clean separation between high-level coordination and task execution. State machines handle
    logic and state transitions, while heavy processing and hardware I/O are offloaded to external services.
    These interactions are managed via declarative <code>Invoke</code> actions, keeping dependencies explicit.
  </div>
</div>

---

CSM provides the system-wide visibility required for advanced resource optimization. By exposing the full
application topology, the model supports automated scheduling and intelligent data placement across the
continuum.

- **CSML**: A declarative DSL for specifying system behavior without the overhead of low-level communication
  boilerplate.
- **Cirrina**: An open-source, Kotlin-based runtime designed to manage state machine lifecycles from edge
  devices to cloud clusters.
