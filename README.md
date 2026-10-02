# Role Model Lens

给普通 AI 对话加入一个有依据的 role model 视角。

不是扮演名人，也不是替你做决定。这个 skill 引导 AI 从人物公开的具体行动中寻找参照，说明证据与推论，再回到你的处境。

## 状态

项目地址：[riversliu1123/role-model-lens](https://github.com/riversliu1123/role-model-lens)。源码公开、商业再分发受限，不采用 MIT，也不标称 OSI 开源项目。

实验版，欢迎试用和反馈。它是一组 Markdown 指令，不是 App，不包含模型、聊天界面、人物数据库或自动事实检查服务。

现有指令在 Codex 中做过内部测试，但仍出现过度类比、回答过长和未经确认便假定用户决定权的问题。不是可靠性认证，也不保证能准确预测某个人会怎么想。其他宿主尚未验证。

## 安装到 Codex

下载或克隆这个仓库，在仓库根目录运行以下 macOS/Linux（sh）命令。Windows 尚未验证。已有同名文件、目录或符号链接时停止，不覆盖：

```sh
(
  set -eu
  test -f skills/role-model-lens/SKILL.md && test -f LICENSE || {
    printf '%s\n' "Run this from the downloaded repository root."
    exit 1
  }
  destination="${CODEX_HOME:-$HOME/.codex}/skills/role-model-lens"
  if [ -e "$destination" ] || [ -L "$destination" ]; then
    printf '%s\n' "Already exists: $destination. Review it before replacing it."
    exit 1
  fi
  mkdir -p "$(dirname "$destination")"
  mkdir "$destination"
  cp -R skills/role-model-lens/. "$destination/"
  cp LICENSE "$destination/LICENSE"
)
```

在下一轮对话中用 `$role-model-lens` 调用。如果宿主尚未识别，重新打开会话后再试。

安装报错时不要继续覆盖目标目录；先检查是否有残留。许可证随安装保留。

无需运行 npm、部署服务或提供本项目专用 API key。你仍需自行使用支持 skill 的 AI 宿主，其模型费用和数据规则由该宿主决定。联网检索能力也由宿主提供；没有可读取的材料时，AI 应说明限制，而不是编造证据。

## 怎么用

以下是输入示例，不是测试结果或名人本人给出的建议：

> 使用 $role-model-lens，加入 Beyoncé 的视角。我和两位朋友共同创作短片，约定最终剪辑共同决定。现在我们对结尾有分歧，帮我找到判断角度。简短一点，并附上支持这个角度的来源。

可以继续追问：

- 她具体做了什么？哪些是事实，哪些是你的推论？
- 这个经历和我的情况有什么不一样？
- 给我一个可以尝试的小行动，不要完整报告。
- 去掉这个人物视角，回到普通讨论。

人物不限于 Beyoncé，但资料是否充足、类比是否适合，都需要在当次对话中判断。没有合适证据时，应明确说没有，而不是硬套人物故事。

## 预期与限制

- 具体行动旁有来源；引用、转述和 AI 解读应能区分。
- 缺少会改变建议的背景时，应先问一个关键问题，或清楚说明建议的条件。
- 名人的成就不能替用户创造决定权，也不能替代专业判断。
- 简短对话优先，不强制输出报告；可以要求更深入的解释。
- 以上是指令目标，不是运行时保证。请检查重要事实及建议是否适用于自己。

## 隐私与第三方材料

仓库不含遥测、后端或自动存储程序。对话仍由你选用的宿主处理；skill 不能控制其保存、搜索、日志或删除行为。避免输入不必要的敏感信息。

本仓库只分发方法指令，不附带文章、歌词、肖像、录音或名人资料库。不代表任何人物授权或背书，不授予第三方材料的使用权。

## 反馈

欢迎在 GitHub Issues 中提交：宿主和模型、去除隐私后的最小问题、实际回答、你认为哪里不合适。尤其欢迎“故事不贴近”“没有比普通回答更有帮助”“来源不支持结论”和“太长”的反馈。

不要提交密钥、完整私人对话、他人个人信息或无权公开的材料。匿名化或使用合成例子即可。

首版只接受问题报告和使用反馈，暂不合并外部代码、指令或文档贡献。你仍可按许可证自行 fork 和免费分享修改版；向本项目提交内容不代表转让版权或授予额外商业授权。

## 使用许可

采用 [Role Model Lens Source-Available License 1.0](LICENSE)。这是未经法律专业审查的自定义许可，不保证法律可执行性；以下是摘要，以许可全文为准。

| 场景 | 使用边界 |
| --- | --- |
| 个人探索、公司内部辅助工作 | 允许 |
| 顾问自行选用 skill 作为内部工具，交付收费报告 | 允许；不向客户提供工具访问权，也不把代运行该 skill 作为服务售卖，无人工编辑量要求 |
| 免费分享原版或修改版 | 保留许可证、署名和相同限制；标明修改 |
| 卖 skill、放入收费课程资料包或付费订阅 | 须另行书面授权 |
| 集成到收费产品，或收费替客户运行本 skill | 须另行书面授权；包括付费产品的免费档位 |
| 自己付费使用通用 AI 宿主来运行 skill | 不因此被禁止 |

判断看向客户提供或承诺的访问和服务，不衡量人工分析占多少：顾问接收客户问题后自行选用它作为内部工具可以；让付费客户使用它，或明确销售代运行本 skill 的服务，须授权，改名“报告”不改变这一点。

许可只约束本项目材料及修改版，不限制未使用这些材料而独立开发的类似产品。首次收到书面违规通知后，有 30 个自然日整改并按条款恢复许可；整改期间须停止被禁止的行为。这不是允许继续违规的宽限期，恢复许可不免除过往责任。之后再次违规须书面批准恢复。

第三方素材不因本许可获得授权。商业授权可通过 [Issues](https://github.com/riversliu1123/role-model-lens/issues) 提交非敏感概要，未回复不代表许可。

## 文件

- `skills/role-model-lens/SKILL.md`：skill 入口。
- `skills/role-model-lens/references/evidence.md`：证据处理规则。
- `skills/role-model-lens/references/host-integration.md`：未来宿主集成建议，不是现成功能。
- `skills/role-model-lens/agents/openai.yaml`：Codex 展示信息。
