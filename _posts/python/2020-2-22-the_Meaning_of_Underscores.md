---
comments: true
title: The Meaing of Underscores in Python. 파이썬에서 언더바, 언더스코어의 의미
published: 2020-2-22
updated: 2020-2-22
tags: [python, underscore]
categories: [development]
---

python에서 언더스코어, underscore의 의미가 무엇인지 알아봅시다.



#### \* 이 글은 Dan Bader의 글 [The Meaing of Underscores in Python](https://dbader.org/blog/meaning-of-underscores-in-python)을 번역한 글입니다.



# 파이썬에서 언더스코어(Underscores)의 의미

파이썬에는 싱글, 더블 언더스코어("dunder")에 대한 다양한 의미와 네이밍 컨벤션이 있습니다. 이를 통해 네임 맹글링(name mangling)이 어떻게 동작하는지, 그것이 클래스에 어떤 영향을 끼치는지 알아봅시다.

싱글 혹은 더블 언더스코어는 파이썬 변수와 메소드 이름에 있어 의미를 가집니다. 어떤 것들은 그저 컨벤션에 불과하기도 하고 프로그래머에게 힌트가 되기도 합니다. 어떤 것들은 파이썬 인터프리터에 의해 강제되기도 합니다.

'파이썬 변수와 메소드 이름에서 싱글과 더블 언더스코어는 무슨 의미일까?'를 궁금해하시는 여러분들을 위해, 글을 적어보았습니다.

이 글에서 저는 아래의 5가지 언더스코어 패턴과 네이밍 컨벤션, 그리고 그들이 어떻게 파이썬 프로그램에 영향을 끼치는지 서술하겠습니다.

- Single Leading Underscore: `_var`
- Single Trailing Underscore: `var_`
- Double Leading Underscore: `__var`
- Double Leading and Trailing Underscore: `__var__`
- Single Underscore: `_`

이 글의 맨 밑으로 가시면 이들에 대한 요약과 짧은 튜토리얼 비디오가 있습니다.

그럼 이제 시작해보죠!



## 1. Single Leading Underscore: `_var`

변수나 메소드에 이름과 관련하여, 접두사 싱글 언더스코어는 컨벤션으로서 의미를 가집니다. 프로그래머에게 힌트이며, 파이썬 커뮤니티에서 동의하는 것들을 의미합니다. 프로그램에는 영향을 끼치지 않죠.

접두사 싱글 언더스코어로 명명된 변수나 메소드는 내부 사용을 위해 만들어진 것이라는 점을 다른 프로그래머에게 알려주는 역할을 합니다. 이 컨벤션은 PEP 8에서 정의되어 있습니다.

이는 파이썬에 의해 강제되지는 않습니다. 파이썬은 private과 public 변수들에 대해 자바처럼 강력한 구별을 가지지는 않습니다. 이는 마치, 누군가 작은 언더스코어 경고를 남긴 것과 같습니다.

>"이봐! 이건 클래스의 public interface로 만들어진 게 아니라고. 그냥 놔두지 그래?"

아래 예시를 보시죠.

```python
class Test:
  def __init__(self):
    self.foo = 11
    self._bar = 23
```

이 클래스를 인스턴스화하고 생성자에서 정의된 `foo` 와 `_bar` 변수에 접근하려하면 무슨 일이 발생할까요? 한 번 알아보죠.

```python
>>> t = Test()
>>> t.foo
11
>>> t._bar
23
```

보셨다시피 `_bar` 변수는 클래스에 도달하여 변수에 접근하여 해당 값에 접근하는 것을 전혀 막지 못합니다.

이것은 단지 싱글 언더스코어 접두사가 파이썬에서는, 최소한 변수와 메소드 네이밍에 있어, 컨벤션에 불과하기 때문입니다. 

<b>하지만, 접두사 싱글 언더스코어는 모듈이 임포트 될 때 영향을 끼칩니다.</b> `my_module`이라는 모듈을 만들었다고 한 번 가정해봅시다.

