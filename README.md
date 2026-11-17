# dsh-superagent-test-project

用于验证 DSH Super Agent 的测试仓库。

## 项目用途

本仓库是 [DeepSeek Harness (DSH) Super Agent](https://github.com/deepseek-ai/dsh) 的测试项目，主要用于：

- **验证 Super Agent 工作流**：测试从 Issue 创建到 PR 提交的完整自动化流程。
- **功能回归测试**：确保 Super Agent 的各项能力（代码修改、文档更新、PR 创建等）按预期运行。
- **示例参考**：作为使用 DSH Super Agent 管理项目的参考示例。
- **练习项目托管**：作为开源项目导入和二次开发的练习平台。

## 项目列表

### schedule

> 路径：[`projects/schedule/`](projects/schedule/)

Python 任务调度库，导入自上游 [dbader/schedule](https://github.com/dbader/schedule)（MIT 许可证）。提供简洁的人性化 API 来调度周期性任务，是本仓库通过 Super Agent 自动导入的第一个练习项目。

#### 项目简介

`schedule` 是一个轻量级的 Python 进程内任务调度器，用直观的链式调用语法替代传统的 cron 配置：

```python
import schedule
import time

def job():
    print("I'm working...")

schedule.every(10).minutes.do(job)
schedule.every().day.at("10:30").do(job)
schedule.every().monday.do(job)

while True:
    schedule.run_pending()
    time.sleep(1)
```

核心特点：零外部依赖、Builder 模式链式 API、内置时区支持（需安装 pytz）、完善的类型标注。

#### 与 Super Agent 测试项目的关系

- **导入方式**：通过 Issue #3 指派 Super Agent（`@super-agent-bot923`）自动完成克隆、目录整理、许可证保留、测试验证和 PR 提交，全流程无需人工写代码。
- **练习用途**：后续可在 `projects/schedule/` 上练习 Super Agent 的代码修改、Bug 修复、功能增强等任务，验证 Agent 的开发辅助能力。
- **上游同步**：上游信息记录在 [`UPSTREAM.md`](projects/schedule/UPSTREAM.md)，如需同步上游更新可创建新 Issue 指派 Agent 操作。

#### 环境依赖

| 依赖项 | 版本要求 | 说明 |
|--------|----------|------|
| Python | ≥ 3.7 | 核心运行时 |
| pytest | — | 运行测试（开发依赖） |
| pytz | — | 时区功能（可选依赖，不安装则跳过时区相关测试） |

schedule 本身**无任何运行时外部依赖**，安装即用。

#### 部署手册

**安装（作为库使用）：**

```bash
cd projects/schedule
pip install -e .
```

**运行测试：**

```bash
cd projects/schedule
pip install pytest        # 安装测试框架
python -m pytest test_schedule.py -v
```

**运行完整测试（含时区测试）：**

```bash
pip install pytz          # 安装时区依赖
python -m pytest test_schedule.py -v
```

**测试结果参考（Python 3.14.3 / pytest 9.2.0）：**

- 40 passed, 41 skipped（未安装 pytz 时跳过全部时区相关测试）
- 详细记录见 [`TEST_RESULTS.md`](projects/schedule/TEST_RESULTS.md)

#### 架构手册

schedule 的核心代码集中在单一模块 [`schedule/__init__.py`](projects/schedule/schedule/__init__.py)（约 945 行），采用 Builder 模式设计，架构简洁明了：

```
schedule/
├── __init__.py          # 核心模块，包含所有类和默认调度器实例
└── py.typed             # PEP 561 类型标注标记
```

**核心类：**

| 类名 | 职责 |
|------|------|
| `Scheduler` | 调度器主体，管理 Job 列表，提供 `run_pending()`、`run_all()`、`every()`、`cancel_job()`、`clear()` 等调度控制方法 |
| `Job` | 单个调度任务，通过 Builder 链式调用设置执行间隔（`.seconds`/`.minutes`/`.hours`/`.days`/`.weeks`）、时间点（`.at()`）、标签（`.tag()`）、截止时间（`.until()`）和执行函数（`.do()`） |
| `CancelJob` | 标记类，Job 函数返回此对象可取消自身 |
| `ScheduleError` | 基础异常类 |
| `ScheduleValueError` | 值异常（继承 ScheduleError） |
| `IntervalError` | 间隔配置异常（继承 ScheduleValueError） |

**执行流程：**

1. `Scheduler.every(interval)` 创建 `Job` 并加入调度器的 jobs 列表
2. 通过 Job 的链式调用（`.minutes`、`.at("10:30")`、`.do(func)`）配置调度参数
3. 主循环中反复调用 `Scheduler.run_pending()`，遍历 jobs 执行到期的任务
4. 每次执行后 Job 自动计算下次运行时间（`_schedule_next_run()`）

**模块级默认实例：**

`schedule/__init__.py` 在模块底部创建了一个全局 `Scheduler` 实例，并将它的公共方法暴露为模块级函数，使得用户可以直接 `import schedule; schedule.every(10).minutes.do(job)` 而无需手动实例化 `Scheduler`。

## 使用说明

### 前置条件

- 拥有 GitHub 账号，并被添加为本仓库的协作者。
- 已安装并配置 [DSH Super Agent](https://github.com/deepseek-ai/dsh)。

### 通过 Issue 触发任务

1. 在本仓库中创建一个 Issue，描述你希望 Super Agent 完成的任务。
2. 在 Issue 内容中 `@super-agent-bot923`，明确指派给 Super Agent。
3. Super Agent 将自动接收任务，创建分支、修改代码并提交 Pull Request。
4. 在 PR 中审查变更，确认无误后合并即可。

## 注意事项

- Super Agent 不会直接推送到 `main` 分支，所有变更均通过 PR 提交。
- 每个 PR 需要人工审查后手动合并。
- 如需停止正在进行的任务，请在对应的 Issue 或 PR 中留言说明。
