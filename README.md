# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Филинский Захар Евгеньевич
- РИ211121
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
Ознакомиться с основными операторами зыка Python на примере реализации линейной регрессии.

## Задание 1
### Написать программы Helo World на Python и Unity
Ход работы:
- Для работы на Python я выбрал компьютерную версию-Anacondaz. Установаил, разобрался и написал первую программу вывода сообщения Hello World. С Unity всё оказалось сложнее. Проделав все необходимые шаги для установки программы, я написал вывод сообщения на Vs code, связал его с main camera в unity и в итоге достиг своей цели.

- 1)Ссылка на скриншоты Python: Выполнение программы - https://github.com/ZaharFilinskiy/DA-in-GameDev-lab1/blob/15f1f56a2042f20330b8e64a7afbfa94793ad7ec/2022-09-24_16-39-16.png , Сохранение программы https://github.com/ZaharFilinskiy/DA-in-GameDev-lab1/blob/15f1f56a2042f20330b8e64a7afbfa94793ad7ec/2%20%D0%BA%D0%B0%D1%80%D1%82%D0%B8%D0%BD%D0%BA%D0%B0.png
- 2)Ссылки на скриншоты Unity : Выполнение программы в консоли - https://github.com/ZaharFilinskiy/DA-in-GameDev-lab1/blob/15f1f56a2042f20330b8e64a7afbfa94793ad7ec/unityy.png ; Текст на VS code - https://github.com/ZaharFilinskiy/DA-in-GameDev-lab1/blob/15f1f56a2042f20330b8e64a7afbfa94793ad7ec/%D1%82%D0%B5%D0%BA%D1%81%D1%82%20%D1%84%D0%B0%D0%B9%D0%BB%D0%B0.png

- Я не очень разобрался нужно ли прикладывать скрины только из репозитория своего, так-что продублировал со скринами с яндекс диска:

- 1)Ссылка на скриншоты Python: Выполнение программы - https://disk.yandex.ru/i/kjYmbIqHo41waQ , Сохранение программы https://disk.yandex.ru/i/ezamsw0aUKc-pA
- 2)Ссылки на скриншоты Unity : Выполнение программы в консоли - https://disk.yandex.ru/i/Sba-vdSdGxjV4w ; Текст на VS code - https://disk.yandex.ru/i/mMjhkEk7OpKsDQ

## Задание 2
### В разделе "ход работы" пошагово выполнить каждый пункт с описанием и примером реализации задачи по теме лабораторной работы.

- Выполнив каждый пункт второго задания лабораторной работы я увидел примеры линейной регресси, изучил новые функции и их применение в языке Python.

```py
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
x=[3, 21, 22, 34, 54, 34, 55, 67, 88, 99]
x=np.array(x)
y=[2, 22, 24, 65, 79, 82, 55, 130, 150, 199]
y=np.array(y)
plt.scatter(x,y)
```
- Выполнение программы: https://github.com/ZaharFilinskiy/DA-in-GameDev-lab1/blob/d9aacfc8ff07303e40f13ff4a308f9f30ed8d186/2022-09-26_18-41-47.png

```py
def model (a, b, x):
    return a*x + b


def loss_function(a, b, x, y):
    num = len(x)
    prediction = model (a,b,x)
    return (0.5/num) * (np.square(prediction-y)).sum()

def optimize(a,b,x,y):
    num=len(x)
    prediction = model(a,b,x)
    da = (1.0/num) * ( (prediction -y)*x).sum()
    db = (1.0/num) * ((prediction -y).sum())
    a = a - Lr*da
    b = b = Lr*db
    return a, b

def iterate(a, b, x, y, times) :
    for i in range(times):
        a,b = optimize(a,b,x,y)
    return a, b
```

```py
a = np.random.rand (1)
print(a)
b = np.random.rand(1)
print (b)
Lr = 0.000001

a,b = iterate(a,b,x,y,1)
prediction=model (a, b, x)
loss = loss_function(a, b, x, y)
print (a, b, loss)
plt.scatter(x, y)
plt.plot(x,prediction)
```
- Выполнение программы: https://github.com/ZaharFilinskiy/DA-in-GameDev-lab1/blob/d9aacfc8ff07303e40f13ff4a308f9f30ed8d186/2022-09-26_18-48-06.png

```py
a,b = iterate(a,b,x,y,1000)
prediction=model (a,b,x)
loss = loss_function(a, b, x, y)
print (a, b, loss)
plt.scatter(x,y)
plt. plot (x, prediction)
```
- Выполнение программы: https://github.com/ZaharFilinskiy/DA-in-GameDev-lab1/blob/d9aacfc8ff07303e40f13ff4a308f9f30ed8d186/2022-09-26_18-49-14.png

## Задание 3
### Должна ли величина loss стремиться к нулю при изменении исходных данных? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ.Какова роль параметра Lr? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ. В качестве эксперимента можете изменить значение параметра.

-Опытным путём я установил что ведечина loss не должна стремиться к нулю при изменении исходных данных. Если loss будет стремиться к нулю то график будет паралельным оси x или же острым углом к этой же оси. 

```py
a = np.random.rand (1)
print(a)
b = np.random.rand(1)
print (b)
Lr = 0.000001

a,b = iterate(a,b,x,y,1)
prediction=model (a, b, x)
loss = 0
print (a, b, loss)
plt.scatter(x, y)
plt.plot(x,prediction)

```
Итог : https://github.com/ZaharFilinskiy/DA-in-GameDev-lab1/blob/ea2c933e63cf9cbc5a1718a1495bfee4f18708dc/2022-09-26_19-29-03.png

-Параметр Lr помогает правильно выстравить кривую и соотвествущие ей оси x и y. Выступает в роли некого коэффициента, домножая на который система координат xy выстраивается в нужное положение.

```py
a = np.random.rand (1)
print(a)
b = np.random.rand(1)
print (b)
Lr = 0.01

a,b = iterate(a,b,x,y,1)
prediction=model (a, b, x)
loss = loss_function(a, b, x, y)
print (a, b, loss)
plt.scatter(x, y)
plt.plot(x,prediction)
```

-Пример что будет если изменить коэффициент: https://github.com/ZaharFilinskiy/DA-in-GameDev-lab1/blob/fabc8f2eba84418b48e07a94a3c5d635a6ba6724/2022-09-26_19-40-57.png

## Выводы

В ходе выполнения данной лабораторной работы я разобрался с установкой необходимого ПО и также смог настроить его. Написал свою первую программу на unity, anacondaz и поближе познакомился с интерфейсом и функционалом программ. Помимо этого, глубже изучил понятие линейной регрессии на языке python.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
