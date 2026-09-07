# 影视资源自动化

![GitHub stars](https://img.shields.io/github/stars/ninggui/media-resource-automation)
![License](https://img.shields.io/github/license/ninggui/media-resource-automation)
[![SkillHub](https://img.shields.io/badge/SkillHub-在线安装-blue)](https://skillhub.cn/skills/media-resource-automation)

资源站搜片→提取磁力→网盘离线→在线观看。

## 这是什么

一个可复用的 AI Agent 技能（Skill），来自真实业务场景沉淀，含完整执行流程、避坑清单与验证步骤。

## 快速使用

将本仓库放入 Agent 技能目录后，用对应触发词调用（见 SKILL.md），Agent 会自动加载并执行完整流程。

## 核心能力

| 能力 | 说明 |
|------|------|
| 资源站搜索与磁力提取 |
| 115/qBittorrent 离线缓存 |
| 去重历史库 |
| 网盘链接整理 |

## 使用方式（安装）

- **Hermes**: 放入 `skills/` 目录
- **Claude**: 放入 `~/.claude/skills/`
- **其他 Agent**: 按对应 SKILL.md 格式放入技能目录
- **SkillHub 一键安装**: https://skillhub.cn/skills/media-resource-automation

## 优势

- 主资源站单源策略（防 ban）
- 1080p+中字/国配优先
- 一键复制输出格式

## 内容结构

- `SKILL.md` — 核心技能定义（触发条件、执行流程、避坑清单）
- `references/` — 可选参考文件

## 许可

MIT
