# FirstClassObject(일급객체)

Python's function is `FirstClassObject`(first citizen)

- 함수내에서 함수정의가 가능(함수를 인자로 받음)
- 모듈화 시키는게 가능!

```python
def square(x):
    return x * x

def cube(x):
    return x * x * x

def quad(x):
    return x * x * x * x

def my_map(func, arg_list):
    result = []
    for i in arg_list:
        result.append(func(i)) # square 함수 호출, func == square
    return result

num_list = [1, 2, 3, 4, 5]

squares = my_map(square, num_list)
cubes = my_map(cube, num_list)
quads = my_map(quad, num_list)

print squares # [1, 4, 9, 16, 25]
print cubes   # [1, 8, 27, 64, 125]
print quads   # [1, 16, 81, 256, 625]
```

## Closure
### 클로저란 퍼스트클래스 함수를 지원하는 언어의 네임 바인딩 기술이다
A closure—unlike a plain function—allows the function to access those captured variables through the closure's copies of their values or references, even when the function is invoked outside their scope.

- Practical Example
```python
def simple_html_tag(tag, msg):
    print '<{0}>{1}<{0}>'.format(tag, msg)
    
simple_html_tag('h1', '심플 헤딩 타이틀')

print '-'*30

# 함수를 리턴하는 함수
def html_tag(tag):
    
    def wrap_text(msg):
        print '<{0}>{1}<{0}>'.format(tag, msg)
        
    return wrap_text

print_h1 = html_tag('h1') 
print print_h1 
print_h1('첫 번째 헤딩 타이틀') 
print_h1('두 번째 헤딩 타이틀') 

print_p = html_tag('p')
print_p('이것은 패러그래프 입니다.')
```

```python
<function html_tag.<locals>.wrap_text at 0x0000017DEB3F4BF8>
<h1>첫 번째 헤딩 타이틀<h1>
<h1>두 번째 헤딩 타이틀<h1>
<p>이것은 패러그래프 입니다.<p>
```

**프리변수**   
tag는 wrap_text안에서 정의되지 않았지만 클로저로 인해 레퍼런스에 맵핑되는 프리변수 이다.

```python
def outer_func(msg):
    def inner_func():
        print('hi',msg)
    return inner_func 

my_func = outer_func('putshong')
my_func()
dir(my_func)
my_func.__closure__
print(my_func.__closure__[0].cell_contents)
```
```python
['__annotations__', '__call__', '__class__', '__closure__','__code__','__defaults__','__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__','__ge__','__get__', '__getattribute__', '__globals__', '__gt__', '__hash__', '__init__', '__kwdefaults__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__','__qualname__','__reduce__','__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__','__subclasshook__']

(<cell at 0x0000017DEB3FB078: str object at 0x0000017DEB41FA08>,)

sean
```
**클로저는 튜플**   
자동으로 객체가 가지게 되는(상속받는) method 에 포함되어있다!  
일급객체이기 때문에!

## Decorator

```python
def decorator_function(original_function):
    def wrapper_function(*args, **kwargs): 
        print '{} 함수가 호출되기전 입니다.'.format(original_function.__name__)
        return original_function(*args, **kwargs) 
    return wrapper_function


@decorator_function
def display():
    print 'display 함수가 실행됐습니다.'


@decorator_function
def display_info(name, age):
    print 'display_info({}, {}) 함수가 실행됐습니다.'.format(name, age)

display()
display_info('John', 25)
```

```python
display 함수가 호출되기전 입니다.
display 함수가 실행됐습니다.
display_info 함수가 호출되기전 입니다.
display_info(john, 25) 함수가 실행됐습니다.
```