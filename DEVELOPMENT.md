# 迭代开发规范

本仓库用 Git 记录每一次代码迭代。规范的目标是让每轮改动都能被追溯、回滚和复查。

## 1. 提交信息规范

采用 Conventional Commits 格式，中文描述：

```
<类型>(<范围>): <简短描述>

<可选正文：为什么这样改>
```

常用类型：

| 类型 | 用途 |
| --- | --- |
| `feat` | 新功能 |
| `fix` | 修复缺陷 |
| `refactor` | 重构，不改变外部行为 |
| `perf` | 性能优化 |
| `docs` | 文档变更 |
| `test` | 测试相关 |
| `chore` | 构建、依赖、配置等杂项 |
| `revert` | 回滚某次提交 |

示例：

```
feat(解析器): 支持带表头的多段表格

原先遇到第二段表头会串行，改为按空行分段后逐段解析。
```

要点：

- 描述用祈使句，一句话说清"改了什么"，不要写"修改了一些东西"。
- 一次提交只做一件事，不要把无关改动混在一起。
- 涉及具体模块时带上范围，便于日后 `git log --grep` 检索。

## 2. 迭代流程

每轮迭代按以下步骤进行：

1. **开一轮迭代**：在 `docs/iterations/` 复制模板，命名为 `NNNN-简短标题.md`
   （`NNNN` 为四位递增序号，如 `0001-初始化仓库.md`）。
2. **开发并提交**：按上面的规范提交，一个逻辑改动一个 commit。
3. **在 CHANGELOG.md 追加一行**：`- [0001](docs/iterations/0001-初始化仓库.md) 描述`。
4. **推送**：`git push origin main`。
5. **回填结果**：把迭代记录里的"结果与结论"补完，再次提交（`docs: 补全 0001 迭代结论`）。

## 3. 分支策略

- `main` 始终可运行。
- 较大的改动或实验性功能另开分支：`feat/简短说明` 或 `exp/简短说明`，
  完成后合并回 `main`，合并时用 `--no-ff` 保留分支历史：

```powershell
git switch -c feat/new-parser
# ... 开发提交 ...
git switch main
git merge --no-ff feat/new-parser
```

- 放弃的实验分支保留在远程也可以，或在迭代记录里注明后删除。

## 4. 标签与里程碑

阶段性成果打标签，便于回到稳定版本：

```powershell
git tag -a v1.0.0 -m "初赛提交版本"
git push origin v1.0.0
```

## 5. 常用命令

```powershell
git status                     # 看当前改动
git log --oneline --graph -15  # 看最近提交图
git diff                       # 看未暂存改动
git add -A; git commit -m "..."  # 暂存并提交
git push origin main           # 推送
git restore <file>             # 丢弃某文件的未提交改动
git revert <commit>            # 安全回滚某次提交（生成反向提交）
```

## 6. 本机 Git 配置说明

本机已做如下**全局**配置（`.gitconfig`），本仓库不再做任何仓库级覆盖：

| 配置项 | 值 | 原因 |
| --- | --- | --- |
| `user.name` | `zzjj-zzjj` | 原先为空，不配置会导致 `git commit` 直接失败 |
| `user.email` | `zzjj-zzjj@users.noreply.github.com` | 用 GitHub noreply 地址，避免暴露真实邮箱 |
| `https.proxy` | **已删除** | 原先指向未运行的 `127.0.0.1:7890`，导致 git 静默失败 |

关于代理：直连 GitHub 正常，因此全局代理已移除。若日后需要走 Clash，
**不要**设全局代理（Clash 不常驻会让所有仓库静默失败），改为只对需要的仓库设置：

```powershell
git config --local https.proxy http://127.0.0.1:7890   # 仅该仓库生效
git config --local --unset https.proxy                  # 撤销
```

查看当前生效配置及来源：

```powershell
git config --list --show-origin
```

排查网络问题的技巧：加 `GIT_CURL_VERBOSE=1` 看真实 HTTP 状态码。
代理不通时的典型症状是 `git ls-remote` **退出码 0 但没有任何输出**，
极易被误判为"仓库是空的"。
