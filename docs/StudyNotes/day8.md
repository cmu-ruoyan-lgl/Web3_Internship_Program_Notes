# Day 8 - 2025年08月09日 

---

## 今日学习目标

- [ ] 深度研究web3前端开发主流框架
- [ ] 学习前端知识
- [ ] 整理相关框架笔记

## 明日学习目标 
- [ ] 黑客松组队设计
- [ ] 设计NFT交易市场项目： ERC721，IPFS，contract， wagmi， viem@2.x， react， tailwind css， gasp 实现加入到市场的nft能够滚动显示， 要求实现NFT定价代理出售， 撤回代理， 购买市场的NFT等功能

## 学习内容

[Build an Ethereum DApp with Next.js, Wagmi, RainbowKit, and Pinata to Mint NFTs on IPFS](https://www.youtube.com/watch?v=2BWdIrfJ3FI&t=60s)


## 学习笔记


---

### 🧩 **一、开源项目与模板（可快速复用）**  
1. **The Stripes NFT 铸造应用**  
   - **特点**：基于 React.js 的高度可配置铸造平台，支持自定义主题色、背景图和多链部署（如 Polygon）。  
   - **技术栈**：React.js + 智能合约集成，通过修改 `config.json` 即可对接自定义合约。  
   - **适用场景**：个人艺术家发行 NFT 或游戏道具确权平台，适合展示合约交互能力。  

2. **The-Weirdos-NFT-Website-Starter-Code**  
   - **特点**：响应式 NFT 集合展示页，含动态效果（打字机动画、彩带特效）和移动端适配。  
   - **技术栈**：React + styled-components + GSAP 动画库，Create React App 脚手架快速启动。  
   - **亮点**：社区活跃，被提名 GitHub 顶级 NFT 开源项目，提供详细视频教程。  

3. **Open Source NFT Marketplace（社区驱动型）**  
   - **特点**：开源 NFT 市场模板，支持 AR 预览 3D 作品，专注跨平台兼容性。  
   - **技术栈**：ReactJS + 以太坊钱包集成，未来计划扩展智能合约后端。  
   - **创新点**：增强现实技术提升用户体验，适合沉浸式艺术展示场景。  

---

### 💼 **二、商业模板（设计成熟，可直接购买）**  
1. **Xhibiter**  
   - **版本**：提供 **HTML 模板**（Tailwind CSS + Webpack）和 **React NextJS 模板**（v13.2）。  
   - **优势**：  
     - 极速加载（500ms 内），Google 评分 97，支持暗黑模式。  
     - 含 13 个主页变体、实时拍卖倒计时、Metamask 集成。  
   - **适用**：高并发交易市场，适合求职 DeFi/NFT 公司时展示性能优化能力。  

2. **Netstorm**  
   - **特点**：多页式 React 模板（17+ 内页），专为数字藏品竞拍设计，含现场拍卖过滤器和作者面板。  
   - **技术栈**：ReactJS + Bootstrap，支持 REST API 调用。  
   - **亮点**：CSS3 动画流畅，内置 1500+ 图标库，适合复杂业务场景前端架构参考。  

3. **Niopy**  
   - **特点**：Vue.js 构建的 15 合 1 模板，含用户面板和明暗模式切换，侧重创作者仪表盘设计。  
   - **技术栈**：Vue JS + Bootstrap 5 + SASS。  
   - **适用**：艺术家个人品牌或收藏品管理平台，突出用户中心功能。  

4. **NFTPO**  
   - **特点**：轻量级登陆页模板，专为 NFT 项目预热设计，含半透明导航栏与渐变按钮。  
   - **技术栈**：NextJS + TailwindCSS，优化 SEO 与图片加载速度。  
   - **优势**：适合快速搭建 MVP，强调第一视觉冲击力。  

---

### ✨ **三、设计亮点与趋势参考**  
- **视觉动效**：  
  - **GSAP 动画**（如 The-Weirdos 的滚动特效）提升用户参与感。  
  - **微交互设计**：NFTPO 的渐变按钮、Netstorm 的 CSS3 悬停动画。  
- **AR 与 3D 集成**：开源 NFT 市场的 AR 预览功能，适合结合 three.js 实现虚拟画廊。  
- **暗黑模式**：Xhibiter 和 Niopy 的双模式切换，适配加密用户偏好。  
- **模块化布局**：Xhibiter 的组件库（图表、下拉菜单）可复用性高。  

---

### ⚙️ **四、技术栈参考（2025 主流选择）**  
| **需求**         | **推荐技术**                     | **案例参考**              |  
|------------------|----------------------------------|---------------------------|  
| 多链兼容         | Wagmi + Viem                     | Xhibiter 多链资产展示     |  
| 动画与交互       | GSAP + Framer Motion             | The-Weirdos 动态效果      |  
| 样式管理         | Tailwind CSS + styled-components | NFTPO 渐变设计            |  
| 性能优化         | NextJS 服务端渲染                | Xhibiter React 版         |  
| 移动端优先       | 响应式框架 + 触控优化            | Niopy 跨设备适配          |  

---

### 💡 **实用建议**  
1. **优先学习开源项目**：克隆 [The-Weirdos 代码库](https://github.com/codebucks27/The-Weirdos-NFT-Website-Starter-Code) 修改组件，理解状态管理与动画实现。  
2. **商用模板二次开发**：购买 Xhibiter React 版（约 $60），在其模块化基础上扩展拍卖逻辑或 DAO 治理功能。  
3. **融合创新点**：  
   - 在铸造平台中增加 **AR 预览**（参考开源 NFT 市场）。  
   - 为交易市场添加 **Gas 优化提示**（类似 Netstorm 的实时过滤器）。  

根据目标公司调整设计重点：应聘艺术类平台侧重 3D/AR 能力；金融类 NFT 项目突出交易流程与数据可视化。


## 问题与思考


## 总结


---