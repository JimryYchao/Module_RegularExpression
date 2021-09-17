# Regex 验证表达式应用

---

- [Regex 验证表达式应用](#regex-验证表达式应用)
  - [第1章 数字验证](#第1章-数字验证)
  - [第2章 字符串验证](#第2章-字符串验证)
  - [第3章 常见情景下的校验表达式](#第3章-常见情景下的校验表达式)

---

## 第1章 数字验证

```csharp
/// <summary>
/// Pattern : Number
/// </summary>
public const string NumberPattern = "^[-+]?(0|[1-9]\\d*)(?:\\.\\d+)?$";

/// <summary>
/// Pattern : Integer
/// </summary>
public const string IntegerPattern = "^[-+]?(0|[1-9]\\d*)$";

/// <summary>
/// Pattern : Natural Number
/// </summary>
public const string NaturalNumberPattern = "^(0|[1-9]\\d*)$";

/// <summary>
/// Pattern : Decimal Number
/// </summary>
public const string DecimalPattern = "^[-+]?(0|[1-9]\\d*)\\.\\d+$";

/// <summary>
/// Pattern : Positive Integer
/// </summary>
public const string PositiveIntegerPattern = "^\\+?[1-9]\\d*$";

/// <summary>
/// Pattern : Negeter Integer
/// </summary>
public const string NegativeIntegerPatttern = "^-[1-9]\\d*$";

/// <summary>
/// Pattern : Positive Number
/// </summary>
public const string PositiveNumberPattern = "^\\+?[1-9]\\d*(\\.\\d+)?$";

/// <summary>
/// Pattern : Negative Number
/// </summary>
public const string NegativeNumberPattern = "^-[1-9]\\d*(\\.\\d+)?$";

/// <summary>
/// Pattern : Fraction
/// </summary>
public const string FractionPattern = "^[1-9]\\d*/[1-9]\\d*$";

/// <summary>
/// Pattern : Percentage
/// </summary>
public const string PercentagePattern = "^[-+]?(0|[1-9]\\d*)(?:\\.\\d+)?%$";

//...

```

---
## 第2章 字符串验证

```csharp
/// <summary>
/// Pattern : Chinese Character
/// </summary>
public const string ChineseCharPattern = "[\u4e00-\u9fa5]+";

/// <summary>
/// Pattern : Japanese Character
/// </summary>
public const string JapaneseCharPattern = "[\u0800-\u4e00]";

/// <summary>
/// Pattern : Korean Character
/// </summary>
public const string KoreanCharPattern = "[(\x3130-\x318F|\xAC00-\xD7A3)]";

/// <summary>
/// Pattern : English Character
/// </summary>
public const string EnglishCharPattern = "[A-Za-z]";

/// <summary>
/// Pattern : Contain Number
/// </summary>
public const string ContainNumberPattern = "[0-9]";

/// <summary>
/// Pattern : Contain Number or Letter
/// </summary>
public const string ContainNumLetterPattern = "[A-Za-z0-9]";

//...

```

---
## 第3章 常见情景下的校验表达式

```csharp
/// <summary>
/// Pattern : QQ Format
/// </summary>
public const string QQNumberPattern = "^[1-9]\\d{4,8}$|^[1-3]\\d{9}$";

/// <summary>
/// Pattern : Email Format
/// </summary>
public const string EmailPattern = "^\\w+((-\\w+)|(\\.\\w+))*@[A-Za-z0-9]+((\\.|-)[A-Za-z0-9]+)*\\.[A-Za-z0-9]+$";

/// <summary>
/// Pattern : IPv4
/// </summary>
public const string IPv4Pattern = "^((25[0-5]|2[0-4]\\d|[01]?\\d\\d?)\\.){3}(25[0-5]|2[0-4]\\d|[01]?\\d\\d?)$";

/// <summary>
/// Pattern : IPv6
/// </summary>
public const string IPv6Pattern = "^([\\da-fA-F]{1,4}:){6}((25[0-5]|2[0-4]\\d|[01]?\\d\\d?)\\.){3}(25[0-5]|2[0-4]\\d|[01]?\\d\\d?)$|^::([\\da-fA-F]{1,4}:){0,4}((25[0-5]|2[0-4]\\d|[01]?\\d\\d?)\\.){3}(25[0-5]|2[0-4]\\d|[01]?\\d\\d?)$|^([\\da-fA-F]{1,4}:):([\\da-fA-F]{1,4}:){0,3}((25[0-5]|2[0-4]\\d|[01]?\\d\\d?)\\.){3}(25[0-5]|2[0-4]\\d|[01]?\\d\\d?)$|^([\\da-fA-F]{1,4}:){2}:([\\da-fA-F]{1,4}:){0,2}((25[0-5]|2[0-4]\\d|[01]?\\d\\d?)\\.){3}(25[0-5]|2[0-4]\\d|[01]?\\d\\d?)$|^([\\da-fA-F]{1,4}:){3}:([\\da-fA-F]{1,4}:){0,1}((25[0-5]|2[0-4]\\d|[01]?\\d\\d?)\\.){3}(25[0-5]|2[0-4]\\d|[01]?\\d\\d?)$|^([\\da-fA-F]{1,4}:){4}:((25[0-5]|2[0-4]\\d|[01]?\\d\\d?)\\.){3}(25[0-5]|2[0-4]\\d|[01]?\\d\\d?)$|^([\\da-fA-F]{1,4}:){7}[\\da-fA-F]{1,4}$|^:((:[\\da-fA-F]{1,4}){1,6}|:)$|^[\\da-fA-F]{1,4}:((:[\\da-fA-F]{1,4}){1,5}|:)$|^([\\da-fA-F]{1,4}:){2}((:[\\da-fA-F]{1,4}){1,4}|:)$|^([\\da-fA-F]{1,4}:){3}((:[\\da-fA-F]{1,4}){1,3}|:)$|^([\\da-fA-F]{1,4}:){4}((:[\\da-fA-F]{1,4}){1,2}|:)$|^([\\da-fA-F]{1,4}:){5}:([\\da-fA-F]{1,4})?$|^([\\da-fA-F]{1,4}:){6}:$";

/// <summary>
/// Pattern : Mobile Tel
/// </summary>
public const string MobTelPattern = "^(\\+?86 |\\+?86)?1[3-9]\\d{9}$";

/// <summary>
/// Pattern : Fixed-Line Tel
/// </summary>
public const string FixedLineTelPattern = "^(((0\\d2|0\\d{2})[- ]?)?\\d{8}|((0\\d3|0\\d{3})[- ]?)?\\d{7})(-\\d{3})?$";

/// <summary>
/// Pattern : Longitude
/// </summary>
public const string LongitudePattern = "^[\\-\\+]?(0?\\d{1,2}(\\.\\d{1,5})*|1[0-7]?\\d{1}(\\.\\d{1,5})*|180(\\.0{1,5})*)$";

/// <summary>
/// Pattern : Latitude
/// </summary>
public const string LatitudePattern = "^[\\-\\+]?([0-8]?\\d{1}(\\.\\d{1,5})*|90(\\.0{1,5})*)$";

//...

```

