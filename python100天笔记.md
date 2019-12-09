day6 
---------------
函数和模块的使用    
* 定义函数   
+  函数的参数   
	
#### 用模块管理函数    
* 两个相同的函数名字可以通过

```Python
import module1 as m1   
import module2 as m2   
```  
* if __name__ == '__main__':的使用   
* 变量作用域

day 7
---------
Python 3.6以后，格式化字符串还有更为简洁的书写方式，就是在字符串前加上字母f，我们可以使用下面的语法糖来简化上面的代码。   
```
a, b = 5, 10
print(f'{a} * {b} = {a * b}')
```
输出为：   
```
5 * 10 = 50
```

遍历列表
```
print(list1) # [1, 3, 300, 7, 100]
# 通过循环用下标遍历列表元素
for index in range(len(list1)):
    print(list1[index])
# 通过for循环遍历列表元素
for elem in list1:
    print(elem)
```
列表排序
