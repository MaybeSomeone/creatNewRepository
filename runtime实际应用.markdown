
Runtime 实际应用

super部分知识补充

//llvm
// oc -> 中间代码 -> 汇编，机器代码


// 数组中无法add nil 数据
//方法交换
[array insertObject: @"Object" atIndex: 0];

1.如何进行方法交换，我们在这件事时必须要意识到要找到对应准确的类才可以进行方法交换，
iOS中的类是类簇，系统会自动选择适合的类调用方法
















































