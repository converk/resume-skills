# resume-skills

两个给 agent 用的 skill：一个把简历变成结构化的简历文件夹，另一个拿简历文件夹去填招聘网站的表单。

## 功能

- **`resume-to-facts/`** —— 读一份或多份 PDF / Word 简历，按「类-实例-属性」生成 Markdown 简历文件夹。简历里没有的信息不会被编造，而是整理成一份清单问你要，直到每个实例的每条属性都有交代。
- **`job-form-filling/`** —— 读简历文件夹，把招聘网站的在线简历或投递表单填好。按模块填写、逐个保存并回读核对，不自动提交。

两个 skill 配套使用：第一个产出简历文件夹，第二个消费它。

## 目录

```text
工作/
├── generated-resume/       # 数据：真实的简历文件夹（把 resumeTemplate 复制过去、填上内容后的产物）
└── resume-skills/          # 技能包：可以整体搬走，里面不放个人数据
    ├── README.md           # 本文件
    ├── job-form-filling/   # skill：按简历文件夹填写招聘网站表单
    └── resume-to-facts/    # skill：把简历整理成简历文件夹
        ├── SKILL.md
        ├── references/
        └── resumeTemplate/ # 模板：13 个类各一份 README.md + 模板.md，外加根 README.md
```

`resumeTemplate/` 归 `resume-to-facts`：建库时把它整份复制到工作区，就成了 `generated-resume/`。`job-form-filling` 不碰模板，只读 `generated-resume/`（类目录里带着同一份 `README.md` 与 `模板.md`）。模板里不放任何个人信息；数据一律放在工作区的 `generated-resume/` 里，不进技能包。

两个 skill 各带一份 `references/resume-folder-spec.md`（同一份结构契约的两份副本）：建库的按它写，填表的按它读，谁都不需要去翻对方的目录。**改结构契约时两份要一起改**，改完可以 `diff` 一下确认内容一致。

## 使用

- 支持 skill 目录的 harness：直接说「把这份简历整理成简历文件夹」「把这个招聘网站的表单填好」，或按该 harness 的方式显式调用对应的 skill。
- 不支持 skill 目录的 harness：把对应的 `SKILL.md` 全文作为系统提示或首轮上下文交给 agent，需要细节时再让它读该技能目录下的 `references/`。
- 只用一次：把「任务描述 + 对应的 `SKILL.md` 全文」贴给任意 agent 即可，不需要安装。

结构和名字都是固定的，不做搜索：数据是工作区根目录下的 `generated-resume/`，模板是 `resume-to-facts/resumeTemplate/`。工作区根目录就是当前工作目录，用户明确指定了别处时按用户的来。

## 安装

把 `resume-to-facts/` 和 `job-form-filling/` 两个目录整体复制到 harness 的 skills 目录即可，目录名就是 skill 名：

- Codex：`$CODEX_HOME/skills`（未设置时是 `~/.codex/skills`）
- 其他 harness：它自己的 skills 目录，或任何会被扫描的目录

两个 skill 目录里只有 `SKILL.md` 与 `references/`，不带 harness 专用的 agent 定义和默认提示词：`SKILL.md` 的 front-matter 只写 `name` 与 `description`，其余 harness 依赖的元数据由各 harness 自己补。

模板跟着 `resume-to-facts/` 一起安装，不需要单独处理；`generated-resume/` 是数据，任何时候都不进 skills 目录。

两个 skill 都不依赖脚本运行时，复制过去即可使用；不需要时直接删掉目录就算卸载。
