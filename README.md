# Инструкция по запуску анализа гистологических изображений в Google Colab

## Подготовка данных

- На ваш Google-диск поместите в *произвольное место*:
  1. Файл с изображением в формате `.mrxs`
  2. Сопутствующую файлу папку (должна иметь такое же имя, как файл `.mrxs` без расширения)
- В *корневую* папку Google-диска поместите файл с предобученной моделью `mobilenet.keras`, файл находится в этом репозитории

---

## 1. Открытие скрипта в Colab

1. Перейдите по ссылке:  
   https://colab.research.google.com/drive/1us09rsiOsFcVbXjbFng6xw6qePzrRVSA

2. Сохраните копию себе на диск:  
   `Файл → Сохранить копию на Диске`

---

## 2. Настройка переменных

В начале файла нужно указать три значения:

```python
SLIDE_FILE_ID = ""     # ID .mrxs-файла
SLIDE_FOLDER_ID = ""   # ID папки с .dat, .ini и другими файлами
SLIDE_NAME = ""        # Название файла, например: "slide-2024-07-04T08-38-39-R7-S15.mrxs"
```

---

## 3. Как получить ID файла и ID папки

1. Откройте ваш Google Диск и найдите нужный файл/папку.
2. Нажмите на троеточие
![image](https://github.com/user-attachments/assets/adbbd461-83f4-4896-9896-f3fa52528348)
и в открывшемся контекстном меню выберите Поделиться -> Открыть доступ


3. Скопируйте ссылку в открывшейся форме предоставления доступа
![image](https://github.com/user-attachments/assets/d20d52e2-424b-479a-87e1-169ec556aa08)

В результате для папки и файла у вас получатся ссылки соответствующего вида
![image](https://github.com/user-attachments/assets/a676e74b-4e0b-4c4d-aecd-a219f5b61502)
выделенная желтым часть и есть нужный ID.
## 4. Запуск

1. Проверьте, что все три переменные заполнены.
2. Убедитесь, что в меню `Среда выполнения → Сменить среду выполнения` выбран GPU.
3. Запустите выполнение программы с помощью `Среда выполнения → Выполнить все`

---

## 5. Результаты

В конце выполнения вы получите:

- `annot_slide.json` — файл аннотаций в формате GeoJSON (можно использовать в QuPath), аннотируются все тайлы, которые модель посчитала "больными"
- `heatmap_slide.png` — тепловая карта с отмеченными зонами
- Статистику: количество тайлов, размеры слайда, число положительных тайлов

Все результаты сохраняются в корневую директорию `/content/`. Вы можете скачать их на свой компьютер, либо же просмотреть прямо в интерфейсе Google Colab.

---

## Примечания
- Модель обучена на маленькой выборке слайдов, поэтому качество классификации может быть не очень хорошим.
- Обработка больших по размеру файлов может занимать значительное время.
- На данный момент модель обучена не находить патологии, а отличать "больные" тайлы (изображения 512x512) от "здоровых", поэтому вполне возможно, что "здоровые" тайлы с "больных" слайдов будут классифицированы как больные, т.е. неверно.
- Если модель не обнаруживает патологий — возможно, на изображении их действительно нет или нужно изменить значение порога `POS_THRESHOLD` в меньшую сторону.
- Если возникает ошибка `OpenSlideError` — проверьте, что загружены оба объекта: `.mrxs` и соответствующая ему папка с метаданными. При обработке некоторых слайдов могут возникать ошибки из-за того, что у них какие-то проблемы с форматом, это издержки проприетарного формата .mrxs.
