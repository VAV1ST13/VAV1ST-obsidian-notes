202402270436

***

[[(Godot) туториал]]

***

https://learnxinyminutes.com/docs/gdscript/

***

GDScript — это динамически типизированный язык сценариев, 
созданный специально для бесплатного игрового движка Godot с открытым исходным кодом. 
Синтаксис GDScript аналогичен синтаксису Python. 
Его основные преимущества — простота использования и тесная интеграция с движком. 
Он идеально подходит для разработки игр.

***

**основы**

***

```
# однострочные комментарии записываются с использованием символа решетки.
```

***

```
"""
многострочные
комментарии
пишуться
с использованием
строки документации.
"""
```

***

```
# Script file сам по себе является классом, и вы можете при желании определить для него имя.
class_name MyClass
```

***

```
# наследование
extends Node2D
```

***

```
# переменные-члены
var x = 8                                          # int (целый)
var y = 1.2                                        # float (дробный)
var b = true                                       # bool (логический)
var s = "Hello World!"                             # string (строка)
var a = [1, false, "brown fox"]                    # Array (массив) - аналогичен списку в Python, но может содержать разные типы переменных одновременно
var d = {
	"key" : "value",
	42 : true
}                                                 # cловарь содержит пары ключ-значение
var p_arr = PoolStringArray(["Hi", "there", "!"]) # массивы пула могут содержать только определенный тип
```

***

```
# встроенные типы векторов
var v2 = Vector2(1, 2)
var v3 = Vector3(1, 2, 3)
```

***

```
# константы
const ANSWER_TO_EVERYTHING = 42
const BREAKFAST = "Spam and eggs!"
```

***

```
# перечисления
enum { ZERO, ONE, TWO, THREE }
enum NamedEnum { ONE = 1, TWO, THREE }
```

***

```
# экспортированные переменные видны в инспекторе.
export(int) var age
export(float) var height
export var person_name = "Bob" # подсказки типа экспорта не нужны, если вы задали значение по умолчанию.
```

***

```
# функции
func foo():
	pass # Ключевое слово pass - это место для будущего кода

func add(first, second):
	return first + second
```

***

```
# значения для печати
func printing():
	print("GDScript ", "is ", " awesome.")
	prints("These", "words", "are", "divided", "by", "spaces.")
	printt("These", "worlds", "are", "divided", "by", "tabs.")
	printraw("This gets printed to system console.")
```

***

```
# математика
func doing_math():
	var first = 9
	var second = 4
	print(first + second) # 12
	print(first - second) # 4
	print(first * second) # 32
	print(first / second) # 2
	print(first % second) # 0
	# Также есть +=, -=, *=, /=, %= и т.д.
	# Однако нет операторов ++ или --
	print(pow(first, 2)) # 64
	print(sqrt(second)) # 2
	printt(PI, TAU, INF, NAN) # встроенные константы
```

***

```
# поток управления
func control_flow():
	x = 8
	y = 2 # y изначально был float, но мы можем изменить его тип на int используя возможности динамической типизации!

	if x < y:
		print("x is smaller than y")
	elif x > y:
		print("x is bigger than y")
	else:
		print("x and y are equal")

	var a = true
	var b = false
	var c = false
	if a and b or not c: # альтернативно вы можете использовать &&, || и !
		print("This is true!")

	for i in range(20): # диапазон GDScript аналогичен диапазону Python.
		print(i) # так что это будет печатать числа от 0 до 19
		
	for i in 20: # в отличие от Python, вы можете напрямую перебирать int
		print(i) # так что это также будет печатать числа от 0 до 19
		
	for i in ["two", 3, 1.0]: # перебираем массив
		print(i)
		
	while x > y:
		printt(x, y)
		y += 1
		
	x = 2
	x = 10
	while x < y:
		x += 1
		if x == 6:
			continue # 6 не будет напечатан из-за оператора continue
		prints("x is equal to:", x)
		if x == 7:
			break # цикл прервется на 7, поэтому 8, 9 и 10 не будут напечатаны
			
	match x:
		1:
			print("Match is similar to switch.")
		2:
			print("However you don't need to put cases befor each value.")
		3:
			print("Furthemore each case breaks on default.")
			break # ОШИБКА! Оператор Break не нужен!
		4: 
			print("If you need fallthrough use continue.")
			continue
		_:
			print("Underscore is a default case.")
			
	# тернарный оператор (одна строка оператора if-else)
	prints("x is", "positive" if x >= 0 else "negative")
```

***

```
# casting
func casting_examples():
	var i = 42
	var f = float(42) # приведение с использованием конструктора переменных
	var b = i as bool # или используя ключевое слово «as»

```

***

```
# Функции переопределения  
# По соглашению встроенные переопределяемые функции начинаются с подчеркивания,  
# но на практике вы можете переопределить практически любую функцию
```

***

```
# _init вызывается при инициализации объекта  
# это конструктор объекта.
func _init():
	# здесь инициализируется внутреннее содержимое объекта.
	pass
```

***

```
# _process вызывается в каждом кадре.
func _process(delta):
	# Аргумент дельта, передаваемый этой функции, представляет собой количество секунд,  
	# который прошел между последним кадром и текущим.
	print("Delta time equals: ", delta)
```

