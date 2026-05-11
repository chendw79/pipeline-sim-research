# Pipeline Simulation Research 📊

> 技术调研报告 — 油气管线瞬态模拟领域

## 调研目标

对 GitHub 上的油气管道储运模拟仿真相关开源项目进行全面调研，回答三个问题：

1. **现有开源方案的能力边界在哪里？**
2. **我们的 PipelineSim 如何定位？**
3. **下一步该做什么？**

## 核心发现

🚀 **开源界缺少 Python 语言的液体管道瞬态模拟器。**

| 项目 | 语言 | Stars | 局限性 |
|------|------|-------|--------|
| auralius/waterhammer | MATLAB | ⭐7 | 需付费MATLAB，无温度场 |
| FSund/transient-pipeline-flow | C++ | ⭐7 | 仅气体，隐式格式 |
| hafmed/water_hammer_simulation | C++ | ⭐3 | 一次性研究代码 |
| **PipelineSim** (ours) | **Python** | **🆕** | **液体 + 温度 + 泵 + 多管段** |

## 文档

- [RESEARCH.md](RESEARCH.md) — 详细调研报告
- [DEVELOPMENT_PLAN.md](DEVELOPMENT_PLAN.md) — 开发路线图

## 相关仓库

- [PipelineSim](https://github.com/chendw79/pipeline-sim) — 主项目：Python 瞬态模拟器

---

*调研时间: 2026年5月 | Orbit 🛸*
