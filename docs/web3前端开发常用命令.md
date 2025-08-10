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


### forge

**典型工作流**：
```bash
forge init my_project     # 初始化项目
forge build               # 编译合约
forge test                # 运行测试（支持模糊测试）
forge test --mt ${函数名称} -vvvvv  # 测试指定测试合约中过的函数
forge script Deploy       # 部署脚本执行
forge fmt                 # 代码格式化
forge install OpenZeppelin/openzeppelin-contracts
forge selectors find "balanceOf(address)"  # 0x70a08231
```

### cast

**常用场景**：
```bash
cast send <合约> "函数" 参数    # 发送交易
cast call <合约> "视图函数"      # 查询状态
cast block latest              # 获取最新区块
cast --to-base 0x1 hex         # 数据格式转换
cast sig "transfer(address)"   # 计算函数选择器
```

### anvil

```bash
anvil    # 本地虚拟网络
```

### make

生成如下makefile
```bash
include .env

deploy_ruoyancoin:
    @echo "deploying contract..."
    forge create src/ruoyancoin.sol:ruoyancoin --private-key ${OWNER_PRIVAYE_KEY} --broadcast --constructor-args ${OWNER_ADDRESS}

mint:
	@echo "Minting tokens..."
	cast send ${CONTRCT_ADDRESS} "mint(uint256)" ${MINT_AMOUNT} --private-key ${OWNER_PRIVATE_KEY}

```


## react



## next.js 15

## wagmi (建议用这个构建web3前端,底层是ether.js)

## rainbowkit

