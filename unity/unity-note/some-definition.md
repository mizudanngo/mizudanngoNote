---
description: 算是一些定义吧~
layout: landing
---

# Some definition

## Unity生命周期函数

{% hint style="info" %}
<mark style="color:green;">Awake</mark>-<mark style="color:yellow;">OnEnable</mark>-<mark style="color:blue;">Start</mark>-<mark style="color:purple;">FixedUpdate</mark>-<mark style="color:blue;">Update</mark>-<mark style="color:purple;">LateUpdate</mark>-OnGUI-<mark style="color:yellow;">OnDisable</mark>-<mark style="color:red;">OnDestroy</mark>
{% endhint %}

Unity 对于脚本的 <mark style="color:red;">禁用/启用</mark> 只是对于变量 enable 的状态改变，受影响的则为生命周期中的函数**（如上）**

<details>

<summary><mark style="color:blue;"><strong>Awake与Start的区别：</strong></mark></summary>

* Awake 在挂载的游戏对象首次激活时候执行，和脚本状态无关
* Start 则在挂载的游戏对象为激活状态且脚本首次激活后在OnEnable函数执行后执行

</details>

<details>

<summary><mark style="color:blue;"><strong>Update与FixedUpdate的区别：</strong></mark></summary>

* Update是每次渲染帧调用一次
* FixedUpdate



</details>

<details>

<summary><mark style="color:blue;">GameObject 相关</mark></summary>

```csharp
public class GameObjectIsNull : MonoBehaviour
{
  private void Awake()
  {
    object o1 = 1, o2 = 1;
    if (o1 == o2) print($"o == 1");                  // × 等于运算 == 对于值类型是计算的值，对于引用类型则是计算的地址
    else print("o1 != o2");                          // √ 判断会进这里(this way)，因为两个对象地址不同

    GameObject go = new GameObject();
    DestroyImmediate(go);

    if (go == null) print($"go是：{go}");             // √ 原因是 GameObject 重载了 == 运算
    if (ReferenceEquals(go, null)) print("RE go");   // × 地址不一样

    object oo = (object)go;
    print($"oo是：{oo}");                             // 一样是 null
    
    if (oo.Equals(null)) print("oo.Equals(null)");   // √ Equals是比较的值
    if (oo is null) print("oo is null");             // × is 是会忽略对象的所有重载，真正意义的去检查这个对象类型的
    if (oo == null) print("oo == null");             // × 由于这里是转换为 System.Object, 所以也屏蔽了子类go写的重载，和 is 有异曲同工之妙
    if (ReferenceEquals(oo, null)) print("RE oo");   // × 该方法直接调用的 == 进行判断返回，引用类型判断的引用地址，不一样

    object oc = null; print(oc == null);             // √
  }
}
```

![](../../.gitbook/assets/Snipaste\_2022-07-10\_13-02-31.png) -- [<mark style="color:blue;">详阅 object.cs</mark>](https://referencesource.microsoft.com/#mscorlib/system/object.cs)<mark style="color:blue;"></mark>



</details>





