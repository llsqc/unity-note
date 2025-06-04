### 基本规则

```json
{
  // JSON 规则1: 整体由大括号 {} 包裹，表示一个对象
  "gameConfig": {
    // JSON 规则2: 键(key)必须用双引号包裹，冒号分隔键值
    "gameName": "冒险之旅", // 值(value)可以是字符串（双引号包裹）
    "version": 1.2,       // 值可以是数字（无需引号）
    "isOnline": true,      // 值可以是布尔值（true/false）
    "maxPlayers": 4,       // 值可以是整数

    // JSON 规则3: 数组用方括号 [] 表示，元素用逗号分隔
    "playableCharacters": [
      {
        "id": "warrior",
        "hp": 150,
        "skills": ["劈砍", "格挡", "冲锋"]
      },
      {
        "id": "mage",
        "hp": 90,
        "skills": ["火球", "冰环", "传送"]
      }
    ],

    // JSON 规则4: 嵌套对象结构
    "difficultySettings": {
      "easy": { "enemyDamage": 0.5, "lootRate": 2.0 },
      "normal": { "enemyDamage": 1.0, "lootRate": 1.0 },
      "hard": { "enemyDamage": 2.0, "lootRate": 0.5 }
    },

    // JSON 规则5: 特殊值 null 表示空值
    "secretLevel": null
  }
}
```