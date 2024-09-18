# Данные
В данном проекте мы будем использовать набор изображений цифр и математических символов (+, -, *, /), а также букв x, y, z и пробельный символ размером 150x150 пикселей.
Перед использованием в качестве входных данных для нейросети, изображения были нормализованы путем деления каждого пикселя на 255. Это позволило привести значения пикселей к диапазону от 0 до 1, что является более удобным форматом для обработки нейронной сетью. Это упростит архитектуру модели и сделает ее более эффективной при обработке входных данных.
Всего в наборе данных содержится 18 классов объектов - цифры от 0 до 9, а также 4 математических символа, 3 буквы латинского алфавита и пробел. Общий размер датасета составляет 9000 картинок. Такой объем данных, содержащий разнообразные образцы рукописного ввода, позволит эффективно обучить нейронную сеть распознавать широкий спектр рукописных цифр и математических знаков. 

# Архитектура модели
В своем проекте я использую последовательную модель (Sequential model) из библиотеки Keras. Она состоит из следующих основных компонентов:

1. Входной слой:
   - Входной размер изображения: 150x150 пикселей, 1 канал 
2. Сверточные слои:
   - Первый блок: два последовательных сверточных слоя с ядром 5x5 и 3x3 пикселей соответственно, активацией ReLU и 32 фильтрами каждый.
   - Второй блок: два сверточных слоя с ядром 3x3, 64 фильтрами и активацией ReLU.
   - Третий блок: два сверточных слоя с ядром 3x3, 96 фильтрами и активацией ReLU.
   - Четвертый блок: два сверточных слоя с ядром 3x3, 128 фильтрами и активацией ReLU.
3. Pooling слои:
   - После каждого блока сверточных слоев применяется операция максимального pooling с размером окна 2x2.
4. Полносвязные слои:
   - Слой Flatten преобразует выходы сверточных слоев в одномерный вектор.
   - Слой Dropout с коэффициентом 0.5 для регуляризации.
   - Финальный полносвязный слой с 18 выходными нейронами и активацией Softmax для классификации на 10 цифр и 8 символов.

# Обучение
1. Разделение данных:
   Используется ImageDataGenerator из Keras для разделения всего набора данных на тренировочную и валидационную выборки. Это важный шаг, который позволяет корректно оценивать качество обучения модели.
3. Параметры обучения:
   Я задаю размер пакета (batch_size) равным 32 и фиксирую случайный затравочный параметр (seed) для воспроизводимости результатов. 
5. Функция потерь и оптимизатор:
   Я компилирую модель, используя функцию потерь categorical_crossentropy для многоклассовой классификации и оптимизатор Adam с начальной скоростью обучения 0.001.
7. Обучение и валидация:
   Я вызываю функцию fit(), передавая ей тренировочный и валидационный генераторы данных. Модель будет обучаться в течение 50 эпох, оценивая качество на валидационной выборке.
9. Callback-функции:
    Также применяются две важные callback-функции:
   - PlotLossesKeras() для визуализации динамики обучения и валидации в реальном времени.
   - ModelCheckpoint() для сохранения лучших весов модели во время обучения.

# Приложение
Для того, чтобы сделать обученную нейронную сеть доступной для пользователей, я решил создать простое веб-приложение с использованием библиотеки Streamlit. Это позволит легко интегрировать модель распознавания рукописного ввода в интерактивный интерфейс.
В моем приложении я реализую следующие основные функциональные возможности:
1. Загрузка обученной модели: При запуске приложения я загружаю сохраненные веса нейронной сети, чтобы иметь возможность использовать ее для распознавания.
2. Пользовательский ввод: Я предоставляю пользователю интерфейс для рисования рукописного текста или символов. Для этого используется Streamlit-компоненты, например, canvas, который позволяет пользователю рисовать мышью или пальцем на сенсорном экране.
3. Распознавание ввода: После того, как пользователь нарисовал свой рукописный пример, мы передаем изображение в обученную нейронную сеть. Модель выполняет классификацию и возвращает распознанные цифры и математические символы.
4. Вычисление математического выражения: Используя распознанные цифры и знаки анализируется, является ли введенный пример простым математическим выражением. Если да, то вычисляется и отображается его результат.
5. Визуализация: Для наглядности я отображаю в приложении изображение, на котором пользователь нарисовал свой пример, а также вывожу распознанные цифры и математические символы, а также результат вычисления, если это была простая математическая задача.
