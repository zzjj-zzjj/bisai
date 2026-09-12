# bisai

比赛项目仓库。本仓库已接入 GitHub，用于代码迭代与版本记录。

- 远程仓库：https://github.com/zzjj-zzjj/bisai （公开）
- 默认分支：`main`

## 快速开始

```powershell
git clone https://github.com/zzjj-zzjj/bisai.git
cd bisai
```

## 迭代记录

所有开发都遵循提交规范与迭代日志流程，详见 [DEVELOPMENT.md](DEVELOPMENT.md)。

每次迭代在 `docs/iterations/` 下新增一份记录，格式为 `NNNN-简短标题.md`，
并同步在 `CHANGELOG.md` 中追加一行。

## 目录结构

```
bisai/
├── README.md              # 项目说明（本文件）
├── DEVELOPMENT.md         # 迭代开发规范
├── CHANGELOG.md           # 迭代记录汇总
├── .gitignore             # 忽略规则
├── .editorconfig          # 编辑器统一配置
└── docs/
    ├── iterations/        # 每轮迭代的详细记录
    └── templates/
        └── iteration.md   # 迭代记录模板
```
