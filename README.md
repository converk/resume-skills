# resume-skills

两个配套的 agent skill：先把简历整理成结构化的简历文件夹，再用 @Browser（Codex 的内置浏览器）填招聘网站的表单。

> 使用 LLM 填写简历存在信息泄露风险，请谨慎考虑使用本仓库。

## 功能

- **resume-to-facts** —— 把一份或多份 PDF / Word 简历整理成「类-实例-属性」结构的 Markdown 简历文件夹。简历里没有的信息不会编造，而是列成清单问你要，直到每条属性都有交代。
- **job-form-filling** —— 读简历文件夹，把招聘网站的在线简历或投递表单填好。按模块填写、逐个保存并回读核对，不自动提交。

  **这个 skill 只用 @Browser（Codex 的内置浏览器）**：每个会话各有一份独立的浏览器，所以多个会话可以同时填不同的网站，附件上传也走同一条通道。

## 安装

建议**只装到具体工作区**，不要装成全局 skill：这两个 skill 只服务于简历这一件事，装全局会在别的项目里占用 skill 名额。

1. 下载本仓库（`git clone https://github.com/converk/resume-skills.git`，或在仓库页面下载 ZIP 后解压），拿到 `resume-to-facts/` 和 `job-form-filling/` 两个目录。

2. 把这两个目录整体复制到工作区的 skills 目录，目录名就是 skill 名：

   - Codex：`<工作区>/.agents/skills/`
   - DeepSeek Harness：`<工作区>/.dsh/skills/`（DSH 也识别 `.agents/skills/`）

   装好后形如：

   ```text
   <工作区>/.agents/skills/
   ├── resume-to-facts/
   └── job-form-filling/
   ```

   注意不要复制到全局目录（`~/.agents/skills`、`<dshHome>/skills`）。模板 `resumeTemplate/` 跟着 `resume-to-facts/` 一起装，不用单独处理。装完如果没生效就重启一下 harness。

3. **建议使用 Codex**：`job-form-filling` 用 Codex 桌面版自带的 @Browser 操作页面，开箱可用，并支持多会话并行。

## 使用

先用 `resume-to-facts` 建简历文件夹（产物是工作区根目录下的 `generated-resume/`），再用 `job-form-filling` 拿它填表。这两个 skill 本身不含个人数据，你的数据只放在 `generated-resume/` 里。

第一步，整理简历：

```text
用 resume-to-facts 把我的简历.pdf 整理成简历文件夹
```

它会读简历、建好 `generated-resume/`，然后把缺的信息一次性列出来问你。答完之后再进入第二步。

第二步，填招聘表单：

```text
@Browser 用 job-form-filling 帮我把这个招聘网站的表单填好，填完先别提交：https://example.com/apply
```

需要同时填多个网站时，每个网站开一个会话，各用各的 @Browser 并行填，互不干扰。登录、短信验证码和上传附件的确认仍然要你自己来。
