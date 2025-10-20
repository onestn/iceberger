# 🧊🍔 Iceberger

**Iceberger** is an open-source service that provides **deep insights into Apache Iceberg metadata tables**  
(`$files`, `$snapshots`, `$history`, `$partitions`, …).  
It helps data engineers and operators **understand, monitor, and optimize** their Iceberg tables with ease.  

---

## ✨ Features
- **File Insight**: Track file counts, sizes, and delete file ratios over time  
- **Snapshot Timeline**: Visualize table changes, row counts, and rollback points  
- **Partition Explorer**: Detect skew, monitor growth, and partition evolution  
- **Optimize/Vacuum Advisor**: Recommendations based on fragmentation & delete vectors  
- **Integration**: Python SDK, CLI, and Airflow operators for automated collection  

---

## 🛠 Architecture
- **Core Engine**: Rust + Apache Arrow for fast metadata scanning  
- **Python SDK / CLI**: Easy scripting and integration with pipelines  
- **Airflow Operators**: Automate daily metadata collection & optimization tasks  
- **Optional Web UI**: Dashboard for files, snapshots, partitions, and cost analysis  

---

## 🚀 Roadmap
- [x] CLI for `$files` and `$snapshots` analysis  
- [ ] Snapshot Timeline dashboard  
- [ ] Optimize/Vacuum Advisor with cost estimation  
- [ ] Partition skew analyzer  
- [ ] Alerts & reporting (Slack, Email)  

---

## 🤝 Contributing
Contributions are welcome!  
See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.  

---

## 📜 License
Apache License 2.0  
