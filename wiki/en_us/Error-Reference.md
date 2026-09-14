<!--
  This page is not a translation of an existing wiki page. It is a new
  reference, so it has no source-page metadata block.
  See docs/translation-guide.md for conventions.
-->

> **English** · no Chinese counterpart yet
>
> | | |
> |---|---|
> | Page type | New reference, not a translation |
> | Generated from | `common/` and `platforms/` source, 2026-09-13 |
> | Last updated | 2026-09-13 |

# Error and log message reference

NekoJS writes its log and exception messages in Chinese. This page lists them
with an English translation, so that an English-speaking user can find out what
a message means.

**How to use this page:** copy the Chinese text out of your log and search for it
here with Ctrl+F. Placeholders such as `{}` are filled in at runtime with the
actual value.

> The proposed code column is not yet printed by NekoJS. It is a suggestion for
> a future change that would prefix each message with a stable code, so that a
> log line can be looked up without copying Chinese text. See the note at the
> end of this page.

---

## Script sync and workspace limits

These come from the in-game workspace sync. They protect the server from a
client sending oversized or too many script files.

| Code | Chinese message | English | What it means |
|---|---|---|---|
| NEKO-1001 | `脚本文件过大: X ( N bytes, 最大 M)` | Script file too large: X (N bytes, maximum M) | One file exceeds the per-file size limit. |
| NEKO-1002 | `脚本数量超过限制: N (最大 M)` | Too many scripts: N (maximum M) | More script files than the limit allows. |
| NEKO-1003 | `脚本总大小超过限制: N bytes (最大 M)` | Total script size over the limit: N bytes (maximum M) | The combined size of all scripts exceeds the limit. |
| NEKO-1004 | `脚本内容超过限制` | Script content over the limit | The content of a sync request exceeds the allowed size. |
| NEKO-1005 | `无法初始化环境目录 [{}]: {}` | Could not create the workspace directory [{}]: {} | NekoJS could not create its working directories. Usually a file permission problem. |
| NEKO-1006 | `扫描脚本目录失败: {}` | Failed to scan the script directory: {} | The script directory could not be read. |
| NEKO-1007 | `跳过无法读取大小的脚本文件，该文件不会同步给客户端: X` | Skipping a script file whose size could not be read; it will not be synced to the client: X | The file is unreadable, so it is left out of the sync. |

---

## Script execution and sandbox limits

These protect the server from a script that never finishes. In every case the
script context is closed and rebuilt on next use, and `/nekojs reload` also
recovers it.

| Code | Chinese message | English | What it means |
|---|---|---|---|
| NEKO-2001 | `脚本求值超时（超过 N 秒，可在 nekojs/config/engine.toml 中调整 scriptEvaluationTimeoutSeconds）：入口脚本的顶层 await 或模块加载可能永不完成` | Script evaluation timed out (over N seconds; adjust `scriptEvaluationTimeoutSeconds` in `nekojs/config/engine.toml`). A top-level await or a module load in the entry script may never complete. | The entry script did not finish evaluating in time. Most often a top-level `await` on a promise that never resolves. |
| NEKO-2002 | `脚本语句累计数达到 scriptStatementLimit（{}），关闭对应脚本环境…` | The script reached `scriptStatementLimit` ({}); its context has been closed. The current evaluation was aborted and the context is rebuilt on next use (`/nekojs reload` also recovers it). | A script executed more statements than allowed, which usually means an infinite loop. |
| NEKO-2003 | `脚本同步执行持续超过 scriptRunawayTimeoutSeconds（{}s）未让出，判定为失控循环…` | A script ran synchronously for more than `scriptRunawayTimeoutSeconds` ({}s) without yielding, and was treated as a runaway loop. Its context has been closed. | A script blocked the thread for too long. |
| NEKO-2004 | `脚本环境 {} 触发 ResourceLimits（失控看门狗 {}s / 语句上限 {}），Graal 已关闭该 Context…` | Script context {} hit the Graal resource limits (runaway watchdog {}s, statement limit {}). Graal has closed the context. | The combined form of the two limits above, reported by the sandbox. |
| NEKO-2005 | `脚本输出行数超过 {} 行上限，后续输出将被丢弃（防止日志无限增长）` | Script output exceeded the limit of {} lines; further output is discarded to stop the log growing without bound. | A script is printing far too much. The rest of its output is dropped. |

---

## Content registration

