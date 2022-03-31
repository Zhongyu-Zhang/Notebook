### 1. Python风格规范
本部分内容参考
[https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/#id1](unittest库单元测试教程)

#### **行长度**

每行不超过80个字符，以下三种情况例外：
  - 长的导入模块语句
  - 注释里的URL,路径以及其他的一些长标记
  - 不便于换行，不包含空格的模块级字符串常量，比如url或者路径

除非是在 with 语句需要三个以上的上下文管理器的情况下，否则不要使用反斜杠连接行。Python会将圆括号, 中括号和花括号中的行隐式的连接起来 , 你可以利用这个特点。如果需要, 你可以在表达式外围增加一对额外的圆括号。
  ```python
  Yes: foo_bar(self, width, height, color='black', design=None, x='foo',
             emphasis=None, highlight=0)

       if (width == 0 and height == 0 and
           color == 'red' and emphasis == 'strong'):

  Yes:  with very_long_first_expression_function() as spam, \
             very_long_second_expression_function() as beans, \
             third_thing() as eggs:
            place_order(eggs, beans, spam, beans)

  No:  with VeryLongFirstExpressionFunction() as spam, \
            VeryLongSecondExpressionFunction() as beans:
           PlaceOrder(eggs, beans, spam, beans)

  Yes:  with very_long_first_expression_function() as spam:
            with very_long_second_expression_function() as beans:
                place_order(beans, spam)
  ```

#### **括号**

宁缺毋滥的使用括号，除非是用于实现行连接, 否则不要在返回语句或条件语句中使用括号。不过在元组两边使用括号是可以的。

```python
Yes: if foo:
         bar()
       while x:
           x = bar()
       if x and y:
           bar()
       if not x:
           bar()
       # For a 1 item tuple the ()s are more visually obvious than the comma.
       onesie = (foo,)
       return foo
       return spam, beans
       return (spam, beans)
       for (x, y) in dict.items(): ...
No:  if (x):
         bar()
     if not(x):
         bar()
     return (foo)
```

#### **缩进**

用4个空格来缩进代码，绝对不要用tab, 也不要tab和空格混用。对于行连接的情况, 你应该要么垂直对齐换行的元素, 或者使用4空格的悬挂式缩进(这时第一行不应该有参数)。

#### **空行**

顶级定义之间空两行, 比如函数或者类定义。方法定义, 类定义与第一个方法之间, 都应该空一行。函数或方法中, 某些地方要是你觉得合适, 就空一行。

#### **空格**

  - 括号内不要有空格。
    ```python
    Yes: spam(ham[1], {eggs: 2}, [])
    No:  spam( ham[ 1 ], { eggs: 2 }, [ ] )
    ```
  - 不要在逗号, 分号, 冒号前面加空格, 但应该在它们后面加(除了在行尾)。
  
  - 在二元操作符两边都加上一个空格, 比如赋值(=), 比较(==, <, >, !=, <>, <=, >=, in, not in, is, is not), 布尔(and, or, not)。至于算术操作符两边的空格该如何使用, 需要你自己好好判断. 不过两侧务必要保持一致。
    
  - 当 = 用于指示关键字参数或默认参数值时, 不要在其两侧使用空格。但若存在类型注释的时候,需要在 = 周围使用空格。
    ```python
    Yes: def complex(real, imag=0.0): return magic(r=real, i=imag)
    Yes: def complex(real, imag: float = 0.0): return Magic(r=real, i=imag)
    
    No:  def complex(real, imag = 0.0): return magic(r = real, i = imag)
    No:  def complex(real, imag: float=0.0): return Magic(r = real, i = imag)
    ```

#### **Shebang**

大部分.py文件不必以#!作为文件的开始，程序的main文件应该以 `#!/usr/bin/python2` 或者 `#!/usr/bin/python3` 开始。其中，`#!` 先用于帮助内核找到Python解释器, 但是在导入模块时, 将会被忽略。因此只有被直接执行的文件中才有必要加入 `#!` 。

#### **注释**

  - 函数和方法
  
    一个函数必须要有文档字符串, 除非它满足以下条件:
    1. 外部不可见
    2. 非常短小
    3. 简单明了
    
    文档字符串应该包含函数做什么, 以及输入和输出的详细描述。通常, 不应该描述”怎么做”, 除非是一些复杂的算法。文档字符串应该提供足够的信息, 当别人编写代码调用该函数时, 他不需要看一行代码, 只要看文档字符串就可以了。对于复杂的代码, 在代码旁边加注释会比使用文档字符串更有意义。关于函数的几个方面应该在特定的小节中进行描述记录， 这几个方面如下文所述。每节应该以一个标题行开始。标题行以冒号结尾。除标题行外, 节的其他内容应被缩进2个空格。
    
    **Args:** 列出每个参数的名字, 并在名字后使用一个冒号和一个空格, 分隔对该参数的描述。如果描述太长超过了单行80字符，使用2或者4个空格的悬挂缩进(与文件其他部分保持一致)。描述应该包括所需的类型和含义。
    
    **Returns:** (或者 Yields: 用于生成器)描述返回值的类型和语义。如果函数返回None, 这一部分可以省略。
    
    **Raises:** 列出与接口有关的所有异常。
    
    ```python
    def fetch_smalltable_rows(table_handle: smalltable.Table,
                            keys: Sequence[Union[bytes, str]],
                            require_all_keys: bool = False,
    ) -> Mapping[bytes, Tuple[str]]:
        """Fetches rows from a Smalltable.
    
        Retrieves rows pertaining to the given keys from the Table instance
        represented by table_handle.  String keys will be UTF-8 encoded.
    
        Args:
            table_handle: An open smalltable.Table instance.
            keys: A sequence of strings representing the key of each table
            row to fetch.  String keys will be UTF-8 encoded.
            require_all_keys: Optional; If require_all_keys is True only
            rows with values set for all keys will be returned.
    
        Returns:
            A dict mapping keys to the corresponding table row data
            fetched. Each row is represented as a tuple of strings. For
            example:
    
            {b'Serak': ('Rigel VII', 'Preparer'),
            b'Zim': ('Irk', 'Invader'),
            b'Lrrr': ('Omicron Persei 8', 'Emperor')}
    
            Returned keys are always bytes.  If a key from the keys argument is
            missing from the dictionary, then that row was not found in the
            table (and require_all_keys must have been False).
    
        Raises:
            IOError: An error occurred accessing the smalltable.
        """
    ```
    
  - 类
    
    类应该在其定义下有一个用于描述该类的文档字符串。如果你的类有公共属性(Attributes), 那么文档中应该有一个属性(Attributes)段。并且应该遵守和函数参数相同的格式。
    
    ```python
    class SampleClass(object):
        """Summary of class here.
    
        Longer class information....
        Longer class information....
    
        Attributes:
            likes_spam: A boolean indicating if we like SPAM or not.
            eggs: An integer count of the eggs we have laid.
        """
    
        def __init__(self, likes_spam=False):
            """Inits SampleClass with blah."""
            self.likes_spam = likes_spam
            self.eggs = 0
    
        def public_method(self):
            """Performs operation blah."""
    ```
    
  - 块注释和行注释
  
    最需要写注释的是代码中那些技巧性的部分。如果你在下次 代码审查 的时候必须解释一下, 那么你应该现在就给它写注释。对于复杂的操作, 应该在其操作开始前写上若干行注释。 对于不是一目了然的代码, 应在其行尾添加注释。为了提高可读性, 注释应该至少离开代码2个空格。
    
    ```python
    # We use a weighted dictionary search to find out where i is in
    # the array.  We extrapolate position based on the largest num
    # in the array and the array size and then do binary search to
    # get the exact number.
    
    if i & (i-1) == 0:        # True if i is 0 or a power of 2.
    ```
    
    另一方面, 绝不要描述代码。假设阅读代码的人比你更懂Python, 他只是不知道你的代码要做什么。
    ```python
    # BAD COMMENT: Now go through the b array and make sure whenever i occurs
    # the next element is i+1
    ```

#### **命名规范**

  - **类**：总是使用首字母大写单词串。如MyClass。内部类可以使用额外的前导下划线。
  
  - **函数与方法**：小写+下划线
  
    *注意*：混合大小写仅被允许用于这种风格已经占据优势的时候，以便保持向后兼容。
  
  - **函数和方法的参数**：如果一个函数的参数名称和保留的关键字冲突，通常使用一个后缀下划线。
  
  - **变量**：小写，由下划线连接各个单词。如`color = WHITE，this_is_a_variable = 1`
    
      1. 不论是类成员变量还是全局变量，均不使用 m 或 g 前缀。
      
      2. 私有类成员使用单一下划线前缀标识。
      
      3. 变量名不应带有类型信息，因为Python是动态类型语言。如`iValue、names_list、dict_obj`等都是不好的命名。
  
  - **常量**：常量名所有字母大写，由下划线连接各个单词如`MAX_OVERFLOW、TOTAL`。
  
  - **文件名**：全小写,可使用下划线
  
  - **包**：应该是简短的、小写的名字。如果下划线可以改善可读性可以加入，如`mypackage`。
  
  - **缩写**：命名应当尽量使用全拼写的单词，缩写的情况有如下两种：
  
    1. 常用的缩写，如XML、ID等，在命名时也应只大写首字母，如XmlParser。
    
    2. 命名中含有长单词，对某个单词进行缩写。这时应使用约定成俗的缩写方式。例如：function 缩写为 fn、text 缩写为 txt、object 缩写为 obj、count 缩写为 cnt、number 缩写为 num等。

### 2. 函数封装时的注意事项
  - 在封装的函数中应避免出项命令行方式或图形界面形式的交互，即不应使用以下Python库，如`matplotlib.pyplot`、`tqdm`。
    
  - 在封装的函数中不应出现主函数`if __name__ == '__main__':`的定义。
    
  - 应为所有封装函数撰写测试代码，测试代码中应尽量自动化，避免人机交互的验证方式，即不应使用以下Python库`print()`、`plt.imshow()`等。可以调用unittests库实现的`assertXXX`系列方法或者`numpy.testing`库下面的`assert`系列方法，参考教程
    [https://blog.csdn.net/qq_27825451/article/details/85774167](unittest库单元测试教程)
    
  - 在函数具有多个返回值时建议使用字典的方式`dic_out = {'result1': return1, 'result2': return2}`存储。

  

