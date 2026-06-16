# Лабораторная работа №3 — OSPF. Фильтрация

## Цель
Настроить OSPF в офисе Москва. Разделить сеть на зоны. Настроить фильтрацию между зонами.

## Задание
1. Маршрутизаторы R14-R15 должны находиться в зоне 0 - backbone
2. Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию
3. Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию
4. Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101

## Топология
![Схема OSPF](images/ospf.png)

## Конфигурации
- [R14](config/routers/R14.cfg) — Area 0 (Backbone)
- [R15](config/routers/R15.cfg) — Area 0 (Backbone) + фильтрация
- [R12](config/routers/R12.cfg) — Area 10 (NSSA)
- [R13](config/routers/R13.cfg) — Area 10 (NSSA)
- [R19](config/routers/R19.cfg) — Area 101 (Stub no-summary)
- [R20](config/routers/R20.cfg) — Area 102 (фильтрация)

## Проверка
```bash
R14# show ip ospf neighbor
R14# show ip route ospf
R19# show ip route
R20# show ip route
R13# show ip route