***

```
# _physics_process вызывается на каждом физическом кадре.
# Это означает, что дельта должна быть постоянной.
func _physics_process(delta):
	# простые движения с использованием векторного сложения и умножения.
	var direction = Vector2(1, 0) # или Vector2.RIGHT
	var speed = 100.0
	self.global_position += direction * speed * delta
	# self ссылается на текущий экземпляр класса
```

***

```
# при переопределении вы можете вызвать родительскую функцию с помощью оператора точки.
# как здесь:
	func get_children():
	# сделайте здесь несколько дополнительных действий
	var r = .get_children() # вызвать реализацию родителя
	return r
```

***

```
# внутренний класс
class InnerClass:
	extends Object
	
	func hello():
	    print("Hello from inner class!")
	    
func use_inner_class():
  var ic = InnerClass.new()
  ic.hello()
  ic.free() # использовать свободную память для очистки
```

***

**доступ к другим узлам дерева сцены**

***

```
# расширяет Node2D

var sprite # в этой переменной будет храниться ссылка

# вы можете получить ссылки на другие узлы в _ready
func _ready() -> void:
	# NodePath полезен для доступа к узлам
	# создайте NodePath, передав его конструктору строку String:
	var path1 = NodePath("path/to/something")
	# или с помощью литерала NodePath:
	var path2 = @"path/to/something"
	# примеры NodePath:
	var path3 = @"Sprite"                    # относительный путь, непосредственный потомок текущего узла
	var path4 = @"Timers/Firerate"           # относительный путь, дочерний путь
	var path5 = @".."                        # родитель текущего узла
	var path6 = @"../Enemy"                  # родной брат текущего узла
	var path7 = @"/root"                     # абсолютный путь, эквивалентный get_tree().get_root()
	var path8 = @"/root/Main/Player/Sprite"  # абсолютный путь к спрайту игрока
	var path9 = @"Timers/Firerate:wait_time" # доступ к свойствам
	var path10 = @"Player:position:x"        # доступ к подсвойствам
	
	# Наконец, чтобы получить ссылку, воспользуйтесь одним из этих способов:
	sprite = get_node(@"Sprite") as Sprite # всегда приводите к ожидаемому типу
	prite = get_node("Sprite") as Sprite   # здесь String неявно приводится к NodePath
	
	sprite = get_node(path3) as Sprite
	sprite = get_node_or_null("Sprite") as Sprite
	sprite = $Sprite as Sprite
	
func _process(delta):
	# Теперь мы можем повторно использовать ссылку в других местах.
	prints("Sprite has global_position of", sprite.global_position)

# Используйте ключевое слово onready, чтобы присвоить значение
# переменной непосредственно перед выполнением _ready.
# Это часто используемый синтаксический сахар.
onready var tween = $Tween as Tween

# Вы можете экспортировать NodePath, чтобы назначить его в инспекторе.
export var nodepath = @""
onready var reference = get_node(nodepath) as Node
```

***

**Сигналы**

Сигнальная система - это реализация в Godot паттерна программирования "Наблюдатель". Вот пример:

```
class_name Player extends Node2D

var hp = 10

signal died() # определить сигнал
signal hurt(hp_old, hp_new) # сигналы могут принимать аргументы

func apply_damage(dmg):
  var hp_old = hp
  hp -= dmg
  emit_signal("hurt", hp_old, hp) # издать сигнал и передать аргументы
  if hp <= 0:
    emit_signal("died")

func _ready():
  # подключите сигнал "died" к функции "_on_death", определенной в self
  self.connect("died", self, "_on_death")

func _on_death():
  self.queue_free() # уничтожить игрока после смерти
```

***

**Тип подсказки**

GDScript может опционально использовать статическую типизацию.

***

```
extends Node

var x: int # определить типизированную переменную
var y: float = 4.2
var z := 1.0 # вывод типа на основе значения по умолчанию с помощью := оператора

onready var node_ref_typed := $Child as Node

export var speed := 50.0

const CONSTANT := "Typed constant."

func _ready() -> void:
  # function returns nothing
  x = "string" # ERROR! Тип не может быть изменен!
  return

func join(arg1: String, arg2: String) -> String:
  # функция принимает две строки и возвращает строку
  return arg1 + arg2

func get_child_at(index: int) -> Node:
  # функция принимает int и возвращает Node
  return get_children()[index]

signal example(arg: int) # ERROR! Сигналы не могут принимать введенные аргументы!
```

***

## дальнейшее чтение

-[Godot’s Website](https://godotengine.org/)
-[Godot Docs](https://docs.godotengine.org/en/stable/)
-[Getting started with GDScript](https://docs.godotengine.org/en/stable/getting_started/scripting/gdscript/index.html)
-[NodePath](https://docs.godotengine.org/en/stable/classes/class_nodepath.html)
-[Signals](https://docs.godotengine.org/en/stable/getting_started/step_by_step/signals.html)
-[GDQuest](https://www.gdquest.com/)
-[GDScript.com](https://gdscript.com/)