# Google、Claude、OpenAI 域名规则

可直接在 Shadowrocket 的配置中订阅。最近核对日期为 2026-11-25。

| 服务 | 规则文件 | Raw 订阅 | 条数 |
| --- | --- | --- | ---: |
| Google · Gmail、Workspace、Gemini、AI Studio、Antigravity、YouTube、Play、Cloud | [Google.list](shadowrocket/Google.list) | [Raw](https://raw.githubusercontent.com/baohao2015/shadowrocket-provider-domains/main/shadowrocket/Google.list) | 869 |
| Anthropic · Claude、Desktop / Cowork、Claude Code、API、MCP、Claude Platform on AWS | [Claude.list](shadowrocket/Claude.list) | [Raw](https://raw.githubusercontent.com/baohao2015/shadowrocket-provider-domains/main/shadowrocket/Claude.list) | 58 |
| OpenAI · ChatGPT、Codex、API、Sora、Prism、OpenAI.fm | [OpenAI.list](shadowrocket/OpenAI.list) | [Raw](https://raw.githubusercontent.com/baohao2015/shadowrocket-provider-domains/main/shadowrocket/OpenAI.list) | 43 |

## 在 Shadowrocket 使用

把需要的行加到现有配置的 `[Rule]` 段，放在会匹配同一流量的宽泛规则和 `FINAL` 之前。可将 `PROXY` 换成你已有的策略组名。

```ini
RULE-SET,https://raw.githubusercontent.com/baohao2015/shadowrocket-provider-domains/main/shadowrocket/Google.list,PROXY
RULE-SET,https://raw.githubusercontent.com/baohao2015/shadowrocket-provider-domains/main/shadowrocket/Claude.list,PROXY
RULE-SET,https://raw.githubusercontent.com/baohao2015/shadowrocket-provider-domains/main/shadowrocket/OpenAI.list,PROXY
```

GitHub 的 `github.com/…/blob/main/…` 是浏览文件的 HTML 页面。点页面上的 **Raw** 会打开 `raw.githubusercontent.com/…/main/…`，返回 `.list` 的原始文本。Shadowrocket 用 Raw 地址读取规则。链接中的 `main` 是分支名，更新该分支后可继续使用同一个订阅地址。 [GitHub 文件说明](https://docs.github.com/en/repositories/working-with-files/using-files/viewing-and-understanding-files#viewing-or-copying-the-raw-file-content)

## 收录范围和维护

- Google 包含已知品牌顶级域、地区域、产品域及其基础设施。Cloud Run、App Engine、Firebase 和 Artifact Registry 的托管后缀也会匹配使用 Google 默认域名的第三方项目。Antigravity 的 API 和区域端点由 `googleapis.com` 覆盖，更新服务由 `run.app` 覆盖。
- Claude 收录官方服务、登录、内容、更新和公开的 Claude Platform on AWS 商用区域端点。后者按具体主机匹配。Claude Code 的部分安装方式和插件还依赖 Google Cloud Storage、npm 或 GitHub，可由相应规则处理。
- OpenAI 收录 ChatGPT、Codex、API、资源和有依据的第三方主机。Intercom 和 SendGrid 的通用后缀可按所用功能另配。旧表中的 `ai.com` 已转为独立服务。 [ai.com 官方公告](https://ai.com/company-news/ai-com-launch)
- 每份清单仅收录域名。 `DOMAIN` 精确匹配，`DOMAIN-SUFFIX` 覆盖域名本身和各级子域名。一条后缀可覆盖多个端点。IP 直连、用户自定义域名和服务未公开的临时端点需按实际连接处理。

以后直接编辑 `shadowrocket/` 下的对应文件。新发现的共享 CDN 端点优先使用 `DOMAIN,主机名`。确认整个域下的流量都适用于同一策略时使用 `DOMAIN-SUFFIX,域名`，并检查现有后缀是否已覆盖。提交到 `main` 后在 Shadowrocket 刷新规则集。

## 来源

- Google 官方网络资料。 [Gmail](https://knowledge.workspace.google.com/admin/gmail/advanced/gmail-firewall-settings?hl=en) · [Gemini](https://knowledge.workspace.google.com/admin/generative-ai/gemini-app/gemini-app-firewall-settings?hl=en) · [Antigravity 企业网络](https://antigravity.google/docs/enterprise) · [Antigravity 更新端点](https://docs.cloud.google.com/docs/security/compliance/deploy-antigravity-us-public-sector) · [Cloud Run](https://docs.cloud.google.com/run/docs/triggering/https-request) · [App Engine](https://docs.cloud.google.com/appengine/docs/standard/how-requests-are-routed) · [Firebase](https://firebase.google.com/docs/projects/learn-more) · [Artifact Registry](https://docs.cloud.google.com/artifact-registry/docs/docker/names) · [reCAPTCHA](https://docs.cloud.google.com/recaptcha/docs/faq)
- Anthropic 和 AWS 官方网络资料。 [Claude Code](https://code.claude.com/docs/en/network-config#network-access-requirements) · [Claude Desktop](https://code.claude.com/docs/en/desktop#network-access-requirements) · [Claude Platform on AWS](https://platform.claude.com/docs/en/build-with-claude/claude-platform-on-aws) · [AWS 商用区域](https://docs.aws.amazon.com/accounts/latest/reference/manage-acct-regions.html#regional-availability)
- OpenAI 官方资料。 [ChatGPT 和 Codex 网络说明](https://help.openai.com/en/articles/9247338-network-recommendations-for-chatgpt-errors-on-web-and-apps) · [ChatGPT Sites](https://openai.com/policies/chatgpt-sites-terms/) · [Prism](https://openai.com/index/introducing-prism/) · [OpenAI.fm](https://developers.openai.com/showcase/openai-fm)
- v2fly 社区规则。展开 Google 的子集并合并重复规则，保留域名匹配范围。 [Google](https://github.com/v2fly/domain-list-community/blob/98c43cad27fa8f02043858763709fe4a95432ffb/data/google) · [Anthropic](https://github.com/v2fly/domain-list-community/blob/98c43cad27fa8f02043858763709fe4a95432ffb/data/anthropic) · [OpenAI](https://github.com/v2fly/domain-list-community/blob/98c43cad27fa8f02043858763709fe4a95432ffb/data/openai) · [上游 MIT 许可证](LICENSE.v2fly)
- 对照 blackmatrix7 的 Loon 列表、Sukka 的 AI 规则、YUDIDIFEI 的 Shadowrocket AI 列表和本机 Shadowrocket 域名规则。 [blackmatrix7](https://github.com/blackmatrix7/ios_rule_script/tree/0b6897dc17e0c513d25449c935a9694818e32f0c/rule/Loon) · [Sukka](https://github.com/SukkaW/Surge/blob/master/Source/non_ip/ai.conf) · [YUDIDIFEI](https://github.com/YUDIDIFEI/Scripts/blob/HEAD/rule/Shadowrocket/AI/AI.list)
