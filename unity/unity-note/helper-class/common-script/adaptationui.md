---
description: 伪 · 调整一下 UI 大小设配 手机屏幕
---

# AdaptationUI

```csharp
  public static class AdaptationUI
  {
    /// <summary>
    /// 伪 · 调整一下 UI 大小设配 手机屏幕
    /// </summary>
    public static void Adaptive(RectTransform rectUI)
    {
      Vector2 rect;

      if (((float)Screen.width / Screen.height) < .5f)
      {
        rect = rectUI.offsetMin;
        rect.y += 20f;
        rectUI.offsetMin = rect;

        rect = rectUI.offsetMax;
        rect.y -= 59f;                              // 750 * 1334 => 59  | 1334 * 750 => 104
        rectUI.offsetMax = rect;
      }
    }
  }
```
