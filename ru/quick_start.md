# MyBee Quick-Start

Создадим тестовую гостевую машину с минимумом шагов. Предполагается, что MyBee уже установлен и вы можете попасть в меню через Shell: http://IP/shell/

1) Выключим ACL по публичному ключу. Внимание! С данной настройкой любой сможет создать виртуальную машину. Используйте это только в 'trusted' окружениях! Выбираем пункт **'Configure Pubkey WhiteList'**:

![quick1.png.png](https://myb.convectix.com/img/quick1.png?raw=true)

Состояние должно измениться на 'disabled'.

2) Данный шаг не обязателен, но рекомендутся заранее прогреть образ - мы должны убедиться, что MyBee имеет доступ до внешних зеркал. Выбираем пункт **'Shell ( warm cloud image )'** и попадаем в shell.

![quick2.png.png](https://myb.convectix.com/img/quick2.png?raw=true)

Пишем 'debian12' и получаем последний образ Linux Debian 12

![quick3.png.png](https://myb.convectix.com/img/quick3.png?raw=true)


3) На своей локальной машине создаем файл с произвольным именем `debian12.json` и следующим содержимым:

```
{
  "imgsize": "12g",
  "ram": "1g",
  "cpus": 2,
  "image": "debian12",
  "pubkey": "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJW9q4NkUjx+jjsuGB7ICNoATJFWvOnN0Q0JhJd7/DD/ k1@mother.my.domain"
}
```

где в поле pubkey вставьте строчку из своего ~/.ssh/*.pub файла (допустимы ED25519, ECDSA и RSA ключи).

4) Используя утилиту `curl`, отправляем запрос на создание ВМ:

```
curl -X POST -H "Content-Type: application/json" -d @debian12.json http://IP/api/v1/create/vm1
```

где IP - адрес MyBee.

![quick4.png.png](https://myb.convectix.com/img/quick4.png?raw=true)

5) Используя запрос `curl -H cid:CID http://IP/api/v1/status/vm1` с корректным CID (client ID), дожидаемся когда будет напечатана информация для соединения с ВМ. Как правило, ВМ доступна для использования в течении ~10 секунд при использовании HDD, при NVME/SSD обычно - быстрее

примечание: CID - это md5 хеш вашего ключа, в нашем случае:

```
md5 -qs 'ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJW9q4NkUjx+jjsuGB7ICNoATJFWvOnN0Q0JhJd7/DD/ k1@mother.my.domain'
190000ee6d0e18a82d6e79a34537a616
```

![quick5.png.png](https://myb.convectix.com/img/quick5.png?raw=true)

6) Используя свою ключ, подключаемся к гостю.

![quick6.png](https://myb.convectix.com/img/quick6.png?raw=true)


---

**<<_**__[Назад: Установка MyBee](get-myb.md)__ $~~~$ | $~~~$ __[Дальше: CLI/shell](shell.md)__**_>>**
