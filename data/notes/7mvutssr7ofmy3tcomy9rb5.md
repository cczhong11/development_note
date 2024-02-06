
# Swift字符串插值和操作

## 字符串插值

在Swift中，字符串插值是一种构建新字符串的方式，可以将常量、变量、字面量和表达式的值包含在长字符串中。格式是将值放在`\()`中。

```swift
let name = "小明"
let greeting = "你好, \(name)!"
print(greeting) // 输出: 你好, 小明!
```

## 常用的字符串操作

### 1. 字符串连接

可以使用`+`运算符来连接两个字符串。

```swift
let firstPart = "Swift"
let secondPart = "UI"
let completeString = firstPart + secondPart
print(completeString) // 输出: SwiftUI
```

### 2. 多行字符串

使用三个双引号(`"""`)来创建多行字符串。

```swift
let multiLineString = """
这是一个
多行
字符串
"""
print(multiLineString)
```

### 3. 字符串长度

使用`.count`属性来获取字符串的长度。

```swift
let message = "Hello, Swift!"
print("字符串长度为 \(message.count)")
```

### 4. 访问字符串中的字符

可以使用下标语法来访问字符串中的特定字符。

```swift
let word = "Hello"
let firstLetter = word[word.startIndex]
print(firstLetter) // 输出: H
```

### 5. 字符串的遍历

使用`for-in`循环遍历字符串中的每个字符。

```swift
for char in "Swift" {
    print(char)
}
```

### 6. 字符串的修改

字符串可以通过各种方法进行修改，例如添加字符、替换子字符串等。

```swift
var str = "I like Swift"
str += " programming"
print(str) // 输出: I like Swift programming
```
