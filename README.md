# make-knowledqe-cards

把粘贴的文章或本地 Markdown/TXT 文件提炼为通常 **5～8 张知识卡片**的 Codex Skill。每张包含标题、核心知识、简明解释、例子或自测问题。

项目及调用名称为 **`make-knowledqe-cards`**，请按此拼写使用。

## 解决什么问题

文章中的知识点可能分散、重复，整篇摘要又不便逐点复习。本项目为 Codex 提供一套提炼规则，把重要内容整理为可独立阅读、方便自测的知识卡片，减少手工筛选和整理的工作。

这是在 Codex 中使用的纯指令 Skill，不包含独立命令行程序、运行脚本或服务。

## 主要功能

- **文本输入**：接受粘贴的文章正文，以及本地 `.md`、`.markdown`、`.txt` 文件。
- **提炼与去重**：优先选择重要知识，一张只讲一个知识点，合并同义或重复内容。
- **数量按内容调整**：通常输出 5～8 张；不足 5 个独立知识点时按实际数量输出；没有可提炼知识时输出 0 张并说明原因。
- **忠于原文**：要求保留作者观点的归属、适用条件及不确定性；遇到矛盾或缺失信息时说明限制，不用外部知识补齐。
- **例子或自测**：优先使用原文例子；没有相关例子时，生成仅凭原文即可回答的自测题，并附参考答案。
- **Markdown 输出**：默认使用中文并在对话中回复；可明确指定其他语言，或要求保存到指定文件，不修改输入文件。

暂不支持网页抓取、PDF、Anki 和图形界面，也不负责其他文件格式的转换。生成结果仍需对照原文核查，规则本身不保证模型每次都完全正确。

## 安装方法

需要可加载本地 Skill 的 Codex 环境；处理本地文件时，Codex 需要有读取该文件的权限。日常使用不需要为本项目安装 Python、Node.js 或其他运行依赖。

将整个 `make-knowledqe-cards` 文件夹复制到目标项目的 `.agents/skills/` 下。以安装到当前项目为例，在本项目根目录执行以下命令（macOS / Linux）：

```sh
mkdir -p .agents/skills
cp -R make-knowledqe-cards .agents/skills/
```

安装后的关键文件位置：

```text
.agents/skills/make-knowledqe-cards/
├── SKILL.md
└── agents/openai.yaml
```

在目标项目中打开 Codex 后即可按名称调用；若未显示新 Skill，重启 Codex 再试。目录及加载规则参见 [OpenAI 官方 Skill 文档](https://learn.chatgpt.com/docs/build-skills)。本项目不提供自动安装器；上述命令由使用者手动执行。

## 使用方法

以下提示词发送给 Codex，不是终端命令。前两种方式需要先完成安装。

### 粘贴文章

```text
$make-knowledqe-cards
请把下面的文章整理为知识卡片：
（在这里粘贴文章正文）
```

### 读取本地文件

```text
$make-knowledqe-cards 请将 ./tests/inputs/01-science.md 整理为知识卡片。
```

将示例路径替换为自己的 Markdown/TXT 文件；相对路径以当前工作目录为准。需要保存时，在请求中追加“请保存为 `./cards.md`”；需要英文时，追加“请使用英文，包括字段名称”。默认只在对话中输出。

### 不安装，直接试用

在当前项目中明确让 Codex 读取 Skill 文件，也可以按同一套规则处理文章：

```text
请读取 make-knowledqe-cards/SKILL.md，按其中规则将 tests/inputs/01-science.md 制作为知识卡片。
```

## 输入输出示例

下面使用项目中的[重复短文测试输入](tests/inputs/05-short-duplicate.txt)，展示“去重后不凑数量”的行为。

输入正文：

```text
阅读时先写下自己想回答的问题。
开始读之前，先把想解答的问题记下来。
也就是说，先记下问题，再开始阅读。
```

对应的[示例输出](tests/outputs/05-short-duplicate.md)：

```markdown
原文的三句话表达同一个知识点，去重后只输出 1 张。

## 卡片 1｜阅读前先写下问题

- **核心知识**：开始阅读前，先写下自己想回答的问题。
- **简明解释**：原文强调“先记录问题，再开始阅读”的顺序，没有进一步说明原因或效果。
- **例子或自测**：问题：原文建议在开始阅读前做什么？参考答案：写下自己想回答的问题。
```

标题行即每张卡片的标题字段，其余三个字段依次为核心知识、简明解释、例子或自测。实际生成的措辞可能不同。

完整的多卡示例：[科普输入](tests/inputs/01-science.md) → [6 张卡片](tests/outputs/01-science.md)。

## 开发与验证

- [SKILL.md](make-knowledqe-cards/SKILL.md)：提炼与输出规则。
- [agents/openai.yaml](make-knowledqe-cards/agents/openai.yaml)：显示名称、简介和默认提示词。
- [测试记录](tests/REPORT.md)：7 组文章的输入、试用输出、原文核对和迭代说明。

本 Skill 使用 Codex 随附的官方 `init_skill.py` 初始化，已通过 `quick_validate.py` 校验。维护 Skill 时，如果本机有官方工具和安装了 PyYAML 的 Python 环境，可在项目根目录重新校验（工具路径因安装方式而异）：

```sh
python3 "${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-creator/scripts/quick_validate.py" ./make-knowledqe-cards
```

官方工具不随本项目分发，Python 和 PyYAML 仅用于上述开发校验。校验器验证 Skill 基本结构；7 组文章测试是单次模型试用与自查，不是自动化语义评测。测试样本为本项目自编材料，不包含第三方文章。

## 许可

[MIT](LICENSE)。
