202401021031
***
[[UEVR]]
[[UEVR документация]]
***
https://praydog.github.io/uevr-docs/usage/overview.html
***
# UEVR подробное руководство
Погрузитесь глубже в UEVR и узнайте, как точно настроить свои VR-возможности с помощью этого исчерпывающего руководства. Мы рассмотрим различные настройки, конфигурации и советы по устранению неполадок, чтобы помочь вам получить максимальное удовольствие от любимых игр на движке Unreal Engine в VR.

## Графический интерфейс пользователя
Внешний графический интерфейс предоставляет интуитивно понятный интерфейс для внедрения VR-функций в выбранную вами игру. Здесь вы можете:

1.Выбрать процесс для инжекции
2.Выбрать желаемую среду выполнения (OpenVR/OpenXR)
3.Включить плагины VR (если необходимо)
4.Настроить параметры предварительной инъекции

![[UEVR frontend-gui.png|500]]

## OpenVR или OpenXR?
OpenVR обычно имеет самую высокую совместимость, но OpenXR обычно обеспечивает более высокую производительность, когда работает, особенно если гарнитура имеет встроенную среду выполнения OpenXR.

OpenVR требует установки SteamVR. 
OpenXR требует наличия действующей среды выполнения OpenXR для гарнитуры, 
но может работать и через SteamVR, если SteamVR установлена в качестве активной среды выполнения.

При использовании виртуального рабочего стола необходимо использовать OpenXR, 
чтобы избежать задержки вращения при движении головы.

## Настройки перед инъекцией
Перед инъекцией вы можете настроить следующие параметры:

-`VR_RenderingMethod`: Выберите один из вариантов: "Родное стерео", "Синхронизированное последовательное" или "Чередование/AFR".
-`VR_SyncedSequentialMethod`: Настройте поведение метода синхронизированного последовательного рендеринга.
-`VR_UncapFramerate`: Включить или отключить отключение частоты кадров

После инъекции остальные параметры будут заполнены автоматически. 
Вы можете изменить эти настройки во внутриигровом меню или через файл `config.txt`.

![[UEVR pre-injection-settings.png|500]]

## In-Game Menu
Внутриигровое меню предлагает дополнительные опции настройки и ярлыки для регулировки параметров на лету. 
Вызовите меню, нажав клавишу **Insert** или **L3+R3** на контроллере.

Во внутриигровое меню можно попасть как через гарнитуру VR, так и с помощью вида рабочего стола, 
чтобы настроить параметры без необходимости надевать гарнитуру.

В последних обновлениях меню также можно управлять, наводя на него контроллер движения для эмуляции мыши.

![[UEVR in-game-menu-1.png|500]]
![[UEVR in-game-menu-2.png|500]]

## In-Game Shortcuts
Удерживая **RT**:

-RT + левый стик: Перемещение камеры влево/вправо/вперед/назад
-RT + правый стик: Перемещение камеры вверх/вниз
-RT + B: Сбросить смещение камеры
-RT + Y: Отцентрировать вид
-RT + X: Сбросить начало координат

## CVars and Fixes
![[cvars.png|500]]

Используйте внутриигровое меню для доступа и изменения различных CVar для исправления сломанных шейдеров и эффектов. 
Инструмент предлагает ряд возможностей для решения распространенных проблем рендеринга.

## Depth Buffer Integration (интеграция буфера глубины)
Хотя интеграция буфера глубины отключена по умолчанию, 
ее включение может значительно улучшить задержку на гарнитурах Oculus при использовании OpenXR с родным временем выполнения Oculus OpenXR. Чтобы включить интеграцию буфера глубины, настройте параметр `VR_EnableDepth`.

![[UEVR depth-buffer-integration.png|250]]

## Configurations
Все конфигурации хранятся для каждой игры в каталоге `%APPDATA%/UnrealVRMod`. 
Вы можете изменять настройки непосредственно в пользовательском интерфейсе или через файл `config.txt`. 
Доступ к этой директории можно получить во внешнем графическом интерфейсе, нажав кнопку "Open Global Dir".

![[UEVR configurations.png|500]]

### Plugins
Плагины можно установить в папку `plugins` в директории конфигурации игры. 
Просто бросьте в нее dll плагина.

## Устранение неполадок и оптимизация
### Оптимальная производительность и совместимость

-настройте графические параметры в игре, чтобы снизить нагрузку
-экспериментируйте с различными методами рендеринга, если вы столкнулись с ошибками рендеринга или сбоями.
-используйте внутриигровое меню и CVars для решения проблем с шейдерами и эффектами
-включите интеграцию буфера глубины для улучшения задержки на гарнитурах Oculus (только OpenXR).
-рассмотрите возможность обновления системы для получения наилучших впечатлений от высококлассных AAA-игр.

Дальнейшие твики могут быть сделаны путем изменения INI-файлов игры, с помощью UUU, UE4SS или других внешних инструментов. 
Различные твики, которые были сделаны для обычной версии игр, могут быть применены и к VR-версии.

### Для тех, кого укачивает
Включите "Decoupled Pitch" в опциях VR. Это остановит движение камеры по вертикали.

==>
[[UEVR привязка VR-контроллера по умолчанию]]