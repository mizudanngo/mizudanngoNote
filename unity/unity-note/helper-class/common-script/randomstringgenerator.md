---
description: 随机 生成字符串
---

# 随机字符串生成

```csharp
  /// <summary>
  /// 随机 生成字符串
  /// </summary>
  public static class RandomStringGenerator
  {
    private const string m_allCharacter = "0123456789" +
                                        "abcdefghijklmnopqrstuvwxyz" +
                                        "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    private static int m_allCharacterLen => m_allCharacter.Length;


    /// <summary>
    /// 生成一个字符串
    /// </summary>
    /// <param name="length">生成字符串的长度</param>
    /// <returns>一个生成的字符串</returns>
    public static string Create(int length)
    {
      StringBuilder m_str = new StringBuilder(length);

      for (int i = 0; i < length; i++)
      {
        m_str.Append(m_allCharacter[Random.Range(0, m_allCharacterLen)]);
      }
      return m_str.ToString();
    }
```
