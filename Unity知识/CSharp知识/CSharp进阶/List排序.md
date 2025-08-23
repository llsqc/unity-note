```c#
using System.Collections.Generic;
using System.Diagnostics.CodeAnalysis;
class Item : IComparable<Item>
{
    public int money;

    public Item(int money)
    {
        this.money = money;
    }

    public int CompareTo(Item other)
    {
        //返回值的含义
        //小于0：放在传入对象的前面
        //等于0:保持当前的位置不变
        //大于0：放在传入对象的后面
        return this.money > other.money ? -1 : 1;
    }
}

class ShopItem
{
    public int id;

    public ShopItem(int id)
    {
        this.id = id;
    }
}

List<Item> itemList = new List<Item>();
itemList.Add(new Item(45));
itemList.Add(new Item(10));

//排序方法，在Item中实现IComparable接口
itemList.Sort();

// 通过委托函数进行排序
List<ShopItem> shopItems = new List<ShopItem>();
shopItems.Add(new ShopItem(2));
shopItems.Add(new ShopItem(1));
shopItems.Add(new ShopItem(4));

//传入委托函数
static int SortShopItem(ShopItem a, ShopItem b)
{
    return a.id > b.id ? 1 : -1;
}
shopItems.Sort(SortShopItem);

//匿名函数
shopItems.Sort(delegate (ShopItem a, ShopItem b)
{
    return a.id > b.id ? 1 : -1;
});

//lambad表达式配合三目运算符
shopItems.Sort((a, b) => { return a.id > b.id ? 1 : -1; });

for (int i = 0; i < shopItems.Count; i++)
{
    Console.WriteLine(shopItems[i].id);
}
//总结
//系统自带的变量(int,float,double.....) 一般都可以直接Sort
//自定义类SOrt有两种方式
//1.继承接口 IComparable
//2.在Sort中传入委托函数
```
