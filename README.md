# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Иванова Ивана Варкравтовна
- РИ000024
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
 разработать оптимальный баланс для десяти уровней игры Dragon Picker

## Задание 1
### Предложите вариант изменения найденных переменных для 10 уровней в игре. Визуализируйте изменение уровня сложности в таблице. 
Всего в игре 4 переменных которые влияют непосредственно на сложность:
1. speed - скорость дракона
2. chanceDirection - шанс дракона изменить направление
3. leftRightDistance - ширина поля по которому может перемещаться дракон
4. timeBetweenEggDrops  - время между сбросами яиц

Чем больше скорость, шанс изменить направление и дистанция, тем сложнее игроку уследить и успеть за движениями дракона. Следовательно, чем больше эти три значения, тем выше итоговый уровень сложности. Затем, время между сбросами должно наоборот, уменьшаться, чтобы у игрока было все меньше времени отреагировать и поймать яйцо. Исходя из всех этих факторов, я выбрал сложность которая считается по формулe: difficulty = (speed * chanceDirection * leftRightDistance) / timeBetweenEggDrops
В идеальном мире мы бы увеличивали сложность логарифмически, но я считаю такой вариант больше подходит для игры головоломки, где игроку медленно дают информацию о механике а потом ставят челлендж побольше. Здесь же обойдемся увеличением значения сложности в примерно 2 раза,т.е. экспоненциально. За основу на 1 берутся значения данные в проекте 

### Таблица значений для первых 10 уровней:

https://docs.google.com/spreadsheets/d/1W6zo_WmD5wk_fI2BTAULWDXZogNzuwYpMtMbWCCUAqg/edit?usp=sharing

## Задание 2
### Создайте 10 сцен на Unity с изменяющимся уровнем сложности.
Проект на со всеми сценами находится по ссылке:
https://github.com/CerafimD/NewDragonpicker


## Задание 3
### Решение в 80+ баллов должно заполнять google-таблицу данными из Python. В Python данные также должны быть визуализированы.
Образец заполнения возьмем с Workshop2, в качестве визуализации используем график, который будем строить и выводить с помощью модуля "matplotlib" 
Скрипт получается следующий:

```py
import enum
import time
import gspread
import matplotlib.pyplot as plt
import numpy as np

class DifficultyIndex(enum.Enum):
    speed = 0
    egg_drop_time = 1
    right_left_distance = 2
    change_direction_chance = 3


class DifficultyData:
    values = [2, 2, 1, 0.01]
    increase_coef = 1,42
    column_names = ("B", "C", "D", "E")
    fields_count = 4
    difficulty = []


    def __init__(self):
        self.append_difficulty()


    def append_difficulty(self):
        self.difficulty.append(
            self.values[DifficultyIndex.speed.value] * self.values[DifficultyIndex.right_left_distance.value] /
            self.values[DifficultyIndex.egg_drop_time.value] * self.values[DifficultyIndex.change_direction_chance.value])


    def icrease_difficulty(self):
        for i in range(self.fields_count):
            self.values[i] *= self.increase_coef
        if self.values[DifficultIndex.right_left_distance.value] > 10:
            self.values[DifficultIndex.right_left_distance.value] = 10
        self.append_difficult_coef()


def update_sheet(row):
    for i in range(difficulty_data.fields_count):
        sh.sheet1.update(("A" + str(row)), row - 1)
        sh.sheet1.update((difficulty_data.column_names[i] + str(row)), difficulty_data.values[i])


gc = gspread.service_account(filename=''unitydatascience-400606-78cc91d889e0.json'')
sh = gc.open("воркшоп3 таблица 1")

sh.sheet1.update(('A1'), "Level number")
sh.sheet1.update(('B1'), "Dragon speed")
sh.sheet1.update(('C1'), "Egg drop time")
sh.sheet1.update(('D1'), "Right-left distance")
sh.sheet1.update(('E1'), "change_direction_chance")

difficulty_data = DifficultData()

for i in range(1, 11):
    time.sleep(5)
    if i != 1: difficulty_data.icrease_difficulty()
    update_sheet(i + 1)
fig, ax = plt.subplots()
x = np.arange(1, 11)
y = np.array(difficulty_data.difficulty_coef)
ax.plot(x, y)
ax.grid()
plt.xticks(range(1, 11, 1), range(1, 11, 1))
plt.yticks(range(1, 50, 2), range(1, 50, 2))
plt.xlabel("Level number")
plt.ylabel("Difficulty")
plt.show()

```

## Выводы
В данной работе я научился опознавать елементы, влияющие на баланс игры, а также понял как корректнее их изменять чтобы создать больше проблем игроку, но чтобы он при этом не ощущал от игры нечестности. Был создан проект на Гитхаюе для Юнити, NewDragonPicker на котором находятся 10 сцен с изменными величинами переменных, которые представляют из себя систему сложностей. Также закрепил изученное во втором воркшопе заполнив гугл таблицу данными для сложности дракона при помощи google API

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
