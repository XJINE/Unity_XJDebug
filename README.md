# Unity_XJDebug

Provides some debug functions to keep logs, to tag control, and to various output. 

## Import to Your Project

You can import this asset from UnityPackage.

- [XJDebug.unitypackage](https://github.com/XJINE/Unity_XJDebug/blob/master/XJDebug.unitypackage)

## How to Use

### Log Control with Tag

Log and the output are able to control with tag. 

Default tag value is ``null`` and it is enabled.
You can switch the setting with ``XJDebug.NullIsEnableTag`` if you need.

EX:
```csharp
XJDebug.EnableTag("TAG_A");
XJDebug.DisableTag("TAG_B");

XJDebug.Log("MESSAGE_1", "TAG_A");
XJDebug.Log("MESSAGE_2", this, "TAG_B");
XJDebug.LogWarning("MESSAGE_3", this);
```
RESULT:
```
TAG_A: MESSAGE_1
: MESSAGE_3
```

### Output Format

Output format is able to customize with ``XJDebug.LogInfo.StringFormat``.
This format is used as ```String.Format``` when the message will be decoded.

Then, following KEYs are replaced with ``XJDebug.LogInfo`` field's value.

| KEY | FIELD                | DETAIL              |
|:----|:---------------------|:--------------------|
| {0} | logType              | UnityEngine.LogType |
| {1} | timeInSinceLevelLoad | Time.sinceLevelLoad |
| {2} | timeInSystem         | System.DateTime.Now |
| {3} | message              | message is keeped as string and null becomes "NULL". |
| {4} | context              | context is keeped as string and null becomes "NULL". |
| {5} | tag                  | Tag                 |

EX:
```csharp
 XJDebug.LogInfo.StringFormat = "{2} ({1}) {3}";
 XJDebug.Log("MESSAGE");
```

RESULT:
```
: 2018/09/07 2:17:39 (134) MESSAGE
```

### Output ICollection

ICollection message such as Array, List and any others are splitted into each item with ``XJDebug.LogInfo.StringFormatSplitter``.

EX:
```csharp
List<int> intList = new List<int>() { 0, 1, 2 };
XJDebug.Log(intList);
```

RESULT:
```
: 0, 1, 2,
```

### Keep Logs

Logs are keeped in ``XJDebug.Logs`` upto ``XJDebug.MaxLogCount``.
By using ``XJDebug.KeepDisableTagLog``, able to keep log without any output.

EX:
```csharp
// Take null tagged logs with LINQ.
XJDebug.Logs.Where<XJDebug.LogInfo>(log => log.tag == null)
```

``XJDebug.LogInfo`` fields are shown in [**Output Format**](#output-icollection) section.

## Limitation

### Reference of Message and Context

"message" and "context" are keeped as string in ``XJDebug.LogInfo`` because "object message" and "Object context" are may be null when ``XJDebug.LogInfo`` instance are referenced.

### To Keep Consle Click Function

In standard Unity editor, we can jump to the call-point of ``Debug.Log / Debug.unityLogger.Log`` in source code with console click. However if make a wrapper function of these, the call-point is set inside the wrapper.

Unfortunately, there are no way to solve this problem from any code. 
To avoid this problem, we have to make a ".dll".

This is the reason why this repository doesn't include any source code of ``XJDebug``.
All of source code of this utility is shown in https://github.com/XJINE/Unity_XJDebugDll

- Reference
    - https://answers.unity.com/questions/176422/debug-wrapper-class.html
    - https://answers.unity.com/questions/1226230/how-to-properly-call-debuglogstring-in-a-custom-lo.html

### Tag Output

To avoid wrong conflict with other debug system which use tag, ``Debug.unityLogger.Log(tag~)`` must be used.
This problem will may be not considered in future.
