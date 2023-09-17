# Домашнее задание к занятию 4. «Оркестрация группой Docker-контейнеров на примере Docker Compose»

## Как сдавать задания

Обязательны к выполнению задачи без звёздочки. Их нужно выполнить, чтобы получить зачёт и диплом о профессиональной переподготовке.

Задачи со звёздочкой (*) — это дополнительные задачи и/или задачи повышенной сложности. Их выполнять не обязательно, но они помогут вам глубже понять тему.

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

Любые вопросы по решению задач задавайте в чате учебной группы.

---


## Важно

Перед отправкой работы на проверку удаляйте неиспользуемые ресурсы.
Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.

Подробные рекомендации [здесь](https://github.com/netology-code/virt-homeworks/blob/virt-11/r/README.md).

---

## Задача 1

Создайте собственный образ любой операционной системы (например, debian-11) с помощью Packer версии 1.5.0 ([инструкция](https://cloud.yandex.ru/docs/tutorials/infrastructure-management/packer-quickstart)).

Чтобы получить зачёт, вам нужно предоставить скриншот страницы с созданным образом из личного кабинета YandexCloud.

![screenshot](https://i.ibb.co/w6WPyF2/Screenshot-from-2023-09-17-22-29-01.png)

## Задача 2

**2.1.** Создайте вашу первую виртуальную машину в YandexCloud с помощью web-интерфейса YandexCloud.

![screenshot](https://i.ibb.co/MnqCkd1/Screenshot-from-2023-09-17-14-37-00.png)        

**2.2.*** **(Необязательное задание)**      
Создайте вашу первую виртуальную машину в YandexCloud с помощью Terraform (вместо использования веб-интерфейса YandexCloud).
Используйте Terraform-код в директории ([src/terraform](https://github.com/netology-group/virt-homeworks/tree/virt-11/05-virt-04-docker-compose/src/terraform)).

Чтобы получить зачёт, вам нужно предоставить вывод команды terraform apply и страницы свойств, созданной ВМ из личного кабинета YandexCloud.

![screenshot](https://i.ibb.co/Y35yLMV/Screenshot-from-2023-09-17-17-16-27.png)  
![screenshot](https://i.ibb.co/jTvFzPh/Screenshot-from-2023-09-17-22-27-54.png)  

## Задача 3

С помощью Ansible и Docker Compose разверните на виртуальной машине из предыдущего задания систему мониторинга на основе Prometheus/Grafana.
Используйте Ansible-код в директории ([src/ansible](https://github.com/netology-group/virt-homeworks/tree/virt-11/05-virt-04-docker-compose/src/ansible)).

Чтобы получить зачёт, вам нужно предоставить вывод команды "docker ps" , все контейнеры, описанные в [docker-compose](https://github.com/netology-group/virt-homeworks/blob/virt-11/05-virt-04-docker-compose/src/ansible/stack/docker-compose.yaml),  должны быть в статусе "Up".

```bash
wolin@wolinubuntu:~$ ssh -i ~/.ssh/id_rsa centos@62.84.118.4
[centos@node01 ~]$ sudo docker ps
CONTAINER ID   IMAGE                              COMMAND                  CREATED          STATUS                    PORTS                                                                              NAMES
eee8c7312aff   prom/node-exporter:v0.18.1         "/bin/node_exporter …"   18 minutes ago   Up 18 minutes             9100/tcp                                                                           nodeexporter
efc22f3854d5   prom/pushgateway:v1.2.0            "/bin/pushgateway"       18 minutes ago   Up 18 minutes             9091/tcp                                                                           pushgateway
d1612fb3c2cc   stefanprodan/caddy                 "/sbin/tini -- caddy…"   18 minutes ago   Up 18 minutes             0.0.0.0:3000->3000/tcp, 0.0.0.0:9090-9091->9090-9091/tcp, 0.0.0.0:9093->9093/tcp   caddy
9fd014fc93ef   prom/alertmanager:v0.20.0          "/bin/alertmanager -…"   18 minutes ago   Up 18 minutes             9093/tcp                                                                           alertmanager
f017af3fda7c   gcr.io/cadvisor/cadvisor:v0.47.0   "/usr/bin/cadvisor -…"   18 minutes ago   Up 18 minutes (healthy)   8080/tcp                                                                           cadvisor
6d3bdb5b8faa   grafana/grafana:7.4.2              "/run.sh"                18 minutes ago   Up 18 minutes             3000/tcp                                                                           grafana
ae98b8f6bb96   prom/prometheus:v2.17.1            "/bin/prometheus --c…"   18 minutes ago   Up 18 minutes             9090/tcp                                                                           prometheus
[centos@node01 ~]$ 

```

## Задача 4

1. Откройте веб-браузер, зайдите на страницу http://<внешний_ip_адрес_вашей_ВМ>:3000.
2. Используйте для авторизации логин и пароль из [.env-file](https://github.com/netology-group/virt-homeworks/blob/virt-11/05-virt-04-docker-compose/src/ansible/stack/.env).
3. Изучите доступный интерфейс, найдите в интерфейсе автоматически созданные docker-compose-панели с графиками([dashboards](https://grafana.com/docs/grafana/latest/dashboards/use-dashboards/)).
4. Подождите 5-10 минут, чтобы система мониторинга успела накопить данные.

Чтобы получить зачёт, предоставьте: 

- скриншот работающего веб-интерфейса Grafana с текущими метриками
<p align="center">
  <img width="1200" height="600" src="https://i.ibb.co/pRsCdNS/Screenshot-from-2023-09-17-22-26-47.png">
</p>

## Задача 5 (*)

Создайте вторую ВМ и подключите её к мониторингу, развёрнутому на первом сервере.

Чтобы получить зачёт, предоставьте:

- скриншот из Grafana, на котором будут отображаться метрики добавленного вами сервера.


