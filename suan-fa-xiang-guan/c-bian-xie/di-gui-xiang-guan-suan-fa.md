# 递归相关算法

<details>

<summary><mark style="color:blue;">关于求 斐波那契数列</mark> </summary>

```csharp
static void Main()
{
  Console.WriteLine(GetNum(6));               // output: 8  求指定项
  Console.WriteLine(GetTotalSum(6));          // output: 20 求前N项和
}

/// <summary>
/// 递归获取斐波那契数列 指定轮次 的值 （非常不建议）
/// </summary>
static int GetNum(int num)
{
  if (num <= 0)
  {
    return 0;
  }

  if (num <= 2)
  {
    return 1;
  }
  return GetNum(num - 1) + GetNum(num - 2);
}

/// <summary>
/// 正常获取斐波那契数列 前 N 项和 （建议写法）
/// </summary>
static int GetTotalSum(int num)
{
  if (num <= 0)
  {
    return 0;
  }
  if (num <= 2)
  {
    return num;
  }

  int num1 = 1, num2 = 1, sum = 2;

  for (int i = 3; i <= num; i++)
  {
    num2 = num1 + num2;
    num1 = num2 - num1;
    sum += num2;
  }
  return sum;
}
```

</details>

<details>

<summary><mark style="color:blue;"><strong>关于回溯算法求组合</strong></mark></summary>

```csharp
static void Main()
{ 
    Stack<int> stack = new Stack<int>();
    Combination(9, 6, 1, stack);
}
    
/// <summary>
/// 回溯算法求 数字 组合
/// </summary>
static void Combination(int len, int targetCount, int start, Stack<int> stack)
{
    if (stack.Count == targetCount)
        // 打印结果
        return;

    //  i <= len 也是可以的，为了优化
    // 即                  数字总数 -（目标组合数  - 已有组合数） + 1
    // 若求 9个数中的6位组合，已有组合为 0 的情况： 9 - （6 - 0） + 1 = 4，为什么要加一是因为遍历的第一位或者说最后一位也是包含在计数里面的
    // 最多只需要从 第 4 个数开始搜索就行了，因为如果从 5 开始搜，则 9-5+1=5，剩下的数字只有 5个了，总目标组合数是 6 个
    // 若求 9个数中的6位组合，已有组合为 1 的情况： 9 - （6 - 1） + 1 = 5， 
    // 若求 9个数中的6位组合，已有组合为 2 的情况： 9 - （6 - 2） + 1 = 6， 
    // 若求 9个数中的6位组合，已有组合为 3 的情况： 9 - （6 - 3） + 1 = 7， 
    // 若求 9个数中的6位组合，已有组合为 4 的情况： 9 - （6 - 4） + 1 = 8， 
    // 若求 9个数中的6位组合，已有组合为 5 的情况： 9 - （6 - 5） + 1 = 9，
    // 最多要从 第 9 位开始，因为只差 一位了，所以必须遍历到尾部
    for (int i = start; i <= len - (targetCount - stack.Count) + 1; i++)
    {
        stack.Push(i);
        // 因为组合要求不重复且顺序不重要，所以要 从当前 开始位置 + 1 往 后面 搜寻
        Combination(len, targetCount, i + 1, stack);
        stack.Pop();
            
    }                
}
```

</details>

<details>

<summary><mark style="color:blue;">关于回溯算法求排列</mark></summary>

```csharp
static void Main()
{
  Stack<int> stack = new Stack<int>();
  
  Permutation(6, 1, stack);
  Permutation(6, 2, stack);
  Permutation(6, 3, stack);
}

/// <summary>
/// 求 一组数字的全排列
/// </summary>
/// <param name="len">数字总长度</param>
/// <param name="PermuteCount">指定排列的位数</param>
/// <param name="stack"></param>
static void Permutation(int len, int PermuteCount, Stack<int> stack)
{
  if (stack.Count == PermuteCount)
  {
    PrintStack_(stack);
    return;
  }

  // 全排列需要考虑顺序, 即 12 和 21 是两种排列，所以 i 只能从第一个数开始，范围也是 全范围
  for (int i = 1; i <= len; i++)
  {
    if (stack.Contains(i))    // TODO: ...
    {
      continue;
    }

    stack.Push(i);
    Permutation(len, PermuteCount, stack);
    stack.Pop();
  }
}
```

</details>









