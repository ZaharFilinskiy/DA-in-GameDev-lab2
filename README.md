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
Познакомиться с програмными средствами для организации передачи данных между инструментами google, Python и Unity.

## Задание 1
### Реализовать совместную работу и передачу данных в связке Python - Google-Sheets – Unity. При выполнении задания используйте видео-материалы и исходные данные, предоставленные преподавателя курса.
Ход работы:
В начале я с помощью подробного видеоурока, облачном сервисе google console, подключил API для работы с google sheets и google drive. Также скачал PyCharm и начал работу.

-Код Реализации записи в google sheets:
```py
import gspread
import numpy as np
gc = gspread.service_account(filename='unitydatascince-aee913d55043.json')
sh = gc.open("UnitySheets")
price = np.random.randint(2000, 10000, 11)
mon = list(range(1,11))
i = 0
while i <= len(mon):
    i += 1
    if i == 0:
        continue
    else:
        tempInf = ((price[i-1]-price[i-2])/price[i-2])*100
        tempInf = str(tempInf)
        tempInf = tempInf.replace('.',',')
        sh.sheet1.update(('A'+ str(i)),str(i))
        sh.sheet1.update(('B' + str(i)), str(price[i-1]))
        sh.sheet1.update(('C' + str(i)), str(tempInf))
        print(tempInf)
```
Настройка PyCharm заняла много времени но всё же оказалась успешна:
![2022-10-08_15-40-32](https://user-images.githubusercontent.com/114186148/194709135-d7a2a1bf-519f-4f86-8a07-4e8f823d65af.png)

-Код проекта на Unity получающий данные из google sheets и воспроизводящий аудио файл в зависимости от значения данных из табилицы:
```py
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip goodSpeak;
    public AudioClip normalSpeak;
    public AudioClip badSpeak;
    private AudioSource selectAudio;
    private Dictionary<string, float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i =1;
    // Start is called before the first frame update
    void Start()
    {
        StartCoroutine(GoogleSheets());
        
    }

    // Update is called once per frame
    void Update()
    {
        if (dataSet["Mon_"+ i.ToString()]<=10 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

         if (dataSet["Mon_"+ i.ToString()]>10 & dataSet["Mon_"+ i.ToString()]<100 & statusStart == false & i != dataSet.Count)
         {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
         }

         if (dataSet["Mon_"+ i.ToString()]>=100 & statusStart == false & i != dataSet.Count)
        {StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

    }
    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1fJITSyqBYrDygVX__jPD5FwKagdtWfSvwYWLjGdwpYE/values/Лист1?key=AIzaSyA8Vrl69AiJRjZTDXvnyhaDaJSZ88aF3Ns");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_"+selectRow[0]),float.Parse(selectRow[2]));

        }
        
    }
    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }

     IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }

     IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}
```
## Задание 2
### Реализовать запись в Google-таблицу набора данных, полученных с помощью линейной регрессии из лабораторной работы № 1.

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
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2.

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
