---
layout: post
title: Ruby 学习笔记
tags:  Ruby  
---


# ruby 语法总结


### 变量作用域 variable scope

变量在不同上下文的环境，有不同的作用域：

- Block: 在 block 里面的变量赋值，是可以访问到外面定义的变量。但是外面的变量赋值是不能访问里面的变量。
- Method: method 里面的 local variable 跟外面的变量是看不到，不能共享，反过来也是。除非考虑这个 method 有改变 variable 的情况。
- Globle varaible, Class variable  , Constant variable

    ? about shadowing :
    ```ruby
    a = 7
    array = [1, 2, 3]
    array.each do |a|
      a += 1
    end
    puts a
    ```


### 对象 object

 Ruby对象是 数据 与 操作该数据的方法的 组合
对象来自于 类 class，创建对象 是通过 `.new` 方法来生成

理解：对象State - ( attribute data)[静]， 和 behaviour (method) [动]

另外本质上，对象储存在内存的一个位置上，通过 object_id 来获取查看。
理解对象object , 和 reference

```ruby
class MyClass
end
my_obj = MyClass.new
```

有哪些对象类型：

| 对象 | 类 |  |
| --- | --- | --- |
| 数值 |  Numeric | Integer, Float |
| 字符串 |  String |  |
|  数组 |  Array |  |
| Hash | Hash |  |
|  正则表达式 |  Regexp |  |
|  文件 |  File | IO： File，Dir |
|  符号 |  Symbol |  |
|  时间 |  Time |  |
|  异常 |  Exception | StandardError (main) |


### 条件判断 conditional statatment

- if ..elsif..else..end

  ```ruby
  puts "Please enter a number between 0 and 100."
  number = gets.chomp.to_i

  if number < 0
    puts "You can't enter a negative number!"
  elsif number <= 50
    puts "#{number} is between 0 and 50"
  elsif number <= 100
    puts "#{number} is between 51 and 100"
  else
    puts "#{number} is above 100"
  end
  ```


- unless .. end  (unless 就是 if not)

  ```ruby
  def reject
    kept_item = []
    self.each do |item|
      unless yield(item)
        kept_item << item
      end
    end
    kept_item
  end
  ```
- case .. when .. else .. end

  ```ruby
  puts "Please enter a number between 0 and 100."
  number = gets.chomp.to_i
  case
  when num < 0
    puts "You can't enter a negative num!"
  when num <= 50
    puts "#{num} is between 0 and 50"
  when num <= 100
    puts "#{num} is between 51 and 100"
  else
    puts "#{num} is above 100"
  end

  # another option

  puts "Please enter a number between 0 and 100."
  number = gets.chomp.to_i
  case num
  when 0..50
    puts "#{num} is between 0 and 50"
  when 51..100
    puts "#{num} is between 51 and 100"
  else
    if num < 0
      puts "You can't enter a negative num!"
    else
      puts "#{num} is above 100"
    end
  end

  ```

###  循环 loop
- while

  ```ruby
  i = 0
  while i < 10 do
    i += 1
  puts "#{i}: ruby. "
  end
  ```

- until

  ```ruby
  i = 0
  until i >= 10
    i += 1
  puts "#{i}: ruby"
  end
  ```
- loop

  ```ruby
  i = 0
  loop do
    i += 1
    puts "#{i}: ruby."

    break if i ==10
  end

  puts "~~~~~~~~~~~~~"
  ```

- for

  ```ruby
  for i in 1..10
    puts "#{i} : ruby"
  end
  ```

- times

  ```ruby
  10.times do |i|
    puts "#{i+1} : ruby. "
  end
  ```
- each

  ```ruby
  (1..10).each {|x| puts "#{x} : ruby" }
  ```

- 循环条件控制 loop condition control

    - break
      ```ruby
        x = 0
      while x <= 10
      if x == 7
        break
      elsif x.odd?
        puts x
      end
      x += 1
      end
      ```
    - next

      ```ruby
      x = 0
      while x <= 10
      if x == 3
        x += 1   # 注意这里，如果没有加1，会一直循环，程序不能进行下一步
        next
      elsif x.odd?
        puts x
      end
      x += 1
      end
      ```

### 数据输入  user input
- ARGV数组 : 命令行接收参数，程序里面的 ARGV[0], ARGV[1] ,  etc, 保存数据作为参数变量. 执行程序 和 参数 一起同时传入
- gets 方法： 等待参数传入，保存作为变量，然后在执行程序，有交互的过程 (gets.chomp 去掉最后换行符)

