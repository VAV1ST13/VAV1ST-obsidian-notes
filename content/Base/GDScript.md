202307141642
***
[[(Godot) туториал]]
***
## вращение объекта
```
func _process(delta):
	rotation.y += 0.1
	*вращение по оси Y со скор. 0,1 рад*
	print(rotation)
	*выводить в консоль как изм. 3 оси вращения объекта*
```

```
func _process(delta):
	rotate(Vector3(0,1,0), 0.1)
	*Vector3 - 3 координаты, 0,1 значение на которое изм. вектор*
	*rotate - умножение вектора, на значение 0.1, чтобы вращать по оси Y, ставим значение 1*
```

```
func _process(delta):
	rotate_y(0.1)
	*вращение по оси Y со скор. 0,1*
```

## перемещение объекта
```
func _process(delta):
	position.z -= 0.05
	*движение по оси Z на 0,05*
```

```
func _process(delta):
	translate(Vector3(0,0,-0.05))
```

## управление WASD
```
const SPEED = 10
*константа скорость игрока*

func _process(delta):
	var dir = Vector3()
	
	if Input.is_action_pressed("ui_left"):
		dir.x = -1
	*если нажать кнопку ui_left, player сдвинется по оси X и т. д.*
	if Input.is_action_pressed("ui_right"):
		dir.x = 1
	if Input.is_action_pressed("ui_up"):
		dir.z = -1
	if Input.is_action_pressed("ui_down"):
		dir.z = 1
	
	if dir:
		translate(SPEED * dir)
		*скорость умнож. на вектор умнож. на delta (сглаживание между кадрами(?))
```

## управление WASD с поворотами

```
extends MeshInstance3D

const SPEED = 10 
# можно менять скорость игрока

func _process(delta):
	var dir = Vector3(0, 0, 0)
	# var - созд. переменной
	# dir - назв. переменной
	var rot = Vector3(0, 0, 0)
	if Input.is_action_pressed("ui_left"):
		rot.y = 1
	if Input.is_action_pressed("ui_right"):
		rot.y = -1
	if Input.is_action_pressed("ui_up"):
		dir.z = -1
	if Input.is_action_pressed("ui_down"):
		dir.z = 1
	
	if dir:
		translate(SPEED * dir * delta)
	if rot: 
		rotate(rot, 0.1)
		# delta - время между фреймами
```

## гравитация
```
# получение значения гравитации из настроек проекта для синхронизации с узлами жесткого тела
var gravity = ProjectSettings.get_setting("physics/3d/default_gravity")
# переменная gravity = величина заданная в настройках Godot

func _physics_process(delta):
	# доб. гравитацию (функция физический процесс)
	if not is_on_floor():
	# если не на полу
		velocity.y -= gravity * delta
		# скорость по оси Y = минус конст. гравитация умнож. на delta
```

## прыжок
```
const JUMP_VELOCITY = 4.5
# константа скорость прыжка

...

# упр. прыжком
	if Input.is_action_just_pressed("ui_accept") and is_on_floor():
	# если нажать "пробел" и "объект на поверхности"
		velocity.y = JUMP_VELOCITY
		# скорость Y = "константа скорость прыжка"
```

## управление WASD + мышь
```
# скрипт: управление wasd, прыжок, гравитация, управление мышью от первого лица
extends CharacterBody3D

const SPEED = 5.0
const JUMP_VELOCITY = 4.5
const ROT = 0.003
const GRAVITY = 9.8

# переменные поворота по оси X и Y
var ROT_X = 0
var ROT_Y = 0

# захват курсора мыши, всегда будет находится внутри игры
func _ready():
	Input.set_mouse_mode(Input.MOUSE_MODE_CAPTURED)

# гравитация
func _physics_process(delta):
	if not is_on_floor():
		velocity.y -= GRAVITY * delta

# прыжок
	if Input.is_action_just_pressed("ui_accept") and is_on_floor():
		velocity.y = JUMP_VELOCITY

# движение
	var dir = Vector3(0, 0, 0)
	# var - созд. переменной
	# dir - назв. переменной
	if Input.is_action_pressed("ui_left"):
		dir.x = -1
	if Input.is_action_pressed("ui_right"):
		dir.x = 1
	if Input.is_action_pressed("ui_up"):
		dir.z = -1
	if Input.is_action_pressed("ui_down"):
		dir.z = 1
	
	if dir:
		translate(SPEED * dir * delta)
		# delta - время между фреймами

	move_and_slide()

func _input(event):
	# функция _input работает в момент, когда происходит любое действие
	# (любая физика работает только в момент "события")
	# (это значит если ничего не трогаешь, то ничего не произойдет, даже падение "фактическая остановка времени")
	if event is InputEventMouseMotion:
	# если event это "движение мыши"
	# то rot_y = движение мыши по X
	# то rot_x = движение мыши по Y
		ROT_Y -= event.relative.x * ROT
		ROT_X -= event.relative.y * ROT
		
		if ROT_X < -1: ROT_X = -1
		if ROT_X > 1: ROT_X = 1
		# ограничения угла поворота камеры 180 град. спереди снизу-вверх
		
		transform.basis = Basis(Vector3(0,1,0), ROT_Y)
		# transform хранит в себе информацию о положении объекта и вращении объекта
		$Camera3D.transform.basis = Basis(Vector3(1,0,0), ROT_X)
		# поворот камеры по оси X
```