```python
# This is my_module.py

def externel_func():
    return 23
  
def _internal_func():
 		return 42
```

이제, 와일드카드 임포트(import *)를 사용하여 모듈의 모든 것들을 가져오게 만든다면, 파이썬은 접두사 싱글 언더스코어로 만들어진 것들을 가져오지 않는 것을 확인할 수 있습니다. (다만 모듈이 `__all__`을 오버라이드하지 않는 한에서 말이죠.)

```python
>>> from my_module import *
>>> external_func()
23
>>> _internal_func():
NameError: "name '_internal_func' is not defined"
```

어쨌든 와일드카드 임포트는 사용하지 않는 것이 좋습니다. 어떤 네임들이 어느 네임스페이스에 있는지 명확하지 않기 때문입니다. 분명함을 위해 일반적인 임포트 방식을 사용하는 것이 낫습니다.

와일드카드 임포트와는 달리, 일반적인 임포트는 접두사 싱글 언더스코어는 영향을 받지 않습니다.

```python
>>> import my_module
>>> my_module.external_func()
23
>>> my_module._interanl_func()
42
```

이 지점이 헷갈리는 부분임을 인지하고 있습니다. 그래도 PEP8 컨벤션을 따르신다면 와일드카드 임포트는 사용하지 않는 편이 낫습니다. 기억해야할 것은 이겁니다.

> 싱글 언더스코어는 내부 사용만을 위한 파이썬 네이밍 컨벤션입니다. 파이썬 인터프리터에 의해 강제되지 않으며 프로그래머에게는 힌트입니다.



## 2. Single Trailing Underscore: `var_`

때론, 변수에 가장 적합한 이름이 이미 키워드로 잡혀있는 경우가 있습니다. 그래서 `class`나 `def`는 파이썬에서 변수 명으로 사용될 수 없습니다. 이 경우, 접미사로 싱글 언더스코어를 붙여서 네이밍 컨플릭트를 피할 수 있습니다.

```python
>>> def make_object(name, class):
SyntaxError: "invalid syntax"

>>> def make_object(name, class_):
...     pass
```

요약하면, 접미사 싱글 언더스코어는 파이썬 키워드들과 네이밍 충돌을 피하기 위한 컨벤션으로 사용됩니다. 이는 PEP 8에 설명되어 있습니다.



## 3. Double Leading Underscore: `__var`

지금까지 다뤘던 네이밍 패턴은 컨벤션의 일종이었습니다. 하지만 파이썬 클래스 속성으로서(변수와 메소드) 더블 언더스코어로 시작하는 것들은 위의 것들과 약간 다릅니다.

접두사 더블 언더스코어는 파이썬 인터프리터로 하여금 하위 클래스와의 네이밍 충돌을 피하기 위해 속성 이름을 재작성하도록 만듭니다.

이것은 *name mangling*(인터프리터가 변수의 이름을 변경해 후에 클래스가 확장되었을 때 네이밍 충돌이 일어나기 어렵게 만드는 것)으로도 불립니다. 

약간 추상적으로 들릴 수도 있겠습니다. 그래서 아래의 코드 예시를 만들어봤습니다.

```python
class Test:
    def __init__(self):
        self.foo = 11
        self._bar = 23
        self.__baz = 23
```

내장 함수인 **`dir()`**을 사용해서 속성들을 한 번 살펴보죠.

```python
>>> t = Test()
>>> dir(t)
['_Test__baz', '__class__', '__delattr__', '__dict__', '__dir__',
 '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__',
 '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__',
 '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__',
 '__setattr__', '__sizeof__', '__str__', '__subclasshook__',
 '__weakref__', '_bar', 'foo']
```

객체의 속성들이 담긴 리스트가 보입니다. 이 리스트를 살펴서 우리가 앞서 만든 변수명 `foo`, `_bar`, `__baz` 를 보면 흥미로운 변화를 발견할 수 있을 겁니다.

