# Работа с jail (CBSDfile)

## Описание

Если вы используетет платформу FreeBSD, помимо работы с API через `curl` и `nubectl`, у вас есть еще одна возможность работы с MyBee - через CBSDfile. Этот файл обрабатывается командой `cbsd` и по этой причине,
на данный момент недоступен для других платформ.

Формат файла, описывающий контейнер для работы через API полностью совпадает с синтаксисом <a target="_blank" href="https://www.bsdstore.ru/en/cbsdfile.html">CBSDfile</a>, работающим локально, но добавляются переменные
`CLOUD_URL` (указывает на сервер MyBee) и `CLOUD_KEY` (указывает путь к pubkey).

## Создание jail

Пример `CBSDfile` для создания контейнера jail1:

```
CLOUD_URL="https://rio-west.example.com"
CLOUD_KEY="ssh-ed25519 AAAA...xxx your@my.domain"

jail_jail1()
{
	imgsize="10g"
	pkg_bootstrap=0
	ssh_wait=0
}
```

Вы можете захотеть при создании контейнера скопировать в окружение произвольный файл (например: prepare.sh) и выполнить его. Например, контейнер с именем `minio1`:
```
CLOUD_URL="https://bravo-east.example.com"
CLOUD_KEY="ssh-ed25519 AAAA...xxx your@my.domain"

jail_minio1()
{
	imgsize="10g"
	pkg_bootstrap=0
	ssh_wait=1
}

postcreate_minio1()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /root/prepare.sh
}
```

и находясь в каталоге с CBSDfile, выполнить команду:
> cbsd up

Также, в одном CBSDfile вы можете описывать неограниченное количество окружений, например:
```
CLOUD_URL="https://bravo-east.example.com"
CLOUD_KEY="ssh-ed25519 AAAA...xxx your@my.domain"

jail_minio1()
{
	imgsize="10g"
	pkg_bootstrap=0
	ssh_wait=1
}
postcreate_minio1()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /root/prepare.sh
}

jail_minio2()
{
	imgsize="10g"
	pkg_bootstrap=0
	ssh_wait=1
}
postcreate_minio2()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /root/prepare.sh
}

jail_minio3()
{
	imgsize="10g"
	pkg_bootstrap=0
	ssh_wait=1
}
postcreate_minio3()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /root/prepare.sh
}
```

и создавать контейнера, указывая имя: `cbsd up minio2`. Если вы запустите `cbsd up` беез указания имен окружений, вы создадите все контейнера массово, при этом если MyBee состоит из нескольких нод, каждый последующий контейнер по round-robin (по-умолчанию) алгоритму будет размещен на следующем по списку хосте.

## Удаление jail

Для удаления jail через CBSDfile служит команда `cbsd destroy [env]`, запущенная в каталоге с CBSDfile.

## Выполнение команд, копирование файлов, login

Находясь в каталоге с CBSDfile, вы можете выполнять различные операции с remote окружением, такие как:

|      команда     |  описание                                                                 |
| ---------------- | ------------------------------------------------------------------------- |
| cbsd login       | выполняет вызов SSH с ключем ~/.ssh/id_ed25519 для входа в контейнер      |
| cbsd jexec <cmd> | выполняет команду <cmd> в контейнере через SSH с ключем ~/.ssh/id_ed25519 |
| cbsd jscp        | копирование файлов, например:                                             |
|                  |  - от вашего окружение в jail: cbsd jscp <local_file> <env>:<remote_file> |
|                  |  - из jail в ваше окружение: cbsd jscp <env>:<remote_file> <local-file>   |

---

Дальше: [Работа с jail (Terraform)](jail_terraform.md)

