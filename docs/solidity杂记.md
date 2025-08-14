## Solidity 中，`memory`、`storage`、`calldata` 和 `indexed`详解

在 Solidity 中，`memory`、`storage`、`calldata` 和 `indexed` 是关键概念，分别涉及数据存储位置和事件日志优化。以下是详细解释：

---

### 1. **`memory`**
- **作用**：声明**临时变量**，仅在函数执行期间存在（类似 RAM）。
- **特点**：
  - 数据在函数调用结束后被清除。
  - 用于处理函数内的**引用类型**（如数组、结构体、字符串）。
  - 可读写。
- **Gas 成本**：读写操作相对便宜（但大数组可能昂贵）。
- **示例**：
  ```solidity
  function process(uint[] memory input) public pure returns (uint) {
      uint[] memory temp = new uint[](input.length); // 临时数组
      temp[0] = input[0]; // 可修改
      return temp[0];
  }
  ```

---

### 2. **`storage`**
- **作用**：指向**合约状态变量**的永久存储（类似硬盘）。
- **特点**：
  - 数据存储在区块链上，永久存在。
  - 用于修改合约的状态变量。
  - 可读写。
- **Gas 成本**：读写操作昂贵（尤其是写入）。
- **示例**：
  ```solidity
  contract Example {
      uint[] public data; // 状态变量（默认在 storage）

      function updateStorage() public {
          uint[] storage ref = data; // 指向 storage 的引用
          ref.push(10); // 直接修改状态变量
      }
  }
  ```

---

### 3. **`calldata`**
- **作用**：**只读**的临时数据位置，用于存储**函数参数**。
- **特点**：
  - 仅适用于**外部函数**（`external`）的参数。
  - 数据来自交易调用（`msg.data`），不可修改。
  - 最省 Gas 的选择（避免复制数据）。
- **示例**：
  ```solidity
  function readOnly(uint[] calldata arr) external pure returns (uint) {
      return arr[0]; // 只能读取，不能修改
  }
  ```

---

### 4. **`indexed`**
- **作用**：标记**事件参数**，使其可被索引（用于日志过滤）。
- **特点**：
  - 一个事件最多 3 个 `indexed` 参数。
  - 索引参数存入日志的 `topics`（而非 `data`），便于链下检索。
  - 适合过滤地址、标识符等简单类型（复杂类型会哈希）。
- **示例**：
  ```solidity
  event Transfer(
      address indexed from,    // 可索引（前端可过滤）
      address indexed to,      // 可索引
      uint value               // 非索引（存储在 data 中）
  );

  // 前端过滤（如 ethers.js）：
  contract.on("Transfer", (from, to, value) => { ... });
  contract.queryFilter("Transfer", { from: someAddress }); // 按 from 过滤
  ```

---

### 关键对比
| 特性          | `memory`         | `storage`       | `calldata`      | `indexed`（事件专用） |
|---------------|------------------|-----------------|-----------------|----------------------|
| **生命周期**  | 函数执行期间     | 永久（链上存储）| 函数执行期间    | 事件日志存储         |
| **读写权限**  | 读写             | 读写            | **只读**        | 只读（日志）         |
| **适用场景**  | 函数内部临时变量 | 状态变量        | 外部函数参数    | 事件参数             |
| **Gas 成本**  | 中等             | 高（尤其写入）  | **最低**        | 增加日志 Gas 成本    |
| **修改影响**  | 不影响状态       | 修改合约状态    | 无影响          | 无直接影响           |

---

### 最佳实践
1. **优先用 `calldata`**：  
   外部函数的参数（如数组）使用 `calldata` 节省 Gas。
2. **避免 `storage` 复制**：  
   直接操作状态变量时，用 `storage` 引用减少复制开销。
3. **`indexed` 用于高频过滤字段**：  
   如地址、交易 ID 等需快速检索的参数。

通过合理使用这些关键字，可显著优化合约的 Gas 效率和可维护性。

## 转账eth的完整流程

![eth](./images/eth转账流程.png)