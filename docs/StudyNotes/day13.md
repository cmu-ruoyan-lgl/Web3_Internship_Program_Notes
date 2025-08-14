# Day 13 - 2025年08月14日 

---

## 今日学习目标

参加两会，整理学习内容
策划黑客松事项

## 学习内容

### Library

在 Solidity 中，**库（Library）** 是可复用的代码模块，用于封装常用功能（如数学运算、地址操作等）。它们类似于合约，但有以下核心区别：

#### 核心特性
1. **无状态性**：不能定义存储变量（但可操作调用合约的存储）
2. **无 ETH 管理**：不能接收或持有 ETH（除非特殊设计）
3. **部署节省**：仅部署一次，多个合约可复用
4. **调用方式**：
   - 内部函数：直接嵌入调用合约（无 Gas 开销）
   - 外部函数：通过 `DELEGATECALL` 执行（操作调用合约的存储）

---

#### 实用场景 & 代码示例

#### 1️⃣ 安全数学运算（防溢出）
```solidity
// 安全数学库
library SafeMath {
    // 加法（防溢出）
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "Overflow!");
        return c;
    }

    // 减法（防下溢）
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a, "Underflow!");
        return a - b;
    }
}

// 使用库的合约
contract Token {
    using SafeMath for uint256; // 绑定到 uint256 类型
    uint256 public totalSupply;
    
    function mint(uint256 amount) public {
        totalSupply = totalSupply.add(amount); // 安全加法
    }
}
```

#### 2️⃣ 地址工具（ETH 安全转账）
```solidity
// 地址工具库
library Address {
    // 安全发送 ETH
    function sendETH(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Insufficient balance");
        (bool success, ) = recipient.call{value: amount}("");
        require(success, "Transfer failed");
    }
}

// 使用库的合约
contract Payment {
    using Address for address; // 绑定到 address 类型
    
    function pay(address payable receiver) public payable {
        receiver.sendETH(msg.value); // 安全发送 ETH
    }
}
```

#### 3️⃣ 数据类型扩展（数组操作）
```solidity
// 数组工具库
library ArrayUtils {
    // 查找元素索引
    function findIndex(uint[] storage arr, uint value) internal view returns (int) {
        for (uint i = 0; i < arr.length; i++) {
            if (arr[i] == value) return int(i);
        }
        return -1; // 未找到
    }
}

contract DataManager {
    using ArrayUtils for uint[]; // 绑定到 uint[] 类型
    uint[] public numbers = [10, 20, 30];
    
    function getIndex(uint value) public view returns (int) {
        return numbers.findIndex(value); // 扩展数组功能
    }
}
```

---

#### 关键使用技巧
1. **绑定数据类型**  
   `using LibName for DataType;`  
   使数据类型可直接调用库函数（如 `myNumber.add(5)`）

2. **存储操作权限**  
   若库函数需修改状态，必须声明为 `internal` 且第一个参数使用 `storage` 引用：
   ```solidity
   function updateStorage(uint[] storage arr) internal {
       arr[0] = 100; // 直接修改调用合约的存储
   }
   ```

3. **Gas 优化**  
   - 内部函数：编译时嵌入代码（零调用开销）
   - 外部函数：通过 `DELEGATECALL` 调用（适合复杂逻辑）

---

#### 注意事项
⚠️ **避免循环依赖**：库不能调用合约或其他库的非内联函数  
⚠️ **版本兼容**：使用与合约相同的 Solidity 版本编译  
⚠️ **存储安全**：通过 `DELEGATECALL` 调用的库可修改调用合约的任意存储

通过合理使用库，可显著提升合约的代码复用性、安全性和可维护性。

## solidity中的自定义类型

在 Solidity 中，**自定义类型（User-Defined Value Types）** 是从 Solidity 0.8.8 版本引入的特性，允许开发者基于现有基本类型创建具有语义的新类型，增强代码可读性和类型安全性。以下是对其核心概念的详细说明：

---

### 一、自定义类型的核心作用
1. **类型安全**  
   避免相同底层类型的值被意外混用（如 `USD` 和 `ETH` 同是 `uint256` 但逻辑不同）。
2. **语义化**  
   通过类型名称显式表达数据的含义（如 `type Balance is uint256`）。
