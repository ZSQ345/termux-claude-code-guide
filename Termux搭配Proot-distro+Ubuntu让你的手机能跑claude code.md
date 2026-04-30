## Termux搭配Proot-distro+Ubuntu让你的手机能跑claude code

### 初衷是为了解决使用Termux按照网上教程结果出现 claude native binary not installed错误

![7c3d3224970f4c85dc7b4444f5092ed9](https://cdn.jsdelivr.net/gh/ZSQ345/gopic/markdownPic/7c3d3224970f4c85dc7b4444f5092ed9.jpg)

#### 1.首先更新软件包

```bash
pkg update && pkg upgrade
```

#### 2.安装proot-distro

```bash
pkg install proot-distro
```

#### 3.安装Ubuntu

```bash
proot-distro install ubuntu
```

**进入Ubuntu中**

```bash
proot-distro login ubuntu
```

处于一个Ubuntu标注用户了

![](https://cdn.jsdelivr.net/gh/ZSQ345/gopic/markdownPic/929616f681c6b691c5c20e98b5d0c153.jpg)

#### 4.安装必要工具,这步时间很长,请耐心等待

执行上面的命令进入Ubuntu后再执行

```bash
apt update
apt install nodejs npm nano -y
```

#### 5.安装CC

```bash
npm install -g @anthropic-ai/claude-code
```

执行`claude`查看

![73e5fb75728a0fdcc3cc8a18259424eb](https://cdn.jsdelivr.net/gh/ZSQ345/gopic/markdownPic/73e5fb75728a0fdcc3cc8a18259424eb.jpg)

能够发现安装成功

鉴于Termux的键盘不太好使,按`Ctrl+c`总是退不出来,可大退软件清除后台重新进入,再进入Ubuntu



#### 6.国内中转和配置设置

```bash
pwd #查看当前目录
ls -la #查看文件(夹)
```

![70c839bf34fffe10140701b5e1fc65f8](https://cdn.jsdelivr.net/gh/ZSQ345/gopic/markdownPic/70c839bf34fffe10140701b5e1fc65f8.jpg)

配置中转

```bash
nano ~/.bashrc
```

到文件末尾添加

```bash
export ANTHROPIC_BASE_URL=https://你的中转地址/api/anthropic
export ANTHROPIC_AUTH_TOKEN=你的KEY
```

保存退出：`Ctrl+X` → `Y` → 回车。
生效：

```bash
source ~/.bashrc
```



当前在`/root`下有`.claude`目录,我们进入

```bash
nano settings.jon
```

里面配置中转,可参考电脑端CC switch配置

我这里直接粘贴无误

```bash
{
  "env": {
    "ANTHROPIC_BASE_URL": "api请求网址",
    "ANTHROPIC_AUTH_TOKEN": "你自己的key",
    "ANTHROPIC_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash[1m]",
    "CLAUDE_CODE_SUBAGENT_MODEL": "deepseek-v4-flash[1m]",
    "CLAUDE_CODE_EFFORT_LEVEL": "max",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1,
    "CLAUDE_CODE_DISABLE_NONSTREAMING_FALLBACK": 1,
    "DISABLE_AUTOUPDATER": "1"
  },
  "includeCoAuthoredBy": false,
  "autoUpdatesChannel": "latest"
}
```

`Ctrl+o`保存

`Ctrl+x`退出

同理可自己加`CLAUDE.md`文件,内容自己设置



#### 7.挂载安卓目录,实现CC操作安卓手机

见`termux-ubuntu-setup.md`,或者直接让CC给你干活即可

