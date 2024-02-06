

# Swift中的数组操作

## 初始化数组

### 1. 空数组

```swift
var emptyArray = [Int]()
```

### 2. 具有默认值的数组

```swift
var defaultValuesArray = Array(repeating: 0, count: 5) // [0, 0, 0, 0, 0]
```

### 3. 字面量初始化

```swift
var literalArray: [String] = ["Apple", "Banana", "Cherry"]
```

## 增加元素

### 1. 使用`append`

```swift
var fruits = ["Apple"]
fruits.append("Banana") // ["Apple", "Banana"]
```

### 2. 使用`+=`运算符

```swift
fruits += ["Cherry"] // ["Apple", "Banana", "Cherry"]
```

## 删除元素

### 1. 使用`remove`

```swift
fruits.remove(at: 0) // 移除第一个元素，剩下["Banana", "Cherry"]
```

### 2. 移除最后一个元素

```swift
fruits.removeLast() // 移除最后一个元素，剩下["Banana"]
```

## 修改元素

直接通过索引修改数组中的元素。

```swift
fruits[0] = "Strawberry" // ["Strawberry"]
```

## 遍历数组

使用`for-in`循环遍历数组。

```swift
for fruit in fruits {
    print(fruit)
}
```

## 总结

数组是Swift中常用的数据结构，用于存储同类型的多个值。理解如何有效地初始化、添加、删除和修改数组中的元素对于任何Swift开发者来说都是基本且重要的技能。
