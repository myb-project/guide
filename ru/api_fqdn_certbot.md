# API FQDN и Certbot

По-умолчанию, MyBee API доступен через http://IP, однако при наличии внешнего IP адреса и доступности MyBee через Internet, вы можете прописать FQDN и автоматически выпустить [Let's Encrypt](https://letsencrypt.org) SSL сертификат через certbot.

Для этого, вам необходимо:

- Прописать IP адрес MyBee в DNS. Например: mybee.example.com. Перед переходом к следующему действию, убедитесь что прописанный хост указывает на IP адрес MyBee;
- Используя пункт меню '16) Set API FQDN ( + certbot )' ввести ваш FQDN (например: mybee.example.com);

Если вы все сделали правильно, при вводе FQDN, MyBee получит SSL сертификат и переведет MyBee API на использование https://, а также пропишен cron запись для своевременного обновления сертификата.

![api_fqdn_cert](https://user-images.githubusercontent.com/926409/178330856-03182cf3-5367-42c8-968b-7f0130baec2d.png)


Если вы хотите выключить или поменять FQDN, сначала выключите API FQDN, выбрав '16) Set API FQDN ( + certbot )' пункт повторно и ответив на вопрос о выключении FQDN/certbot утвердительно.


---

Дальше: [Работа с jails (curl)](jail_curl.md)
