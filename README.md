# Google、Claude、OpenAI 域名规则

| 服务 | XML plist | Shadowrocket | 规则数 |
| --- | --- | --- | ---: |
| Google · Gmail、Workspace、Gemini、AI Studio、Antigravity、YouTube、Play、Cloud | [Google.plist](Google.plist) | [Google.list](shadowrocket/Google.list) | 863 |
| Anthropic · Claude 网页、Desktop / Cowork、Claude Code、API、MCP | [Claude.plist](Claude.plist) | [Claude.list](shadowrocket/Claude.list) | 24 |
| OpenAI · ChatGPT、Codex、API、Sora、Prism | [OpenAI.plist](OpenAI.plist) | [OpenAI.list](shadowrocket/OpenAI.list) | 42 |

每份 plist 包含两个字符串数组。 `Domain` 精确匹配主机名，`DomainSuffix` 匹配域名本身及其子域名。对应的 `.list` 使用 `DOMAIN` 和 `DOMAIN-SUFFIX`，与 plist 的匹配范围一致。规则数按条目计算，一条后缀可覆盖多个服务，例如 `google.com` 同时覆盖 `mail.google.com` 和 `gemini.google.com`。

## 在 Shadowrocket 使用

用 `.list` 生成带策略的规则行，再把输出粘贴到当前配置的 `[Rule]` 段。以下命令在仓库根目录运行。把文件名换成要用的服务，把 `PROXY` 换成你的策略名。把这些行放在会匹配同一流量的宽泛规则和 `FINAL` 之前。

```sh
awk -v policy=PROXY '!/^#/ && NF { print $0 "," policy }' shadowrocket/OpenAI.list
```

如果你把 `.list` 托管在设备可访问的 HTTPS 地址，也可以使用 `RULE-SET,https://你的地址/OpenAI.list,PROXY`。GitHub 私有仓库的文件需要授权访问，本地粘贴方式无需额外托管。ChatGPT 和 Codex 的代理还需要支持发往 `chatgpt.com` 的 WebSocket。 [OpenAI 网络说明](https://help.openai.com/en/articles/9247338-network-recommendations-for-chatgpt-errors-on-web-and-apps)

## 收录范围

- Google 覆盖已知地区域、品牌顶级域、Google API 和常见产品域，也包含社区规则记录的广告、统计和历史服务域。Google Cloud 和 Firebase 的通用项目托管域如 `run.app`、`appspot.com`、`web.app` 维持原有分流。Antigravity 的两个 `run.app` 更新主机有独立的精确规则。
- Claude 和 OpenAI 收录服务域名、登录、资源、更新，以及有来源的特定第三方主机。Claude Code 安装方式或插件可能另需 `storage.googleapis.com`、`registry.npmjs.org` 或 `raw.githubusercontent.com`，这些通用依赖由相应的服务规则处理。OpenAI 的部分支持和邮件流程使用共享 Intercom 或 SendGrid 域，按需在现有配置中处理。
- 这是基于公开资料和现有规则整理的域名快照。新增或动态端点、按 URL 路径区分的流量、IP 直连与第三方连接器取决于实际使用场景。

## 来源

- Google 官方网络资料。 [Gmail](https://knowledge.workspace.google.com/admin/gmail/advanced/gmail-firewall-settings?hl=en) · [Gemini](https://knowledge.workspace.google.com/admin/generative-ai/gemini-app/gemini-app-firewall-settings?hl=en) · [Antigravity 企业网络](https://antigravity.google/docs/enterprise) · [Antigravity 部署与更新端点](https://docs.cloud.google.com/docs/security/compliance/deploy-antigravity-us-public-sector)
- Anthropic 官方网络资料。 [Claude Code](https://code.claude.com/docs/en/network-config#network-access-requirements) · [Claude Desktop](https://code.claude.com/docs/en/desktop#network-access-requirements)
- OpenAI 官方资料。 [ChatGPT 和 Codex 网络说明](https://help.openai.com/en/articles/9247338-network-recommendations-for-chatgpt-errors-on-web-and-apps) · [ChatGPT Sites](https://openai.com/policies/chatgpt-sites-terms/) · [Prism](https://openai.com/index/introducing-prism/)
- v2fly 社区规则。展开 Google 的子集并合并重复规则，保留域名匹配类型。 [Google](https://github.com/v2fly/domain-list-community/blob/98c43cad27fa8f02043858763709fe4a95432ffb/data/google) · [Anthropic](https://github.com/v2fly/domain-list-community/blob/98c43cad27fa8f02043858763709fe4a95432ffb/data/anthropic) · [OpenAI](https://github.com/v2fly/domain-list-community/blob/98c43cad27fa8f02043858763709fe4a95432ffb/data/openai) · [上游 MIT 许可证](LICENSE.v2fly)
- Shadowrocket 社区规则和本机配置用于交叉核对。 [blackmatrix7](https://github.com/blackmatrix7/ios_rule_script/tree/0b6897dc17e0c513d25449c935a9694818e32f0c/rule/Shadowrocket)