3. **零运行时成本**  
   编译后被优化为底层类型，不增加 gas 消耗。

---

### 二、语法定义
```solidity
// 使用 'type' 关键字定义新类型
type NewTypeName is UnderlyingType;

// 示例：定义表示价格的类型
type Price is uint256;
```

---

### 三、使用步骤
#### 1. 类型转换
自定义类型与底层类型需显式转换：
```solidity
// 包装（底层类型 → 自定义类型）
Price price = Price.wrap(100); 

// 解包（自定义类型 → 底层类型）
uint256 rawValue = Price.unwrap(price);
```

#### 2. 运算符重载（可选）
为自定义类型定义运算符（需通过库）：
```solidity
library PriceLib {
    // 加法操作
    function add(Price a, Price b) internal pure returns (Price) {
        return Price.wrap(Price.unwrap(a) + Price.unwrap(b));
    }
}

// 绑定运算符
using { PriceLib.add as + } for Price global;

// 使用
Price total = Price.wrap(200) + Price.wrap(300); // 直接使用 '+'
```

---

### 四、实际应用案例
#### 场景：防止货币单位混淆
```solidity
type USD is uint256;
type ETH is uint256;

contract PaymentProcessor {
    USD public usdBalance;
    ETH public ethBalance;

    // 存款 USD
    function depositUSD(USD amount) external {
        usdBalance = USD.wrap(USD.unwrap(usdBalance) + USD.unwrap(amount));
    }

    // 存款 ETH（类型安全：禁止传入 USD 类型）
    function depositETH(ETH amount) external {
        ethBalance = ETH.wrap(ETH.unwrap(ethBalance) + ETH.unwrap(amount));
    }
}
```
- **类型错误将被编译器捕获**：  
  `depositETH(USD.wrap(100))` 会编译失败，因为参数类型不匹配。

---

### 五、重要注意事项
1. **底层类型限制**  
   只能基于**值类型**（如 `uint`、`address`、`bytes32`）定义，不支持复杂类型。
2. **全局唯一性**  
   同一作用域内类型名不可重复。
3. **ABI 透明性**  
   在合约 ABI 中会显示为底层类型（外部调用时无类型信息）。
4. **运算符需手动定义**  
   默认不支持算术运算（如 `+`、`-`），需通过库扩展。

---

### 六、对比其他方案
| **特性**         | 自定义类型             | 结构体（Struct）       |
|------------------|-----------------------|-----------------------|
| 存储成本         | 同底层类型（0 开销）   | 额外存储空间          |
| 运行时安全性     | 编译时检查             | 需手动验证            |
| 运算符支持       | 需显式重载             | 不支持                |
| 语义表达         | 轻量级                 | 适合复杂数据结构      |

---

### 总结
Solidity 的自定义类型是提升合约安全性和可读性的高效工具，尤其适合需要**区分逻辑含义相同底层类型**的场景（如货币单位、度量单位）。通过编译时类型检查，它能有效预防低级错误，且无需牺牲 gas 效率。

## solidity中的gas优化方案

在 Solidity 中优化 Gas 消耗是智能合约开发的核心技能，以下是经过实践验证的 15+ 省 Gas 技巧，附带详细说明和代码示例：

---

### 一、存储优化（存储操作是最昂贵的）
1. **状态变量打包**
   ```solidity
   // 优化前：3个存储槽
   uint256 a; // 槽0
   uint256 b; // 槽1
   bool c;    // 槽2 (浪费31字节)

   // 优化后：1个存储槽
   uint128 a; // 槽0 (前16字节)
   uint128 b; // 槽0 (后16字节)
   bool c;    // 槽0 (最后1字节)
   ```
   - 原则：将 <32 字节的变量连续声明，自动打包进同一存储槽

2. **使用 `immutable` 和 `constant`**
   ```solidity
   address immutable owner;  // 部署时设置，存储于字节码
   uint256 constant FEE = 500; // 直接编译进字节码
   ```

3. **最小化存储写入**
   ```solidity
   // 避免重复写入相同值
   function update(uint256 newVal) external {
       if (value != newVal) { // 检查是否变化
           value = newVal;
       }
   }
   ```

---

