202206140026
***
[[00 AR]]
***
*описание*
стрим-интервью Бредли Линча с Карлом Гуттагом о проблемах разработки AR
***
*ссылка*
https://youtu.be/Lc1ozD_Rcds

***
AR Design Challenges (not in order)
1. Cost
   Цена
2. *Transparency*, Distortion, and visual artifacts
   *Прозрачность*, искажения и визуальные артефакты
3. *Eye relief [Enough for Glasses?]*
   (как далеко последняя линза в оптике от твоего глаза)
4. Blocking vision both forward and periphery (safety)
   Загораживание обзора как вперед, так и по периферии (безопасность)
5. Field of View (FOW) - Bigger linear FOV **squares** other problems
   Угол обзора.
- 25 to 50 is an area difference 4x
  от 25 до 50 град.
  (в Quest2 прим. 90 град.)
6. Resolution [Angular and number of pixels]
   Разрешение (колич. пикселей на угол обзора)
7. Eye box size [Makes seeing/finding image easier]
   Размер рамки для глаз [облегчает просмотр/поиск изображения]
8. *Brightness* **to the eye**
   Яркость на глаз
- Too dim and you can't see the virtual image
  Слишком тусклый, и вы не сможете увидеть виртуальное изображение
- Too bright and you can't see the real world
  Слишком яркий, и вы не сможете увидеть реальный мир
9. Contrast ["black" and "pictures frame"]
   Контрастность
10. Uniformity & color quality and chromatic aberrations
    Однородность, качество цвета и хроматические аберрации
11. Vergence-Accommodation Conflict (VAC) and related issues
- Most AR focus at ~2m, the hands are at ~0,5m
12. Size, Weight & form-factor [also weight distribution]
- Ear and nose can tolerate <50 grams for long periods
- Straps and other support cause many user issues
13. Eye safety/protection (particularly with glass waveguides)
14. Variability in head and face shapes
15. *Power efficiency* [battery size/weight & **heat**]
16. Variability in IPD (interpupillary distance) and head shapes
17. "Situational awareness" [safety]
18. Social issues
- Privacy, nerd, ugly, seeing eyes, glowing eyes, etc.
19. Computing and Communication (power, size, & weight)
20. Ruggedness and protection
21. Portability (**wear it all day or it fits in your pocket**)
22. Vision Correction

Often Improving One Aspect Has Negative Effects on Others => N-Dimensional Chess
***
*перевод*
Проблемы дизайна AR (не по порядку)
1. Цена
2. *Прозрачность*, искажения и визуальные артефакты
3. *Eye relief [Достаточно для очков?]* 
`как далеко последняя линза в оптике находится далеко от глаза`
`нужно иметь около 25мм от зрачка, чтобы носить очки`
`весь список - это список проблем, которые нужно решить для выхода на массовый рынок`
`для AR есть большая выборка локальных/нишевых рынков (прим. военные, медицина)`
4. Блокировка зрения спереди и на периферии (безопасность)
5. Поле зрения (FOV) - Большие линейные **квадраты** FOV другие проблемы
- От 25 до 50 град. разница в площади 4x
6. Разрешение (угловое и количество пикселей)
7. Размер коробки для глаз (облегчает видение/поиск изображения)
8. *Яркость* **на глаз**
- Слишком тусклый, и вы не можете увидеть виртуальное изображение 
- Слишком ярко, и вы не можете увидеть реальный мир
9. Контраст ("черный" и "рамки картинки")
10. Однородность и качество цвета и хроматические аберрации
11. Vergence-Accommodation Conflict (VAC) и связанные вопросы
- Большая часть AR фокусируется на ~2м, руки на уровне ~0,5м
12. Размер, вес и форм-фактор [также распределение веса] 
- *ухо и нос могут переносить <50 граммов в течение длительных периодов времени* 
- ремни и другая поддержка вызывают многие проблемы у пользователей
13. Безопасность глаз/защита (особенно с помощью стеклянных волноводов)
14. Изменчивость к форме головы и лица
15. *Эффективность питания* (размер батареи/веса и **тепло**)
16. Изменчивость в IPD (межзрачковое расстояние) и форме головы
17. «Ситуационная осведомленность» (Безопасность)
18. Социальные проблемы 
- конфиденциальность, нерд, уродливость, видимость глаз, светящиеся глаза и т. д.
19. Вычисления и связь (мощность, размер и вес)
20. Прочность и защита
21. Портативность **(носить весь день на себе или в кармане)**
22. Коррекция зрения

**Часто улучшение одного аспекта оказывает негативное влияние на других => n-мерные шахматы**
***
Near Eye Display Resolution (Arcminutes/Pixel)
- Measured in "arc minutes per pixel" or Pixels/Degree
	- With near eye displays, size is measured in angles
- Good human vision is said to be limited to ~1 arcminute ("retinal")
	- ~0.5 is the limit for exceptionally good vision to see
	- *1.5 (40 pixels/degree) is "good enough" for most purposes*
		- Lower angular resolution makes text slower to read, higher cost more
	- >2 means everything looks "chunky" and low resolution
- 140 Horizontal FOV at 1 arcminute is 8400 pixels
***
*перевод*
Разрешение дисплеев на глаз (аркминута/пиксель)
- измерено в "аркминута на пиксель" или пиксель/градус
	- с дисплеем на глаз, размер измеряется в углах
- Хорошее человеческое зрение ограничено ~1 аркминутой ("сетчатка")
	- ~0,5 - это предел для исключительно хорошего видения, чтобы увидеть
	- *1,5 (40 пикселей/градус) "достаточно хорошо" для большинства целей*
		- более низкое угловое разрешение делает текст сложнее для чтения, более высокое увеличивает стоимость
	- >2 означает, что всё выглядит "chunky" и низким разрешением
- 140 град. горизонтального FOV в 1 аркминуте 8400 пикселей.
***
*закладка 7:36*