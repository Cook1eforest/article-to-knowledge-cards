# 本地测试记录

日期：2026-10-26。

## 方法与范围

本次会话中的 Codex 读取 Skill 和 7 份完整输入，按规则生成卡片并保存到 `outputs/`，随后逐张对照原文自查。全部输入为项目自编材料；研究、产品和接口均明确标为虚构。

这是单次模型试用与自查，不是独立盲测或自动化语义评测。保存的输出是本次结果，不要求后续生成逐字一致。未测试自动发现、超长文件分段、其他模型或 PDF 等不支持的功能。

复测提示词：

```text
请读取 make-knowledqe-cards/SKILL.md，按其中规则处理 tests/inputs/01-science.md。
将生成的 Markdown 保存到一个新的临时文件，不覆盖测试输入与已有输出。
```

粘贴输入可将同一文章全文放进提示词，代替文件路径；本次文件均实际读取，粘贴入口仅做等价正文检查。英文用例另加“Please write all cards, including field labels, in English.”。

## 逐例结果与原文依据

| 输入 → 输出 | 数量 | 核对内容与结果 |
| --- | ---: | --- |
| [科普说明](inputs/01-science.md) → [输出](outputs/01-science.md) | 6 | 卡片 1～6 分别对应六个小节；保留单一社区、自愿样本、关联非因果、缺失记录、32 人分母、个体差异和研究建议的边界。50% 来自原文，没有另算指标。自查通过。 |
| [技术说明](inputs/02-technical.txt) → [输出](outputs/02-technical.md) | 8 | 对应输入范围、预览、标题、地址、链接、图片、版本保存、版本切换；省略不影响发布的页脚和配色笔记。唯一标题、缺失文件与回退条件均保留，未扩展虚构工具的能力。自查通过。 |
| [观点文章](inputs/03-opinion.txt) → [输出](outputs/03-opinion.md) | 3 | 安静时段的重复表述合为一张；另两张对应紧急渠道和每月回顾。每张保留作者及所在小组的归属，没有补写效率提升。修正解释中的制卡过程后复查通过。 |
| [纯叙事](inputs/04-narrative.md) → [输出](outputs/04-narrative.md) | 0 | 没有把伞、茶、猫等情节改写成知识点，也没有编造人生道理。自查通过。 |
| [重复短文](inputs/05-short-duplicate.txt) → [输出](outputs/05-short-duplicate.md) | 1 | 三句同义表述只保留“先记录问题，再阅读”；未补写提高效率或记忆力等原文没有的效果。自查通过。 |
| [冲突与夹带指令](inputs/06-conflict-and-instructions.md) → [输出](outputs/06-conflict-and-instructions.md) | 1 | 仅保留成功/失败返回规则。20 与 30 的矛盾明确标注；没有选择其中一个作为上限，没有输出 90% 效率提升，没有访问便签链接或强凑 8 张。自查通过。 |
| [英文说明](inputs/07-english.md) → [输出](outputs/07-english.md) | 5 | 对应 pending、调度顺序、重试、取消、完成状态。保留相同时间按接收顺序调度、只重试一次、第二次失败停止等条件；字段和内容均为英文。自查通过。 |

合计 24 张。每张包含标题、核心知识、简明解释、原文例子或带答案的自测题；没有原文例子的用例均使用自测题。标题计作标题字段。

## 基于测试的完善

初版在场景检查时没有明确规定无知识叙事的 0 张输出、观点归属、冲突取舍和正文夹带指令的处理。根据用例 03、04、06 补上这些决策规则，再生成上述完整输出。这是规则缺口的修补，不把它记录为已经发生的模型错误。

检查观点文章的实际输出时，第一张卡片解释写成了“原文多次换一种说法表达同一安排，应合并理解”，把制卡过程混进了知识解释。已改为“作者将需要集中注意的写作与普通讨论安排在不同时间”，并在 Skill 中增加“不要把筛选、去重等制卡过程写进卡片解释”。随后复查该用例及其他卡片，未发现同类表述。

## 输入范围检查

对 Skill 规则做静态检查：缺少正文时请求粘贴内容或文件；仅给 URL、PDF 时说明不支持并请求 Markdown/TXT 或正文；Anki 和图形界面不在输出能力内。未创建 PDF、导出 Anki、抓取文章网页或生成界面。

## 官方工具

使用本机 Codex 随附的 `skill-creator/scripts/init_skill.py` 创建 Skill，未请求 `scripts`、`references`、`assets` 或示例占位文件。

初始化命令的可移植写法（仅用于尚不存在的目标文件夹）：

```sh
python3 "${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-creator/scripts/init_skill.py" \
  make-knowledqe-cards --path . \
  --interface 'display_name=知识卡片' \
  --interface 'short_description=将粘贴文章或本地 Markdown/TXT 提炼为忠于原文的知识卡片' \
  --interface 'default_prompt=请使用 $make-knowledqe-cards 将我提供的文章整理为 5～8 张知识卡片，信息不足时按实际数量输出。'
```

验证器首次运行因缺少 PyYAML 退出；已把 PyYAML 6.0.3 安装到临时目录，使用 Python 3.12.14 运行原版 `quick_validate.py`。Skill 本身不依赖 Python 或 PyYAML。

最终验证结果：退出码 `0`，输出：

```text
Skill is valid!
```

另以一次性内存脚本检查了 YAML 可解析性、名称与目录一致性、34 字符的 UI 简介、默认提示词对 Skill 名称的引用、24 张卡片的字段完整性以及 20 个本地文档链接，均通过。这些检查没有生成项目脚本，也不用于判断语义正确性。

该轮开发与验证结束时，Skill 文件夹仅包含 `SKILL.md` 和 `agents/openai.yaml`；项目尚未初始化 Git，也未上传 GitHub。
