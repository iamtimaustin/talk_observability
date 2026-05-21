---
# try also 'default' to start simple
theme: default
background: https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?q=80&w=2564&auto=format&fit=crop
# some information about your slides (markdown enabled)
title: Observability in Practice
info: |
  ## Observability in Practice: logs, metrics, and APM traces with Datadog
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# apply UnoCSS classes to the current slide
class: text-center my-auto
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable Comark Syntax: https://comark.dev/syntax/markdown
comark: true
# duration of the presentation
duration: 60min
---

# Observability in Practice
### Logs, Metrics, and APM Traces with Datadog

<div class="mt-8 opacity-80 text-lg">
  A Dev Talk for Mid-Senior Engineers
</div>

<div class="mt-16 text-sm opacity-60">
  Datadog Observability Series • 60-70 min
</div>

<style>
h1 {
  background-color: #7000FF;
  background-image: linear-gradient(45deg, #7000FF 10%, #E000FF 90%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide.
-->

---
transition: fade-out
---

# Talk Overview

A production-grounded session designed to shift how you reason about your codebase.

<div class="grid grid-cols-2 gap-8 mt-8">
  <div class="p-6 rounded-lg border border-purple-500/20 bg-purple-900/10 backdrop-blur-md text-left">
    <h3 class="text-purple-400 font-bold mb-2">🎯 Objective</h3>
    <p class="text-sm opacity-90 leading-relaxed">Not just explaining the tooling, but changing how you write and review code to make systems fundamentally <strong>reason-able</strong>.</p>
  </div>
  <div class="p-6 rounded-lg border border-purple-500/20 bg-purple-900/10 backdrop-blur-md text-left">
    <h3 class="text-purple-400 font-bold mb-2">⏱️ Runtime</h3>
    <p class="text-sm opacity-90 leading-relaxed">60-70 Minutes including Q&A. Grounded in real Datadog usage and two production case studies.</p>
  </div>
</div>

<div class="mt-8 p-4 rounded-lg bg-gray-50 dark:bg-gray-800/40 text-left">
  <span class="font-semibold text-purple-400">The Core Pillars We'll Cover:</span>
  <div class="flex gap-8 mt-4 text-sm justify-around text-center">
    <div>🪵 <strong class="text-purple-300 block mt-1">Logs</strong><span class="opacity-60 text-xs">What happened & when</span></div>
    <div>📊 <strong class="text-purple-300 block mt-1">Metrics</strong><span class="opacity-60 text-xs">Behavior over time</span></div>
    <div>🗺️ <strong class="text-purple-300 block mt-1">Traces</strong><span class="opacity-60 text-xs">End-to-end request flow</span></div>
  </div>
</div>

---
transition: slide-up
---

# 01 · The Pain Before Observability
<div class="text-lg text-red-400 font-semibold mb-4 text-left">A familiar scene: Production is down.</div>

<div class="grid grid-cols-3 gap-4 mt-6 text-left">
  <div class="p-4 rounded bg-red-900/10 border border-red-500/20">
    <div class="text-2xl mb-2">💬</div>
    <span class="font-bold text-sm block mb-1 text-red-300">Slack Guessing</span>
    <p class="text-xs opacity-75 leading-relaxed">Engineers throwing out wild theories in incident channels with zero data.</p>
  </div>
  <div class="p-4 rounded bg-red-900/10 border border-red-500/20">
    <div class="text-2xl mb-2">🪵</div>
    <span class="font-bold text-sm block mb-1 text-red-300">Tailing Raw Logs</span>
    <p class="text-xs opacity-75 leading-relaxed">Staring at thousands of unparsed stdout lines scrolling past in a terminal.</p>
  </div>
  <div class="p-4 rounded bg-red-900/10 border border-red-500/20">
    <div class="text-2xl mb-2">🔄</div>
    <span class="font-bold text-sm block mb-1 text-red-300">Hopeful Re-deploying</span>
    <p class="text-xs opacity-75 leading-relaxed">"Let's just bounce the service and see if it clears the error state."</p>
  </div>
</div>

<div class="mt-8 border-l-4 border-red-500 pl-4 py-2 bg-red-500/5 text-left">
  <p class="italic text-sm">
    "This is the world of <strong>monitoring</strong> without <strong>observability</strong>. You know the system is sick, but you have no way to ask arbitrary questions about its internal behavior."
  </p>
</div>

---
layout: center
class: text-center
---

# The Paradigm Shift

<div class="text-2xl my-6 font-light">
  A system that <span class="text-red-400 font-semibold">runs</span> is not the same as a system you can <span class="text-green-400 font-semibold">reason about</span>.
</div>

<div class="max-w-xl mx-auto text-sm opacity-80 leading-relaxed">
  Our goal isn't just to keep the lights on; it's to build systems where, at 2 AM, an on-call engineer can confidently answer:
  <div class="text-purple-400 mt-2 font-mono">"Exactly why did this specific user's checkout fail?"</div>
</div>

---

# 02 · The Three Pillars — A Mental Model

Before touching Datadog, we need a shared understanding of what these tools actually do.

<div class="grid grid-cols-3 gap-6 mt-6 text-left">
  <div class="p-5 rounded-lg bg-gray-800/50 border border-gray-700">
    <div class="text-purple-400 text-lg font-bold mb-2">🪵 Logs</div>
    <div class="text-xs uppercase tracking-wider opacity-60 mb-2">The Breadcrumbs</div>
    <p class="text-sm opacity-85 leading-relaxed">Discrete events in time. Tell you <strong>what</strong> happened at a specific millisecond, written for debugging.</p>
  </div>
  <div class="p-5 rounded-lg bg-gray-800/50 border border-gray-700">
    <div class="text-purple-400 text-lg font-bold mb-2">📊 Metrics</div>
    <div class="text-xs uppercase tracking-wider opacity-60 mb-2">The Vital Signs</div>
    <p class="text-sm opacity-85 leading-relaxed">Aggregated numbers over time. Tell you <strong>how healthy</strong> the system is overall (CPU, latencies, rates).</p>
  </div>
  <div class="p-5 rounded-lg bg-gray-800/50 border border-gray-700">
    <div class="text-purple-400 text-lg font-bold mb-2">🗺️ Traces</div>
    <div class="text-xs uppercase tracking-wider opacity-60 mb-2">The Map</div>
    <p class="text-sm opacity-85 leading-relaxed">Distributed journeys. Follow <strong>a single request</strong> across service boundaries, databases, and queues.</p>
  </div>
</div>

---
layout: two-cols
layoutClass: gap-12
---

# The Diagnostics Analogy

When a patient arrives in the ER, a doctor doesn't just guess or start random surgery. They use a structured diagnostic pipeline.

<div class="mt-8 border-l-4 border-purple-500 pl-4 py-2 bg-purple-500/5 text-left">
  <span class="font-semibold text-purple-400 text-sm">Unified Diagnostic Workflow:</span>
  <p class="text-xs opacity-75 mt-1 leading-relaxed">You rarely need all three diagnostic layers at once, but when you are diagnosing a complex pathology, they must all be available and <strong>perfectly aligned</strong>.</p>
</div>

::right::

<div class="space-y-4 mt-12 text-sm text-left">
  <div class="flex items-start gap-3">
    <div class="bg-blue-500/20 text-blue-400 p-2 rounded text-xs font-bold w-20 text-center">VITALS</div>
    <div>
      <strong>Metrics</strong>
      <p class="text-xs opacity-75 mt-1">Check heart rate, blood pressure. (Is the system alive and functioning right now?)</p>
    </div>
  </div>
  <div class="flex items-start gap-3">
    <div class="bg-yellow-500/20 text-yellow-400 p-2 rounded text-xs font-bold w-20 text-center">HISTORY</div>
    <div>
      <strong>Logs</strong>
      <p class="text-xs opacity-75 mt-1">Read the chart, prior symptoms. (What events occurred leading up to this moment?)</p>
    </div>
  </div>
  <div class="flex items-start gap-3">
    <div class="bg-red-500/20 text-red-400 p-2 rounded text-xs font-bold w-20 text-center">IMAGING</div>
    <div>
      <strong>Traces</strong>
      <p class="text-xs opacity-75 mt-1">Order an MRI or CT scan. (Where is the exact flow of fluids or signals blocked?)</p>
    </div>
  </div>
</div>

---
layout: two-cols
layoutClass: gap-12
---

# Unified Observability

The magic of Datadog isn't having three separate tabs. It's the **instant correlation pivot** between them.

<div class="mt-8 border-l-4 border-purple-500 pl-4 py-2 bg-purple-500/5 text-left">
  <span class="font-semibold text-purple-400 text-sm">Rapid MTTR:</span>
  <p class="text-xs opacity-75 mt-1 leading-relaxed">
    Moving seamlessly from a metric spike to the exact offending database query trace, and then straight to the error log line.
  </p>
</div>

<div class="mt-8 text-sm opacity-80 text-left">
  This correlation loop cuts incident resolution time from <strong>hours</strong> to <strong>seconds</strong>.
</div>

::right::

```mermaid {theme: 'neutral', scale: 0.7}
flowchart TD
    A[📈 Metric Spike] -->|Click to Pivot| B[🗺️ Trace View]
    B -->|Identify Hotspot| C[🔍 Context Span]
    C -->|Extract Logs| D[🪵 Correlated Logs]
    D -->|Actionable Bug| E[🛠️ Root Cause Resolved!]
    style A fill:#7000FF,stroke:#fff,color:#fff
    style B fill:#3e1c69,stroke:#fff,color:#fff
    style C fill:#1e1035,stroke:#fff,color:#fff
    style D fill:#1e1035,stroke:#fff,color:#fff
    style E fill:#052e16,stroke:#22c55e,color:#fff
```

---
layout: two-cols
layoutClass: gap-8
---

# 03 · Logs — Structured is Not Optional

Logs are not for reading. Logs are for **querying**.

<div class="p-4 rounded-lg bg-red-900/10 border border-red-500/20 text-sm mt-4 text-left">
  <h3 class="text-red-400 font-bold mb-2">❌ Unstructured (Human-Only)</h3>
  <pre class="font-mono text-[10px] text-red-200">[ERROR] Something went wrong with user 1234</pre>
  <ul class="text-[11px] list-disc pl-4 mt-2 space-y-1 opacity-75">
    <li>Requires regular expressions to extract data</li>
    <li>Impossible to aggregate rates or counts</li>
    <li>No trace correlation linked automatically</li>
  </ul>
</div>

::right::

<div class="p-4 rounded-lg bg-green-900/10 border border-green-500/20 text-sm mt-12 text-left">
  <h3 class="text-green-400 font-bold mb-2">✅ Structured JSON (Machine-Readable)</h3>
  <pre class="font-mono text-[10px] text-green-200">{
  "level": "error",
  "message": "payment_failed",
  "user_id": "1234",
  "order_id": "abc-789",
  "amount_cents": 4999,
  "error_code": "card_declined",
  "trace_id": "abc123def456",
  "duration_ms": 342
}</pre>
  <ul class="text-[11px] list-disc pl-4 mt-2 space-y-1 opacity-75">
    <li>Datadog indexes every field instantly</li>
    <li>Filter, group, and alert on exact keys</li>
  </ul>
</div>

---

# Anatomy of a Production-Ready Log

To make logs highly queryable under pressure, ensure these fields are automatically injected:

<div class="grid grid-cols-2 gap-6 mt-6 text-left">
  <div class="space-y-4">
    <div class="p-3 bg-gray-800/40 rounded border-l-4 border-purple-500">
      <strong class="text-purple-400 text-sm font-mono block mb-1">trace_id / span_id</strong>
      <p class="text-xs opacity-75 leading-relaxed">The absolute thread. Connects the log line directly to the APM trace waterfall.</p>
    </div>
    <div class="p-3 bg-gray-800/40 rounded border-l-4 border-purple-500">
      <strong class="text-purple-400 text-sm font-mono block mb-1">user_id / tenant_id</strong>
      <p class="text-xs opacity-75 leading-relaxed">Contextual metadata. Allows filtering logs for a specific customer report.</p>
    </div>
  </div>
  <div class="space-y-4">
    <div class="p-3 bg-gray-800/40 rounded border-l-4 border-purple-500">
      <strong class="text-purple-400 text-sm font-mono block mb-1">duration_ms</strong>
      <p class="text-xs opacity-75 leading-relaxed">Timing data. Helps filter out slow operations in the log analytics tab.</p>
    </div>
    <div class="p-3 bg-gray-800/40 rounded border-l-4 border-purple-500">
      <strong class="text-purple-400 text-sm font-mono block mb-1">error_code</strong>
      <p class="text-xs opacity-75 leading-relaxed">Standardized codes (e.g. `rate_limit_exceeded`) rather than messy custom string messages.</p>
    </div>
  </div>
</div>

---

# Log Levels as Signal, Not Noise

If everything is flagged as `ERROR`, nothing is. Use strict heuristics to maintain operational signal:

<table class="w-full text-left border-collapse mt-8 text-sm">
  <thead>
    <tr class="border-b border-gray-700 bg-gray-850/50">
      <th class="p-3 text-purple-400 font-bold w-24">Level</th>
      <th class="p-3 text-purple-400 font-bold">Heuristic / Meaning</th>
      <th class="p-3 text-purple-400 font-bold">Production Behavior</th>
    </tr>
  </thead>
  <tbody>
    <tr class="border-b border-gray-800">
      <td class="p-3"><span class="bg-gray-700/30 text-gray-300 px-2 py-0.5 rounded text-xs font-mono">DEBUG</span></td>
      <td class="p-3">Highly detailed trace information for local development.</td>
      <td class="p-3 opacity-60">Filtered out or omitted in prod.</td>
    </tr>
    <tr class="border-b border-gray-800">
      <td class="p-3"><span class="bg-blue-500/20 text-blue-300 px-2 py-0.5 rounded text-xs font-mono">INFO</span></td>
      <td class="p-3">Normal system events worth recording (e.g. server startup, payment done).</td>
      <td class="p-3 opacity-80">Emitted normally, highly throttled.</td>
    </tr>
    <tr class="border-b border-gray-800">
      <td class="p-3"><span class="bg-yellow-500/20 text-yellow-300 px-2 py-0.5 rounded text-xs font-mono">WARN</span></td>
      <td class="p-3">Something unexpected, but handled successfully (e.g. database retry).</td>
      <td class="p-3 opacity-85">Emitted. Indicator of systemic decay.</td>
    </tr>
    <tr>
      <td class="p-3"><span class="bg-red-500/20 text-red-300 px-2 py-0.5 rounded text-xs font-mono">ERROR</span></td>
      <td class="p-3">Operation failed. A human needs to investigate or be paged.</td>
      <td class="p-3 font-semibold text-red-400">Triggers alert thresholds and alarms.</td>
    </tr>
  </tbody>
</table>

---

# 04 · Metrics — Guided Decision Making

Metrics aggregated over time provide quantitative evidence of system behavior.

<div class="grid grid-cols-3 gap-6 mt-8 text-left">
  <div class="p-4 bg-gray-800/40 rounded border border-gray-700/60">
    <div class="text-2xl mb-2">🔢</div>
    <h3 class="text-purple-400 font-bold text-sm mb-1">Counters</h3>
    <p class="text-xs opacity-75 leading-relaxed">Cumulative sums over time. Resets on service restarts.</p>
    <div class="mt-6 text-[11px] font-mono opacity-60 bg-black/20 p-1.5 rounded">e.g. requests_processed_total</div>
  </div>
  <div class="p-4 bg-gray-800/40 rounded border border-gray-700/60">
    <div class="text-2xl mb-2">🌡️</div>
    <h3 class="text-purple-400 font-bold text-sm mb-1">Gauges</h3>
    <p class="text-xs opacity-75 leading-relaxed">Point-in-time value. Can fluctuate up and down representing state.</p>
    <div class="mt-6 text-[11px] font-mono opacity-60 bg-black/20 p-1.5 rounded">e.g. active_connections_gauge</div>
  </div>
  <div class="p-4 bg-gray-800/40 rounded border border-gray-700/60">
    <div class="text-2xl mb-2">📊</div>
    <h3 class="text-purple-400 font-bold text-sm mb-1">Histograms</h3>
    <p class="text-xs opacity-75 leading-relaxed">Distribution of values. Crucial for measuring percentiles (p95, p99).</p>
    <div class="mt-6 text-[11px] font-mono opacity-60 bg-black/20 p-1.5 rounded">e.g. request_duration_seconds</div>
  </div>
</div>

---

# The RED Method

The industry standard framework for measuring service health. Implement this for every endpoint.

<div class="grid grid-cols-3 gap-6 mt-8 text-left">
  <div class="p-5 rounded-lg border-l-4 border-blue-500 bg-blue-900/10">
    <span class="text-xs uppercase font-bold text-blue-400 block mb-1">R</span>
    <h3 class="text-lg font-bold mb-1">Rate</h3>
    <p class="text-xs opacity-75 leading-relaxed">The volume of requests your service is processing per second.</p>
    <div class="mt-8 text-[11px] font-mono bg-black/30 p-2 rounded">sum:requests.rate{*}</div>
  </div>
  <div class="p-5 rounded-lg border-l-4 border-red-500 bg-red-900/10">
    <span class="text-xs uppercase font-bold text-red-400 block mb-1">E</span>
    <h3 class="text-lg font-bold mb-1">Errors</h3>
    <p class="text-xs opacity-75 leading-relaxed">The fraction of requests failing (HTTP 5xx, uncaught exceptions).</p>
    <div class="mt-8 text-[11px] font-mono bg-black/30 p-2 rounded">sum:requests.errors{*}</div>
  </div>
  <div class="p-5 rounded-lg border-l-4 border-purple-500 bg-purple-900/10">
    <span class="text-xs uppercase font-bold text-purple-400 block mb-1">D</span>
    <h3 class="text-lg font-bold mb-1">Duration</h3>
    <p class="text-xs opacity-75 leading-relaxed">Response times distribution. Focus exclusively on p95 or p99 latencies.</p>
    <div class="mt-8 text-[11px] font-mono bg-black/30 p-2 rounded">p95:requests.latency{*}</div>
  </div>
</div>

---

# Business Metrics vs. Infrastructure

Infrastructure metrics are table stakes. Custom business metrics are what drive product decisions and priority.

<div class="grid grid-cols-2 gap-8 mt-6 text-left">
  <div class="p-5 bg-gray-800/40 rounded-lg border border-gray-700 text-sm">
    <h3 class="text-purple-400 font-bold mb-3">🛠️ Infrastructure Vitals</h3>
    <ul class="space-y-2 list-disc pl-4 opacity-80 text-xs leading-relaxed">
      <li>Is the CPU overloaded or throttled?</li>
      <li>Is memory leaking over time?</li>
      <li>Are thread connection pools exhausted?</li>
    </ul>
    <div class="mt-8 text-[11px] font-mono opacity-50 bg-black/10 p-1 rounded">system.cpu.idle, system.mem.used</div>
  </div>
  <div class="p-5 bg-gray-800/40 rounded-lg border border-gray-700 text-sm">
    <h3 class="text-purple-400 font-bold mb-3">💼 Custom Conversion Funnel</h3>
    <ul class="space-y-2 list-disc pl-4 opacity-80 text-xs leading-relaxed">
      <li>How many checkout funnels were started?</li>
      <li>What is the payment success percentage?</li>
      <li>Where are users dropping off?</li>
    </ul>
    <div class="mt-8 text-[11px] font-mono opacity-50 bg-black/10 p-1 rounded">orders.checkout.started, orders.checkout.completed</div>
  </div>
</div>

<div class="mt-6 text-center text-sm font-semibold text-purple-300">
  💡 Crucial rule: Agree on team-wide tagging standards (e.g. <code>env:prod</code>, <code>service:checkout</code>) from day one.
</div>

---
layout: two-cols
layoutClass: gap-12
---

# 05 · Case Study: Conversion Funnel

Using structured custom metrics to diagnose business-level degradation.

<div class="mt-8 space-y-6 text-sm text-left">
  <div>
    <h3 class="text-purple-400 font-bold mb-1">🔍 The Discovery</h3>
    <p class="text-xs opacity-75 leading-relaxed">A Datadog dashboard monitored checkout. We spotted a massive delta between <code>orders.checkout.started</code> and <code>completed</code>.</p>
  </div>
  <div>
    <h3 class="text-purple-400 font-bold mb-1">📊 The Evidence</h3>
    <p class="text-xs opacity-75 leading-relaxed">A <strong>78.8% conversion rate</strong> was observed, but the <strong>5xx error rate spiked to 0.89%</strong> during peak traffic, triggering page alarms.</p>
  </div>
  <div>
    <h3 class="text-purple-400 font-bold mb-1">💡 The Outcome</h3>
    <p class="text-xs opacity-75 leading-relaxed">Turned "I think payment is slow" into "I know payment fails for 1.1% of checkouts." We got the roadmap allocation to resolve it.</p>
  </div>
</div>

::right::

<div class="flex items-center justify-center h-full mt-4">
  <img class="rounded border border-purple-500/20 shadow-lg shadow-purple-500/10 max-h-80 object-contain" src="./public/assets/datadog_metrics_dashboard.png" alt="Datadog Metrics Dashboard" />
</div>

---

# 06 · APM Traces — Following the Request

Logs and metrics tell you *that* something is broken. Traces tell you *where*.

<div class="grid grid-cols-2 gap-8 mt-6 text-left">
  <div class="p-5 bg-gray-800/40 rounded-lg border border-gray-700">
    <h3 class="text-red-400 font-bold text-sm mb-2">Without Tracing</h3>
    <p class="text-xs opacity-75 leading-relaxed">
      A request fails. You guess which database query is responsible, add manual log statements, rebuild the container, deploy to staging, run a test, and scroll logs.
    </p>
    <div class="mt-6 text-xs font-bold text-red-400">🛑 Resolution Time: Hours</div>
  </div>
  <div class="p-5 bg-gray-800/40 rounded-lg border border-gray-700">
    <h3 class="text-green-400 font-bold text-sm mb-2">With Distributed Tracing</h3>
    <p class="text-xs opacity-75 leading-relaxed">
      A request fails. You view the trace waterfall. The system shows that service A waited 5.2s for service B, which was blocked by a single un-indexed SQL query on PostgreSQL.
    </p>
    <div class="mt-6 text-xs font-bold text-green-400">🚀 Resolution Time: Seconds</div>
  </div>
</div>

---

# Trace Vocab: Spans & Context Propagation

Understanding the engineering mechanics of distributed tracing.

<div class="grid grid-cols-3 gap-6 mt-8 text-left">
  <div class="p-4 bg-gray-800/40 rounded border border-gray-700/60">
    <h3 class="text-purple-400 font-bold text-sm mb-2">1. Trace</h3>
    <p class="text-xs opacity-75 leading-relaxed">The end-to-end journey of a single incoming request. Contains all microservice hops, from gateway to DB.</p>
  </div>
  <div class="p-4 bg-gray-800/40 rounded border border-gray-700/60">
    <h3 class="text-purple-400 font-bold text-sm mb-2">2. Span</h3>
    <p class="text-xs opacity-75 leading-relaxed">A single unit of work within a trace. Has a start time, duration, and status (e.g. database query, local function call).</p>
  </div>
  <div class="p-4 bg-gray-800/40 rounded border border-gray-700/60">
    <h3 class="text-purple-400 font-bold text-sm mb-2">3. Context Propagation</h3>
    <p class="text-xs opacity-75 leading-relaxed">Injecting and extracting <code>trace_id</code> headers across HTTP, TCP, or queues so downstream systems stay linked.</p>
  </div>
</div>

<div class="mt-6 p-4 rounded bg-purple-900/15 border border-purple-500/20 text-center text-xs">
  ⚠️ Frameworks auto-instrument standard calls, but you must define <strong>custom spans</strong> around slow background tasks and complex business logic.
</div>

---

# Reading a Waterfall View

How to spot architectural performance bugs from a trace layout.

<div class="grid grid-cols-3 gap-6 mt-6 text-left">
  <div class="p-4 bg-gray-800/30 rounded border-t-4 border-blue-500">
    <h4 class="font-bold text-sm mb-1">📏 Span Width</h4>
    <p class="text-xs opacity-75 leading-relaxed">Width corresponds directly to time. Focus exclusively on the widest horizontal bars; they represent your latency bottleneck.</p>
  </div>
  <div class="p-4 bg-gray-800/30 rounded border-t-4 border-yellow-500">
    <h4 class="font-bold text-sm mb-1">🕳️ Empty Gaps</h4>
    <p class="text-xs opacity-75 leading-relaxed">Gaps between blocks indicate un-instrumented execution: garbage collection (GC), local CPU-bound computations, or network delays.</p>
  </div>
  <div class="p-4 bg-gray-800/30 rounded border-t-4 border-red-500">
    <h4 class="font-bold text-sm mb-1">🔁 Sequential Spans (N+1)</h4>
    <p class="text-xs opacity-75 leading-relaxed">Dozens of tiny identical spans running in series mean you're executing database query lookups inside a loop. Batch them!</p>
  </div>
</div>

---

# 07 · Case Study: Flame Graph Literacy

Before showing the finding, let's learn how to read a flame graph without looking lost in group meetings.

<div class="grid grid-cols-3 gap-6 mt-8 text-left">
  <div class="p-4 rounded bg-gray-800/40 border border-gray-700/60 text-sm">
    <h4 class="text-purple-400 font-bold mb-1">🔥 X-Axis isn't Time</h4>
    <p class="text-xs opacity-75 leading-relaxed">The width indicates <strong>stack population percentage</strong> (how often the sampler saw this function active). Wide bars are expensive.</p>
  </div>
  <div class="p-4 rounded bg-gray-800/40 border border-gray-700/60 text-sm">
    <h4 class="text-purple-400 font-bold mb-1">🥞 Y-Axis is Stack Depth</h4>
    <p class="text-xs opacity-75 leading-relaxed">The bottom is the entry point (e.g. main handler), moving up to leaf-level execution code blocks.</p>
  </div>
  <div class="p-4 rounded bg-gray-800/40 border border-gray-700/60 text-sm">
    <h4 class="text-purple-400 font-bold mb-1">🚨 Wide Near Top</h4>
    <p class="text-xs opacity-75 leading-relaxed">A wide bar at the top of the stack is actively burning CPU cycles. A wide bar in the middle waiting for children is an I/O wait.</p>
  </div>
</div>

---
layout: two-cols
layoutClass: gap-12
---

# Case Study: Resolving the Hotspot

An unexpected database bottleneck is diagnosed under operational stress.

<div class="mt-8 space-y-6 text-sm text-left font-light">
  <div>
    <h3 class="text-purple-400 font-bold mb-1">📉 The Incident</h3>
    <p class="text-xs opacity-75 leading-relaxed">Our main API endpoint was timing out intermittently. Logs were flooded but showed no obvious trace.</p>
  </div>
  <div>
    <h3 class="text-purple-400 font-bold mb-1">🔍 The Trace</h3>
    <p class="text-xs opacity-75 leading-relaxed">The trace graph immediately isolated a <code>db_call</code> blocking for <strong>5.91 seconds</strong>. It was a <code>SELECT * FROM transaction_log</code> operation lacking an index.</p>
  </div>
  <div>
    <h3 class="text-purple-400 font-bold mb-1">🛠️ The Fix</h3>
    <p class="text-xs opacity-75 leading-relaxed">Added a composite index, reduced the span duration to 15ms, and instantly dropped p99 API latencies to double-digits.</p>
  </div>
</div>

::right::

<div class="flex items-center justify-center h-full mt-4">
  <img class="rounded border border-purple-500/20 shadow-lg shadow-purple-500/10 max-h-80 object-contain" src="./public/assets/datadog_flame_graph.png" alt="Datadog Flame Graph" />
</div>

---

# 08 · Designing for Observability — The Hard Shift

Observability is not a feature you bolt on after the code is written. It is a core design constraint.

<div class="mt-8 p-6 bg-purple-900/15 border-l-4 border-purple-500 rounded-r-lg max-w-2xl mx-auto text-left">
  <div class="text-xs uppercase tracking-wider text-purple-400 font-bold mb-1">The 2 AM On-Call Heuristic</div>
  <p class="text-lg italic font-semibold leading-relaxed">
    "If this code breaks in production at 2am, can I confidently diagnose the root cause from Datadog dashboards and logs alone, without SSHing into a server or running a local test?"
  </p>
</div>

<div class="mt-8 text-center text-sm opacity-80">
  If the answer is **no**, your PR is not ready to ship.
</div>

---

# Practical Code-Level Habits

Transition these principles into daily coding practices:

<div class="grid grid-cols-2 gap-6 mt-6 text-sm text-left">
  <div class="space-y-4">
    <div class="p-3 bg-gray-800/40 rounded border-l-4 border-green-500">
      <strong class="text-green-300 block mb-1">🪵 Meaningful State Changes</strong>
      <p class="text-xs opacity-75 leading-relaxed">Log events when states transition. <code>payment.initiated</code>, <code>payment.succeeded</code>. Avoid logging loop indices.</p>
    </div>
    <div class="p-3 bg-gray-800/40 rounded border-l-4 border-green-500">
      <strong class="text-green-300 block mb-1">🧹 Exception Hygiene</strong>
      <p class="text-xs opacity-75 leading-relaxed">Never write empty <code>catch</code> blocks. Log the stack trace and relevant entity context.</p>
    </div>
  </div>
  <div class="space-y-4">
    <div class="p-3 bg-gray-800/40 rounded border-l-4 border-green-500">
      <strong class="text-green-300 block mb-1">🚀 Context Over Loops</strong>
      <p class="text-xs opacity-75 leading-relaxed">Inject correlation headers before spawning async microservice threads or background queues.</p>
    </div>
    <div class="p-3 bg-gray-800/40 rounded border-l-4 border-green-500">
      <strong class="text-green-300 block mb-1">🏷️ Consistent Tagging</strong>
      <p class="text-xs opacity-75 leading-relaxed">Always attach key-value pairs (<code>env:prod</code>, <code>region:us-east</code>) to traces and custom events.</p>
    </div>
  </div>
</div>

---

# Observability-Driven Code Reviews

Make observability a mandatory review milestone. Ask these questions in every Pull Request:

<div class="grid grid-cols-2 gap-8 mt-8 text-sm text-left">
  <div class="space-y-4">
    <div class="flex gap-2">
      <span class="text-purple-400 font-bold">1.</span>
      <p class="opacity-85 leading-relaxed">"Does this new code path emit sufficient logging to identify when it fails?"</p>
    </div>
    <div class="flex gap-2">
      <span class="text-purple-400 font-bold">2.</span>
      <p class="opacity-85 leading-relaxed">"Are we capturing trace context through this new async rabbitMQ queue worker?"</p>
    </div>
  </div>
  <div class="space-y-4">
    <div class="flex gap-2">
      <span class="text-purple-400 font-bold">3.</span>
      <p class="opacity-85 leading-relaxed">"Are the new metrics named consistently with our team naming registry?"</p>
    </div>
    <div class="flex gap-2">
      <span class="text-purple-400 font-bold">4.</span>
      <p class="opacity-85 leading-relaxed">"Will this swallowed database exception black hole our alerts?"</p>
    </div>
  </div>
</div>

<div class="mt-8 p-4 rounded bg-gray-800/30 text-center text-xs border border-gray-700/50">
  This shifts observability from a platform-team bottleneck to a **shared engineering discipline**.
</div>

---

# 09 · Putting It Together in Datadog

How a unified observability platform streamlines real-world debugging:

<div class="flex justify-around items-center mt-12 text-sm text-center relative">
  <div class="w-48 p-4 rounded bg-red-900/10 border border-red-500/20 z-10">
    <div class="text-xs text-red-400 font-bold uppercase mb-1">Step 1</div>
    <strong>Metric Spikes</strong>
    <p class="text-[11px] opacity-75 mt-2">Alert triggers: 5xx rate jumps to 4%.</p>
  </div>
  
  <div class="text-2xl opacity-40">➔</div>

  <div class="w-48 p-4 rounded bg-purple-900/10 border border-purple-500/20 z-10">
    <div class="text-xs text-purple-400 font-bold uppercase mb-1">Step 2</div>
    <strong>Filter Traces</strong>
    <p class="text-[11px] opacity-75 mt-2">Isolate traces degrading in that specific window.</p>
  </div>
  
  <div class="text-2xl opacity-40">➔</div>

  <div class="w-48 p-4 rounded bg-blue-900/10 border border-blue-500/20 z-10">
    <div class="text-xs text-blue-400 font-bold uppercase mb-1">Step 3</div>
    <strong>Locate Span</strong>
    <p class="text-[11px] opacity-75 mt-2">Trace waterfall points directly to a slow DB span.</p>
  </div>

  <div class="text-2xl opacity-40">➔</div>

  <div class="w-48 p-4 rounded bg-green-900/10 border border-green-500/20 z-10">
    <div class="text-xs text-green-400 font-bold uppercase mb-1">Step 4</div>
    <strong>Inspect Logs</strong>
    <p class="text-[11px] opacity-75 mt-2">Correlated logs show the exact syntax exception.</p>
  </div>
</div>

<div class="mt-12 text-center text-xs opacity-60">
  This seamless pivot is only possible if your services consistently propagate and log their <strong>trace_id</strong>.
</div>

---

# 10 · Close: Where to Start on Monday

Abstract advice changes nothing. Try these three concrete tasks starting Monday morning:

<div class="grid grid-cols-3 gap-6 mt-8 text-left">
  <div class="p-5 rounded bg-gray-800/40 border border-gray-700/60">
    <div class="text-2xl mb-2">🪵</div>
    <h3 class="text-purple-400 font-bold text-sm mb-1">1. In Your Next PR</h3>
    <p class="text-xs opacity-75 leading-relaxed">Add just <strong>one</strong> structured field (e.g. <code>user_id</code> or <code>duration_ms</code>) to an existing raw string log.</p>
  </div>
  <div class="p-5 rounded bg-gray-800/40 border border-gray-700/60">
    <div class="text-2xl mb-2">📊</div>
    <h3 class="text-purple-400 font-bold text-sm mb-1">2. In Your Next Endpoint</h3>
    <p class="text-xs opacity-75 leading-relaxed">Implement the three <strong>RED Method metrics</strong> (Rate, Errors, Duration) using Datadog client libraries.</p>
  </div>
  <div class="p-5 rounded bg-gray-800/40 border border-gray-700/60">
    <div class="text-2xl mb-2">🗺️</div>
    <h3 class="text-purple-400 font-bold text-sm mb-1">3. In Your Next Bugfix</h3>
    <p class="text-xs opacity-75 leading-relaxed">Wrap the suspected bottleneck method in a <strong>custom tracer span</strong> to visualize it on the flame graph.</p>
  </div>
</div>

<div class="mt-6 text-center text-sm font-semibold text-purple-300">
  📈 Observability compounds. Small, incremental additions save massive incident hours later.
</div>

---
layout: center
class: text-center
---

# Q&A & Open Discussion

<div class="text-lg my-6 font-light">
  What observability blind spots or incidents have caught you off-guard?
</div>

<div class="text-xs opacity-50 space-y-1">
  <p>Datadog Observability Series</p>
  <p>Presented as an Internal Dev Talk</p>
</div>