### 方法 method

方法是对象的相关操作。 把对象的所有操作封装成方法，可以重复调用。

```ruby
def say(words='hello')
  puts words + '.'
end

say()
say("hi")
say("how are you")
say("I'm fine")
```

- return

默认是最后一行 `return` 返回值，也注意需要中途返回值要用到 `return` 。
另外 `puts` 返回值是 `nil`

- 方法参数

  - 默认参数：从右到左
  - 参数位置一一对应， 如果赋值给参数 少，或者 多了，都出现错误提示 `wrong number of arguments`
  - 参数个数不确定，以 `*` 定义，就可以把这些不确定的参数封装成数组 Array
  - 关键字参数 keyword argument , 传入 hash 定义参数，key 作为关键字参数，value 作为关键字参数的值

- 定义带块的方法（ 自己来实现各种 Enumerable)


  ```ruby
  def select
    new_arr = []
    self.each do |element|
      if yield(element)
        new_arr.push(element)
      end
    end
    new_arr
  end

  ```



### 类 class

类是什么？ 类就是对象的种类,对象的类型模板。（种类：依据事物本身的性质、特点划分的门类）

每个对象都是某个类的一个实例，类定义了一个对象需要响应的一组方法。 类可以扩展或子类化其他类，从而继承或重载父类中的方法。 类还可以包含模块，或者从中继承方法。(From Book - Ruby programming language)

- 类定义了一个对象需要响应的一组方法 - 定义类集中两点：状态和行为,也就是属性值和方法（attribute data and methods ）
  - 定义，如何生成实例对象
  - 存取器 访问实例变量，或对实例变量赋值  : attr_reader, attr_writter, attr_accessor （ getter, setter).  
  - 实例变量， 类变量，常量，理解好self

- 限制方法的调用 public, private , protected.
  - `private` methods are not accessible outside of the class definition at all, and are only accessible from inside the class when called without self. ( 举例：Rails 文档的blog例子 Strong parameters)
  - from outside the class, `protected` methods act just like private methods.from inside the class, protected methods are accessible just like public methods.

    ```ruby
    def foo
      puts "method defined in top level context"
    end

    class Example
      def my_method
        foo
      end
    end
    Example.new.my_method # => 输出 "method defined in top level context",
                            # 顶级上下文中定义的方法会成为所有类的私有方法：

    class Example
      def my_method
        foo
      end

      private         
      def foo
      puts "method defined in top level context"
      end      
    end
    Example.new.my_method # => 输出 "method defined in top level context"
    ```

    ```ruby
    class Animal
      def a_public_method
        "Will this work? " + self.a_protected_method
      end

    protected

      def a_protected_method
        "Yes, I'm protected!"
      end
    end

    fido = Animal.new
    fido.a_public_method        # => Will this work? Yes, I'm protected!
    ```
- 类可以扩展或子类化其他类

  ```ruby
  class 类名 < 父类名  
    类定义
  end
  ```
- 类还可以包含模块（include)

  ```ruby
    module M
      def method
        "meth"
      end
    end

    class C
      include M # 包含 M 模块
    end

    c = C.new
    p c.meth  # => meth
  ```

### 模块 module

如果说类表现的是事物的实体（数据）及其行为，那么模块表现的就是事物的行为部分

```ruby
module Swimmable
  def swim
    "I'm swimming!"
  end
end

class Animal; end

class Fish < Animal
  include Swimmable         # mixing in Swimmable module
end
```


模块包含类，实现命名空间功能 - 解决相同名字冲突的问题，用 module 来做一层隔绝

```ruby
module Mammal
  class Dog
    def speak(sound)
      p "#{sound}"
    end
  end

  class Cat
    def say_name(name)
      p "#{name}"
    end
  end
end

buddy = Mammal::Dog.new
kitty = Mammal::Cat.new
buddy.speak('Arf!')           # => "Arf!"
kitty.say_name('kitty')       # => "kitty"
```


直接调用模块的方法。 不需要用 class 来生成对象，再调用方法

```ruby
class Hi
  def hello(n)
     n
  end
end

a = Hi.new
puts a.hello('say hello')




module Greeting
  def self.hello(param)
    param
  end
end

puts Greeting.hello('say hello')

```

? extend  

### 块 block

