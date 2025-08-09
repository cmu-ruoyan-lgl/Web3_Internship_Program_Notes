## Cursor

### 终端

在/Users/ruoyan/Library/Application Support/Cursor/User/keybindings.json中修改快捷键
#### 创建一个新的cusor内置终端并聚焦
```json
{
  "key": "ctrl+cmd+n",
  "command": "workbench.action.terminal.new"
}
```

#### 聚焦到cursor内置终端
```json
{
  "key": "alt+w",
  "command": "workbench.action.terminal.focus"
}
```

#### 在cursor内置的终端之间切换
```json
{
  "key": "alt+q",
  "command": "workbench.action.terminal.focusNext"
}
```

#### 关闭当前cursor内置的终端
```json
{
  "key": "alt+r",
  "command": "workbench.action.terminal.kill"
}
```

### AI CHAT

#### 打开ai chat页面，同时引用选定当前内容，如果已经打开则隐藏ai chat页面
```json
{
  "key": "cmd+l",
  "command": "aichat.newchataction"
}
```

### PAGE

#### 在打开的页面（标签）中切换
```json
{
  "key": "ctrl+tab",
  "command": "workbench.action.quickOpenNavigateNextInEditorPicker",
  "when": "inEditorsPicker && inQuickOpen"
}
```

#### 聚焦到当前页面（标签）
```json
{
  "key": "alt+e",
  "command": "workbench.action.focusActiveEditorGroup",
  "when": "terminalFocus || panelFocus || sideBarFocus"
}
```

#### 关闭当前页面（标签）
```json
{
  "key": "alt+x",
  "command": "workbench.action.closeActiveEditor",
  "when": "editorIsOpen"
}
```

#### 打开上一个页面（标签）
```json
{
  "key": "alt+cmd+left",
  "command": "workbench.action.nextEditor"
}
```

#### 打开下一个页面（标签）
```json
{
  "key": "alt+cmd+right",
  "command": "workbench.action.nextEditor"
}
```

workbench.action.focusActiveEditorGroup
workbench.action.focusSideBar
workbench.action.focusPanel

#### 将焦点置于主侧栏
```json
{
  "key": "cmd+0",
  "command": "workbench.action.focusSideBar"
}
```

#### 查看: 聚焦到面板中
```json
{
  "key": "alt+t",
  "command": "workbench.action.focusPanel"
}
```


## foundry

### foge
forge init new_folder
cd new_folder

### cast

### anvil

## react



## next.js 15

