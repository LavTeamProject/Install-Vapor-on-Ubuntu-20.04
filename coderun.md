4. [Ход конём](https://coderun.yandex.ru/problem/knight-move)
  
**Условие задачи:**
Дана прямоугольная доска N×M (N строк и M столбцов). В левом верхнем углу находится шахматный конь, которого необходимо переместить в правый нижний угол доски. В данной задаче конь может перемещаться на две клетки вниз и одну клетку вправо или на одну клетку вниз и две клетки вправо. Необходимо определить, сколько существует различных маршрутов, ведущих из левого верхнего в правый нижний угол.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем размеры доски N и M
let input = readLine()!.split(separator: " ").map { Int($0)! }
let n = input[0] // Количество строк
let m = input[1] // Количество столбцов

// Создаем двумерный массив для хранения количества способов добраться до каждой клетки
// dp[i][j] будет содержать количество маршрейсов до клетки (i, j)
var dp = Array(repeating: Array(repeating: 0, count: m + 1), count: n + 1)

// Начальная позиция: левый верхний угол (1, 1)
dp[1][1] = 1

// Проходим по всем клеткам доски
for i in 1...n {
    for j in 1...m {
        // Если в текущую клетку можно попасть, обновляем возможные следующие позиции
        if dp[i][j] > 0 {
            // Ход 1: две клетки вниз и одну вправо
            let nextRow1 = i + 2
            let nextCol1 = j + 1
            if nextRow1 <= n && nextCol1 <= m {
                dp[nextRow1][nextCol1] += dp[i][j]
            }
            
            // Ход 2: одну клетку вниз и две вправо
            let nextRow2 = i + 1
            let nextCol2 = j + 2
            if nextRow2 <= n && nextCol2 <= m {
                dp[nextRow2][nextCol2] += dp[i][j]
            }
        }
    }
}

// Выводим количество способов добраться до правого нижнего угла (n, m)
print(dp[n][m])
```

---
12. [Длина кратчайшего пути](https://coderun.yandex.ru/problem/shortest-path-length)
  
**Условие задачи:**
Дан неориентированный граф. Найдите длину минимального пути между двумя вершинами.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем количество вершин
let n = Int(readLine()!)!

// Считываем матрицу смежности
var graph: [[Bool]] = []
for _ in 0..<n {
    let row = readLine()!.split(separator: " ").map { Int($0)! == 1 }
    graph.append(row)
}

// Считываем начальную и конечную вершины
let vertices = readLine()!.split(separator: " ").map { Int($0)! - 1 } // Переводим в 0-индексацию
let start = vertices[0]
let end = vertices[1]

// Реализуем BFS для поиска кратчайшего пути
var queue: [Int] = [start]
var distances: [Int] = Array(repeating: -1, count: n)
distances[start] = 0

while !queue.isEmpty {
    let current = queue.removeFirst()
    
    // Если дошли до конечной вершины, выходим
    if current == end {
        break
    }
    
    // Проверяем всех соседей
    for neighbor in 0..<n {
        if graph[current][neighbor] && distances[neighbor] == -1 {
            distances[neighbor] = distances[current] + 1
            queue.append(neighbor)
        }
    }
}

print(distances[end])
```

---
14. [Блохи](https://coderun.yandex.ru/problem/fleas)
15. [Путь спелеолога](https://coderun.yandex.ru/problem/speleologist-way)
28. [НВП с восстановлением ответа](https://coderun.yandex.ru/problem/nvp-with-response-recovery)
33. [Расстояние по Левенштейну](https://coderun.yandex.ru/problem/levenstein-distance)
34. [Космический мусорщик](https://coderun.yandex.ru/problem/space-scavenger)
39. [Откуда достижима первая вершина](https://coderun.yandex.ru/problem/first-vertex)
43. [Два коня](https://coderun.yandex.ru/problem/two-horses)
44. [Числа](https://coderun.yandex.ru/problem/numbers)
51. [Номер появления слова](https://coderun.yandex.ru/problem/word-appearance-number)
52. [Словарь синонимов](https://coderun.yandex.ru/problem/dictionary-synonyms)
54. [Полиглоты](https://coderun.yandex.ru/problem/polyglots)
55. [Злые свинки](https://coderun.yandex.ru/problem/angry-pigs)
57. [Инопланетный геном](https://coderun.yandex.ru/problem/alien-genome)
58. [OpenCalculator](https://coderun.yandex.ru/problem/open-calculator)
60. [Кубики](https://coderun.yandex.ru/problem/cubes)
61. [Пересечение множеств](https://coderun.yandex.ru/problem/intersection-sets)
62. [Количество различных чисел](https://coderun.yandex.ru/problem/number-different-numbers)
64. [Сапёр](https://coderun.yandex.ru/problem/sapper)
69. [Больше своих соседей](https://coderun.yandex.ru/problem/more-your-neighbors)
70. [Ближайшее число](https://coderun.yandex.ru/problem/nearest-number)
71. [Определить вид последовательности](https://coderun.yandex.ru/problem/determine-type-sequence)
72. [Возрастает ли список?](https://coderun.yandex.ru/problem/list-growing)
74. [Узник замка Иф](https://coderun.yandex.ru/problem/castle-if)
75. [Метро](https://coderun.yandex.ru/problem/metro)
80. [Телефонные номера](https://coderun.yandex.ru/problem/phone-numbers)
81. [Треугольник](https://coderun.yandex.ru/problem/triangle)
82. [Кондиционер](https://coderun.yandex.ru/problem/conditioner)
83. [Клавиатура](https://coderun.yandex.ru/problem/keyboard)
84. [Пирамида](https://coderun.yandex.ru/problem/pyramid)
86. [Банковские счета](https://coderun.yandex.ru/problem/bank-accounts)
88. [Контрольная по ударениям](https://coderun.yandex.ru/problem/control-accent)
91. [Сумма номеров](https://coderun.yandex.ru/problem/sum-of-numbers)
93. [Город Че](https://coderun.yandex.ru/problem/city-of-che)
96. [Подстрока](https://coderun.yandex.ru/problem/substring)
99. [Двоичный поиск](https://coderun.yandex.ru/problem/binary-search)
100. [Приближенный двоичный поиск](https://coderun.yandex.ru/problem/bpproximate-binary-search)
101. [Дипломы](https://coderun.yandex.ru/problem/diplomas)
103. [Расстановка ноутбуков](https://coderun.yandex.ru/problem/arrangement-laptops)
106. [Провода](https://coderun.yandex.ru/problem/wires)
108. [Медиана объединения](https://coderun.yandex.ru/problem/median-union)
114. [Кассы](https://coderun.yandex.ru/problem/cash-registers)
120. [Высота дерева](https://coderun.yandex.ru/problem/tree-height)
121. [Глубина добавляемых элементов](https://coderun.yandex.ru/problem/depth-added-elements)
124. [Вывод листьев](https://coderun.yandex.ru/problem/leaf-conclusion)
125. [Вывод развилок](https://coderun.yandex.ru/problem/fork-conclusion)
126. [Вывод веток](https://coderun.yandex.ru/problem/branches-conclusion)
127. [АВЛ-сбалансированность](https://coderun.yandex.ru/problem/avl-balance)
129. [Родословная: подсчет уровней](https://coderun.yandex.ru/problem/pedigree-counting-levels)
130. [Гистограмма](https://coderun.yandex.ru/problem/histogram)
132. [Коллекционер Диего](https://coderun.yandex.ru/problem/collector-diego)
134. [Хорошая строка](https://coderun.yandex.ru/problem/good-line)
135. [Операционные системы lite](https://coderun.yandex.ru/problem/lite-operating-systems)
137. [Минимальный прямоугольник](https://coderun.yandex.ru/problem/minimum-rectangle)
138. [Сумма в прямоугольнике](https://coderun.yandex.ru/problem/rectangle-sum)
139. [Скучная лекция](https://coderun.yandex.ru/problem/boring-lecture)
140. [Стек с защитой от ошибок](https://coderun.yandex.ru/problem/stack-protection-from-errors)
141. [Правильная скобочная последовательность](https://coderun.yandex.ru/problem/correct-bracket-sequence)
142. [Постфиксная запись](https://coderun.yandex.ru/problem/postfix-entry)
143. [Сортировка вагонов lite](https://coderun.yandex.ru/problem/sorting-of-wagons-lite)
144. [Великое Лайнландское переселение](https://coderun.yandex.ru/problem/great-lineland-migration)
145. [Очередь с защитой от ошибок](https://coderun.yandex.ru/problem/queue-with-error-protection)
146. [Игра в пьяницу](https://coderun.yandex.ru/problem/drunkard-game)
147. [Дек с защитой от ошибок](https://coderun.yandex.ru/problem/dec-with-error-protection)
148. [Хипуй](https://coderun.yandex.ru/problem/hipuy)
149. [Пирамидальная сортировка](https://coderun.yandex.ru/problem/pyramid-sorting)
150. [Три единицы подряд](https://coderun.yandex.ru/problem/three-blocks-row)
151. [Кузнечик](https://coderun.yandex.ru/problem/grasshopper)
175. [Уникальные пользователи](https://coderun.yandex.ru/problem/unique-users)
220. [Нормализация показателей](https://coderun.yandex.ru/problem/normalization-of-indicators)
272. [Простая подсказка](https://coderun.yandex.ru/problem/simple-suggest)
290. [Игра](https://coderun.yandex.ru/problem/game)
294. [Сумма различных](https://coderun.yandex.ru/problem/summ-of-the-various)
302. [Восстановить матрицу](https://coderun.yandex.ru/problem/restore-the-matrix)
304. [Проверка палиндрома](https://coderun.yandex.ru/problem/palindroming-check)
310. [Кодирование длин серий](https://coderun.yandex.ru/problem/rle-test)
374. [Оценка](https://coderun.yandex.ru/problem/mark)
405. [Тетрамино](https://coderun.yandex.ru/problem/tetramino)
410. [Сложить и вычесть](https://coderun.yandex.ru/problem/calc-expression)
455. [Села батарейка](https://coderun.yandex.ru/problem/dead-battery)
456. [Запускайте гуся](https://coderun.yandex.ru/problem/release-the-goose)
458. [Восстановление отчётов](https://coderun.yandex.ru/problem/report-restoration)
467. [Суеверный коллекционер](https://coderun.yandex.ru/problem/next-lucky-ticket)
469. [Игра в города](https://coderun.yandex.ru/problem/city-games)
470. [Чёрное и белое](https://coderun.yandex.ru/problem/black-and-white)
471. [Журнал без дат](https://coderun.yandex.ru/problem/log-without-dates)
475. [Баг в БД](https://coderun.yandex.ru/problem/bug-in-library)
476. [Градиент](https://coderun.yandex.ru/problem/gradient)
478. [Сокращение маршрута](https://coderun.yandex.ru/problem/route-reduction)
543. [Классы подобия треугольников](https://coderun.yandex.ru/problem/triangle-similarity)
4728. [Крош, Ежик и квадратичная игра](https://coderun.yandex.ru/problem/krosh-and-game)
4729. [Крош и строка](https://coderun.yandex.ru/problem/krosh-and-string)
5202. [Юля, Никита и задачи](https://coderun.yandex.ru/problem/season-tasks)
5204. [Умножай и транспонируй!](https://coderun.yandex.ru/problem/matrix-operations)
5205. [Ещё одна задача на теорию чисел](https://coderun.yandex.ru/problem/gcd-and-lcm)
5206. [Выставление тегов](https://coderun.yandex.ru/problem/calculate-tags)
5316. [Снежки](https://coderun.yandex.ru/problem/snowballs)
5317. [В город на ярмарку](https://coderun.yandex.ru/problem/new-year-fair)
5321. [Наряжаем ёлку](https://coderun.yandex.ru/problem/decorating-tree)







**14. Блохи**

**Условие задачи:**
На клеточном поле, размером NxM (2 ≤ N, M ≤ 250) сидит Q (0 ≤ Q ≤ 10000) блох в различных клетках. «Прием пищи» блохами возможен только в кормушке — одна из клеток поля, заранее известная. Блохи перемещаются по полю странным образом, а именно, прыжками, совпадающими с ходом обыкновенного шахматного коня. Длина пути каждой блохи до кормушки определяется как количество прыжков. Определить минимальное значение суммы длин путей блох до кормушки или, если собраться блохам у кормушки невозможно, то сообщить об этом. Сбор невозможен, если хотя бы одна из блох не может попасть к кормушке.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем входные данные
let line = readLine()!.split(separator: " ").map { Int($0)! }
let n = line[0]
let m = line[1]
let s = line[2] - 1 // Переводим в 0-индексацию
let t = line[3] - 1 // Переводим в 0-индексацию
let q = line[4]

// Создаем массив для хранения расстояний от каждой клетки до кормушки
var distances: [[Int]] = Array(repeating: Array(repeating: -1, count: m), count: n)

// Реализуем BFS из кормушки
var queue: [(Int, Int)] = [(s, t)]
distances[s][t] = 0

// Возможные ходы коня
let moves = [(-2, -1), (-2, 1), (-1, -2), (-1, 2), (1, -2), (1, 2), (2, -1), (2, 1)]

while !queue.isEmpty {
    let (row, col) = queue.removeFirst()
    
    // Проверяем все возможные ходы
    for (dr, dc) in moves {
        let newRow = row + dr
        let newCol = col + dc
        
        // Проверяем, что новая позиция внутри поля и еще не посещена
        if newRow >= 0 && newRow < n && newCol >= 0 && newCol < m && distances[newRow][newCol] == -1 {
            distances[newRow][newCol] = distances[row][col] + 1
            queue.append((newRow, newCol))
        }
    }
}

// Считаем сумму расстояний для всех блох
var totalDistance = 0
var isPossible = true

for _ in 0..<q {
    let flea = readLine()!.split(separator: " ").map { Int($0)! - 1 } // Переводим в 0-индексацию
    let fleaRow = flea[0]
    let fleaCol = flea[1]
    
    if distances[fleaRow][fleaCol] == -1 {
        isPossible = false
        break
    }
    
    totalDistance += distances[fleaRow][fleaCol]
}

if isPossible {
    print(totalDistance)
} else {
    print(-1)
}
```

---

**15. Путь спелеолога**

**Условие задачи:**
Пещера представлена кубом, разбитым на N частей по каждому измерению (то есть на N^3 кубических клеток). Каждая клетка может быть или пустой, или полностью заполненной камнем. Исходя из положения спелеолога в пещере, требуется найти, какое минимальное количество перемещений по клеткам ему требуется, чтобы выбраться на поверхность. Переходить из клетки в клетку можно, только если они обе свободны и имеют общую грань.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем размер пещеры
let n = Int(readLine()!)!

// Создаем трехмерный массив для хранения пещеры
var cave: [[[Character]]] = []
var startLevel = 0
var startRow = 0
var startCol = 0

// Считываем описание пещеры
for level in 0..<n {
    _ = readLine() // Пустая строка
    var levelData: [[Character]] = []
    for row in 0..<n {
        let line = Array(readLine()!)
        levelData.append(line)
        
        // Ищем начальную позицию спелеолога
        if let col = line.firstIndex(of: "S") {
            startLevel = level
            startRow = row
            startCol = col
        }
    }
    cave.append(levelData)
}

// Реализуем BFS для поиска кратчайшего пути
var queue: [(Int, Int, Int)] = [(startLevel, startRow, startCol)]
var distances: [[[Int]]] = Array(repeating: Array(repeating: Array(repeating: -1, count: n), count: n), count: n)
distances[startLevel][startRow][startCol] = 0

// Возможные движения (вверх, вниз, север, юг, запад, восток)
let moves = [(-1, 0, 0), (1, 0, 0), (0, -1, 0), (0, 1, 0), (0, 0, -1), (0, 0, 1)]

while !queue.isEmpty {
    let (level, row, col) = queue.removeFirst()
    
    // Если достигли поверхности (уровень 0), выводим результат
    if level == 0 {
        print(distances[level][row][col])
        exit(0)
    }
    
    // Проверяем все возможные движения
    for (dl, dr, dc) in moves {
        let newLevel = level + dl
        let newRow = row + dr
        let newCol = col + dc
        
        // Проверяем, что новая позиция внутри пещеры
        if newLevel >= 0 && newLevel < n && newRow >= 0 && newRow < n && newCol >= 0 && newCol < n {
            // Проверяем, что клетка свободна и еще не посещена
            if (cave[newLevel][newRow][newCol] == "." || cave[newLevel][newRow][newCol] == "S") && distances[newLevel][newRow][newCol] == -1 {
                distances[newLevel][newRow][newCol] = distances[level][row][col] + 1
                queue.append((newLevel, newRow, newCol))
            }
        }
    }
}
```

---

**28. НВП с восстановлением ответа**

**Условие задачи:**
Дана последовательность, требуется найти её наибольшую возрастающую подпоследовательность. Последовательность x называется подпоследовательностью последовательности y, если x получается из y удалением нескольких (возможно, нуля или всех) элементов. Наибольшая возрастающая подпоследовательность - строго возрастающая подпоследовательность наибольшей длины.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем длину последовательности
let n = Int(readLine()!)!

// Считываем саму последовательность
let sequence = readLine()!.split(separator: " ").map { Int($0)! }

// Массив для хранения длины НВП, заканчивающейся на i-ом элементе
var dp: [Int] = Array(repeating: 1, count: n)

// Массив для восстановления пути
var prev: [Int] = Array(repeating: -1, count: n)

// Заполняем массив dp
for i in 1..<n {
    for j in 0..<i {
        if sequence[j] < sequence[i] && dp[j] + 1 > dp[i] {
            dp[i] = dp[j] + 1
            prev[i] = j
        }
    }
}

// Находим индекс элемента, на котором заканчивается НВП максимальной длины
var maxLength = 0
var maxIndex = 0
for i in 0..<n {
    if dp[i] > maxLength {
        maxLength = dp[i]
        maxIndex = i
    }
}

// Восстанавливаем НВП
var result: [Int] = []
var currentIndex = maxIndex
while currentIndex != -1 {
    result.append(sequence[currentIndex])
    currentIndex = prev[currentIndex]
}

// Выводим результат
result.reversed().forEach { print($0, terminator: " ") }
print()
```

---

**33. Расстояние по Левенштейну**

**Условие задачи:**
Дана текстовая строка. С ней можно выполнять следующие операции: 1. Заменить один символ строки на другой символ. 2. Удалить один произвольный символ. 3. Вставить произвольный символ в произвольное место строки. Минимальное количество таких операций, при помощи которых можно из одной строки получить другую, называется стоимостью редактирования или расстоянием Левенштейна. Определите расстояние Левенштейна для двух данных строк.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем строки
let str1 = readLine()!
let str2 = readLine()!

let m = str1.count
let n = str2.count

// Создаем матрицу для динамического программирования
var dp: [[Int]] = Array(repeating: Array(repeating: 0, count: n + 1), count: m + 1)

// Преобразуем строки в массивы символов для удобства доступа
let arr1 = Array(str1)
let arr2 = Array(str2)

// Инициализируем первую строку и первый столбец
for i in 0...m {
    dp[i][0] = i
}
for j in 0...n {
    dp[0][j] = j
}

// Заполняем матрицу
for i in 1...m {
    for j in 1...n {
        if arr1[i - 1] == arr2[j - 1] {
            dp[i][j] = dp[i - 1][j - 1] // Символы совпадают, операция не нужна
        } else {
            // Выбираем минимальную стоимость среди трех операций
            dp[i][j] = min(
                dp[i - 1][j] + 1,     // Удаление
                dp[i][j - 1] + 1,     // Вставка
                dp[i - 1][j - 1] + 1  // Замена
            )
        }
    }
}

// Выводим результат
print(dp[m][n])
```

---

**39. Откуда достижима первая вершина**

**Условие задачи:**
Дан ориентированный граф, возможно, с петлями и кратными ребрами. Необходимо найти все вершины, из которых достижима первая вершина.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем количество вершин и ребер
let line = readLine()!.split(separator: " ").map { Int($0)! }
let n = line[0]
let m = line[1]

// Создаем список смежности для обратного графа
var reverseGraph: [[Int]] = Array(repeating: [], count: n + 1)

// Считываем ребра и строим обратный граф
for _ in 0..<m {
    let edge = readLine()!.split(separator: " ").map { Int($0)! }
    let from = edge[0]
    let to = edge[1]
    reverseGraph[to].append(from) // Переворачиваем направление ребра
}

// Реализуем DFS из первой вершины в обратном графе
var visited: [Bool] = Array(repeating: false, count: n + 1)
var stack: [Int] = [1] // Первая вершина (с номером 1)
visited[1] = true

while !stack.isEmpty {
    let current = stack.removeLast()
    
    // Проверяем всех предшественников в обратном графе
    for predecessor in reverseGraph[current] {
        if !visited[predecessor] {
            visited[predecessor] = true
            stack.append(predecessor)
        }
    }
}

// Выводим все достижимые вершины
for i in 1...n {
    if visited[i] {
        print(i)
    }
}
```

---

**43. Два коня**

**Условие задачи:**
На стандартной шахматной доске (8х8) живут 2 шахматных коня: Красный и Зелёный. Обычно они беззаботно скачут по просторам доски, пощипывая шахматную травку, но сегодня особенный день: у Зелёного коня День Рождения. Зеленый конь решил отпраздновать это событие вместе с Красным. Но для осуществления этого прекрасного плана им нужно оказаться на одной клетке. Заметим, что Красный и Зелёный шахматные кони сильно отличаются от черного с белым: они ходят не по очереди, а одновременно, и если оказываются на одной клетке, никто никого не съедает. Сколько ходов им потребуется, чтобы насладиться праздником?

**Решение задачи на Swift:**

```swift
import Foundation

// Функция для преобразования шахматной нотации в координаты
func parsePosition(_ pos: String) -> (Int, Int) {
    let col = Int(pos.first!.asciiValue! - Character("a").asciiValue!) // 0-7
    let row = Int(String(pos.last!))! - 1 // 0-7
    return (row, col)
}

// Считываем позиции коней
let redPos = readLine()!
let greenPos = readLine()!

let (redRow, redCol) = parsePosition(redPos)
let (greenRow, greenCol) = parsePosition(greenPos)

// Если кони уже на одной клетке
if redRow == greenRow && redCol == greenCol {
    print(0)
    exit(0)
}

// BFS для поиска кратчайшего пути
// Состояние: (redRow, redCol, greenRow, greenCol)
var visited: [[[Bool]]] = Array(repeating: Array(repeating: Array(repeating: false, count: 8), count: 8), count: 8)
var queue: [(Int, Int, Int, Int, Int)] = [(redRow, redCol, greenRow, greenCol, 0)] // (redRow, redCol, greenRow, greenCol, steps)
visited[redRow][redCol][greenRow] = true

// Возможные ходы коня
let moves = [(-2, -1), (-2, 1), (-1, -2), (-1, 2), (1, -2), (1, 2), (2, -1), (2, 1)]

while !queue.isEmpty {
    let (rRow, rCol, gRow, gCol, steps) = queue.removeFirst()
    
    // Проверяем все возможные комбинации ходов
    for (dr1, dc1) in moves {
        let newRRow = rRow + dr1
        let newRCol = rCol + dc1
        
        // Проверяем, что позиция красного коня на доске
        if newRRow >= 0 && newRRow < 8 && newRCol >= 0 && newRCol < 8 {
            for (dr2, dc2) in moves {
                let newGRow = gRow + dr2
                let newGCol = gCol + dc2
                
                // Проверяем, что позиция зеленого коня на доске
                if newGRow >= 0 && newGRow < 8 && newGCol >= 0 && newGCol < 8 {
                    // Если кони встретились
                    if newRRow == newGRow && newRCol == newGCol {
                        print(steps + 1)
                        exit(0)
                    }
                    
                    // Если эта комбинация позиций еще не была посещена
                    if !visited[newRRow][newRCol][newGRow] {
                        visited[newRRow][newRCol][newGRow] = true
                        queue.append((newRRow, newRCol, newGRow, newGCol, steps + 1))
                    }
                }
            }
        }
    }
}

// Если не нашли путь
print(-1)
```

---

**44. Числа**

**Условие задачи:**
Витя хочет придумать новую игру с числами. В этой игре от игроков требуется преобразовывать четырехзначные числа не содержащие нулей при помощи следующего разрешенного набора действий: 1. Можно увеличить первую цифру числа на 1, если она не равна 9. 2. Можно уменьшить последнюю цифру на 1, если она не равна 1. 3. Можно циклически сдвинуть все цифры на одну вправо. 4. Можно циклически сдвинуть все цифры на одну влево. Например, применяя эти правила к числу 1234 можно получить числа 2234, 1233, 4123 и 2341 соответственно. Точные правила игры Витя пока не придумал, но пока его интересует вопрос, как получить из одного числа другое за минимальное количество операций.

**Решение задачи на Swift:**

```swift
import Foundation

// Функция для применения операций к числу
func applyOperation(_ number: Int, _ operation: Int) -> Int? {
    var digits = Array(String(number))
    
    switch operation {
    case 1: // Увеличить первую цифру на 1
        if digits[0] < "9" {
            digits[0] = Character(String(Int(String(digits[0]))! + 1))
            return Int(String(digits))!
        }
    case 2: // Уменьшить последнюю цифру на 1
        if digits[3] > "1" {
            digits[3] = Character(String(Int(String(digits[3]))! - 1))
            return Int(String(digits))!
        }
    case 3: // Циклический сдвиг вправо
        let lastDigit = digits.removeLast()
        digits.insert(lastDigit, at: 0)
        return Int(String(digits))!
    case 4: // Циклический сдвиг влево
        let firstDigit = digits.removeFirst()
        digits.append(firstDigit)
        return Int(String(digits))!
    default:
        break
    }
    
    return nil // Операция не может быть применена
}

// Считываем начальное и конечное числа
let start = Int(readLine()!)!
let end = Int(readLine()!)!

// Если числа совпадают
if start == end {
    print(start)
    exit(0)
}

// BFS для поиска кратчайшего пути
var visited: [Bool] = Array(repeating: false, count: 10000)
var queue: [(Int, [Int])] = [(start, [start])] // (число, путь)
visited[start] = true

while !queue.isEmpty {
    let (current, path) = queue.removeFirst()
    
    // Применяем все возможные операции
    for op in 1...4 {
        if let next = applyOperation(current, op) {
            // Если достигли конечного числа
            if next == end {
                let fullPath = path + [next]
                fullPath.forEach { print($0) }
                exit(0)
            }
            
            // Если это новое число, добавляем его в очередь
            if !visited[next] {
                visited[next] = true
                queue.append((next, path + [next]))
            }
        }
    }
}
```

---

**51. Номер появления слова**

**Условие задачи:**
Во входном файле (вы можете читать данные из файла input.txt) записан текст. Словом считается последовательность непробельных символов идущих подряд, слова разделены одним или большим числом пробелов или символами конца строки. Для каждого слова из этого текста подсчитайте, сколько раз оно встречалось в этом тексте ранее.

**Решение задачи на Swift:**

```swift
import Foundation

// Словарь для хранения количества появлений каждого слова
var wordCount: [String: Int] = [:]

// Читаем весь текст
var text = ""
while let line = readLine() {
    text += line + " "
}

// Разбиваем текст на слова
let words = text.split(separator: " ").map { String($0) }

// Обрабатываем каждое слово
for word in words {
    let count = wordCount[word, default: 0]
    print(count)
    wordCount[word, default: 0] += 1
}
```

Хорошо, вот следующие 10 задач из списка с решениями на Swift:

**101. Дипломы**

**Условие задачи:**
Когда Петя учился в школе, он часто участвовал в олимпиадах по информатике, математике и физике. Так как он был достаточно способным мальчиком и усердно учился, то на многих из этих олимпиад он получал дипломы. К окончанию школы у него накопилось n дипломов, причём, как оказалось, все они имели одинаковые размеры: w — в ширину и h — в высоту. Сейчас Петя учится в одном из лучших российских университетов и живёт в общежитии со своими одногруппниками. Он решил украсить свою комнату, повесив на одну из стен свои дипломы за школьные олимпиады. Так как к бетонной стене прикрепить дипломы достаточно трудно, то он решил купить специальную доску из пробкового дерева, чтобы прикрепить её к стене, а к ней — дипломы. Для того чтобы эта конструкция выглядела более красиво, Петя хочет, чтобы доска была квадратной и занимала как можно меньше места на стене. Каждый диплом должен быть размещён строго в прямоугольнике размером w на h. Дипломы запрещается поворачивать на 90 градусов. Прямоугольники, соответствующие различным дипломам, не должны иметь общих внутренних точек. Требуется написать программу, которая вычислит минимальный размер стороны доски, которая потребуется Пете для размещения всех своих дипломов.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем входные данные
let input = readLine()!.split(separator: " ").map { Int($0)! }
let n = input[0] // Количество дипломов
let w = input[1] // Ширина диплома
let h = input[2] // Высота диплома

// Если дипломов нет, доска не нужна
if n == 0 {
    print(0)
    exit(0)
}

// Бинарный поиск по размеру стороны квадратной доски
var left = max(w, h)
var right = max(w, h) * n

// Функция для проверки, помещаются ли все дипломы на доску со стороной size
func canPlace(_ size: Int) -> Bool {
    let rows = size / h // Количество рядов по высоте
    let cols = size / w // Количество колонок по ширине
    return rows * cols >= n
}

while left < right {
    let mid = (left + right) / 2
    if canPlace(mid) {
        right = mid
    } else {
        left = mid + 1
    }
}

print(left)
```

---

**103. Расстановка ноутбуков**

**Условие задачи:**
В школе решили на один прямоугольный стол поставить два прямоугольных ноутбука. Ноутбуки нужно поставить так, чтобы их стороны были параллельны сторонам стола. Определите, какие размеры должен иметь стол, чтобы оба ноутбука на него поместились, а площадь стола была минимальна.

**Решение задачи на Swift:**

```swift
let input = readLine()!.split(separator: " ").map { Int($0)! }
let a = input[0], b = input[1], c = input[2], d = input[3]

// Все возможные варианты расположения
let variants = [
    (a + c, max(b, d)),           // ноутбуки в ряд по горизонтали
    (max(a, c), b + d),           // ноутбуки в ряд по вертикали
    (a + d, max(b, c)),           // первый горизонтально, второй вертикально
    (max(a, d), b + c)            // первый вертикально, второй горизонтально
]

let minVariant = variants.min { $0.0 * $0.1 < $1.0 * $1.1 }!
print(minVariant.0, minVariant.1)
```

---

**100. Приближенный двоичный поиск**

**Условие задачи:**
Для каждого из чисел второй последовательности найдите ближайшее к нему в первой.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем размеры массивов
let nk = readLine()!.split(separator: " ").map { Int($0)! }
let n = nk[0] // Размер первого массива
let k = nk[1] // Размер второго массива

// Считываем первый массив (отсортирован)
let firstArray = readLine()!.split(separator: " ").map { Int($0)! }

// Считываем второй массив
let secondArray = readLine()!.split(separator: " ").map { Int($0)! }

// Функция для бинарного поиска ближайшего элемента
func findClosest(_ target: Int, in array: [Int]) -> Int {
    var left = 0
    var right = array.count - 1
    
    // Если target меньше или равен первому элементу
    if target <= array[left] {
        return array[left]
    }
    
    // Если target больше или равен последнему элементу
    if target >= array[right] {
        return array[right]
    }
    
    // Бинарный поиск
    while left < right {
        let mid = (left + right) / 2
        
        if array[mid] == target {
            return array[mid]
        }
        
        if target < array[mid] {
            right = mid
        } else {
            left = mid + 1
        }
    }
    
    // Сравниваем два ближайших элемента
    let leftDiff = abs(array[left - 1] - target)
    let rightDiff = abs(array[left] - target)
    
    if leftDiff < rightDiff || (leftDiff == rightDiff && array[left - 1] < array[left]) {
        return array[left - 1]
    } else {
        return array[left]
    }
}

// Для каждого элемента второго массива находим ближайший в первом
for target in secondArray {
    let closest = findClosest(target, in: firstArray)
    print(closest)
}
```

---

**99. Двоичный поиск**

**Условие задачи:**
Реализуйте двоичный поиск в массиве

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем размеры массивов
let nk = readLine()!.split(separator: " ").map { Int($0)! }
let n = nk[0] // Размер первого массива
let k = nk[1] // Размер второго массива

// Считываем первый массив
let firstArray = readLine()!.split(separator: " ").map { Int($0)! }

// Считываем второй массив
let secondArray = readLine()!.split(separator: " ").map { Int($0)! }

// Функция для бинарного поиска
func binarySearch(_ target: Int, in array: [Int]) -> Bool {
    var left = 0
    var right = array.count - 1
    
    while left <= right {
        let mid = (left + right) / 2
        
        if array[mid] == target {
            return true
        } else if array[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    
    return false
}

// Для каждого элемента второго массива проверяем наличие в первом
for target in secondArray {
    if binarySearch(target, in: firstArray) {
        print("YES")
    } else {
        print("NO")
    }
}
```

---

**96. Подстрока**

**Условие задачи:**
В этой задаче Вам требуется найти максимальную по длине подстроку данной строки, такую что каждый символ встречается в ней не более k раз.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем входные данные
let nk = readLine()!.split(separator: " ").map { Int($0)! }
let n = nk[0] // Длина строки
let k = nk[1] // Максимальное количество повторений символа

// Считываем строку
let s = readLine()!

// Используем алгоритм двух указателей
var charCount: [Character: Int] = [:]
var left = 0
var maxLength = 0
var bestStart = 0

for right in 0..<n {
    let rightChar = s[s.index(s.startIndex, offsetBy: right)]
    charCount[rightChar, default: 0] += 1
    
    // Если какой-то символ встречается больше k раз, сдвигаем левый указатель
    while charCount[rightChar]! > k {
        let leftChar = s[s.index(s.startIndex, offsetBy: left)]
        charCount[leftChar]! -= 1
        if charCount[leftChar] == 0 {
            charCount.removeValue(forKey: leftChar)
        }
        left += 1
    }
    
    // Обновляем максимальную длину и позицию начала
    let currentLength = right - left + 1
    if currentLength > maxLength {
        maxLength = currentLength
        bestStart = left
    }
}

print(maxLength, bestStart + 1) // Нумерация с 1
```

---

**93. Город Че**

**Условие задачи:**
В центре города Че есть пешеходная улица - одно из самых популярных мест для прогулок жителей города. По этой улице очень приятно гулять, ведь вдоль улицы расположено n забавных памятников. Девочке Маше из города Че нравятся два мальчика из её школы, и она никак не может сделать выбор между ними. Чтобы принять окончательное решение, она решила назначить обоим мальчикам свидание в одно и то же время. Маша хочет выбрать два памятника на пешеходной улице, около которых мальчики будут её ждать. При этом она хочет выбрать такие памятники, чтобы мальчики не увидели друг друга. Маша знает, что из-за тумана мальчики увидят друг друга только в том случае, если они будут на расстоянии не более r метров. Маша заинтересовалась, а сколько способов есть выбрать два различных памятника для организации свиданий?

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем входные данные
let nr = readLine()!.split(separator: " ").map { Int($0)! }
let n = nr[0] // Количество памятников
let r = nr[1] // Максимальное расстояние, на котором мальчики могут увидеть друг друга

// Считываем расстояния памятников от начала улицы
let distances = readLine()!.split(separator: " ").map { Int($0)! }

var count = 0

// Для каждого памятника i находим количество памятников j (j > i), таких что расстояние > r
for i in 0..<n {
    // Используем бинарный поиск для нахождения первого памятника, который находится на расстоянии > r от памятника i
    let targetDistance = distances[i] + r
    var left = i + 1
    var right = n
    
    while left < right {
        let mid = (left + right) / 2
        if distances[mid] > targetDistance {
            right = mid
        } else {
            left = mid + 1
        }
    }
    
    // Количество подходящих памятников для памятника i
    count += n - left
}

print(count)
```

---

**91. Сумма номеров**

**Условие задачи:**
Вася очень любит везде искать своё счастливое число K. Каждый день он ходит в школу по улице, вдоль которой припарковано N машин. Он заинтересовался вопросом, сколько существует отрезков из подряд идущих машин таких, что сумма их номеров равна K. Помогите Васе узнать ответ на его вопрос.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем входные данные
let nk = readLine()!.split(separator: " ").map { Int($0)! }
let n = nk[0] // Количество машин
let k = nk[1] // Счастливое число

// Считываем номера машин
let carNumbers = readLine()!.split(separator: " ").map { Int($0)! }

var count = 0
var prefixSum = 0
var sumCount: [Int: Int] = [0: 1] // Словарь для хранения количества префиксных сумм

// Используем метод префиксных сумм
for i in 0..<n {
    prefixSum += carNumbers[i]
    
    // Проверяем, существует ли префиксная сумма (prefixSum - k)
    if let existingCount = sumCount[prefixSum - k] {
        count += existingCount
    }
    
    // Обновляем словарь с текущей префиксной суммой
    sumCount[prefixSum, default: 0] += 1
}

print(count)
```

---

**88. Контрольная по ударениям**

**Условие задачи:**
Учительница задала Пете домашнее задание — в заданном тексте расставить ударения в словах, после чего поручила Васе проверить это домашнее задание. Вася очень плохо знаком с данной темой, поэтому он нашел словарь, в котором указано, как ставятся ударения в словах. К сожалению, в этом словаре присутствуют не все слова. Вася решил, что в словах, которых нет в словаре, он будет считать, что Петя поставил ударения правильно, если в этом слове Петей поставлено ровно одно ударение. Оказалось, что в некоторых словах ударение может быть поставлено больше, чем одним способом. Вася решил, что в этом случае если то, как Петя поставил ударение, соответствует одному из приведённых в словаре вариантов, он будет засчитывать это как правильную расстановку ударения, а если не соответствует, то как ошибку. Вам дан словарь, которым пользовался Вася и домашнее задание, сданное Петей. Ваша задача — определить количество ошибок, которое в этом задании насчитает Вася.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем количество слов в словаре
let n = Int(readLine()!)!

// Словарь для хранения правильных вариантов ударений
var accentDictionary: [String: Set<String>] = [:]

// Считываем слова из словаря
for _ in 0..<n {
    let word = readLine()!
    let lowercasedWord = word.lowercased()
    if accentDictionary[lowercasedWord] == nil {
        accentDictionary[lowercasedWord] = Set<String>()
    }
    accentDictionary[lowercasedWord]!.insert(word)
}

// Считываем текст Пети
let peteText = readLine()!

// Разбиваем текст на слова
let words = peteText.split(separator: " ").map { String($0) }

var errorCount = 0

// Проверяем каждое слово
for word in words {
    let lowercasedWord = word.lowercased()
    let accentCount = word.filter { $0.isUppercase }.count
    
    // Если нет заглавных букв или больше одной заглавной буквы - ошибка
    if accentCount != 1 {
        errorCount += 1
        continue
    }
    
    // Если слово есть в словаре
    if let validAccents = accentDictionary[lowercasedWord] {
        // Проверяем, совпадает ли ударение с одним из вариантов в словаре
        if !validAccents.contains(word) {
            errorCount += 1
        }
    }
    // Если слова нет в словаре, то по условию оно считается правильным (т.к. accentCount == 1)
}

print(errorCount)
```

---

**86. Банковские счета**

**Условие задачи:**
Некоторый банк хочет внедрить систему управления счетами клиентов, поддерживающую следующие операции: Пополнение счета клиента. Снятие денег со счёта. Запрос остатка средств на счёте. Перевод денег между счетами клиентов. Начисление процентов всем клиентам. Вам необходимо реализовать такую систему. Клиенты банка идентифицируются именами (уникальная строка, не содержащая пробелов). Первоначально у банка нет ни одного клиента. Как только для клиента проводится операция пополнения, снятия или перевода денег, ему заводится счёт с нулевым балансом. Все дальнейшие операции проводятся только с этим счётом. Сумма на счету может быть как положительной, так и отрицательной, при этом всегда является целым числом.

**Решение задачи на Swift:**

```swift
import Foundation

// Словарь для хранения балансов клиентов
var balances: [String: Int] = [:]

// Функция для получения баланса клиента (если счета нет, создаем с нулевым балансом)
func getBalance(_ name: String) -> Int {
    if balances[name] == nil {
        balances[name] = 0
    }
    return balances[name]!
}

// Функция для установки баланса клиента
func setBalance(_ name: String, _ amount: Int) {
    balances[name] = amount
}

// Обрабатываем команды
while let line = readLine(), !line.isEmpty {
    let parts = line.split(separator: " ").map { String($0) }
    let command = parts[0]
    
    switch command {
    case "DEPOSIT":
        let name = parts[1]
        let amount = Int(parts[2])!
        setBalance(name, getBalance(name) + amount)
        
    case "WITHDRAW":
        let name = parts[1]
        let amount = Int(parts[2])!
        setBalance(name, getBalance(name) - amount)
        
    case "BALANCE":
        let name = parts[1]
        if balances[name] != nil {
            print(getBalance(name))
        } else {
            print("ERROR")
        }
        
    case "TRANSFER":
        let name1 = parts[1]
        let name2 = parts[2]
        let amount = Int(parts[3])!
        setBalance(name1, getBalance(name1) - amount)
        setBalance(name2, getBalance(name2) + amount)
        
    case "INCOME":
        let percent = Int(parts[1])!
        for (name, balance) in balances {
            if balance > 0 {
                let income = balance * percent / 100
                setBalance(name, balance + income)
            }
        }
        
    default:
        break
    }
}
```

---

**84. Пирамида**

**Условие задачи:**
Для строительства двумерной пирамиды используются прямоугольные блоки, каждый из которых характеризуется шириной и высотой. Можно поставить один блок на другой, только если ширина верхнего блока строго меньше ширины нижнего (блоки нельзя поворачивать). Самым нижним в пирамиде может быть блок любой ширины. По заданному набору блоков определите, пирамиду какой наибольшей высоты можно из них построить.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем количество блоков
let n = Int(readLine()!)!

// Считываем блоки и сортируем по ширине в порядке убывания
var blocks: [(width: Int, height: Int)] = []
for _ in 0..<n {
    let wh = readLine()!.split(separator: " ").map { Int($0)! }
    blocks.append((wh[0], wh[1]))
}

blocks.sort { $0.width > $1.width }

// Используем динамическое программирование для нахождения максимальной высоты
var maxHeight = 0
var dp: [Int] = Array(repeating: 0, count: n)

for i in 0..<n {
    dp[i] = blocks[i].height // Высота пирамиды, заканчивающейся на блоке i
    
    // Проверяем все предыдущие блоки
    for j in 0..<i {
        // Если блок j может быть под блоком i
        if blocks[j].width > blocks[i].width {
            dp[i] = max(dp[i], dp[j] + blocks[i].height)
        }
    }
    
    maxHeight = max(maxHeight, dp[i])
}

print(maxHeight)
```


Хорошо, вот следующие 10 задач из списка с решениями на Swift:

**83. Клавиатура**

**Условие задачи:**
На региональном этапе Всероссийской олимпиады школьников по информатике в 2009 году предлагалась следующая задача. Всем известно, что со временем клавиатура изнашивается, и клавиши на ней начинают залипать. Конечно, некоторое время такую клавиатуру ещё можно использовать, но для нажатий клавиш приходиться использовать большую силу. При изготовлении клавиатуры изначально для каждой клавиши задается количество нажатий, которое она должна выдерживать. Если знать эти величины для используемой клавиатуры, то для определенной последовательности нажатых клавиш можно определить, какие клавиши в процессе их использования сломаются, а какие — нет. Требуется написать программу, определяющую, какие клавиши сломаются в процессе заданного варианта эксплуатации клавиатуры.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем количество клавиш
let n = Int(readLine()!)!

// Считываем количество нажатий, которое выдерживает каждая клавиша
let durability = readLine()!.split(separator: " ").map { Int($0)! }

// Считываем общее количество нажатий
let k = Int(readLine()!)!

// Считываем последовательность нажатых клавиш
let presses = readLine()!.split(separator: " ").map { Int($0)! }

// Создаем массив для подсчета нажатий на каждую клавишу
var pressCount = Array(repeating: 0, count: n)

// Подсчитываем количество нажатий на каждую клавишу
for press in presses {
    let keyIndex = press - 1 // Переводим в 0-индексацию
    pressCount[keyIndex] += 1
}

// Определяем, сломаются ли клавиши
for i in 0..<n {
    if pressCount[i] > durability[i] {
        print("YES")
    } else {
        print("NO")
    }
}
```

---

**82. Кондиционер**

**Условие задачи:**
В офисе, где работает программист Пётр, установили кондиционер нового типа. Этот кондиционер отличается особой простотой в управлении. У кондиционера есть всего лишь два управляемых параметра: желаемая температура и режим работы. Кондиционер может работать в четырёх режимах: «freeze» — охлаждение. В этом режиме кондиционер может только уменьшать температуру. Если температура в комнате и так не больше желаемой, то он выключается. «heat» — нагрев. В этом режиме кондиционер может только увеличивать температуру. Если температура в комнате и так не меньше желаемой, то он выключается. «auto» — автоматический режим. В этом режиме кондиционер может как увеличивать, так и уменьшать температуру в комнате до желаемой. «fan» — вентиляция. В этом режиме кондиционер осуществляет только вентиляцию воздуха и не изменяет температуру в комнате. Кондиционер достаточно мощный, поэтому при настройке на правильный режим работы он за час доводит температуру в комнате до желаемой. Требуется написать программу, которая по заданной температуре в комнате `t_room`, установленным на кондиционере желаемой температуре `t_cond` и режиму работы определяет температуру, которая установится в комнате через час.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем температуры
let temperatures = readLine()!.split(separator: " ").map { Int($0)! }
let tRoom = temperatures[0]
let tCond = temperatures[1]

// Считываем режим работы
let mode = readLine()!

// Определяем температуру через час в зависимости от режима
let resultTemp: Int
switch mode {
case "freeze":
    resultTemp = min(tRoom, tCond)
case "heat":
    resultTemp = max(tRoom, tCond)
case "auto":
    resultTemp = tCond
case "fan":
    resultTemp = tRoom
default:
    resultTemp = tRoom // На случай некорректного режима
}

print(resultTemp)
```

---

**81. Треугольник**

**Условие задачи:**
Даны три натуральных числа. Возможно ли построить треугольник с такими сторонами? Если это возможно, выведите строку YES, иначе выведите строку NO. Треугольник — это три точки, не лежащие на одной прямой.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем три числа - стороны треугольника
let sides = readLine()!.split(separator: " ").map { Int($0)! }

// Сортируем стороны для удобства проверки
let sortedSides = sides.sorted()

// Проверяем неравенство треугольника
// Сумма двух меньших сторон должна быть больше третьей стороны
if sortedSides[0] + sortedSides[1] > sortedSides[2] {
    print("YES")
} else {
    print("NO")
}
```

---

**80. Телефонные номера**

**Условие задачи:**
Телефонные номера в адресной книге мобильного телефона имеют один из следующих форматов: +7<код><номер>, 8<код><номер>, <номер>, где <номер> — это семь цифр, а <код> — это три цифры или три цифры в круглых скобках. Если код не указан, то считается, что он равен 495. Кроме того, в записи телефонного номера может стоять знак “-” между любыми двумя цифрами. На данный момент в адресной книге телефона Васи записано всего три телефонных номера и он хочет записать туда ещё один. Но он не может понять, не записан ли уже такой номер в телефонной книге. Помогите ему! Два телефонных номера совпадают, если у них равны коды и равны номера. Например, +7(916)0123456 и 89160123456 — это один и тот же номер.

**Решение задачи на Swift:**

```swift
import Foundation

// Функция для нормализации номера телефона
func normalizePhoneNumber(_ phone: String) -> (code: String, number: String) {
    // Удаляем все нецифровые символы
    let digits = phone.filter { $0.isNumber }
    
    // Определяем код и номер
    let code: String
    let number: String
    
    if digits.count == 11 {
        // Номер начинается с 7 или 8
        if digits.first == "7" || digits.first == "8" {
            code = String(digits.dropFirst().prefix(3))
            number = String(digits.suffix(7))
        } else {
            // Это случай, когда 11 цифр, но не начинается с 7 или 8
            // Скорее всего, это ошибка, но обработаем по аналогии
            code = String(digits.prefix(3))
            number = String(digits.suffix(7))
        }
    } else if digits.count == 10 {
        // Номер начинается с кода
        code = String(digits.prefix(3))
        number = String(digits.suffix(7))
    } else {
        // Номер без кода, код по умолчанию 495
        code = "495"
        number = digits
    }
    
    return (code, number)
}

// Считываем номер, который Вася хочет добавить
let newPhone = readLine()!
let normalizedNewPhone = normalizePhoneNumber(newPhone)

// Считываем три номера из адресной книги
for _ in 0..<3 {
    let existingPhone = readLine()!
    let normalizedExistingPhone = normalizePhoneNumber(existingPhone)
    
    // Сравниваем коды и номера
    if normalizedNewPhone.code == normalizedExistingPhone.code && 
       normalizedNewPhone.number == normalizedExistingPhone.number {
        print("YES")
    } else {
        print("NO")
    }
}
```

---

**75. Метро**

**Условие задачи:**
На некоторых кросс-платформенных станциях метро (как, например, «Третьяковская») на разные стороны платформы приходят поезда разных направлений. Таня договорилась встретиться с подругой на такой станции, но поскольку подруга приехала из другого часового пояса, то из-за джетлага сильно проспала, и Тане пришлось долго её ждать. Поезда всегда ходят точно по расписанию, и Таня знает, что поезд стоит на платформе ровно одну минуту, а интервал между поездами (время, в течение которого поезда у платформы нет) составляет `a` минут для поездов на первом пути и `b` минут для поездов на втором пути. То есть на первый путь приезжает поезд и стоит одну минуту, затем в течение `a` минут поезда у платформы нет, затем в течение одной минуты у платформы стоит следующий поезд и т.д. Пока Таня стояла на платформе, она насчитала `n` поездов на первом пути и `m` поездов на втором пути. Определите минимальное и максимальное время, которое Таня могла провести на платформе, или сообщите, что она точно сбилась со счёта. Все поезда, которые видела Таня, она наблюдала в течение всей минуты, то есть Таня не приходит и не уходит с платформы посередине той минуты, когда поезд стоит на платформе.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем параметры
let a = Int(readLine()!)! // Интервал для первого пути
let b = Int(readLine()!)! // Интервал для второго пути
let n = Int(readLine()!)! // Количество поездов на первом пути
let m = Int(readLine()!)! // Количество поездов на втором пути

// Функция для вычисления минимального и максимального времени для одного пути
func getTimeBounds(interval: Int, trains: Int) -> (min: Int, max: Int)? {
    if trains == 0 {
        return (0, 0)
    }
    
    // Минимальное время: поезда идут подряд с минимальными интервалами
    let minTime = trains + (trains - 1) * interval
    
    // Максимальное время: между поездами максимальное количество свободного времени
    // Это немного сложнее, но можно рассмотреть, что между n поездами n-1 интервал
    // и у нас есть свобода в размещении этих интервалов
    // Минимальное общее время для n поездов: n минут на поезда + (n-1) * interval минут интервалов
    // Максимальное время зависит от того, как мы можем "растянуть" интервалы
    
    // Для максимального времени: предположим, что Таня пришла в начале и ушла в конце
    // Последний поезд ушел в момент времени: время прибытия последнего поезда + 1 минута
    // Время прибытия последнего поезда: (n-1) * (interval + 1) минут после первого поезда
    // Плюс 1 минута на сам поезд = (n-1) * (interval + 1) + 1
    // Но Таня могла прийти в любой момент, когда первый поезд есть, и уйти в любой момент, когда последний поезд есть
    
    // Более простой подход:
    // Минимальное время: n поездов + (n-1) * interval
    // Максимальное время: если у нас n поездов, то у нас n-1 промежутков между ними
    // Каждый промежуток может быть от interval до бесконечности, но мы ограничены временем второго пути
    
    // На самом деле, нужно решать систему неравенств
    // Пусть Таня пришла в момент времени t_start и ушла в момент t_end
    // Для первого пути: 
    // Количество поездов, которые она видела = количество целых точек [k*(a+1), k*(a+1)+1] пересекающихся с [t_start, t_end]
    // Это равно n
    
    // Минимальное время - когда интервалы между поездами минимальны
    // Максимальное время - когда интервалы между поездами максимальны, но согласованы с вторым путем
    
    // Попробуем другой подход:
    // Для n поездов на первом пути с интервалом a:
    // Минимальное время присутствия: n + (n-1) * a
    // Максимальное время присутствия: n + (n-1) * a + (a-1) = n + n*a - a + a - 1 = n + n*a - 1
    // Нет, это тоже не совсем верно.
    
    // Правильный подход:
    // Пусть первый поезд на первом пути прибыл в момент 0.
    // Тогда поезда прибывают в моменты: 0, (a+1), 2*(a+1), ..., (n-1)*(a+1)
    // Таня видит поезда в моменты [0,1], [a+1, a+2], ..., [(n-1)*(a+1), (n-1)*(a+1)+1]
    // Если Таня пришла в момент t_start и ушла в момент t_end (t_start <= t_end)
    // То количество видимых поездов = количество отрезков [k*(a+1), k*(a+1)+1], пересекающихся с [t_start, t_end]
    // Отрезок [k*(a+1), k*(a+1)+1] пересекается с [t_start, t_end], если:
    // k*(a+1) <= t_end AND t_start <= k*(a+1)+1
    // k*(a+1) >= t_start-1 AND k*(a+1) <= t_end
    // (t_start-1)/(a+1) <= k <= t_end/(a+1)
    
    // Нам нужно, чтобы таких k было ровно n.
    // Минимальное t_start: такой, что количество k = n
    // Максимальное t_end: такой, что количество k = n
    
    // Минимальное время присутствия Тани:
    // Чтобы увидеть n поездов с минимальным временем, Таня должна прийти максимально поздно, чтобы застать первый поезд,
    // и уйти максимально рано, чтобы не застать (n+1)-й поезд.
    // Минимальное время: Таня приходит в момент 0 и уходит в момент, когда она гарантированно видит n поездов.
    // Последний поезд, который она видит, прибывает в момент (n-1)*(a+1).
    // Она должна уйти не раньше, чем этот поезд уйдет, т.е. в момент (n-1)*(a+1)+1.
    // Но она может уйти сразу после этого, т.е. в момент (n-1)*(a+1)+1.
    // Минимальное время: (n-1)*(a+1)+1 - 0 + 1 = (n-1)*(a+1) + 2? Нет.
    // Минимальное время присутствия: чтобы увидеть n поездов, нужно как минимум n минут на поезда.
    // Между n поездами n-1 интервал. Минимальный интервал - a минут.
    // Минимальное общее время: n + (n-1)*a
    
    // Максимальное время присутствия:
    // Таня может прийти заранее и уйти с опозданием, но видеть только n поездов.
    // Представим, что поезда прибывают в 0, a+1, 2*(a+1), ...
    // Чтобы увидеть только эти n поездов, Таня должна прийти не позже, чем уйдет n-й поезд (в момент (n-1)*(a+1)+1)
    // и уйти не раньше, чем придет 0-й поезд (в момент 0).
    // Но это не дает максимального времени.
    
    // Правильная формулировка:
    // Пусть Таня пришла в момент t0 и ушла в момент t1 (t1 >= t0).
    // Для первого пути с n поездами:
    // Минимальное t0, при котором она еще видит n поездов: ...
    // Максимальное t1, при котором она еще видит n поездов: ...
    
    // Давайте посмотрим на пример:
    // a=1, n=5. Поезда в моменты 0, 2, 4, 6, 8.
    // Таня должна видеть 5 поездов.
    // Минимальное время: она приходит в 0, уходит в 9. Время = 9.
    // Максимальное время: она приходит в 0, уходит в 9. Время = 9.
    // Нет, подождите.
    // Поезда: [0,1], [2,3], [4,5], [6,7], [8,9].
    // Минимальное время: прийти в 0, уйти в 1. Время = 1. Но тогда она видит только 1 поезд? Нет.
    // Она видит поезд, если отрезки пересекаются.
    // Прийти в 0, уйти в 1: пересекаются [0,1]. 1 поезд.
    // Прийти в 1, уйти в 2: пересекаются [0,1] и [2,3]? Нет, [0,1] и [1,2] пересекаются в точке 1. Считается?
    // По условию: "Таня не приходит и не уходит посередине минуты поезда".
    // Значит, моменты прихода/ухода - целые.
    
    // Прийти в 0, уйти в 1: время 1, видит поезд в [0,1]. 1 поезд.
    // Прийти в 0, уйти в 2: время 2, видит поезда в [0,1] и [2,3]? Нет, [0,1] и [2,2] - пересечение в точке 1? 
    // Нет, [0,1] и [2,2] не пересекаются.
    // Прийти в 0, уйти в 3: время 3, видит [0,1] и [2,3]. 2 поезда.
    
    // Значит, чтобы видеть поезд в [k*(a+1), k*(a+1)+1], Таня должна быть в интервале 
    // [k*(a+1), k*(a+1)+1] (включительно по обеим границам, так как она может прийти/уйти в целый момент,
    // а поезд занимает интервал длины 1).
    
    // Чтобы видеть ровно n поездов с номерами 0,1,...,n-1:
    // Таня должна прийти не позже, чем уйдет поезд n-1: t0 <= (n-1)*(a+1)+1
    // Таня должна уйти не раньше, чем придет поезд 0: t1 >= 0
    // Таня должна прийти не раньше, чем придет поезд 0: t0 >= 0
    // Таня должна уйти не позже, чем уйдет поезд n-1: t1 <= (n-1)*(a+1)+1
    
    // Но это не все ограничения. Она не должна видеть поезд n:
    // Поезд n приходит в момент n*(a+1). Чтобы его не видеть, нужно t0 > n*(a+1) ИЛИ t1 < n*(a+1)
    // Но если t0 > n*(a+1), то она не видит и предыдущие поезда.
    // Значит, t1 < n*(a+1).
    
    // Она не должна видеть поезд -1:
    // Поезд -1 приходит в момент -1*(a+1) = -(a+1). Чтобы его не видеть, нужно t0 > -(a+1)+1 = -a ИЛИ t1 < -(a+1)
    // Так как t0 >= 0, условие t0 > -a выполняется автоматически.
    // Условие t1 < -(a+1) означало бы t1 < 0, что противоречит t1 >= 0.
    // Значит, поезд -1 не мешает.
    
    // Итак, для n поездов (0,1,...,n-1):
    // 0 <= t0 <= (n-1)*(a+1)+1
    // 0 <= t1 <= (n-1)*(a+1)+1
    // t1 < n*(a+1) (чтобы не видеть поезд n)
    // t0 > -(a+1) (чтобы не видеть поезд -1) - выполняется, так как t0 >= 0
    
    // Время присутствия: t1 - t0 + 1
    
    // Минимальное время: минимизировать t1 - t0 + 1
    // Это достигается при максимальном t0 и минимальном t1.
    // Максимальный t0: min((n-1)*(a+1)+1, ...). 
    // Минимальный t1: max(0, ...).
    
    // Чтобы видеть все n поездов:
    // t0 <= (n-1)*(a+1)+1
    // t1 >= 0
    // Для поезда 0 (в момент [0,1]): t0 <= 1 и t1 >= 0
    // Для поезда n-1 (в момент [(n-1)*(a+1), (n-1)*(a+1)+1]): t0 <= (n-1)*(a+1)+1 и t1 >= (n-1)*(a+1)
    
    // Итак:
    // 0 <= t0 <= 1
    // (n-1)*(a+1) <= t1 < n*(a+1)
    
    // Минимальное время: t1 - t0 + 1. Минимум при t1 = (n-1)*(a+1), t0 = 1.
    // Время = (n-1)*(a+1) - 1 + 1 = (n-1)*(a+1). Нет, t0 не может быть больше 1.
    // t0 может быть от 0 до 1. Чтобы минимизировать t1-t0, берем t0=1, t1=(n-1)*(a+1).
    // Время = (n-1)*(a+1) - 1 + 1 = (n-1)*(a+1).
    
    // Нет, давайте аккуратнее.
    // Чтобы видеть поезд k (в момент [k*(a+1), k*(a+1)+1]), нужно:
    // t0 <= k*(a+1)+1 и t1 >= k*(a+1)
    
    // Для поездов 0,...,n-1:
    // t0 <= min(1, (a+1)+1, ..., (n-1)*(a+1)+1) = 1
    // t1 >= max(0, (a+1), ..., (n-1)*(a+1)) = (n-1)*(a+1)
    
    // И дополнительные ограничения:
    // t0 >= max(0, ...) = 0
    // t1 <= min((n-1)*(a+1)+1, ..., 1*(a+1)+1, 0*(a+1)+1) = 1? Нет.
    // t1 <= (n-1)*(a+1)+1
    
    // И ограничения, чтобы не видеть другие поезда:
    // Не видеть поезд n: t1 < n*(a+1)
    // Не видеть поезд -1: t0 > -a (выполняется)
    
    // Итого:
    // 0 <= t0 <= 1
    // (n-1)*(a+1) <= t1 < n*(a+1)
    
    // Минимальное время: t1 - t0 + 1. Минимум при t0=1, t1=(n-1)*(a+1).
    // Время = (n-1)*(a+1) - 1 + 1 = (n-1)*(a+1).
    
    // Максимальное время: t1 - t0 + 1. Максимум при t0=0, t1=n*(a+1)-1.
    // Время = n*(a+1) - 1 - 0 + 1 = n*(a+1).
    
    // Проверим на примере a=1, n=5:
    // Поезда в [0,1], [2,3], [4,5], [6,7], [8,9].
    // Минимальное время: t0=1, t1=8. Время=8. Видит поезда с 1 по 8: это поезда в [2,3],[4,5],[6,7],[8,9]? 
    // Нет, t0=1, t1=8 означает присутствие в моменты 1,2,3,4,5,6,7,8.
    // Видит [0,1] (1 пересекается), [2,3], [4,5], [6,7], [8,9] (8 пересекается). Да, 5 поездов.
    
    // Максимальное время: t0=0, t1=9. Время=10. Видит поезда в [0,1],[2,3],[4,5],[6,7],[8,9]. Да, 5 поездов.
    // t1 не должен быть 10, иначе увидит поезд в [10,11].
    
    // Формулы:
    // Минимальное время для одного пути: (n-1)*(a+1)
    // Максимальное время для одного пути: n*(a+1) - 1
    
    // Нет, в примере минимальное время 8 = (5-1)*(1+1) = 4*2 = 8. Верно.
    // Максимальное время 10. Формула n*(a+1) - 1 = 5*2 - 1 = 9. Не совпадает.
    
    // Ошибка в максимальном времени.
    // t1 < n*(a+1). Максимальное целое t1 = n*(a+1) - 1.
    // t0 >= 0. Минимальное t0 = 0.
    // Максимальное время = (n*(a+1) - 1) - 0 + 1 = n*(a+1).
    
    // В примере: 5*(1+1) = 10. Совпадает.
    
    let minTime = (trains - 1) * (interval + 1)
    let maxTime = trains * (interval + 1) - 1
    
    return (minTime, maxTime)
}

// Получаем границы времени для каждого пути
guard let firstPathBounds = getTimeBounds(interval: a, trains: n) else {
    print(-1)
    exit(0)
}

guard let secondPathBounds = getTimeBounds(interval: b, trains: m) else {
    print(-1)
    exit(0)
}

// Находим пересечение временных интервалов
let overallMinTime = max(firstPathBounds.min, secondPathBounds.min)
let overallMaxTime = min(firstPathBounds.max, secondPathBounds.max)

// Проверяем, возможно ли такое время
if overallMinTime > overallMaxTime {
    print(-1)
} else {
    print(overallMinTime, overallMaxTime)
}
```

---

**74. Узник замка Иф**

**Условие задачи:**
За многие годы заточения узник замка Иф проделал в стене прямоугольное отверстие размером `D × E`. Замок Иф сложен из кирпичей, размером `A × B × C`. Определите, сможет ли узник выбрасывать кирпичи в море через это отверстие, если стороны кирпича должны быть параллельны сторонам отверстия.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем размеры кирпича
let brickDimensions = readLine()!.split(separator: " ").map { Int($0)! }
let a = brickDimensions[0]
let b = brickDimensions[1]
let c = brickDimensions[2]

// Считываем размеры отверстия
let holeDimensions = readLine()!.split(separator: " ").map { Int($0)! }
let d = holeDimensions[0]
let e = holeDimensions[1]

// Сортируем размеры кирпича и отверстия
let sortedBrick = [a, b, c].sorted()
let sortedHole = [d, e].sorted()

// Проверяем, можно ли протолкнуть кирпич через отверстие
// Кирпич можно повернуть, поэтому проверяем все возможные ориентации
// Нам нужно, чтобы две самые маленькие стороны кирпича были <= соответствующих сторон отверстия
if sortedBrick[0] <= sortedHole[0] && sortedBrick[1] <= sortedHole[1] {
    print("YES")
} else {
    print("NO")
}
```

---

**72. Возрастает ли список?**

**Условие задачи:**
Дан список. Определите, является ли он монотонно возрастающим (то есть верно ли, что каждый элемент этого списка строго больше предыдущего). Выведите YES, если массив монотонно возрастает и NO в противном случае.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем список чисел
let numbers = readLine()!.split(separator: " ").map { Int($0)! }

// Проверяем, является ли список строго возрастающим
var isIncreasing = true

if numbers.count > 1 {
    for i in 1..<numbers.count {
        if numbers[i] <= numbers[i-1] {
            isIncreasing = false
            break
        }
    }
}

if isIncreasing {
    print("YES")
} else {
    print("NO")
}
```

---

**71. Определить вид последовательности**

**Условие задачи:**
По последовательности чисел во входных данных определите ее вид:
* CONSTANT — последовательность состоит из одинаковых значений
* ASCENDING — последовательность является строго возрастающей
* WEAKLY ASCENDING — последовательность является нестрого возрастающей
* DESCENDING — последовательность является строго убывающей
* WEAKLY DESCENDING — последовательность является нестрого убывающей
* RANDOM — последовательность не принадлежит ни к одному из вышеупомянутых типов

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем последовательность чисел
var numbers: [Int] = []
while let line = readLine(), let number = Int(line), number != -2000000000 {
    numbers.append(number)
}

// Определяем вид последовательности
if numbers.count <= 1 {
    print("CONSTANT")
} else {
    var isConstant = true
    var isAscending = true
    var isWeaklyAscending = true
    var isDescending = true
    var isWeaklyDescending = true
    
    for i in 1..<numbers.count {
        if numbers[i] != numbers[i-1] {
            isConstant = false
        }
        if numbers[i] <= numbers[i-1] {
            isAscending = false
        }
        if numbers[i] < numbers[i-1] {
            isWeaklyAscending = false
        }
        if numbers[i] >= numbers[i-1] {
            isDescending = false
        }
        if numbers[i] > numbers[i-1] {
            isWeaklyDescending = false
        }
    }
    
    if isConstant {
        print("CONSTANT")
    } else if isAscending {
        print("ASCENDING")
    } else if isWeaklyAscending {
        print("WEAKLY ASCENDING")
    } else if isDescending {
        print("DESCENDING")
    } else if isWeaklyDescending {
        print("WEAKLY DESCENDING")
    } else {
        print("RANDOM")
    }
}
```

---

**70. Ближайшее число**

**Условие задачи:**
Напишите программу, которая находит в массиве элемент, самый близкий по величине к данному числу.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем размер массива
let n = Int(readLine()!)!

// Считываем элементы массива
let array = readLine()!.split(separator: " ").map { Int($0)! }

// Считываем число x
let x = Int(readLine()!)!

// Находим элемент, самый близкий к x
var closest = array[0]
var minDifference = abs(array[0] - x)

for i in 1..<array.count {
    let difference = abs(array[i] - x)
    if difference < minDifference {
        minDifference = difference
        closest = array[i]
    }
}

print(closest)
```

---

**69. Больше своих соседей**

**Условие задачи:**
Дан список чисел. Определите, сколько в этом списке элементов, которые больше двух своих соседей. Выведите количество таких элементов.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем список чисел
let numbers = readLine()!.split(separator: " ").map { Int($0)! }

// Подсчитываем количество элементов, больших своих соседей
var count = 0

// Проверяем элементы с 1 по n-2 (у них есть оба соседа)
for i in 1..<(numbers.count - 1) {
    if numbers[i] > numbers[i-1] && numbers[i] > numbers[i+1] {
        count += 1
    }
}

print(count)
```


Хорошо, вот следующие 10 задач из списка с решениями на Swift:

**64. Сапёр**

**Условие задачи:**
Вам необходимо построить поле для игры «Сапер» по его конфигурации — размерам и координатам расставленных на нём мин. Поле состоит из клеток с минами и пустых клеток. Клетки с миной обозначаются символом `*`. Пустые клетки содержат число `k_{i,j}`, `0 ≤ k_{i,j} ≤ 8` — количество мин на соседних клетках. Соседними клетками являются восемь клеток, имеющих смежный угол или сторону.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем размеры поля и количество мин
let firstLine = readLine()!.split(separator: " ").map { Int($0)! }
let n = firstLine[0] // Количество строк
let m = firstLine[1] // Количество столбцов
let k = firstLine[2] // Количество мин

// Создаем поле и инициализируем его нулями
var field: [[Int]] = Array(repeating: Array(repeating: 0, count: m), count: n)

// Создаем булев массив для отметки мин
var mines: [[Bool]] = Array(repeating: Array(repeating: false, count: m), count: n)

// Считываем координаты мин и отмечаем их
for _ in 0..<k {
    let mineCoords = readLine()!.split(separator: " ").map { Int($0)! - 1 } // Переводим в 0-индексацию
    let row = mineCoords[0]
    let col = mineCoords[1]
    mines[row][col] = true
}

// Для каждой клетки без мины подсчитываем количество соседних мин
let directions = [(-1, -1), (-1, 0), (-1, 1), (0, -1), (0, 1), (1, -1), (1, 0), (1, 1)]

for i in 0..<n {
    for j in 0..<m {
        if !mines[i][j] { // Если в клетке нет мины
            var count = 0
            for (dx, dy) in directions {
                let ni = i + dx
                let nj = j + dy
                if ni >= 0 && ni < n && nj >= 0 && nj < m && mines[ni][nj] {
                    count += 1
                }
            }
            field[i][j] = count
        }
    }
}

// Выводим поле
for i in 0..<n {
    for j in 0..<m {
        if mines[i][j] {
            print("*", terminator: " ")
        } else {
            print(field[i][j], terminator: " ")
        }
    }
    print() // Новая строка после каждой строки поля
}
```

---

**62. Количество различных чисел**

**Условие задачи:**
Дан список чисел, который может содержать до 100000 чисел. Определите, сколько в нём встречается различных чисел.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем список чисел
let numbers = readLine()!.split(separator: " ").map { Int($0)! }

// Используем Set для хранения уникальных чисел
let uniqueNumbers = Set(numbers)

// Выводим количество уникальных чисел
print(uniqueNumbers.count)
```

---

**61. Пересечение множеств**

**Условие задачи:**
Даны два списка чисел, которые могут содержать до 10000 чисел каждый. Выведите все числа, которые входят как в первый, так и во второй список в порядке возрастания.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем первый список чисел
let list1 = Set(readLine()!.split(separator: " ").map { Int($0)! })

// Считываем второй список чисел
let list2 = Set(readLine()!.split(separator: " ").map { Int($0)! })

// Находим пересечение множеств
let intersection = list1.intersection(list2).sorted()

// Выводим результат
print(intersection.map { String($0) }.joined(separator: " "))
```

---

**60. Кубики**

**Условие задачи:**
Аня и Боря любят играть в разноцветные кубики. У каждого из них свой набор, и в каждом наборе все кубики различны по цвету. Номер любого цвета — это целое число в пределах от 0 до 10⁹. Определите:
1. Количество цветов, которые есть в обоих наборах.
2. Отсортированные номера этих цветов.
3. Количество цветов, которые есть только у Ани.
4. Отсортированные номера этих цветов.
5. Количество цветов, которые есть только у Бори.
6. Отсортированные номера этих цветов.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем количество кубиков у Ани и Бори
let nm = readLine()!.split(separator: " ").map { Int($0)! }
let n = nm[0] // Количество кубиков у Ани
let m = nm[1] // Количество кубиков у Бори

// Считываем номера цветов кубиков Ани
var annaColors: Set<Int> = []
for _ in 0..<n {
    annaColors.insert(Int(readLine()!)!)
}

// Считываем номера цветов кубиков Бори
var borisColors: Set<Int> = []
for _ in 0..<m {
    borisColors.insert(Int(readLine()!)!)
}

// Находим цвета, которые есть у обоих
let commonColors = annaColors.intersection(borisColors).sorted()
print(commonColors.count)
if !commonColors.isEmpty {
    print(commonColors.map { String($0) }.joined(separator: " "))
}

// Находим цвета, которые есть только у Ани
let onlyAnnaColors = annaColors.subtracting(borisColors).sorted()
print(onlyAnnaColors.count)
if !onlyAnnaColors.isEmpty {
    print(onlyAnnaColors.map { String($0) }.joined(separator: " "))
}

// Находим цвета, которые есть только у Бори
let onlyBorisColors = borisColors.subtracting(annaColors).sorted()
print(onlyBorisColors.count)
if !onlyBorisColors.isEmpty {
    print(onlyBorisColors.map { String($0) }.joined(separator: " "))
}
```

---

**58. OpenCalculator**

**Условие задачи:**
В новой программе OpenCalculator появилась новая возможность – можно настроить, какие кнопки отображаются. Петя настроил калькулятор так, что он отображает только кнопки с цифрами x, y, z. Определите, сможет ли Петя ввести число N, а если нет, то какое минимальное количество кнопок надо дополнительно отобразить на экране для его ввода.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем доступные цифры
let availableDigits = Set(readLine()!.split(separator: " ").map { Int(String($0.first!))! })

// Считываем число N
let n = readLine()!

// Находим уникальные цифры в числе N
let requiredDigits = Set(n.compactMap { Int(String($0)) })

// Находим цифры, которых не хватает
let missingDigits = requiredDigits.subtracting(availableDigits)

// Выводим количество недостающих цифр
print(missingDigits.count)
```

---

**57. Инопланетный геном**

**Условие задачи:**
Геном жителей системы Тау Кита содержит 26 видов оснований, для обозначения которых будем использовать буквы латинского алфавита от A до Z. Важную роль в геноме играют пары соседних оснований. Степенью близости одного генома другому геному называется количество пар соседних оснований первого генома, которые встречаются во втором геноме. Определите степень близости первого генома второму геному.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем первый геном
let genome1 = readLine()!

// Считываем второй геном
let genome2 = readLine()!

// Создаем множество пар оснований из второго генома
var pairsGenome2: Set<String> = []
for i in 0..<genome2.count - 1 {
    let startIndex = genome2.index(genome2.startIndex, offsetBy: i)
    let endIndex = genome2.index(startIndex, offsetBy: 2)
    let pair = String(genome2[startIndex..<endIndex])
    pairsGenome2.insert(pair)
}

// Подсчитываем количество пар из первого генома, которые есть во втором
var similarity = 0
for i in 0..<genome1.count - 1 {
    let startIndex = genome1.index(genome1.startIndex, offsetBy: i)
    let endIndex = genome1.index(startIndex, offsetBy: 2)
    let pair = String(genome1[startIndex..<endIndex])
    if pairsGenome2.contains(pair) {
        similarity += 1
    }
}

// Выводим степень близости
print(similarity)
```

---

**55. Злые свинки**

**Условие задачи:**
Птицы в игре представляются точками на плоскости. Выстрел сбивает только ближайшую птицу находящуюся на линии огня. При этом сбитая птица, падая, сбивает всех птиц, находящихся ровно под ней. Две птицы не могут находиться в одной точке. По заданному расположению птиц необходимо определить, какое минимальное количество выстрелов необходимо, чтобы все птицы были сбиты.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем количество птиц
let n = Int(readLine()!)!

// Словарь для хранения множества y-координат для каждой x-координаты
var xCoords: [Int: Set<Int>] = [:]

// Считываем координаты птиц
for _ in 0..<n {
    let coords = readLine()!.split(separator: " ").map { Int($0)! }
    let x = coords[0]
    let y = coords[1]
    
    if xCoords[x] == nil {
        xCoords[x] = Set<Int>()
    }
    xCoords[x]!.insert(y)
}

// Минимальное количество выстрелов равно количеству уникальных x-координат
print(xCoords.count)
```

---

**54. Полиглоты**

**Условие задачи:**
Каждый из N школьников некоторой школы знает M_i языков. Определите, какие языки знают все школьники и языки, которые знает хотя бы один из школьников.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем количество школьников
let n = Int(readLine()!)!

var allLanguages: Set<String> = Set<String>()
var commonLanguages: Set<String>? = nil

// Обрабатываем каждого школьника
for i in 0..<n {
    // Считываем количество языков у школьника
    let m = Int(readLine()!)!
    
    // Считываем языки школьника
    var studentLanguages: Set<String> = Set<String>()
    for _ in 0..<m {
        studentLanguages.insert(readLine()!)
    }
    
    // Добавляем языки в множество всех языков
    allLanguages = allLanguages.union(studentLanguages)
    
    // Для первого школьника инициализируем commonLanguages
    if commonLanguages == nil {
        commonLanguages = studentLanguages
    } else {
        // Находим пересечение с языками предыдущих школьников
        commonLanguages = commonLanguages!.intersection(studentLanguages)
    }
}

// Выводим результаты
let common = commonLanguages!.sorted()
print(common.count)
for language in common {
    print(language)
}

let all = allLanguages.sorted()
print(all.count)
for language in all {
    print(language)
}
```

---

**52. Словарь синонимов**

**Условие задачи:**
Вам дан словарь, состоящий из пар слов. Каждое слово является синонимом к парному ему слову. Все слова в словаре различны. Для одного данного слова определите его синоним.

**Решение задачи на Swift:**

```swift
import Foundation

// Считываем количество пар синонимов
let n = Int(readLine()!)!

// Создаем словарь для хранения синонимов
var synonyms: [String: String] = [:]

// Считываем пары синонимов
for _ in 0..<n {
    let pair = readLine()!.split(separator: " ").map { String($0) }
    let word1 = pair[0]
    let word2 = pair[1]
    
    synonyms[word1] = word2
    synonyms[word2] = word1
}

// Считываем слово, для которого нужно найти синоним
let word = readLine()!

// Выводим синоним
print(synonyms[word]!)
```

---

**51. Номер появления слова**

**Условие задачи:**
Во входном файле записан текст. Словом считается последовательность непробельных символов идущих подряд, слова разделены одним или большим числом пробелов или символами конца строки. Для каждого слова из этого текста подсчитайте, сколько раз оно встречалось в этом тексте ранее.

**Решение задачи на Swift:**

```swift
import Foundation

// Словарь для хранения количества появлений каждого слова
var wordCount: [String: Int] = [:]

// Читаем весь текст
var text = ""
while let line = readLine() {
    text += line + " "
}

// Разбиваем текст на слова и обрабатываем их
let words = text.split(separator: " ").map { String($0) }

for word in words {
    let count = wordCount[word, default: 0]
    print(count)
    wordCount[word, default: 0] += 1
}
```


