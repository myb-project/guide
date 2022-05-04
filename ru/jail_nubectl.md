# Работа с jail (nubectl)

## Описание

Работа с MyBee через [`curl`](jail_curl.md) демонстрациует в первую очередь [простоту базового API](api.md), но предполагается, что вы легким образом обернете API вызовы в любой фаворитный для вас язык программирования и сможете работать с частным облаком более комфортным способом.

Утилита `nubectl` - один из примеров подобной обертки, написанной в качестве примера на языке GO, что позволяет получить бинарный файл для всех современных ОС: Linux, BSD, MacOS и Windows.

Получить `nubectl` утилиту вы можете на титульной странице своей инсталляции MyBee или с сайта <a target="_blank" href="https://myb.convectix.com/download/">MyBee</a>.

Утилита `nubectl` является 'тонким клиентом' для MyBee и позволяет выполнять операции по созданию/уничтожению окружений, а также выполнять вход через SSH, в том числе с платформы Microsoft Windows.

Для работы, вам необходимо указать через переменные окружения `CLOUD_URL` и `CLOUD_KEY` (или аргументы командной строки: `-cloud_url` и `-ssh_key` соответственно) сервер MyBee и путь к файлу вашего публичного ключа.

## Создание jail

Для создания контейнера через `nubectl`, используйте аргумент `create container` с именем создаваемого окружения, например:
```
env CLOUD_URL="https://remote-api1.example.com" CLOUD_KEY=/usr/home/user/.ssh/authorized_keys /nubectl create container test1
```

Либо вы можете использовать параметры утилиты `nubectl`, например в Windows ОС (также, вам может понадобится параметр `-ssh_key` для указания пути к приватному ключу при входе в контейнер):
```
nubectl create container --cloud_key="c:\authorized_keys" --cloud_url=http://IP
nubectl ssh container --cloud_key="c:\authorized_keys" --cloud_url=http://IP [--ssh_key=c:\id_ed25519]
```

## Получение статуса

Для получения статуса своего 'namespace' (ваш публичный ключ), используйте `status`, например:
```
env CLOUD_URL="https://remote-api1.example.com" CLOUD_KEY=/usr/home/user/.ssh/authorized_keys nubectl status
{
  "servers": [
    {
      "instanceid": "test1",
      "type": "container",
      "profile": "native",
      "hw": "0/0/10g",
      "ssh_string": "ssh root@10.0.100.8 -p22"
    },
    {
      "instanceid": "minio3",
      "type": "container",
      "profile": "native",
      "hw": "0/0/10g",
      "ssh_string": "ssh root@10.0.100.4 -p22"
    },
    {
      "instanceid": "minio2",
      "type": "container",
      "profile": "native",
      "hw": "0/0/10g",
      "ssh_string": "ssh root@10.0.100.6 -p22"
    },
    {
      "instanceid": "minio1",
      "type": "container",
      "profile": "native",
      "hw": "0/0/10g",
      "ssh_string": "ssh root@10.0.100.3 -p22"
    }
  ],
  "clusters": [
    {
      "total_environment": 4,
      "total_cpus": 2,
      "total_ram": 2,
      "total_imgsize": 2
    }
  ]
}
```
Если вы хотите увидеть статус отдельного окружения, используйте `status ENV`:
```
env CLOUD_URL="https://remote-api1.example.com" CLOUD_KEY=/usr/home/user/.ssh/authorized_keys nubectl status test`
```


## Удаление jail

Для удаления, используйте команду `destroy`:
```
env CLOUD_URL="remote-api1.example.com" CLOUD_KEY=/usr/home/user/.ssh/authorized_keys nubectl destroy test1
```

## Вход в контейнер через SSH

Для входа, используйте команду `ssh`:
```
env CLOUD_URL="remote-api1.example.com" CLOUD_KEY=/usr/home/user/.ssh/authorized_keys nubectl ssh test1
```

## Demo

[![asciicast](https://asciinema.org/a/492150.svg)](https://asciinema.org/a/492150)

---

Дальше: [Работа с ВМ (nubectl)](bhyve_nubectl.md)