- `self.foo` 변수는 속성 리스트에서 마치 수정되지 않은 것처럼 `foo`로 나타났습니다.
- `self_bar`도 마찬가지죠. 마치 제가 이전에 말씀드린 것처럼 접두사 싱글 언더스코어는 컨벤션에 불과하는 것을 의미하죠.
- 하지만 `self.__baz`는 뭔가 다릅니다. `__baz`로 된 변수가 위 리스트에 없습니다.

그러면 `__baz`에는 무슨 일이 벌어진 걸까요?

자세히 살펴보면 `_Test__baz`가 있는 걸 확인할 수 있습니다. 이것이 파이썬 인터프리터가 적용한 *name mangling*입니다. 이는 하위 클래스들에서 오버라이드가 되는 것을 막고 변수를 보호하기 위함입니다.

Test 클래스를 확장하여 생성자를 통해 현재 있는 속성들을 오버라이드 해보죠.

```python
class ExtendedTest(Test):
    def __init__(self):
        super().__init__()
        self.foo = 'overridden'
        self._bar = 'overridden'
        self.__baz = 'overridden'
```

이제 `foo`, `_bar`, `__baz` 변수들에 어떤 일이 벌어질 것 같나요? 한 번 보시죠.

```python
>>> t2 = ExtendedTest()
>>> t2.foo
'overridden'
>>> t2._bar
'overridden'
>>> t2.__baz
AttributeError: "'ExtendedTest' object has no attribute '__baz'"
```

잠깐만요, 왜 `t2.__baz`에 접근하려는데 **AttributeError**가 뜨는 걸까요? Name mangling이 다시 도졌군요. 심지어 이 객체가  `__baz`는 없다고 합니다.

```python
>>> dir(t2)
['_ExtendedTest__baz', '_Test__baz', '__class__', '__delattr__',
 '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__',
 '__getattribute__', '__gt__', '__hash__', '__init__', '__le__',
 '__lt__', '__module__', '__ne__', '__new__', '__reduce__',
 '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__',
 '__subclasshook__', '__weakref__', '_bar', 'foo', 'get_vars']
```

보시다시피 `__baz`는 `_ExtendedTest__baz`로 바뀌어있습니다.

```python
>>> t2._ExtendedTest__baz
'overridden'
```

하지만 `_Test__baz`는 여전히 살아있죠.

```python
>>> t2._Test__baz
42
```

더블 언더스코어 name mangling은 프로그래머에게 완전히 명백합니다. 아래 예시를 보시면 이해가 될 겁니다.

```python
class ManglingTest:
    def __init__(self):
        self.__mangled = 'hello'

    def get_mangled(self):
        return self.__mangled

>>> ManglingTest().get_mangled()
'hello'
>>> ManglingTest().__mangled
AttributeError: "'ManglingTest' object has no attribute '__mangled'"
```

name mangling이 메소드 이름에도 적용이 될까요? 됩니다. 클래스 하에서 더블 언더스코어("dunder")로 시작하는 모든 이름들에 적용이 됩니다.

```python
class MangledMethod:
    def __method(self):
        return 42

    def call_it(self):
        return self.__method()

>>> MangledMethod().__method()
AttributeError: "'MangledMethod' object has no attribute '__method'"
>>> MangledMethod().call_it()
42
```

여기에, 아마도 놀라운, name mangling에 관한 예시가 있습니다.

```python
_MangledGlobal__mangled = 23

class MangledGlobal:
    def test(self):
        return __mangled

>>> MangledGlobal().test()
23
```

이 경우에, 저는 전역 변수로서 `_MangledGlobal__mangled` 변수를 선언했습니다. 그리고 이 변수를 클래스인 `MangledGlobal`의 내부에서 접근을 시도했죠. name mangling 덕분에 `_MangledGlobal__mangled` 전역 변수를 마치 test() 메소드의 `__mangled`처럼 참조할 수 있었습니다.

