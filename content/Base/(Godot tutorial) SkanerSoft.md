202307051752
***
[[(Godot) туториал]]
***
https://www.youtube.com/watch?v=1igO4n4vWoI&list=PLf0k8CBUad-uBels10n74EIGQ45natifs&index=1
***
*с чего начать создание 3D игры*
[[202311082216]]

*камера и трансформации объектов: движение, вращение, создание 3D игры*
[[202311082219]]

*плавная камеры от 3-его лица*
[[202311082222]]

###### камера от первого лица
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
###### с чего начать создание своей первой игры
[анимация (55:13)](https://youtu.be/1MGsAQRL6sE?si=icwsoFJoJqUwWpva&t=3313)
[взаимодействие с coin (59:02)](https://youtu.be/1MGsAQRL6sE?si=Gtr8GDuOclIr_QZ7&t=3542)
[счетчик coin (1:00:45)](https://youtu.be/1MGsAQRL6sE?si=lSBR6dhohwwZSJ3R&t=3645)
[эффект удаления coin (1:01:17)](https://youtu.be/1MGsAQRL6sE?si=sRn1wfcSTEP5IKkh&t=3677)