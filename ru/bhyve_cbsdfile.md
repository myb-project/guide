# Работа с bhyve VM (CBSDfile)

## Описание

Если вы используетет платформу FreeBSD, помимо работы с API через [`curl`](bhyve_curl.md) и [`nubectl`](bhyve_nubectl.md), у вас есть еще одна возможность работы с MyBee - через CBSDfile. Этот файл обрабатывается командой `cbsd` и по этой причине,
на данный момент недоступен для других платформ.

Формат файла, описывающий контейнер для работы через API полностью совпадает с синтаксисом <a target="_blank" href="https://www.bsdstore.ru/en/cbsdfile.html">CBSDfile</a>, работающим локально, но добавляются переменные
`CLOUD_URL` (указывает на сервер MyBee) и `CLOUD_KEY` (указывает путь к pubkey).

## Создание ВМ

Пример `CBSDfile` для создания виртуальноый машины vm1:

```
CLOUD_URL="https://rio-west.example.com"
CLOUD_KEY="ssh-ed25519 AAAA...xxx your@my.domain"

bhyve_vm1()
{
	image="centos7"
	vm_cpus="1"
	vm_ram="1g"
	imgsize="10g"
	ssh_wait=0
}
```

Вы можете захотеть при создании ВМ скопировать в окружение произвольный файл (например: prepare.sh) и выполнить его. Например, контейнер с именем `minio1`:
```
CLOUD_URL="https://bravo-east.example.com"
CLOUD_KEY="ssh-ed25519 AAAA...xxx your@my.domain"

bhyve_minio1()
{
	image="centos7"
	vm_cpus="1"
	vm_ram="1g"
	imgsize="10g"
	ssh_wait=1
}

postcreate_minio1()
{
	bscp prepare.sh ${jname}:prepare.sh
	bexec /home/centos/prepare.sh
}
```

и находясь в каталоге с CBSDfile, выполнить команду:
> sudo cbsd up

:bangbang: | :information_source: Обратите внимание! В отличие от работы с jail, где команды jscp/jexec работают от пользователя 'root', в случае с VM вы имеете доступ в непривелигированного пользователя, имя которого зависит от дистрибутива (имя для работы с пользователем вы можете увидеть в статусе вашего окружения). Таким образом, копируя файл внутрь контейнера, вы можете его сохранить или в каталоге пользователя ( /home/<user> ), или в общедоступной для записи директории (например /tmp). Аналогично работает bexec - если вам необходимо выполнить команду от пользователя 'root', добавляйте префикс sudo, например: `bexec sudo whoami`
:---: | :---

Также, в одном CBSDfile вы можете описывать неограниченное количество окружений, например:
```
CLOUD_URL="https://bravo-east.example.com"
CLOUD_KEY="ssh-ed25519 AAAA...xxx your@my.domain"

bhyve_minio1()
{
	image="centos7"
	vm_cpus="1"
	vm_ram="1g"
	imgsize="10g"
	ssh_wait=1

}
postcreate_minio1()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /home/centos/prepare.sh
}

bhyve_minio2()
{
	image="centos7"
	vm_cpus="1"
	vm_ram="1g"
	imgsize="10g"
	ssh_wait=1
}
postcreate_minio2()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /home/centos/prepare.sh
}

bhyve_minio3()
{
	image="centos7"
	vm_cpus="1"
	vm_ram="1g"
	imgsize="10g"
	ssh_wait=1
}
postcreate_minio3()
{
	jscp prepare.sh ${jname}:prepare.sh
	jexec /home/centos/prepare.sh
}
```

и создавать виртуальную машину, указывая имя: `sudo cbsd up minio2`. Если вы запустите `sudo cbsd up` беез указания имен окружений, вы создадите все виртуальные машины массово, при этом если MyBee состоит из нескольких нод, каждая последующая ВМ по round-robin (по-умолчанию) алгоритму будет размещена на следующем по списку хосте.

## Удаление ВМ

Для удаления ВМ через CBSDfile служит команда `sudo cbsd destroy [env]`, запущенная в каталоге с CBSDfile.

## Выполнение команд, копирование файлов, login, статус

Находясь в каталоге с CBSDfile, вы можете выполнять различные операции с remote окружением, такие как:

|      команда      |  описание                                                                 |
| ----------------- | ------------------------------------------------------------------------- |
| cbsd login        | выполняет вызов SSH с ключем ~/.ssh/id_ed25519 для входа в ВМ             |
| cbsd bexec <cmd>  | выполняет команду <cmd> в ВМ через SSH с ключем ~/.ssh/id_ed25519         |
| cbsd bscp         | копирование файлов, например:                                             |
|                   |  - от вашего окружение в ВМ: cbsd bscp <local_file> <env>:<remote_file>   |
|                   |  - из ВМ в ваше окружение: cbsd bscp <env>:<remote_file> <local-file>     |
| cbsd status [env] | показать статус окружений                                                 |

---

Дальше: [Работа с bhyve (Terraform)](bhyve_terraform.md)

