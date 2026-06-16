# Лабораторная работа №8 — BGP. Фильтрация

## Цель
Настроить фильтрацию в офисе Москва. Настроить фильтрацию в офисе С.-Петербург.

## Задание
1. Москва: фильтрация транзитного трафика (AS-Path filter)
2. С.-Петербург: фильтрация транзитного трафика (Prefix-list)
3. Киторн → Москва: только default route
4. Ламас → Москва: default route + префикс СПб
5. Полная IP связность всех сетей

## Топология
[Текстовая схема фильтрации](images/filter.txt)

## Конфигурации
- [R14](config/routers/R14.cfg) — AS-Path filter
- [R15](config/routers/R15.cfg) — AS-Path filter
- [R22](config/routers/R22.cfg) — Киторн (prefix-list)
- [R21](config/routers/R21.cfg) — Ламас (prefix-list)
- [R18](config/routers/R18.cfg) — Prefix-list

## Проверка
```bash
R14# show ip bgp
R18# show ip bgp
R22# show ip bgp
R21# show ip bgp
