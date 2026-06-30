
## 最终方案：Windows 本地开发 + Debian Git 服务器

### 架构

```
Debian (10.0.0.144)
  └── /home/wedo/vc/mathlive.git  ← Git bare 远程仓库（纯存储，无工作目录）
        ↑ git push / git clone (SSH)
        ↓
Windows 本地 d:\VC\debian\mathlive  ← 唯一工作目录
  ├── Trae IDE 编辑代码
  ├── Git Bash 运行 npm install / npm start
  └── 浏览器预览 http://localhost:9029/dist/smoke/
```

### 前置条件

- Debian 已开启 SSH 服务（已确认运行中）
- Debian 用户 `wedo`，IP `10.0.0.144`
- Windows 已安装 **Git for Windows**（自带 Git Bash）
- Windows 已安装 **Node.js**（>= 21.0.0）

### 第一步：Debian 上准备 bare 远程仓库

有两种方式，任选其一：

#### 方式 A：从零创建 bare 仓库（推荐，仓库尚未使用）

```bash
# Debian 上执行
cd /home/wedo/vc
git init --bare mathlive.git
```

然后从 Windows 推送初始代码：

```bash
# Windows Git Bash 上执行
cd /d/VC/debian/mathlive
git remote add origin wedo@10.0.0.144:/home/wedo/vc/mathlive.git
git push -u origin master
```

#### 方式 B：将已有普通仓库转换为 bare 仓库（仓库已有历史）

如果 Debian 上 `/home/wedo/vc/mathlive` 已经是普通 Git 仓库，且 Windows 已经 clone 了它，可以将其转换为 bare 仓库。有两种子方式：

**方式 B1：原地转换（保留 remote 配置）**

直接在原仓库目录修改 Git 配置，原地转换为 bare 仓库。这种方式会保留原有的 remote（上游）配置。

```bash
# Debian 上执行
cd /home/wedo/vc/mathlive

# 1. 如果有未推送的本地修改，先推送到另一个分支或备份

# 2. 原地转换为 bare 仓库
git config --bool core.bare true
```

转换后，仓库路径不变（仍然是 `/home/wedo/vc/mathlive`），Windows 端无需修改 remote URL。

**方式 B2：克隆为 bare 仓库（更干净）**

创建一个新的 bare 仓库目录，只包含 Git 对象，不保留工作目录。

```bash
# Debian 上执行
cd /home/wedo/vc

# 1. 备份原仓库
mv mathlive mathlive.old

# 2. 用 --bare 重新克隆，只保留 Git 对象
git clone --bare mathlive.old mathlive.git

# 3. 确认 bare 仓库内容
ls mathlive.git  # 应看到 HEAD, config, objects, refs 等

# 4. 删除旧仓库
rm -rf mathlive.old
```

然后更新 Windows 本地仓库的远程地址：

```bash
# Windows Git Bash 上执行
cd /d/VC/debian/mathlive
git remote set-url origin wedo@10.0.0.144:/home/wedo/vc/mathlive.git
```

> bare 仓库没有工作目录（working tree），只存储 Git 对象，专门用作远程仓库。它仍然支持 `git remote`、`git fetch`、`git push` 等所有 Git 功能。使用 bare 仓库可以直接 `git push`，不会出现 `denyCurrentBranch` 错误。

### 第二步：Windows 克隆代码

打开 **Git Bash**，执行：

```bash
cd /d/VC/debian
git clone wedo@10.0.0.144:/home/wedo/vc/mathlive.git
cd mathlive
```

> 首次连接会提示输入 `wedo` 的 Debian 登录密码。

### 第三步：设置 npm 国内镜像

```bash
npm config set registry https://registry.npmmirror.com
```

### 第四步：安装依赖并启动

```bash
npm install
npm start
```

启动后 esbuild dev server 会运行在 `http://localhost:9029/`。

### 第五步：浏览器预览

打开 `http://localhost:9029/dist/smoke/`

### 日常开发流程

```bash
# 1. 在 Trae IDE 中打开 d:\VC\debian\mathlive 编辑代码

# 2. 在 Git Bash 中启动 dev server（会自动 watch 文件变化）
cd /d/VC/debian/mathlive
npm start

# 3. 浏览器刷新查看效果

# 4. 改完了推回 Debian 备份
git add .
git commit -m "改动说明"
git push
```

### 如果 Debian 上已有未推送的修改

先在 Debian 上推送一次，确保远程仓库是最新的：

```bash
# Debian 上执行
cd /home/wedo/vc/mathlive.old  # 或原仓库目录
git push origin master
```

然后再在 Windows 上 clone。

### 要点总结

| 项目 | 说明 |
|------|------|
| **工作目录** | Windows 本地硬盘，性能好 |
| **Debian 角色** | bare 仓库，仅做 Git 远程仓库，不直接改代码 |
| **npm 操作** | Windows 本地硬盘，无 Samba 兼容问题 |
| **代码同步** | `git push` / `git pull` 通过 SSH 走局域网 |
| **预览** | Windows 浏览器访问 localhost |
| **bare 仓库优势** | 允许直接 `git push`，不会出现 `denyCurrentBranch` 错误 |