파이썬 인터프리터는 `__mangled`가 더블 언더스코어로 시작했기 때문에  자동으로 `_MangledGlobal__mangled`로 이름을 확장시킵니다. 이는 name mangling이 클래스 속성에만 구속되어 있지 않다는 것을 보여줍니다. 이는 클래스의 더블 언더스코어로 시작하는 모든 이름들에 적용됩니다.

**Now this was a lot of stuff to absorb.**

솔직히 말씀드리면, 처음부터 이 예시들과 설명들이 떠올랐던 건 아닙니다. 이를 위해 다소간 연구와 편집이 필요했습니다. 파이썬을 수년간 사용했지만 규칙들과 이처럼 특수한 케이스들이 항상 떠오르는 것은 아닙니다.

때로는 개발자에게 가장 중요한 스킬은 '패턴 인식'과 어디를 찾아야하는지 아는 것입니다. 이로 인해 약간 압도당한 느낌을 받으셨더라도 걱정하실 필요 없습니다. 시간을 가지고 이 글의 예시들을 시험해보세요.

name mangling에 대해서 제가 보여준 아이디어들과 행위들에 익숙해지기 위해 충분히 활용해보세요. 만약 와일드한 상황에 부딪히게 된다면, 다큐멘테이션에서 어디를 찾아야할지 알게 될 거에요.

>## ⏰ 토막지식: 파이썬에서 dunder가 뭔가요?
>
>파이썬 유저들이 파이썬에 대해 이야기 할 때 dunder라는 것을 들어본 적이 있으실 겁니다. 이것이 무엇인지 궁금했다면 답을 알려드리죠.
>
>더블 언더스코어는 파이썬 커뮤니티에서 자주 dunder로 불립니다. 이는 더블 언더스코어가 파이썬 코드에서 자주 나타나지만 발음하기 번거롭기 때문에 줄여서 부르는 겁니다.
>
>예를 들어 `__baz`를 발음하실 땐 "dunder baz"로 부를 수 있습니다. 마찬가지로 `__init__` 역시 "dunder init"으로 불릴 수 있습니다. "dunder init dunder"로 불려야하는 것 아니냐고 물으실 수 있지만 그저 네이밍 컨벤션의 유별난 점이라 생각하시면 됩니다.
>
>마치 파이썬 개발자들 간의 *secret handshake* 라고나 할까요🙂



## 4. Double Leading and Trailing Underscore: `__var__`

놀랍게도, name mangling은 더블 언더스코어가 앞뒤로 붙었을 때는 적용되지 않습니다. 앞뒤 더블 언더스코어는 아무 탈이 없습니다. 

```python
class PrefixPostfixTest:
    def __init__(self):
        self.__bam__ = 42

>>> PrefixPostfixTest().__bam__
42
```

하지만 앞뒤로 더블 언더스코어가 붙은 이름들은 파이썬에서 이미 예약어로서 특수하게 사용되는 경우가 많습니다. 이는 생성자를 `__init__`으로 작성하고, `__call__`을 사용하는 것과 같은 규칙을 의미합니다.

이 dunder 메소드들은 *magic methods*로 불립니다. 하지만 파이썬 커뮤니티의 많은 사용자들, 저도 포함해서 별로 ___맘에 들지는 않습니다.___

충돌을 피하기 위해 앞뒤 더블 언더스코어를 사용하지 않는 것이 최선입니다.



## 5. Single Underscore: `_`

컨벤션에 따라, 싱글 언더스코어는 임시적이거나 중요하지 않은 변수들에 사용됩니다.

예를 들어, 아래 반복문에서처럼 인덱스에 접근할 필요 없다면 `_`를 임시 변수로 사용할 수 있습니다.

```python
>>> for _ in range(32):
...     print('Hello, World.')
```

