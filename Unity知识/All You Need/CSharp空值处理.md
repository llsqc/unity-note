#### 1. ​**​空条件运算符 `?.` 和 `?[]`​**​

- ​**​作用​**​：安全访问对象成员或集合元素（避免 `NullReferenceException`）。
- ​**​行为​**​：
    - 若操作数为 `null`，返回 `null`；
    - 非 `null` 时正常执行操作。
- ​**​示例​**​：
    
    ```c#
    string name = person?.Name;      // 若 person 为 null，返回 null
    int? count = users?.Count;       // 安全访问集合属性
    ```
    

#### 2. ​**​空值断言运算符 `!`​**​

- ​**​作用​**​：显式告知编译器“此处不为 `null`”，跳过 null 检查。
- ​**​风险​**​：若实际为 `null` 会引发 `NullReferenceException`。
- ​**​示例​**​：
    
    ```c#
    string mandatory = GetInput()!;  // 断言返回值非空
    ```
    

#### 3. ​**​空合并运算符 `??`​**​

- ​**​作用​**​：为 `null` 提供默认值，替代 `if-null` 检查。
- ​**​行为​**​：
    - 左操作数非 `null` 时返回该值；
    - 左操作数为 `null` 时返回右操作数。
- ​**​示例​**​：
    
    ```c#
    string display = username ?? "访客";    // 若 username 为 null，返回"访客"
    int age = user?.Age ?? -1;              // 结合 ?. 提供默认值
    ```
    

---

### ​**​联合使用技巧​**​

|场景|示例|效果|
|---|---|---|
|安全访问 + 默认值|`string id = order?.Customer?.Id ?? "N/A"`|防多层 null，提供兜底值|
|断言 + 合并|`Process(item! ?? defaultItem)`|确保非空后提供备选|

---

### ​**​安全准则​**​

1. 优先用 `?.` 防御空引用
2. 用 `??` 替代 `if-null` 逻辑链
3. 慎用 `!` —— 仅在对非空有 100% 把握时使用