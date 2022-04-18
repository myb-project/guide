# ACL и безопасность

## Описание

MyBee дистрибутив на данный момент позиционируется как инструмент, применяемый в 'Trusted environments' и частной инфраструктуре и по этой причине, не должен быть использован в публичных сетях общего пользования. Тем не менее, для ограничений использования API вы можете через консоль администратора настроить следующие ограничения:

1) Ввести 'whitelist' по IP адресам, которые могут работать с API. Все IP адреса, которые НЕ присутствуют в списке, будут получать 'permission deny' при работе с API;
2) Ввести 'whitelist' для 'pubkey', которые могут работать с API. Все 'pubkey' (и соответственно, CID-токены от них), которые НЕ присутствуют в списке, будут получеть 'permission deny' при работе с API;
3) Установите и проксируйте перед MyB API собственный балансер, на котором вы можете сделать дополнительную защиту, сертификаты и тд.;

## ACL IP

Для включения фильтра по IP адресам, используйте доступ в консоль администратора


---

Дальше: [Базовые эндпоинты API](api.md)