또 싱글 언더스코어를 unpacking expressions에서 중요하지 않은 변수들을 풀어헤치는데 사용할 수도 있습니다. 다시 한 번 말하지만, 이 의미는 "컨벤션에 따라" 다르며 파이썬 인터프리터에 의해 발생하는 특별한 작용은 없습니다. 싱글 언더스코어는 그저 이러한 목적들에 의해 사용되는 변수 이름입니다.

아래 코드 예시를 보시면, tuple car를 풀어헤치는데 제가 관심을 두는 변수는 오직 `color`와 `mileage` 뿐입니다. 하지만 모든 변수들을 성공적으로 언패킹하기 위해선 튜플의 변수들을 다른 변수들에 모두 할당해야 하죠. 이때 `_`가 유용합니다.

```python
>>> car = ('red', 'auto', 12, 3812.4)
>>> color, _, _, mileage = car

>>> color
'red'
>>> mileage
3812.4
>>> _
12
```

게다가 임시변수로서 `_`는 대부분의 파이썬 REPL에서 특별한 변수로서 사용됩니다. 이는 인터프리터에 의해 만들어진 마지막 explression을 의미합니다.

```python
>>> 20 + 3
23
>>> _
23
>>> print(_)
23

>>> list()
[]
>>> _.append(1)
>>> _.append(2)
>>> _.append(3)
>>> _
[1, 2, 3]
```







## 📓 파이썬 언더스코어의 네이밍 패턴과 요약

| 패턴                                       | 예시      | 의미                                                         |
| ------------------------------------------ | --------- | ------------------------------------------------------------ |
| **Single Leading Underscore**              | `_var`    | 내부 사용을 위한 네이밍 컨벤션. <br />파이썬 인터프리터에 의해 강제되지 않음(와일드카드 임포트할 시에는 아님)<br />프로그래머에게는 일종의 힌트 |
| **Single Trailing Underscore**             | `var_`    | 파이썬 키워드와의 충돌을 막기 위한 컨벤션                    |
| **Double Leading Underscore**              | `__var`   | 클래스에서 name mangling을 일으킴. 파이썬 인터프리터에 의해 강제됨 |
| **Double Leading and Trailing Underscore** | `__var__` | 파이썬에서 특별한 메소드들을 의미함. 코드 작성시 이러한 네이밍을 피할 것. |
| **Single Underscore**                      | `_`       | 임시적이거나 중요하지 않은 변수들에 사용됨<br />또한 파이썬 REPL에서 인터프리터의 마지막 expression으로도 사용됨 |



## 📺 언더스코어 패턴 - 튜토리얼 비디오

<center><iframe width="560" height="315" src="https://www.youtube.com/embed/ALZmCy2u0jQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe></center>

**Related Articles**:

- [A Python Riddle: The Craziest Dict Expression in the West](https://dbader.org/blog/python-mystery-dict-expression) – Let’s pry apart this slightly unintuitive Python dictionary  expression to find out what’s going on in the uncharted depths of the  Python interpreter.
- [Writing Clean Python With Namedtuples](https://dbader.org/blog/writing-clean-python-with-namedtuples) – Python comes with a specialized “namedtuple” container type that  doesn’t seem to get the attention it deserves. It’s one of these amazing features in Python that’s hidden in plain sight.
- [Fundamental Data Structures in Python](https://dbader.org/blog/fundamental-data-structures-in-python) – In this article series we’ll take a tour of some fundamental data  structures and implementations of abstract data types (ADTs) available  in Python’s standard library.
- [Records, Structs, and Data Transfer Objects in Python](https://dbader.org/blog/records-structs-and-data-transfer-objects-in-python) – How to implement records, structs, and “plain old data objects” in  Python using only built-in data types and classes from the standard  library.
- [Python Iterators: A Step-By-Step Introduction](https://dbader.org/blog/python-iterators) – Understanding iterators is a milestone for any serious Pythonista.  With this step-by-step tutorial you’ll understanding class-based  iterators in Python, completely from scratch.