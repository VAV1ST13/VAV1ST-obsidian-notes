202206081648
***
[[00 Obsidian]]
***
*описание:*
Синхронизация ведения БД Obsidian через GitHub.
***
# инструкции
## для владельца БД
...
## для др. пользователей
1. Зарегистрироваться на github
2. Передать username чтобы добавить их в репозиторий
```
В репозитории владельца БД
1. Зайти в settings
2. Collaborators
3. Add people
4. ввести ник др. пользователя для синхронизации
5. Add username ...
6. username должен принять инвайт
```
3. Скачать-установить git (всё next)
4. Открыть папку, где должно находится новое хранилище
5. ПКМ - Git Bush Here
6. Откроется консоль, необходимо ввести команду
7. 
```bash
git clone https://github.com/VAV1ST13/obsidian-vault.git vav1st-vault
git config --global user.email "свой email"
git config --global user.name "ник"
```
(выйдет окно со входом гитхаба, выбрать первую кнопку, в браузере подтвердить подключение)
# команды
бэкап - сохраняет изменения и отправялет в базу
пулл - принимает изменения
Есть изменения?
Проверка
123
228
