# Получение и установка MyBee

# Установка через ISO на выделенный сервер

Вы можете получить установочный ISO образ с официального сайта [в разделе Downloads](https://myb.convectix.com/download/).
Инсталляция использует обычный 'bsdinstall' инсталлятор FreeBSD и процедура инсталляции MyBee ничем не отличается от 
обычного процесса [установки FreeBSD](https://docs.freebsd.org/en/books/handbook/bsdinstall/#bsdinstall-start)

:bangbang: | :warning: Обратите внимание! Если вы НЕ создаете дополнительного (непривелигированного) пользователя при инсталляции MyBe, обязательно используйте сложные пароли для пользователя 'root', поскольку в этом слуаче по-умолчанию будет разрешен вход пользователем 'root' через SSH (вы можете заблокировать доступ административную консоль)
:---: | :---


# Установка через FreeBSD rescue ( например Hetzner Rescue System )

:bangbang: | :warning: Предупреждение! Эти инструкции приводят к уничтожению информации на дисках того компьютера, куда устанавливается MyBee.
:---: | :---

1) Во-первых, вы должны перезагрузить свой Hetzner сервер в Rescue режиме с использованием FreeBSD 13.x. Убедитесь, что вы выбрали правильную архитектуру и ОС:

![mybee_hz1](https://user-images.githubusercontent.com/926409/163261607-a1d909fc-d909-4eaa-9273-83c70d9f3409.png)


2) После перезагрузки сервера и доступа к оболочке с правами root используйте команду «fetch», чтобы получить сценарий установки:

```
fetch https://myb.convectix.com/auto
```

Скрипт представляет собой sh-сценарии, поэтому просто запустите его через /bin/sh:

```
sh auto
```

![mybee_hz2](https://user-images.githubusercontent.com/926409/163675520-f2784da1-e62c-42ba-91ac-927a0e6ef012.png)


3) Если вы знакомы с программой установки bsdinstall, вам будут знакомы следующие шаги.  Все дальнейшие шаги аналогичны обычной установке FreeBSD. Исключением является лишь то, что по окончанию работы скрипта, вы вернетесь в 'rescue' shell Hetzner - просто перезагрузите сервер командой 'reboot' самостоятельно.

# инсталлятор MyB/FreeBSD

:information_source: Если вы знакомы процессом инсталляции FreeBSD и установщиком bsdinstall,  то можете [пропустить эту  главу](shell.md).

Обязательно вводите полное имя (FQDN) сервера:

![mybee_hz3](https://user-images.githubusercontent.com/926409/163675559-4ceb5b37-b5cf-4421-9632-aee829c4a855.png)

MyB использует богатые возможности и стабильность ZFS, возможность использования UFS отключена:

![mybee_hz4](https://user-images.githubusercontent.com/926409/163675561-135cc875-142e-4610-9c22-6506bb8325d9.png)

Подходите к выбору типа RAID с умом, в зависимости от того, какое количество дисков вы имеете и какие ваши цели.

![mybee_hz5](https://user-images.githubusercontent.com/926409/163675562-29b2cffc-d658-4db5-8ccb-3599dd4980e8.png)
![mybee_hz6](https://user-images.githubusercontent.com/926409/163675563-eb5b3bb4-0dde-403f-a97a-9efbe30504ac.png)

В нашем примере, мы имеем только два диска, которые в предыдущем шаге были использованы в качестве stripe, без высокой доступности в случае сбоя любого диска:

![mybee_hz7](https://user-images.githubusercontent.com/926409/163675564-2ebfd4d9-337a-4f54-8d6b-6fb1124e1890.png)

Это ваш последний шанс!

![mybee_hz8](https://user-images.githubusercontent.com/926409/163675565-afd6a60c-9af2-43b2-8ebd-603f4a979975.png)

Выбирайте стойкий пароль для пользователя 'root'. Если вы не будете создавать дополнительного пользователя на следующем шаге, MyB по-умолчанию разрешает доступ пользователя 'root' удаленно через SSH.

![mybee_hz9](https://user-images.githubusercontent.com/926409/163675566-fc65fee4-782c-46a4-a097-8ee1e0d5e18a.png)

Выбор и настройка сетевого интерфейса.

![mybee_hz10](https://user-images.githubusercontent.com/926409/163675543-1ea23001-9a67-4fbc-a329-c48d13f5fead.png)

IPv4 обязателен:

![mybee_hz11](https://user-images.githubusercontent.com/926409/163675545-5ad1f06e-c2c2-43d7-ab18-2b8ecc072981.png)



![mybee_hz12](https://user-images.githubusercontent.com/926409/163675546-fd344806-6ddf-437e-9e9f-300994c6754f.png)
![mybee_hz13](https://user-images.githubusercontent.com/926409/163675547-8b6256b3-2e15-4a4e-9036-6aae1ed9253e.png)

Без настройки DNS серверов, MyB не сможет получить gold образы из репозитория. Если у вас нет предпочитаемых DNS серверов, можете использовать публичные '8.8.8.8', '8.8.4.4' от Google:

![mybee_hz14](https://user-images.githubusercontent.com/926409/163675549-1417a25c-fff1-4189-b94c-743b97bc98fd.png)

Выбирете наиболее комфортный для вас часовой пояс:

![mybee_hz15](https://user-images.githubusercontent.com/926409/163675550-22527c00-ded5-4d9f-af68-816197602e0e.png)
![mybee_hz16](https://user-images.githubusercontent.com/926409/163675551-b7446919-20d7-4c96-86a1-b332d8b81ef8.png)

Вы можете создать непривелигированного пользователя для доступа к серверу удаленно.

![mybee_hz17](https://user-images.githubusercontent.com/926409/163675552-0bb4dd4d-6104-45f5-be4d-4ecaff00c41b.png)

:bangbang: | :warning: Обязательно добавьте создаваемого пользователя в группу 'wheel', иначе вы не сможете переключится в 'root' через команду 'su'
:---: | :---

![mybee_hz18](https://user-images.githubusercontent.com/926409/163675553-98c8eee6-c966-489c-a9a3-5c30d4561478.png)

На данном этапе вы сделали все, чтобы MyB продолжила установку, завершайте работу с bsdinstall.

![mybee_hz19](https://user-images.githubusercontent.com/926409/163675554-10af0f73-d95e-49d2-b041-0c61ef16c334.png)

Выберете 'no' для выхода из bsdinstall и в случае с Hetzner, выходом в 'rescue' шелл:

![mybee_hz20](https://user-images.githubusercontent.com/926409/163675558-72a96aca-b7cf-4c0a-97c7-23e719e09abd.png)

Вам осталось только перезагрузить сервер через команду: **shutdown -r now**

После перезагрузок, вы можете использовать доступ на IP сервера через SSH и для получения стартовой информации, обратится на сервер через броузер: http://IP 


---

Дальше: [CLI/shell](shell.md)
