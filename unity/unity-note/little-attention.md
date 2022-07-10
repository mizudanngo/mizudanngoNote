---
description: 一些小型注意点 - 技巧类
---

# Little attention

<details>

<summary><mark style="color:blue;">Animator 或者 Shader 设置属性</mark></summary>

将字符串转换为对应数值，否则每次unity调用都会帮你转换一次，如若调用频繁则会造成性能损耗。

```csharp
    int parameterId = Animator.StringToHash("Jump");
    anim.SetTrigger(parameterId);

    int propertyId = Shader.PropertyToID("_MainMap");
    material.SetTexture(propertyId, selectedTexture);

    int propertyId = Shader.PropertyToID("_MainColor");
    shader.SetGlobalColor(propertyId, selectableColor);
```

</details>

<details>

<summary></summary>



</details>







