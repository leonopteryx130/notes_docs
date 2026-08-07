# trim

在很多编程语言中都有 trim 的概念，作用是**去掉字符串两端的空白字符**（空格、制表符、换行等），返回新字符串，一般不修改原字符串。

常见变体：

| 作用 | JS | Java | Python | Go |
|------|----|------|--------|-----|
| 两端 | `trim` | `trim` / `strip` | `strip` | `TrimSpace` |
| 仅左侧 | `trimStart` | `stripLeading` | `lstrip` | `TrimLeft` / `TrimPrefix` |
| 仅右侧 | `trimEnd` | `stripTrailing` | `rstrip` | `TrimRight` / `TrimSuffix` |

---

### JavaScript

```js
const s = "  hello  \n";
s.trim();      // "hello"
s.trimStart(); // "hello  \n"（别名 trimLeft）
s.trimEnd();   // "  hello"（别名 trimRight）
```

去掉的是 Unicode 空白（含普通空格、`\t`、`\n`、全角空格等）。字符串不可变，原串不变。

---

### Java

```java
String s = "  hello  \n";
s.trim();           // 去掉码点 ≤ U+0020 的字符
s.strip();          // Java 11+，按 Unicode 空白规则去掉两端
s.stripLeading();   // 仅左侧
s.stripTrailing();  // 仅右侧
```

注意：`trim()` 只认 ASCII 空白（≤ 空格），全角空格等去不掉；需要 Unicode 空白时用 `strip()`。

---

### Python

```python
s = "  hello  \n"
s.strip()   # "hello"
s.lstrip()  # "hello  \n"
s.rstrip()  # "  hello"

# 还可指定要去掉的字符集（不是前缀/后缀匹配，而是两端出现在集合中的字符都会去掉）
"...hello...".strip(".")  # "hello"
"xxhelloxx".strip("x")    # "hello"
```

---

### Go

```go
import "strings"

s := "  hello  \n"
strings.TrimSpace(s)           // "hello"，按 unicode.IsSpace 去掉两端空白
strings.Trim(s, " \n")         // 去掉两端属于 cutset 的字符
strings.TrimLeft(s, " ")       // 仅左侧（cutset）
strings.TrimRight(s, "\n")     // 仅右侧（cutset）
strings.TrimPrefix(s, "  ")    // 去掉固定前缀（最多一次）
strings.TrimSuffix(s, "\n")    // 去掉固定后缀（最多一次）
```

`TrimLeft` / `TrimRight` 按字符集裁剪；要精确去掉某个前缀/后缀，用 `TrimPrefix` / `TrimSuffix`。
