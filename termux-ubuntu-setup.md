# Termux + proot-distro Ubuntu 环境搭建指南

> 目标：在 Android Termux 中运行 Ubuntu，并自动挂载手机存储到 `/sdcard`

---

## 一、前置条件

- 已安装 [Termux](https://f-droid.org/packages/com.termux/)
- 已安装 proot-distro：`pkg install proot-distro`
- 已安装 Ubuntu：`proot-distro install ubuntu`

---

## 二、获取存储权限（Termux 中执行）

```bash
termux-setup-storage
```

执行后会弹出 Android 权限请求，点击 **允许**。完成后 Termux 的 `~/storage/` 目录会出现符号链接指向手机各文件夹。

---

## 三、配置一键登录别名（Termux 中执行）

> 以下两个文件均位于 Termux 的 home 目录，即 `/data/data/com.termux/files/home/`

### 1. 创建 `~/.bashrc`

```bash
cat > ~/.bashrc << 'EOF'
# 一键进 Ubuntu，自动挂载手机存储到 /sdcard
alias ubuntu='proot-distro login ubuntu --bind /storage/emulated/0:/sdcard'
EOF
```

### 2. 创建 `~/.bash_profile`

Termux 的 bash 默认以 login shell 启动，只读取 `.bash_profile` 或 `.profile`，不会自动读取 `.bashrc`。因此需要 `.bash_profile` 来桥接：

```bash
cat > ~/.bash_profile << 'EOF'
# Load .bashrc for interactive shells
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi
EOF
```

### 3. 重启 Termux 使配置生效

完全关闭 Termux（从后台划掉），重新打开。输入 `ubuntu` 即可一键进入，无需手动敲 bind 参数。

---

## 四、日常使用流程

```
打开 Termux → 输入 ubuntu → 自动进入 Ubuntu + /sdcard 可见
```

退出 Ubuntu 回到 Termux：`exit`

---

## 五、注意事项

| 事项 | 说明 |
|------|------|
| 别名位置 | `.bashrc` 和 `.bash_profile` 要放在 **Termux home**，不是 Ubuntu 的 `/root` |
| 首次配置需重启 | 修改 profile 文件后必须**完全关闭重启 Termux**，当前 session 不会生效 |
| 临时登录（无别名） | `proot-distro login ubuntu --bind /storage/emulated/0:/sdcard` |
| 挂载路径 | Ubuntu 内访问 `/sdcard` 即对应手机内部存储根目录 |

---

## 六、文件结构一览

```
Termux home (/data/data/com.termux/files/home/)
├── .bashrc            # 别名定义
├── .bash_profile      # login shell → .bashrc
└── storage/           # termux-setup-storage 创建的符号链接
    ├── dcim -> ...
    ├── downloads -> ...
    ├── pictures -> ...
    └── ...

Ubuntu proot
└── /sdcard            # --bind 挂载的手机存储
```

---

*写于 2026-04-30，老公和伊蕾娜共同整理 (｡•̀ᴗ-)✧*
