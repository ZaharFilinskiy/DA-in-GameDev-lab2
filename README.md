# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
- Филинский Захар Евгеньевич
- РИ211121
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | # | 20 |

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
В начале я с помощью подробного видеоурока в облачном сервисе google console подключил API для работы с google sheets и google drive. Также скачал PyCharm и начал работу.

-Код Реализации записи в google sheets на PyCharm:
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
-Настройка PyCharm заняла много времени но всё же оказалась успешна:
![2022-10-08_15-40-32](https://user-images.githubusercontent.com/114186148/194709135-d7a2a1bf-519f-4f86-8a07-4e8f823d65af.png)

-Результаты в google sheets:
![2022-10-08_15-40-40](https://user-images.githubusercontent.com/114186148/194709344-b3367585-7e11-4480-8ca6-900af55327d7.png)

Далее глянул видеоуроки на unity, ознакомился с "материалами для лабораторной работы" и приступил к работе.

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
Результаты Unity:
![2022-10-08_18-14-57](https://user-images.githubusercontent.com/114186148/194709497-64704880-294d-49df-946a-1da67dfa2042.png)

Совместно с Гугл Таблицей:
![2022-10-08_18-15-17](https://user-images.githubusercontent.com/114186148/194709609-1b041d2a-9dc9-468e-a6d8-5358d042004e.png)


## Задание 2
### Реализовать запись в Google-таблицу набора данных, полученных с помощью линейной регрессии из лабораторной работы № 1.

В данном задании я не совсем понял какие именно данные нужно было занести в гугл таблицу и взял данные координат точек по x и y, крайней лабораторной работы, посчитав ,что в любом случае это тоже практика. Немного деформировал код из предыдущего задания и достиг результата. 
Также разобрался с кодом и системой записи данных в google sheets.

```py
import gspread
import numpy as np
gc = gspread.service_account(filename='unitydatascince-aee913d55043.json')
sh = gc.open("UnitySheets")
mon = list(range(1,11))

x=[0, 21, 22, 29, 54, 34, 55, 67, 88, 99, 100]
y=[1, 22, 24, 70, 79, 82, 55, 140, 150, 200, 300]
i = 0
while i <= len(mon):
    i += 1
    if i == 0:
        continue
    else:

        sh.sheet1.update(('A'+ str(i)),str(i))
        sh.sheet1.update(('B' + str(i)), str(x[i-1]))
        sh.sheet1.update(('C' + str(i)), str(y[i-1]))
```
-Результат в таблице: 

![2022-10-09_23-47-01](https://user-images.githubusercontent.com/114186148/194774257-c0c547ef-6d5c-4418-8c6c-dce99ccefa38.png)

## Задание 3
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2.


## Выводы

В процессе выполнения лабораторной работы, мной были интегрированы между собой инструменты unity, google и python благодаря чему я научился передавать данные между ними, глубже погрузился в мир игровой валюты благодаря новым знаниям, решил лабораторные задания, тем самым закрепив новые умения.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
