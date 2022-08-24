# useful-bash-commands

### OS architecture
``` bash
dpkg --print-architecture
uname -a
lsb_release -a
```

# Install specific version of node js
``` bash
sudo npm cache clean -f
sudo npm install -g n
sudo n stable
or 
sudo n 14.18.1
```

# Авто обновление браузера при разработке
Enbale live server on VSCode 

<img src="https://user-images.githubusercontent.com/89765480/184475711-fe4d7636-1a12-41d4-8f6d-96145907f5b9.png" width="900px" height="300px">


Enable auto save on VS code
File -> Autosave (after delay)

Заполнение матрицы спиралью
```Python
# Принимаем параметры матрицы
# Альтернативный код: n, m = [int(i) for i in input().split()]
n, m = map(int, input().split())

# Создаем нулевую матрицу размером 'n x m'
matrix = [[0] * m for _ in range(n)]

# Задаем параметры направления смещения
# row_dir - смещение по строкам
# col_dir - смещение по столбцам
# Как видно в нашем случае мы двинемся горизонтально вправо
row_dir, col_dir = 0, 1

# Задаем координаты стартовой ячейки
row, column = 0, 0

# Цикл-счетчик порядкового номера от 1 до n * m
for counter in range(1, n * m + 1):
    # Присваиваем текущей ячейке номер счетчика
    matrix[row][column] = counter
    # Проверяем, не пора ли сделать поворот?
    # Проблема выхода за пределы матрицы решается делением с остатком
    if matrix[(row + row_dir) % n][(column + col_dir) % m]:
        # Поворачиваем направление смещения по часовой стрелке на 90 градусов
        row_dir, col_dir = col_dir, -row_dir
    # Задаем координаты следующей ячейки в соответствии с направлением смещения
    row += row_dir
    column += col_dir

# Распечатываем заполненную матрицу
for row in matrix:
    # переводим матрицу в строковый формат для эстетики отображения
    print(*(f'{e:<3}' for e in row), sep='')
```