### 二、内存与 Calldata 优化
4. **优先使用 `calldata`**
   ```solidity
   // 比 memory 节省拷贝成本
   function process(uint[] calldata arr) external {
       uint len = arr.length;
       // 直接读取 calldata
   }
   ```

5. **内存变量缓存状态读取**
   ```solidity
   function calculate() external {
       // 高成本：多次 SLOAD
       uint total = value1 + value1 * value2; 
       
       // 优化：1次 SLOAD
       uint v1 = value1;
       uint total = v1 + v1 * value2;
   }
   ```

---

### 三、函数与操作优化
6. **短路模式排序**
   ```solidity
   // 将最可能失败的检查放前面
   require(msg.sender == owner, "Not owner");
   require(balance[msg.sender] >= amount, "Insufficient balance");
   ```

7. **循环优化**
   ```solidity
   for(uint i=0; i<arr.length; i++) { 
       // 高成本：每次循环读取 arr.length
   }

   // 优化：缓存长度
   uint len = arr.length;
   for(uint i=0; i<len; i++) {
       // 只读1次长度
   }
   ```

8. **使用 `external` 替代 `public`**
   ```solidity
   // public：参数拷贝到 memory
   function process(uint[] memory arr) public 
   
   // external：直接使用 calldata
   function process(uint[] calldata arr) external
   ```

---

### 四、数据类型优化
9. **小整数打包**
   ```solidity
   struct User {
       uint64 id;      // 8字节
       uint32 score;   // 4字节
       address wallet;// 20字节
   } // 总32字节 = 1存储槽
   ```

10. **使用 `bytes32` 替代 `string`**
    ```solidity
    bytes32 name; // 固定大小，低Gas操作
    string name;  // 动态类型，操作成本高
    ```

---

### 五、高级优化技巧
11. **汇编打包读写**
    ```solidity
    function readSlot(uint slot) view returns (bytes32 val) {
        assembly {
            val := sload(slot)
        }
    }
    ```

12. **EIP-2929 热地址优化**
    - 对已访问过的存储槽/地址，后续操作 Gas 更低
    - 设计：将频繁访问的数据放在固定存储位置

13. **Gas Refund 机制**
    ```solidity
    // 清空存储槽可获得 Gas 返还
    delete storageSlot; // 返还 15,000 Gas
    ```

---

### 六、部署与架构优化
14. **代理合约模式**
    - 通过代理合约升级逻辑，避免重复部署
    - 使用 OpenZeppelin TransparentProxy

15. **合约大小优化**
    - 使用库合约分离逻辑
    - 避免过长的错误信息
    ```solidity
    require(condition, "Long error message..."); // 消耗更多部署Gas
    require(condition); // 使用默认错误
    ```

---

### 七、Gas 消耗对比表
| 操作                | Gas 成本      | 优化建议               |
|---------------------|--------------|-----------------------|
| SSTORE (新值)       | 20,000-22,100| 减少存储写入次数       |
| SSTORE (修改值)     | 2,900-5,000  | 避免冗余修改           |
| SLOAD               | 2,100        | 用内存变量缓存         |
| CALLDATALOAD        | 3            | 优先用 calldata        |
| 内存数组写入        | 3-10/元素    | 控制数组大小           |
| Keccak256 哈希      | 30-70/字节   | 避免大数据哈希         |

---

### 实战示例：转账优化
```solidity
// 优化前
function transfer(address to, uint amount) public {
    require(balances[msg.sender] >= amount);
    balances[msg.sender] -= amount;
    balances[to] += amount; // 可能触发新存储槽（高成本）
}

// 优化后：使用 OpenZeppelin SafeMath 库
import "@openzeppelin/contracts/utils/math/SafeMath.sol";

function safeTransfer(address to, uint amount) public {
    balances[msg.sender] = balances[msg.sender].sub(amount);
    balances[to] = balances[to].add(amount);
}
```

---

### 最佳实践总结：
1. **黄金法则**：减少存储写入（SSTORE）
2. 使用 Solidity 0.8+ 编译器并启用优化器：`solc --optimize --runs=200`
3. 对热路径代码（高频调用函数）重点优化
4. 使用 Gas 跟踪工具：Hardhat Gas Reporter、Etherscan Gas Tracker
5. 优先使用经过审计的优化库（OpenZeppelin、Solmate）


## 学习笔记


## 问题与思考


## 总结


---
