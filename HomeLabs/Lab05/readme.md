# Лабораторная работа №5 — EIGRP

## Цель
Настроить EIGRP в офисе С.-Петербург. Использовать named EIGRP.

## Задание
1. В офисе С.-Петербург настроить EIGRP
2. R32 получает только маршрут по умолчанию
3. R16-17 анонсируют только суммарные префиксы
4. Использовать EIGRP named-mode для настройки сети
5. Настройка осуществляется одновременно для IPv4 и IPv6

## Топология
[Текстовая схема EIGRP](images/eigrp.txt)

## Конфигурации
- [R16](config/routers/R16.cfg) — суммаризация
- [R17](config/routers/R17.cfg) — суммаризация
- [R18](config/routers/R18.cfg) — redistribution static
- [R32](config/routers/R32.cfg) — фильтрация (только default)
- [SW9](config/switches/SW9.cfg) — EIGRP + VRRP
- [SW10](config/switches/SW10.cfg) — EIGRP + VRRP

## Проверка
```bash
R16# show ip eigrp neighbors
R16# show ip route eigrp
R32# show ip route
