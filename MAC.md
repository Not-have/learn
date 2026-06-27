# MAC

## 一、常用指令

### 1、隐藏文件

```bash
# 打开
defaults write com.apple.finder AppleShowAllFiles -bool true

# 隐藏
defaults write com.apple.finder AppleShowAllFiles -bool false

# 重启
killall Finder
```

| 方法     | 操作方式           | 优点                                         | 缺点                                                       |
| -------- | ------------------ | -------------------------------------------- | ---------------------------------------------------------- |
| 快捷键   | Cmd + Shift + .    | 非常快捷，临时查看某个目录时特别方便         | 只对当前窗口有效，关闭窗口或切换目录后需要重新按下快捷键   |
| 终端命令 | defaults write ... | 永久生效，一次设置，终身受益（除非你再次修改 | 需要打开终端并输入命令，对不熟悉命令行的用户可能稍显复杂。 |
