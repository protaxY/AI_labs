# Отчет по Лабораторной работе №1
# Многослойный персептрон (свой фреймворк)

| Студент | *Федоров Антон Сергеевич* |
|------|------|
| Группа  | *7* |

# Вспомогательные классы и функции
Нейронную сеть буду составлять по кусочкам, описывая составные части. Таким образом можно добиться модульности сети, что существенно упросит конструирование архитектуры и использования разных подходов. В частности, опишу линейный слой, три алгорима его обучения, softmax слой, четыре передаточные функции, три функции потерь. Названия методов указаны в ноутбуке. Все слои нейросети описаны однообразно: имеется конструктор, метод forward, отвечающий за обработку слоем входных результатов, и метод backward, необходимый для обучения и пересчета весов. Линейный слой имеет метод update, который старается уменьшить функцию потерь, согласно заданному алгоритму обучения.

# Класс Net
Подготовив необходимые составляющие, могу описать см класс нейронной сети. В качестве полей он имеет список слоев и функцию потерь. Подразумевается, что функция применяется только в самом конце к предсказанным результатам. Аналогично составляющим сети, имею методы forward и backward, последовательно вызывающие forward и backward у слоев соответственно. Обучение происходит посредством применения метода train_epoch. Он вызывает forward, backward и после подсчета результатов пересчитывает веса на линейных слоях, вызывая update, где это возможно. Для упрощения работы train_epoch спрятан в методе fit, который в цикле вызывает train_epoch. Также добавлены методы для упрощения анализа результатов. get_loss_acc возвращает результат функции потерь и точность модели на переданной выборке. fit_and_plot обучает модель, строя по результатам работы графики. draw_confusion_matrix_and_graphics строит матрицу ошибок.

# Обучение и результаты
Для начала, построю однослойную сеть. Обучение производилось на датасете MNIST. Использовалась однослойная сеть с передаточной функцией ReLU и функцией потерь CrossEntropyLoss. За алгоритм обучения был взят метод градиентного спуска в 10 итераций, learning rate был равен 0.000001.

![Результат однослойной сети](https://sun9-64.userapi.com/impf/115J1Gukrpt8So-yUYnSC7w9px3iYgAhpAlrKQ/1sAzTf0HBaE.jpg?size=1427x603&quality=96&sign=cae5d98e98736b299a66f4638cec6d01&type=album)
![Матрица ошибок однослойной сети](https://sun9-88.userapi.com/impf/xM_OtI4sPIuJMcVu0-nBPepFXmtYmhN5XLHzHg/zFyoK2cMIxw.jpg?size=410x408&quality=96&sign=80fe85f5698cbf6fc28f910ccde49d34&type=album)

Точность предсказаний 85%.

Теперь попробую трехслойную сеть. В промежуточных слоях буду использовать 100 нейронов. Остальные параметры оставлю теми же, только величину число итерацией обучения до 25.

![Результат многослойной сети](https://sun9-45.userapi.com/impf/5T2Oyq8bG5JiImf6XY-7BNcuFDzmg0ZKPo-z_g/EnDdGpbAi_4.jpg?size=1420x605&quality=96&sign=25056f03b307982e30c0c2ccf30f9df6&type=album)
![Матрица ошибок многослойной сети](https://sun1-89.userapi.com/impf/bv4kiR58l3b5oIdjkHQsek9_t22zfrANVB9MXA/nMAnfcNxGXE.jpg?size=410x412&quality=96&sign=f3a7c202820b1de28597908bab60010a&type=album)

Точность предсказаний стала немного лучше.

# Другие гиперпараметры
Менялись различные параметры многослойной сети, взятой в качестве baseline-а. В частности, изменялось число слоев, передаточная функция, функция потерь, число нейронов, алгоритм обучения. Подробные результаты каждого эксперимента приведены в ноутбуке.

Из экспериментов выяснилось, что изначальные параметры сети были довольно удачными. Лучший результат получился на сети с двумя слоями, сотней нейронов, передаточной функцией ReLU, функцией потерь CrossEntropyLoss и алгоритмом обучения NesterovAcceleratedGradient.

![Попытка улучшить результат](https://sun9-36.userapi.com/impf/ArQqtwnpkEO79wjrtx_ha2gkHPRKsM-okFN0Lg/2S9PkaoGLQ8.jpg?size=1405x605&quality=96&sign=9afbed32beb8c621eb1ef28acd4bd9cb&type=album)

# FashionMNIST
Использую baseline-многослойную сеть для анализа датасета FashionMNIST.

![FashionMNIST](https://sun9-52.userapi.com/impf/WNJqcy6ePOtCrVWbu4KC9CBu2OBaSBf2Ta-jVw/UQ09hGlhpmE.jpg?size=1416x605&quality=96&sign=9f37e09ab778a979b307e5e53028ef99&type=album)

Результат 77%. Попробую улучшить. Добавлю слой, увеличу количество нейронов до 150 и число итераций обучения до 50.

![улучшенный FashionMNIST](https://sun9-25.userapi.com/impf/gK8e2gwkFO8hHwei3jF9iYFwR8hmT6yoTjJOjQ/LUlWUGf3C-U.jpg?size=1435x600&quality=96&sign=1acb9875cb5919cc4549972891eb2549&type=album)

Результат стал 81%. Улучшение считаю успешным.