# axelot

В docker-compose собран LAMP стэк, с доступом через nginx в режиме обратного прокси.

Зарегистрированы два доменных имени axelot.ml и axelot.tk , и сделал виртуальных 2 хоста на апаче по каждому доменному имени.

Доступ к сайтам через nginx по https с сертификатами от let's encrypt, и блокировкой по географическому признаку (разрешена только зона RU).

Результат: https://axelot.tk https://axelot.ml и весь проект на https://github.com/morozoffdn/axelot