| Code | Chinese message | English | What it means |
|---|---|---|---|
| NEKO-3001 | `目标类型必须是 LivingEntity: X` | The target type must be a `LivingEntity`: X | An AI goal was given a target that is not a living entity. |
| NEKO-3002 | `无法从 EntityType 推断目标类（NeoForge 不暴露实体类），请传实体 id 字符串或 Java 类: X` | Cannot infer the target class from an `EntityType`, because NeoForge does not expose the entity class. Pass an entity ID string or a Java class instead: X | Pass `'minecraft:zombie'` or `Java.type(...)` rather than the entity type object. |
| NEKO-3003 | `未知目标实体（无内置映射，可用 Java.type(...) 传类）: X` | Unknown target entity; there is no built-in mapping. Use `Java.type(...)` to pass the class: X | The entity ID is not one NekoJS maps automatically. |
| NEKO-3004 | `无法解析目标: X` | Could not resolve the target: X | The value given for an AI goal target could not be interpreted. |
| NEKO-3005 | `未知 capability（支持 item/energy/fluid）: X` | Unknown capability; `item`, `energy`, and `fluid` are supported: X | `CapabilityEvents.register` was given a capability name outside the supported set. |
| NEKO-3006 | `同名绑定 '{}' 已注册，后者被忽略（首胜），被拒绝绑定的 valueType: {}` | A binding named '{}' is already registered; the later one is ignored (first registration wins). The rejected binding's value type was {}. | Two plugins registered the same global binding name. |

---

## Recipes and data

| Code | Chinese message | English | What it means |
|---|---|---|---|
| NEKO-4001 | `RecipeFilterAdapter: unknown key 'X'（未知键），仅接受文档中列出的过滤键，防止条件被静默丢弃` | `RecipeFilterAdapter`: unknown key 'X'. Only the filter keys listed in the documentation are accepted, so that a condition is never silently discarded. | A recipe filter used a key that does not exist, most often a spelling mistake. |
| NEKO-4002 | `无法转换为 JSON: X` | Could not convert to JSON: X | A value passed to a loot table could not be serialised. |
| NEKO-4003 | `JSON 必须是对象，得到: X` | JSON must be an object, but got: X | A loot table was given an array or a primitive where an object was required. |

---

## Startup and client

| Code | Chinese message | English | What it means |
|---|---|---|---|
| NEKO-5001 | `正在为 {} 注册 {} 个事件组...` | Registering {} event groups for {}... | Normal startup progress. Not an error. |
| NEKO-5002 | `[client] 正在加载 CLIENT 脚本...` | `[client]` Loading CLIENT scripts... | Normal startup progress. Not an error. |
| NEKO-5003 | `[client] CLIENT 脚本加载失败` | `[client]` Failed to load CLIENT scripts | A client script failed. Check the error panel with `/nekojs view_all_errors`. |
| NEKO-5004 | `[client] 初始资源刷新失败` | `[client]` Initial resource refresh failed | The first resource reload after startup failed. |
| NEKO-5005 | `{} 脚本 after 依赖排序存在问题：{}` | There is a problem with the `after:` dependency order of the {} scripts: {} | A `// after:` declaration refers to a script that does not exist, or the declarations form a cycle. |
| NEKO-5006 | `无法读取脚本 {}，已跳过：{}` | Could not read script {}; it has been skipped: {} | A script file could not be read and was left out of the load. |
| NEKO-5007 | `jsconfig 合并失败（X）：编辑器可能读不到 probe 生成的类型配置` | Failed to merge `jsconfig` (X). Your editor may not pick up the type configuration that probe generated. | The `jsconfig.json` file could not be updated, so editor completion may not work. |
| NEKO-5008 | `jsconfig include 合并失败（X）：编辑器可能不扫描 probe 输出目录` | Failed to merge the `jsconfig` include list (X). Your editor may not scan the probe output directory. | As above, for the `include` field specifically. |

---

## About the proposed codes

The codes in the first column are **not printed by NekoJS today**. They are a
suggestion.

At present an English-speaking user has to copy Chinese text out of a log and
search for it here. That works, but it is awkward, and it breaks if the wording
of a message is ever changed.

Printing a stable code as a prefix, for example:

```
[NEKO-2002] 脚本语句累计数达到 scriptStatementLimit（50000000），关闭对应脚本环境…
```

would let any reader look the message up without copying Chinese, and would
survive rewording. **The Chinese text would not change**; the code would be
added in front of it. This is the approach used by, for example, TypeScript and
Rust error codes.

A shorter alternative is to add a pointer instead of a code, such as
`（for EN, see wiki -> errors）`, though that makes each log line longer.

Neither change has been made. This page is useful as it stands, and the code
column becomes the lookup key if the maintainer decides the change is worth
making.

## Coverage

This page covers the log and exception messages found in `common/` and
`platforms/`. Messages built up from variables, and the labels used in the
in-game error panel, are not listed. If you meet a Chinese message that is not
here, please open an issue with the text and it will be added.
