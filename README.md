
# Система уведомлений NOTIF

## Назначение

Система уведомлений NOTIF предназначена для организации уведомлений больших групп пользователей (например, членов какой-либо общественной организации или политического движения) о предстоящих событиях, акциях и т. п. Систему NOTIF крайне трудно заблокировать; вместе с тем обращение к системе (чтение уведомлений) производится web-интерфейсом по протоколу https путем запроса к стандартному серверу DoH, что не вызывает подозрений при анализе трафика пользователя средствами СОРМ и ТСПУ.

## Краткое описание

Информационные сообщения (уведомления) размещаются в кодированном виде в txt-записи DNS выбранного администратором системы домена. Шифрование при размещении сообщений не используется; при работе с системой больших групп пользователей оно не имеет смысла, так как ключи шифрования в такой ситуации невозможно сохранить в секрете. Своеобразным «ключом» является имя используемого домена, известное только пользователям системы.

В исходном тексте сообщения допустимы только 40 символов: русские буквы и цифры, остальные символы вырезаются. При кодировании буквы переводятся в верхний регистр, Ё заменяется на Е, цифры 0, 3 и 4 — на буквы О, З и Ч соответственно. Максимальный объем исходного текста сообщения — около 300 символов.

## Подготовка к использованию системы

Единственным предварительным действием перед использованием системы является регистрация на любом бесплатном сервисе, позволяющем выбрать домен третьего уровня и администрировать txt-записи DNS этого домена (таких сервисов существует несколько, например, [freedns.afraid.org](https://freedns.afraid.org) и подобные ему). Можно использовать и другие способы (например, владение собственным доменом второго уровня), позволяющие администрировать txt-записи DNS. Важно, что при блокировке используемого домена система продолжит работать, поскольку DNS-записи реплицируются (копируются) по всей системе DNS-серверов.

## Работа с системой

### Публикация сообщения

Для публикации сообщения:

1. Откройте локально браузером файл `write.html` и введите сообщение в верхнее окно. По мере ввода сообщения над верхним окном будет отображаться количество символов в закодированном сообщении, а в нижнем окне — закодированное сообщение. Первые три символа закодированного сообщения являются текущей датой (первый символ — номер месяца в шестнадцатеричной системе от 0 до b, второй и третий символы — число месяца в десятичной системе). Закодированное сообщение обрамлено кавычками. Как только количество символов закодированного сообщения станет равным 255 (что соответствует примерно 300 символам исходного сообщения), оно начнет отображаться красным цветом, что означает превышение максимально допустимого размера исходного сообщения.
1. Скопируйте закодированное сообщение (вместе с кавычками) в буфер обмена.
1. Отредактируйте txt-запись DNS выбранного вами домена (см. выше), указав в качестве ее нового значения закодированное сообщение вместе с кавычками.

### Чтение сообщения

Любой пользователь, знающий имя используемого системой домена, может прочитать текущее сообщение. Для чтения сообщения:

1. Сохраните файл `read.html` на свой компьютер или телефон.
1. Отредактируйте переменные `domen` и `doh` в строке 3 этого файла. В качестве значения переменной `domen` укажите имя используемого домена (по умолчанию указано имя тестового домена `notif.my.to`), а в качестве значения переменной `doh` — ip-адрес или доменное имя любого публичного сервера DoH (DNS over HTTPS); по умолчанию указан адрес DoH-сервера Google `8.8.8.8` (можно указать и его доменное имя `dns.google`). Крайне маловероятно, что этот сервер будет заблокирован, однако если это произойдет, укажите адрес или доменное имя любого другого сервера DoH.
1. Запустите файл `read.html` браузером локально; в окне браузера вы увидите сообщение. При этом система СОРМ или ТСПУ увидит только ваш факт обращения к серверу DoH по протоколу https, что не является подозрительным. Имя домена, txt-запись DNS которого запрашивается, провайдеру или другим наблюдателям не видно.