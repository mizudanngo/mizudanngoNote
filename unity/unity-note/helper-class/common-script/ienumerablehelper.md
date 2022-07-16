---
description: 可迭代对象 助手类
---

# IEnumerableHelper

```csharp
  public delegate bool Predicate<in T>(T obj);                 // 断言
  public delegate bool Comparison<in T>(T x, T y);             // 比较

  /// <summary>
  /// 可迭代对象 的 助手类
  /// </summary>
  public static class IEnumerableHelper
  {
    #region Change Transform
    /// <summary>
    /// 获取 可迭代对象可迭代的的总数
    /// </summary>
    /// <typeparam name="TSource"></typeparam>
    /// <param name="source"></param>
    /// <returns></returns>
    public static int Count<TSource>(this IEnumerable<TSource> source)
    {
      int count = 0;
      IEnumerator<TSource> enumerator = source.GetEnumerator();
      while (enumerator.MoveNext())
      {
        count++;
      }
      return count;
    }

    /// <summary>
    /// 将 可迭代对象 转换为 列表
    /// </summary>
    /// <typeparam name="TSource"></typeparam>
    /// <param name="source"></param>
    /// <returns></returns>
    public static List<TSource> ToList<TSource>(this IEnumerable<TSource> source)
    {
      List<TSource> result = new List<TSource>();
      IEnumerator<TSource> enumerator = source.GetEnumerator();
      
      while(enumerator.MoveNext())
      {
        result.Add(enumerator.Current);
      }
      return result;
    }

    /// <summary>
    /// 将 可迭代对象 转换为 数组
    /// </summary>
    /// <typeparam name="TSource"></typeparam>
    /// <param name="source"></param>
    /// <returns></returns>
    public static TSource[] ToArray<TSource>(this IEnumerable<TSource> source)
    {
      TSource[] result = new TSource[source.Count()];
      IEnumerator<TSource> enumerator = source.GetEnumerator();

      for (int i = 0; enumerator.MoveNext(); i++)
      {
        result[i] = enumerator.Current;
      }
      return result;
    }

    #endregion

    /// <summary>
    /// 通过 condition 搜寻可迭代对象中的 Extremum(极值元素)
    /// <para>回调第一个参数为当前极值，后一个为比较值，返回值为 true 则更换 比较值为新的 极值</para>
    /// </summary>
    /// <typeparam name="T"></typeparam>
    /// <param name="array"></param>
    /// <param name="condition"></param>
    /// <returns></returns>
    public static T FindExtremum<T>(this IEnumerable<T> enumerable, Comparison<T> condition)
    {
      if (enumerable == null)
        throw new ArgumentNullException($"enumerable");
      if (condition == null)
        throw new ArgumentNullException($"condition");

      IEnumerator<T> enumerator = enumerable.GetEnumerator();
      T extremum;
      if (enumerator.MoveNext())
      {
        extremum = enumerator.Current;
        while (enumerator.MoveNext())
        {
          if (condition(extremum, enumerator.Current))
          {
            extremum = enumerator.Current;
          }
        }
        return extremum;
      }
      throw new Exception("the enumerable is empty !");
    }

    #region  array/list/dictionary  Select 
    /// <summary>
    /// 把数组内每一个元素通过selector 改变后，生成一个全新的数组
    /// </summary>
    /// <typeparam name="TSource"></typeparam>
    /// <typeparam name="TResult"></typeparam>
    /// <param name="source"></param>
    /// <param name="selector"></param>
    /// <returns></returns>
    public static TResult[] Select<TSource, TResult>(this TSource[] source, Func<TSource, TResult> selector)
    {
      if (source == null)
        throw new ArgumentNullException("source");
      if (selector == null)
        throw new ArgumentNullException("selector");


      TResult[] result = new TResult[source.Length];
      for (int i = 0; i < source.Length; i++)
        result[i] = selector(source[i]);
      return result;
    }

    /// <summary>
    /// 把列表内每一个元素通过selector 改变后，生成一个全新的列表
    /// </summary>
    /// <typeparam name="TSource"></typeparam>
    /// <typeparam name="TResult"></typeparam>
    /// <param name="source"></param>
    /// <param name="selector"></param>
    /// <returns></returns>
    public static List<TResult> Select<TSource, TResult>(this List<TSource> source, Func<TSource, TResult> selector)
    {
      if (source == null)
        throw new ArgumentNullException("source");
      if (selector == null)
        throw new ArgumentNullException("selector");


      List<TResult> result = new List<TResult>(source.Count);
      for (int i = 0; i < source.Count; i++)
        result.Add(selector(source[i]));
      return result;
    }

    /// <summary>
    /// 把字典内每一个元素通过selector 改变后，生成一个全新的字典
    /// </summary>
    /// <typeparam name="TKey"></typeparam>
    /// <typeparam name="TSource"></typeparam>
    /// <typeparam name="TResult"></typeparam>
    /// <param name="source"></param>
    /// <param name="selector"></param>
    /// <returns></returns>
    public static Dictionary<TKey, TResult> Select<TKey, TSource, TResult>(this Dictionary<TKey, TSource> source, Func<TSource, TResult> selector)
    {
      if (source == null)
        throw new ArgumentNullException("source");
      if (selector == null)
        throw new ArgumentNullException("selector");


      Dictionary<TKey, TResult> result = new Dictionary<TKey, TResult>(source.Count);
      foreach (var item in source.Keys)
        result.Add(item, selector(source[item]));
      return result;
    }
    #endregion

    #region array/list/dictionary  GetValue
    /// <summary>
    /// 随机获取 IList 列表里面的索引值  - - - 有待商议  - - -
    /// </summary>
    /// <typeparam name="T"></typeparam>
    /// <param name="source"></param>
    /// <returns></returns>
    public static int RandomIndex<T>(this IList<T> source)
    {
      if (source.Count == 0) 
        throw new Exception($"The IList is empty");

      return UnityEngine.Random.Range(0, source.Count);
    }

    /// <summary>
    /// 随机获取 IList 列表里 Count 个索引值
    /// </summary>
    /// <typeparam name="T"></typeparam>
    /// <param name="source"></param>
    /// <param name="count"></param>
    /// <returns></returns>
    public static int[] RandomIndexs<T>(this IList<T> source, int count)
    {
      if (source.Count == 0)
        throw new Exception($"The IList is empty");
      if (source.Count <= count)
        throw new Exception("The Count is Out of bounds");              // 唔姆，也有可能是 少于的情况则返回自身

      int[] indexs = new int[count];
      List<int> sourceIndex = new List<int>(source.Count);
      for (int i = 0; i < source.Count; i++) sourceIndex.Add(i);
      
      for (int i = 0; i < count; i++)
      {
        int index = UnityEngine.Random.Range(0, sourceIndex.Count);
        indexs[i] = sourceIndex[index];
        sourceIndex.RemoveAt(index);
      }
      return indexs;
    }

    /// <summary>
    /// 随机获取 Dictionary字典 里一个键
    /// </summary>
    /// <typeparam name="TKey"></typeparam>
    /// <typeparam name="TValue"></typeparam>
    /// <param name="dict"></param>
    /// <returns></returns>
    public static TKey RandomValue<TKey, TValue>(this Dictionary<TKey, TValue> dict)
    {
      if (dict.Count == 0) 
        throw new Exception($"The Dictionary is empty");

      List<TKey> keys = dict.Keys.ToList();
      return keys[UnityEngine.Random.Range(0, keys.Count)];
    }

    /// <summary>
    /// 随机获取 Dictionary字典 里面 Count 个键值
    /// </summary>
    /// <typeparam name="TKey"></typeparam>
    /// <typeparam name="TValue"></typeparam>
    /// <param name="dict"></param>
    /// <param name="count"></param>
    /// <returns></returns>
    public static TKey[] RandomValues<TKey, TValue>(this Dictionary<TKey, TValue> dict, int count)
    {
      if (dict.Count == 0)
        throw new Exception($"The Dictionary is empty");
      if (dict.Count <= count) 
        return dict.Keys.ToArray();

      List<TKey> keys = dict.Keys.ToList();
      TKey[] targetKeys = new TKey[count];
      int index;

      for (int i = 0; i < count; i++)
      {
        index = UnityEngine.Random.Range(0, keys.Count);
        targetKeys[i] = keys[index];
        keys.RemoveAt(index);
      }
      return targetKeys;
    }
    #endregion
  }
```
