# 方法

### 方法重写

Java equals/hashcode约定指出，如果我们有两个对象，根据它们的equals()方法判断是相等的，它们也必须有相同的hashCode()结果。

hashCode()实现步骤：

1. 创建一个result变量并给它分配一个素数。
2. 利用equals()方法使用的每个字段，并计算一个int值来表示字段的hashcode。
3. 通过将前面的结果乘以一个素数，将字段计算来的hashcode与现有结果组合在一起