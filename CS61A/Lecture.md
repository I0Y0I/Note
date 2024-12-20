## Lecture 2
函数定义 Def statement，包括函数名、参数和函数体，函数体中应包含返回值，即便为空，最好也写上返回值；
环境的概念，当执行一个函数时，会新建一个frame，并将传入参数绑定在这个frame中，local frame 和 global frame共同组成了这个函数的运行环境，当调用表达式 expression时，会优先从local frame中查找，其次从 global frame中查找。
## Lecture 3
### 交互模式运行
```bash
python -i xxx.py
```
可以执行一些文件并进入交互模式。
### Doctest
在`def`行下面加入一行`"""`注释，被称为这个函数的文档注释
注释中可以输入一些交互式模式下的expression和输出，作为这个函数的单元测试
```python
def add(a, b):
    """Return sum of a and b.

    >>> o = add(1, 2)
    >>> o
    3
    """
    return a + b

if __name__ == "__main__":
    import doctest
    doctest.testmod()
```
最后一段也可以不加，通过
```python
python -m doctest -v xxx.py
```
运行
### ok
如果想要使用ok测评系统，可以在命令后加`--local`本地运行。
### 参数默认值
```python
def f(a, b=2):
	return b
```
### 条件语句
`elif` is short for `else if`
```python
if x < 0:
	 return -x
elif x > 0:
	return x
else
	return 0
```
False Value：`False`, `0`, `""`, `None`
### 循环语句
```python
while i < 3:
	i = i + 1
```
## Lecture 4
### and or的短路
`a and b` 只有当`a == True`才会执行b
`a or b` 只有当`a == False` 才会执行a
### assert
assert some condition, if false, raise AssertionError with string.
```python
assert 3 > 2, 'Math is broken'
```

### high order函数
函数可以作为传入参数用于计算
函数也可作为一个返回值，可以在一个函数体中定义另一个函数
```python
def make_adder(n):
	def adder(x):
		return x + n
	return adder
```
## Homework: Hog
游戏规则
* 两玩家轮流丢骰子，每回合可以丢0~10任意个骰子，如果不发生以下特殊情况，玩家当局得分为骰子点数和，各局分数和达到100的玩家获胜。
* Sow Sad: 当丢出骰子有一个1，该回合得分为1。
* Boar Brawl: 选择丢0个骰子时，该回合得分为对手分数十位数与自己得分个位数差绝对值的三倍，如果相等，则获得1分
* Sus Fuss: 如果加上本回合获得的点数后，玩家分数正好有3个或4个因数，该玩家的得分加上该回合点数后，额外增大至下一个素数
## Lecture 5
### Environment
函数的def处于什么位置，调用该函数是函数的parent frame就是这个环境，例如
```python
def f():
	return 
```
`f()`的parent frame 为global frame
而
```python
def a():
	def f():
		return
	return f
```
`f()`的parent frame为产生`f`的`a()`frame
### lambda expression
```python
f = lambda x: x * x
g = lambda a, b: c + d
h = lambda: 3
```
f equals a function with formal parameter x that returns the value of "x * x"
lambda函数可以有任意多个参数，也可以没有参数
lambda函数内只能有一个表达式，作为返回值
和 `def`的区别在于def会给生成的函数一个名字，而`lambda`会将生成的函数记为`λ(x) <line x>`
lambda函数依然可以作为返回值，例如
```python
def make_adder(n):
	return lambda x: x + n
```
### Currying
将一个多参数函数转换成一个单参数的高阶函数。
高阶函数指将函数作为参数或返回值为函数的函数。
### range
```
for i in range(start, stop, step)
```
step可以是负的
### 同名函数
Python中禁止出现参数数量不同的同名函数：
```python
def f(a):
	return 1

def f(a, b)
	return 2

f(1)
# 会报错
```
## Lecture 6
### Decorators
```python
@g
def f():
	return 
```
等价于：
```
def f():
	return
f = g(f)
```
示例：
```python
def trace_fn(fn):
	def traced(arg):
		print("calling", fn, "on argument", arg)
		return fn(arg)
	return traced

@trace_fn
def add1(a):
	return a + 1
```
## Lecture 8
### Recursion
1. make up a base case
2. 
### Recursion and Iteration
take fact as an example:
$$
4! = 4\cdot 3\cdot 2\cdot 1
$$
Using Iteration:
$$
n! = \prod_{k=1}^n k
$$
```python
def fact(n):
	total, k = 1, 1
	while k <= n:
		total, k = total*k, k+1
	return total
```
Using Recursion:
$$
n! = \begin{aligned}&1, &if\ n=0\\&n\cdot(n-1)!,\ &otherwise\end{aligned}
$$
```python
def fact(n):
	if n == 0:
		return 1
	else:
		return n * fact(n-1)
```
### Verify Recursion
有点像递推法，首先确认base case是正确的，然后假设fact(n-1)是对的，判断fact(n)是否正确。