202204131425
***
Для передачи данных служат протоколы:
- *TCP* (*Transmission Control Protocol*) протокол упр. передачи данных
- *UPD* (*User Datagram Protocol*) протокол пользовательских дейтаграмм. 
Применяется когда не треб. подтверждение приема
(DNS-запросы, IP-телефония)

Передача данных по протоколу *TCP* предусматривает наличие подтверждений получения информации.
Если передающая сторона не получит в установленные сроки необходимого подтверждения, то данные будут переданы повторно.

Поэтому протокол *TCP* относят к протоколам, предусматривающим соединение (*connection oriented*), а *UPD* - нет (*connection less*)

Протокол *Internet Control Message Protocol* (*ICMP*, протокол управляющих сообщений интернета) исп. для передачи данных о параметрах сети.
Он вкл. типы пакетов: *ping, destination unreachable, TTL exceeded* и т. д.
***
=> [[протоколы IPv6]]