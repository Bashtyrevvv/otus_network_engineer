# Лабораторная работа №2 — PBR (Policy-Based Routing)

## 🎯 Цель
Настроить политику маршрутизации в офисе Чокурдах. Распределить трафик между 2 линками.

## 📋 Задание
1. Настроить политику маршрутизации в офисе Чокурдах
2. Распределить трафик между двумя линками с провайдером
3. Настроить отслеживание линка через технологию IP SLA (только для IPv4)
4. Настроить для офиса Лабытнанги маршрут по-умолчанию

---

## 📊 Топология
![Схема PBR](images/pbr.png)

---

## 🔧 Выполненные задачи

### 1. Настройка политики маршрутизации в Чокурдахе

На R28 настроены маршруты по-умолчанию через два линка:

ip route 0.0.0.0 0.0.0.0 100.0.0.13 25 name "to R25 (ISP)" track 1
ip route 0.0.0.0 0.0.0.0 100.0.0.25 50 name "to R26 (ISP)" track 2

### 2. Распределение трафика между линками

Трафик из VLAN 100 направляется через R26 с помощью PBR:

ip access-list extended PBR-TRAFFICVLAN100-ACL
remark ===[ Traffic from VLAN 100 ]===
permit ip 10.67.1.0 0.0.0.127 any

route-map PBR-TRAFFIC permit 10
match ip address PBR-TRAFFICVLAN100-ACL
set ip next-hop verify-availability 100.0.0.25 10 track 2
set ip next-hop verify-availability 100.0.0.13 20 track 1

interface Ethernet0/2.100
ip policy route-map PBR-TRAFFIC

### 3. Отслеживание линков (IP SLA)

Настроен IP SLA для отслеживания доступности линков:

ip sla 1
icmp-jitter 100.0.0.13 source-ip 100.0.0.14 num-packets 7
frequency 7
ip sla schedule 1 life forever start-time now

ip sla 2
icmp-jitter 100.0.0.25 source-ip 100.0.0.26 num-packets 7
frequency 7
ip sla schedule 2 life forever start-time now

track 1 ip sla 1 reachability
track 2 ip sla 2 reachability

### 4. Маршрут по-умолчанию для Лабытнанги

На R27 настроен маршрут по-умолчанию:

ip route 0.0.0.0 0.0.0.0 100.0.0.17

---

## 📁 Конфигурации

- [R28 — Чокурдах (PBR + IP SLA)](config/routers/R28.cfg)
- [R27 — Лабытнанги (маршрут по-умолчанию)](config/routers/R27.cfg)

---

## 🔧 Проверка

```bash
# Проверка IP SLA
R28# show ip sla statistics

# Проверка PBR
R28# show route-map
R28# show ip policy

# Проверка маршрутов
R28# show ip route
R27# show ip route