块 就是 一大块代码块 ，可以看成没有名字的方法，配合关联方法使用。
以do.. end , 或 {} 组织代码 。
主要是循环，常用于配合迭代器方法使用
![each](http://source.shekoufang.com/Screenshot%202016-10-25%2000.26.30.png)


Proc, Lambda , 把块封装成对象： Proc对象， Lambda 对象 ，可以复用
了解两者的差异？
  - Lambda 行为像 method，可以看着匿名函数。本身行为 如 method， 可以直接 return
  - Proc 行为像 block 。 要在方法里面 return


### IO , File , Dir 类
- 分析文件名 parse filename string
- 文件，目录 操作 navigate and examine directories and files
- 文件的读写

### 错误处理与异常 Exception handling
- raise   提示错误信息
- rescue   如何处理错误信息

```ruby
def factorial(n)
  raise TypeError unless n.is_a? Integer
  raise ArgumentError if n < 1
  return 1 if n == 1
  n * factorial(n - 1)
end

begin
  x = factorial(4)
rescue ArgumentError => e
  puts 'Try again with a value >= 1'
rescue TypeError => e
  puts 'Try again with an Integer'
else
  puts x
ensure
  puts 'The process of factorial calculation is completed'
end
```


### Numeric
- Integer
- Float



Big number  : 1_000_000_000_000_000_000

to_s , even? , odd? , times , round , eql?(numeric) , == , equal? , rand(number)

[equal?](http://stackoverflow.com/questions/7156955/whats-the-difference-between-equal-eql-and)


### String

format string : format("%.2f", argument)
https://ruby-doc.org/core-2.2.0/Kernel.html#method-i-format

casecmp , upcase , downcase, capitalize , chomp, reverse , include? index, match , =~ ,  sub , gsub, delete, to_sym, count

[about 'count'](http://stackoverflow.com/questions/5305638/stringcount-options)

### Array

### Hash

### Enumerable

- search and filter | 搜索判断，及过滤
  - any? # 只要有一个条件满足，就返回 true
  - all? # 所有条件满足，返回 true
  - detect (alias - find) # 判断 block 传入所满足的条件，满足的话，返回第一元素。
  - drop # 丢弃前面几个元素，返回余下的元素
  - find_all # 返回所有满足条件的元素
  - find_index # 判断 block 传入所满足的条件，满足的话，返回第一元素 的那个位置。
  - include? # 判断 collection 包含某个元素
  - none? # 所有的元素都不满足条件
  - one?  # 只有一个条件满足， 返回 true，多了，少了都不符合
  - reject  # 满足条件的元素过滤不要，留下没有满足条件的
  - select  # 选择满足条件的元素

- grouping 分组
  - chunk # 依次判断顺序 分组 ，接收两个 block ？
  - group_by # 返回 hash，key 为条件的结果，value 为元素的值
  - partition # 返回两个 array， 第一个 array 为判断真的所有元素，其余的元素放到另外一个 array

- iterate
  - cycle # 遍历多次
  - each # 一次遍历每一个元素
  - each_slice # n 个 一起 遍历
  - each_with_index # 除了 each 每个元素，而且还拿到了元素的位置
  - reverse_each # 相对于 each， 反向 each 元素

- iterate and create new collection 不但遍历，而且还生成新的 collection
  - each_with_object # 接收两个元素，|element, result| 一个是遍历的元素，一个是结果 object
  - flat_map # map 的升级版， 是相对于 nested 嵌套的 array，
  - map # 返回新的 collection
  - inject # 类似于 each_with_object, 多用于计算和， 不过 前面为结果，后面为元素 |sum, element|
  - sort # 排序
  - zip # 对象的每个元素相应跟参数合并，对象的元素少于参数，省略配对参数，多于的话，以 nil 补充

- stats 统计数字
  - count
  - max , :max_by
  - min,  :max_by
  - minmax, :minmax_by


- efficiency 效率
  - lazy  # 当数据很大的时候，没有必要先存入 memery，  lazy 是最后计算结果，load 到 memery ?

    ```ruby
    a = [1,2,3,4,2,5].lazy.map { |x| x * 10 }.select { |x| x > 30 } #=> no evaluation
    a.to_a #=> [40, 50], evaluation performed - no intermediate arrays generated.
    # [ruby lazy method](http://railsware.com/blog/2012/03/13/ruby-2-0-enumerablelazy/)
    ```


### Comparable
    在 class 里面 include Comparable，
    在 method 定义 :
    def <=> other
      self.name <=> other.name
    end
    自己定义的 class 就有 Comparable 功能
