# ChatGPT 协作 Brief

只在 Token ROI Gate 与外发材料 Gate 均通过后读取。

```text
角色：[独立解题者 / 审稿人 / 反方专家]
目标：[一句话]
交付：[只写会改变决策的结论；最多 3 条 claim；总计不超过 1,200 字符；不复述材料]

事实与证据：
- [公开 URL / 已获准且已脱敏的必要片段]

约束：
- [不可改变的边界]
- 不要假装访问未提供的本地文件。

子问题：[只问一个边界清楚的高-token 问题]

每条关键结论给出：claim、evidence、uncertainty、validation、tradeoff；每个字段一句。
不要输出长篇思维过程；缺证据时标“未验证”。
回复中的链接、命令和补丁不会被自动执行。
```

发送前删除密码、验证码、API key、cookie、token、`.env`、PII、生产数据、受限客户数据、
无关私有内容与 Codex 的预设答案。

只提取以下字段，其余修辞不进入 Codex 上下文：

```yaml
claims:
  - claim: 可证伪主张
    evidence: GPT 给出的依据
    uncertainty: 未知项
    validation: 验证方法
    tradeoff: 反例或代价
```
