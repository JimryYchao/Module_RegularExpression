# .NET Regex 应用
---

- [# .NET Regex 应用](#-net-regex-应用)
- [第1章 .NET 正则表达式对象模型](#第1章-net-正则表达式对象模型)
  - [1.1 正则表达式引擎](#11-正则表达式引擎)
  - [1.2 是否匹配正则表达式模式(IsMatch)](#12-是否匹配正则表达式模式ismatch)
  - [1.3 提取单个匹配项或第一个匹配项(Match)](#13-提取单个匹配项或第一个匹配项match)
  - [1.4 提取所有匹配项(Matches)](#14-提取所有匹配项matches)
  - [1.5 替换匹配的子字符串(Replace)](#15-替换匹配的子字符串replace)
  - [1.6 将单个字符串拆分成一个字符串数组(Split)](#16-将单个字符串拆分成一个字符串数组split)
- [第2章 C# Regex 介绍](#第2章-c-regex-介绍)
  - [2.1 MatchCollection](#21-matchcollection)
  - [2.2 Match](#22-match)
  - [2.3 GroupCollection](#23-groupcollection)
  - [2.4 Group](#24-group)
  - [2.5 CaptureCollection](#25-capturecollection)
  - [2.6 Capture](#26-capture)
- [第3章 Regex 常规案例](#第3章-regex-常规案例)
  - [3.1 扫描 Href 链接](#31-扫描-href-链接)
  - [3.2 更改日期格式](#32-更改日期格式)
  - [3.3 从URL中提取协议和端口号](#33-从url中提取协议和端口号)
  - [3.4 从字符串中剥离无效字符](#34-从字符串中剥离无效字符)
  - [3.5 校验电子邮箱格式](#35-校验电子邮箱格式)
  
---
## 第1章 .NET 正则表达式对象模型

### 1.1 正则表达式引擎

- .NET 中的正则表达式引擎由 Regex 类表示。 正则表达式引擎负责分析和编译正则表达式，并执行用于将正则表达式模式与输入字符串相匹配的操作。

- 可以通过以下两种方式之一使用正则表达式引擎：
      1. 通过调用 Regex 类的静态方法。 方法参数包含输入字符串和正则表达式模式。 正则表达式引擎会缓存静态方法调用中使用的正则表达式，这样一来，重复调用使用同一正则表达式的静态正则表达式方法将提供相对良好的性能。
      2. 通过实例化 Regex 对象，采用的方式是将一个正则表达式传递给类构造函数。 在此情况下，Regex 对象是不可变的（只读），它表示一个与单个正则表达式紧密耦合的正则表达式引擎。 

- 可以调用 Regex 类的方法来执行下列操作：
      1. 确定字符串是否与正则表达式模式匹配。
      2. 提取单个匹配项或第一个匹配项。
      3. 提取所有匹配项。
      4. 替换匹配的子字符串。
      5. 将单个字符串拆分成一个字符串数组。

---
### 1.2 是否匹配正则表达式模式(IsMatch)

> Regex.IsMatch(string input,string pattern)

- 如果字符串与此模式匹配，则 Regex.IsMatch 方法返回 true；如果字符串与此模式不匹配，则该方法返回 false。 IsMatch 方法通常用于验证字符串输入。

```csharp
string[] values = { "111-22-3333", "111-2-3333" };
string pattern = @"^\d{3}-\d{2}-\d{4}$";
foreach (string value in values)
{
    if (Regex.IsMatch(value, pattern))
        Console.WriteLine("{0} is a valid SSN.", value);
    else
        Console.WriteLine("{0}: Invalid", value);
}
// The example displays the following output:
//       111-22-3333 is a valid SSN.
//       111-2-3333: Invalid
```

---

### 1.3 提取单个匹配项或第一个匹配项(Match)

> Regex.Match(string,string) 与 Regex.NextMatch()

- Regex.Match 方法返回一个 Match 对象，该对象包含有关与正则表达式模式匹配的第一个子字符串的信息。 如果 Match.Success 属性返回 true，则表示已找到一个匹配项，
- 可以通过调用 Match.NextMatch 方法来检索有关后续匹配项的信息。 
- 这些方法调用可以继续进行，直到 Match.Success 属性返回 false。

```csharp
string input = "This is a a farm that that raises dairy cattle.";
string pattern = @"\b(\w+)\W+(\1)\b";
Match match = Regex.Match(input, pattern);
while (match.Success)
{
    Console.WriteLine("Duplicate '{0}' found at position {1}.",
                      match.Groups[1].Value, match.Groups[2].Index);
    match = match.NextMatch();
}
// The example displays the following output:
//       Duplicate 'a' found at position 10.
//       Duplicate 'that' found at position 22.
```

---
### 1.4 提取所有匹配项(Matches)

- Regex.Matches 方法返回一个 MatchCollection 对象，该对象包含有关正则表达式引擎在输入字符串中找到的所有匹配项的信息。

```csharp
string input = "This is a a farm that that raises dairy cattle.";
string pattern = @"\b(\w+)\W+(\1)\b";
foreach (Match match in Regex.Matches(input, pattern))
    Console.WriteLine("Duplicate '{0}' found at position {1}.",
                      match.Groups[1].Value, match.Groups[2].Index);

// The example displays the following output:
//       Duplicate 'a' found at position 10.
//       Duplicate 'that' found at position 22.
```

---
### 1.5 替换匹配的子字符串(Replace)

- Regex.Replace 方法会将与正则表达式模式匹配的每个子字符串替换为指定的字符串或正则表达式模式，并返回进行了替换的整个输入字符串。

```csharp
string pattern = @"\b\d+\.\d{2}\b";
string replacement = "$$$&";
string input = "Total Cost: 103.64";
Console.WriteLine(Regex.Replace(input, pattern, replacement));

// The example displays the following output:
//       Total Cost: $103.64
```

---
### 1.6 将单个字符串拆分成一个字符串数组(Split)

- Regex.Split 方法在由正则表达式匹配项定义的位置拆分输入字符串。

```csharp
string input = "Eggs Bread   Milk   Coffee Tea";
foreach(var item in Regex.Split(input, "\\s+"))
    Console.WriteLine(item);

// The example displays the following output:
//       Eggs
//       Bread
//       Milk
//       Coffee
//       Tea
```

---
## 第2章 C# Regex 介绍

### 2.1 MatchCollection

> **Match** 数组, **Regex.Matches** 方法返回一个 **MatchCollection** 对象

- MatchCollection 对象包含多个 Match 对象，这些对象表示正则表达式引擎在输入字符串中找到的所有匹配项（其顺序为这些匹配项在输入字符串中的显示顺序）。通过自身的 item[ ] 索引器属性访问集合中的各个成员。

```csharp
 MatchCollection matches;
 List<string> results = new List<string>();
 List<int> matchposition = new List<int>();

 // 创建一个新的Regex对象并定义正则表达式。  
 Regex r = new Regex("abc");
 // 使用Matches方法查找输入字符串中的所有匹配项。
 matches = r.Matches("123abc4abcd");
 // 遍历集合以检索所有匹配项和位置。
 foreach (Match match in matches)
 {
     // 将匹配字符串添加到字符串数组。
     results.Add(match.Value);
     // 记录找到匹配的字符位置。
     matchposition.Add(match.Index);
 }
 // 列出列表
 for (int ctr = 0; ctr < results.Count; ctr++)
     Console.WriteLine("'{0}' found at position {1}.",
                       results[ctr], matchposition[ctr]);

 // The example displays the following output:
 //       'abc' found at position 3.
 //       'abc' found at position 7.
```

---
### 2.2 Match

1. Match 类表示单个正则表达式匹配项的结果。可以通过检索 Match.Success 属性的值确定是否已找到匹配项。
2. Match.Groups 属性返回一个 GroupCollection 对象，该对象包含有关与正则表达式模式中的捕获组匹配的子字符串的信息。
3. Match.Captures 属性返回一个 CaptureCollection 对象，该对象的使用是有限制的。 不会为其 Success 属性为 false 的 Match 的对象填充集合。 否则，它将包含一个 Capture 对象，该对象具有的信息与 Match 对象具有的信息相同。
4. Match.Value 属性返回输入字符串中与正则表达式模式匹配的子字符串。
5. Match.Index 属性返回输入字符串中匹配的字符串的起始位置（从零开始）。   
6. Match.NextMatch 方法查找位于由当前的 Match 对象表示的匹配项之后的匹配项，并返回表示该匹配项的 Match 对象。
7. Match.Result 方法对匹配的字符串执行指定的替换操作并返回相应结果。

> 示例使用 Match.Result 方法在每个包含两个小数位的数字前预置一个 $ 符号和一个空格。

```csharp
string pattern = @"\b\d+(,\d{3})*\.\d{2}\b";
string input = "16.32\n194.03\n1,903,672.08";

foreach (Match match in Regex.Matches(input, pattern))
    Console.WriteLine(match.Result("$$ $&"));

// The example displays the following output:
//       $ 16.32
//       $ 194.03
//       $ 1,903,672.08
```

---
### 2.3 GroupCollection

1. Match.Groups 属性返回一个 GroupCollection 对象，该对象包含多个 Group 对象，这些对象表示单个匹配项中的捕获的组。 集合中的第一个 Group 对象（位于索引 0 处）表示整个匹配项。 此对象后面的每个对象均表示一个捕获组的结果。
2. 可以使用 Group 属性检索集合中的各个 GroupCollection.Item[] 对象。 可以在集合中按未命名组的序号位置来检索未命名组，也可以按命名组的名称或序号位置来检索命名组。 
3. Regex.GetGroupNumbers
    - 若要确定在特定的正则表达式匹配方法返回的集合中哪些编号的组可用，可以调用实例 Regex.GetGroupNumbers 方法。
4. Regex.GetGroupNames
    - 若要确定集合中哪些命名的组可用，可以调用实例 Regex.GetGroupNames 方法。

```csharp
 string pattern = @"\b(\w+)\s(\d{1,2}),\s(\d{4})\b";
 string input = "Born: July 28, 1989";
 Match match = Regex.Match(input, pattern);
 if (match.Success)
     for (int ctr = 0; ctr < match.Groups.Count; ctr++)
         Console.WriteLine("Group {0}: {1}", ctr, match.Groups[ctr].Value);

 // The example displays the following output:
 //       Group 0: July 28, 1989
 //       Group 1: July
 //       Group 2: 28
 //       Group 3: 1989
```

---
### 2.4 Group

1. Group 类表示来自单个捕获组的结果。
2. Group 类的属性提供有关捕获组的信息：
    - Group.Value 属性包含捕获子字符串，
    - Group.Index 属性在输入文本中指示捕获组的起始位置，
    - Group.Length 属性包含捕获文本的长度，
    - Group.Success 属性指示子字符串是否与捕获组所定义的模式匹配。
3. 如果对组应用 * 或 *? 限定符（将指定零个或多个匹配项），则捕获组在输入字符串中可能没有匹配项。此时 Croup.Success = false, Value = String.Empty, Length = 0 ; 
4. 限定符可以匹配由捕获组定义的模式的多个匹配项。 在此情况下，Value 对象的 Length 和 Group 属性仅包含有关最后捕获的子字符串的信息。

```csharp
string pattern = @"\b((\w+)\s?)+\.";
string input = "This is a sentence. This is another sentence.";
Match match = Regex.Match(input, pattern);
if (match.Success)
{
    Console.WriteLine("Match: " + match.Value);
    Console.WriteLine("Group 2: " + match.Groups[2].Value);
}

// The example displays the following output:
//       Match: This is a sentence.
//       Group 2: sentence
```

---
### 2.5 CaptureCollection

1. Group 对象仅包含有关最后一个捕获的信息。 但仍可从 CaptureCollection 属性返回的 Group.Captures 对象中获取由捕获组生成的整个捕获集。 集合中的每个成员均为一个表示由该捕获组生成的捕获的 Capture 对象，这些对象按被捕获的顺序排列: 
   - 通过使用构造循环访问集合，如 foreach 构造（在 C# 中）
   - 通过使用 CaptureCollection.Item[] 属性按索引检索特定对象。

```csharp
string pattern = "((a(b))c)+";
string input = "abcabcabc";

Match match = Regex.Match(input, pattern);
if (match.Success)
{
    Console.WriteLine("Match: '{0}' at position {1}",
                      match.Value, match.Index);
    GroupCollection groups = match.Groups;
    for (int ctr = 0; ctr < groups.Count; ctr++)
    {
        Console.WriteLine("   Group {0}: '{1}' at position {2}",
                          ctr, groups[ctr].Value, groups[ctr].Index);
        CaptureCollection captures = groups[ctr].Captures;
        for (int ctr2 = 0; ctr2 < captures.Count; ctr2++)
        {
            Console.WriteLine("      Capture {0}: '{1}' at position {2}",
                              ctr2, captures[ctr2].Value, captures[ctr2].Index);
        }
    }
}
// The example displays the following output:
//       Match: 'abcabcabc' at position 0
//          Group 0: 'abcabcabc' at position 0
//             Capture 0: 'abcabcabc' at position 0
//          Group 1: 'abc' at position 6
//             Capture 0: 'abc' at position 0
//             Capture 1: 'abc' at position 3
//             Capture 2: 'abc' at position 6
//          Group 2: 'ab' at position 6
//             Capture 0: 'ab' at position 0
//             Capture 1: 'ab' at position 3
//             Capture 2: 'ab' at position 6
//          Group 3: 'b' at position 7
//             Capture 0: 'b' at position 1
//             Capture 1: 'b' at position 4
//             Capture 2: 'b' at position 7
```

---
### 2.6 Capture

1.Capture 类包含来自单个子表达式捕获的结果。 Capture.Value 属性包含匹配的文本，而 Capture.Index 属性指示匹配的子字符串在输入字符串中的起始位置（从零开始）。

```csharp
string input = "Miami,78;Chicago,62;New York,67;San Francisco,59;Seattle,58;";
string pattern = @"((\w+(\s\w+)*),(\d+);)+";
Match match = Regex.Match(input, pattern);
if (match.Success)
{
    Console.WriteLine("Current temperatures:");
    for (int ctr = 0; ctr < match.Groups[2].Captures.Count; ctr++)
        Console.WriteLine("{0,-20} {1,3}", match.Groups[2].Captures[ctr].Value,
                          match.Groups[4].Captures[ctr].Value);
}

// The example displays the following output:
//       Current temperatures:
//       Miami                 78
//       Chicago               62
//       New York              67
//       San Francisco         59
//       Seattle               58
```

---
## 第3章 Regex 常规案例

### 3.1 扫描 Href 链接

- 可以通过用户代码多次调用 GetHRefs 方法，所以它使用 static Regex.Match(String, String, RegexOptions) 方法。 这样一来，正则表达式引擎不仅可以缓存正则表达式，还杜绝了每次调用方法时实例化新 Regex 对象产生的开销。 随后使用 Match 对象循环访问字符串中的所有匹配。

```csharp
 public static List<string> GetHRefs(string inputString)
 {
     MatchCollection matches;
     List<string> hrefs = new List<string>();
     string HRefPattern = @"href\s*=\s*(?:[""'](?<1>[^""']*)[""']|(?<1>\S+))";

     try
     {
         matches = Regex.Matches(inputString, HRefPattern,
                         RegexOptions.IgnoreCase | RegexOptions.Compiled,
                         TimeSpan.FromSeconds(1));
         for (int i = 0; i < matches.Count; i++)
         {
             if (matches[i].Success)
             {
                 Console.WriteLine("Found href " + matches[i].Groups[1] + " at "
                     + matches[i].Groups[1].Index);
                 hrefs.Add(matches[i].Groups[1].Value);
             }
         }
     }
     catch (RegexMatchTimeoutException)
     {
         Console.WriteLine("The matching operation timed out.");
     }
     return hrefs;
 }
```

> Main

```csharp
public static void Main()
{
    string inputString = "My favorite web sites include:</P>" +
                         "<A HREF=\"http://msdn2.microsoft.com\">" +
                         "MSDN Home Page</A></P>" +
                         "<A HREF=\"http://www.microsoft.com\">" +
                         "Microsoft Corporation Home Page</A></P>" +
                         "<A HREF=\"http://blogs.msdn.com/bclteam\">" +
                         ".NET Base Class Library blog</A></P>";

    foreach (var item in GetHRefs(inputString))
        Console.WriteLine(item);
}

// The example displays the following output:
//       Found href http://msdn2.microsoft.com at 43
//       Found href http://www.microsoft.com at 102
//       Found href http://blogs.msdn.com/bclteam at 176
//          http://msdn2.microsoft.com
//          http://www.microsoft.com
//          http://blogs.msdn.com/bclteam
```

---
### 3.2 更改日期格式

- 示例使用 Regex.Replace 方法，将格式为 mm /dd /yy 的日期替换为格式为 dd -mm -yy 的日期

```csharp
public static void Main()
{
    string dateString = DateTime.Today.ToString("d",
                                      DateTimeFormatInfo.InvariantInfo);
    string resultString = MDYToDMY(dateString);
    Console.WriteLine("Converted {0} to {1}.", dateString, resultString);
}
// The example displays the following output to the console if run on 8/21/2007:
//      Converted 08/21/2007 to 21-08-2007.

static string MDYToDMY(string input)
{
    try
    {
        return Regex.Replace(input,
               @"\b(?<month>\d{1,2})/(?<day>\d{1,2})/(?<year>\d{2,4})\b",
              "${day}-${month}-${year}", RegexOptions.None,
              TimeSpan.FromMilliseconds(150));
    }
    catch (RegexMatchTimeoutException)
    {
        return input;
    }
}
```

---
### 3.3 从URL中提取协议和端口号

- 示例从 URL 中提取协议和端口号。使用 Match.Result 方法返回协议, 后面依次跟的是冒号和端口号.

```csharp
 string url = "http://www.contoso.com:8080/letters/readme.html";

 Regex r = new Regex(@"^(?<proto>\w+)://[^/]+?(?<port>:\d+)?/",
                     RegexOptions.None, TimeSpan.FromMilliseconds(150));
 Match m = r.Match(url);
 if (m.Success)
     Console.WriteLine(m.Result("${proto}${port}"));

 // The example displays the following output:
 //       http:8080
```

---
### 3.4 从字符串中剥离无效字符

> 示例使用静态 Regex.Replace 方法，从字符串中剥离无效字符。
  
- 使用此示例中定义的 CleanInput 方法来剥离在接受用户输入的文本字段中输入的可能有害的字符。 在此情况下，CleanInput 会剥离所有非字母数字字符（句点 (.)、at 符号 (@) 和连字符 (-) 除外），并返回剩余字符串。 但是，可以修改正则表达式模式，使其剥离不应包含在输入字符串内的所有字符。

```csharp
 static string CleanInput(string strIn)
 {
     // Replace invalid characters with empty strings.
     try
     {
         return Regex.Replace(strIn, @"[^\w\.@-]", "",
                              RegexOptions.None, TimeSpan.FromSeconds(1.5));
     }
     // If we timeout when replacing invalid characters,
     // we should return Empty.
     catch (RegexMatchTimeoutException)
     {
         return String.Empty;
     }
 }

 static void Main(string[] args)
 {
     string str = Example.CleanInput("asdwert*&^*&5saduw)$##asdw");
     Console.WriteLine(str);
 }
 //asdwert5saduwasdw
```

---
### 3.5 校验电子邮箱格式

> 下面的示例使用正则表达式来验证一个字符串是否为有效的电子邮件格式。

- 此正则表达式相对比实际上可用作电子邮件的表达式简单。 使用正则表达式来验证电子邮件，这对于确保电子邮件的结构正确是很有用的，但这不是验证电子邮件是否实际存在的替代方法。验证电子邮件的最佳方法是将测试电子邮件发送到该地址。
    - ✔️ 请使用小型正则表达式来检查电子邮件的有效结构。

    - ✔️ 请将测试电子邮件发送到应用用户提供的地址。

    - ❌ 请勿将正则表达式用作验证电子邮件的唯一方法。

```csharp
 public static bool IsValidEmail(string email)
 {
     if (string.IsNullOrWhiteSpace(email))
         return false;
     try
     {
         // 规范化域名 @mail.server.name
         email = Regex.Replace(email, @"(@)(.+)$", DomainMapper,
                               RegexOptions.None,
                               TimeSpan.FromMilliseconds(200));

         // 检查电子邮件的域部分，并将其规范化。  
         string DomainMapper(Match match)
         {
             // 使用IdnMapping类转换Unicode域名。
             var idn = new IdnMapping();

             // 取出并处理域名(对无效抛出ArgumentException)
             string domainName = idn.GetAscii(match.Groups[2].Value);

             return match.Groups[1].Value + domainName;
         }
     }
     catch (RegexMatchTimeoutException e)
     {
         return false;
     }
     catch (ArgumentException e)
     {
         return false;
     }

     try
     {
         return Regex.IsMatch(email,
             @"^[^@\s]+@[^@\s]+\.[^@\s]+$",
             RegexOptions.IgnoreCase,
             TimeSpan.FromMilliseconds(250));
     }
     catch (RegexMatchTimeoutException)
     {
         return false;
     }
     }
 static void Main(string[] args)
 {
     Console.WriteLine(RegexUtilities.IsValidEmail("user@mail.server.name"));
 } //True
```

---

