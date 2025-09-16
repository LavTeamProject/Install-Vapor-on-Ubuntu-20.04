## 356. Line Reflection

**Условие задачи:**  
Дано n точек на двумерной плоскости, определите, существует ли такая прямая, параллельная оси y, что отражение заданных точек относительно этой прямой даст те же точки.

**Идея решения:**  
Если существует линия, параллельная оси y, относительно которой точки симметричны, то эта линия должна проходить по середине между самыми левыми и самыми правыми точками. Для каждой точки (x, y) должна существовать соответствующая точка (2 * mid - x, y), где mid — координата x центральной линии. Используем множество для быстрой проверки наличия отражённых точек.

**Оценка сложности:**  
Время: O(n), Память: O(n)

**Решение задачи (Swift):**  
```swift
func isReflected(_ points: [[Int]]) -> Bool {
    guard !points.isEmpty else { return true }
    
    let min_x = points.map { $0[0] }.min()!
    let max_x = points.map { $0[0] }.max()!
    let mid = (min_x + max_x) / 2.0
    
    let set = Set(points.map { ($0[0], $0[1]) })
    
    for point in points {
        let reflectedX = 2 * mid - Double(point[0])
        if !set.contains((Int(reflectedX), point[1])) {
            return false
        }
    }
    
    return true
}
```


## 1493. Longest Subarray of 1's After Deleting One Element

**Условие задачи:**  
Дан двоичный массив `nums`, нужно удалить один элемент из него.  
Верните размер наибольшего непустого подмассива, содержащего только единицы, в полученном массиве. Если такого подмассива нет, верните 0.

**Идея решения:**  
Используем слайдинг-окно (двух указателей), разрешая максимум одну "ошибку" (то есть один ноль). Для каждого положения окна считаем длину подмассива, содержащего не более одного нуля. Максимальная длина при этом будет ответом.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift):**  
```swift
func longestSubarray(_ nums: [Int]) -> Int {
    var left = 0
    var zeros = 0
    var maxLength = 0
    
    for right in 0..<nums.count {
        if nums[right] == 0 {
            zeros += 1
        }
        
        while zeros > 1 {
            if nums[left] == 0 {
                zeros -= 1
            }
            left += 1
        }
        
        maxLength = max(maxLength, right - left)
    }
    
    return maxLength
}
```


## 228. Summary Ranges

**Условие задачи:**  
Дан отсортированный массив уникальных целых чисел `nums`.  
Промежуток `[a, b]` — это множество всех целых чисел от `a` до `b` (включительно).  

Верните наименьший отсортированный список промежутков, которые точно покрывают все числа в массиве. То есть каждый элемент массива должен быть покрыт ровно одним из промежутков, и не должно существовать такого целого числа `x`, которое находится в одном из промежутков, но не в массиве.

Каждый промежуток `[a, b]` должен быть представлен как:
- `"a->b"`, если `a != b`
- `"a"`, если `a == b`

**Идея решения:**  
Проходим по массиву и объединяем последовательные числа в диапазоны. Если текущее число на 1 больше предыдущего, продолжаем текущий диапазон. Иначе закрываем текущий диапазон и начинаем новый.

**Оценка сложности:**  
Время: O(n), Память: O(1) (если не считать выходной массив)

**Решение задачи (Swift):**  
```swift
func summaryRanges(_ nums: [Int]) -> [String] {
    var result: [String] = []
    guard !nums.isEmpty else { return result }
    
    var start = nums[0]
    
    for i in 1..<nums.count {
        if nums[i] != nums[i - 1] + 1 {
            if start == nums[i - 1] {
                result.append("\(start)")
            } else {
                result.append("\(start)->\(nums[i - 1])")
            }
            start = nums[i]
        }
    }
    
    if start == nums.last! {
        result.append("\(start)")
    } else {
        result.append("\(start)->\(nums.last!)")
    }
    
    return result
}
```


## 443. String Compression

**Условие задачи:**  
Дан массив символов `chars`, сожмите его по следующему алгоритму:  
Начните с пустой строки `s`. Для каждой группы последовательных одинаковых символов в `chars`:  
- Если длина группы равна 1, добавьте символ в `s`.  
- Иначе добавьте символ и длину группы.  

Сжатая строка `s` не должна возвращаться отдельно, а должна быть записана обратно в исходный массив `chars`.  
Обратите внимание, что длины групп, равные 10 или более, будут разбиты на несколько символов в `chars`.  

После модификации массива верните новую его длину.  
Алгоритм должен использовать только постоянное дополнительное пространство.  

Примечание: Символы в массиве за пределами возвращённой длины не имеют значения и могут быть проигнорированы.

**Идея решения:**  
Проходим по массиву, подсчитывая длину каждой группы одинаковых символов. Затем пишем результат непосредственно в массив `chars`, используя указатель для записи. При этом преобразуем длину группы в символы (например, 12 → '1', '2') и добавляем их в массив.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift):**  
```swift
func compress(_ chars: inout [Character]) -> Int {
    guard !chars.isEmpty else { return 0 }
    
    var writeIndex = 0
    var readIndex = 0
    
    while readIndex < chars.count {
        let currentChar = chars[readIndex]
        var count = 0
        var tempReadIndex = readIndex
        
        // Подсчитываем длину текущей группы
        while tempReadIndex < chars.count && chars[tempReadIndex] == currentChar {
            count += 1
            tempReadIndex += 1
        }
        
        // Записываем символ
        chars[writeIndex] = currentChar
        writeIndex += 1
        
        // Записываем длину, если она > 1
        if count > 1 {
            let countString = String(count)
            for char in countString {
                chars[writeIndex] = char
                writeIndex += 1
            }
        }
        
        readIndex = tempReadIndex
    }
    
    return writeIndex
}
```

---

## 281. Zigzag Iterator

**Условие задачи:**  
Даны два массива целых чисел `v1` и `v2`. Реализуйте итератор, который возвращает элементы этих массивов поочерёдно.

Реализуйте класс `ZigzagIterator`:
- `ZigzagIterator(List<int> v1, List<int> v2)` — инициализирует объект двумя массивами.
- `boolean hasNext()` — возвращает `true`, если у итератора есть ещё элементы, иначе `false`.
- `int next()` — возвращает текущий элемент и перемещает итератор к следующему.

**Идея решения:**  
Используем два указателя для отслеживания текущей позиции в каждом массиве и переменную для определения, из какого массива брать следующий элемент (по очереди). Переключаемся между массивами при каждом вызове `next()`.

**Оценка сложности:**  
Время: O(1) для `hasNext()` и `next()`, Память: O(1)

**Решение задачи (Swift):**  
```swift
class ZigzagIterator {
    private var v1: [Int]
    private var v2: [Int]
    private var index1 = 0
    private var index2 = 0
    private var turn = 0 // 0 - взять из v1, 1 - из v2
    
    init(_ v1: [Int], _ v2: [Int]) {
        self.v1 = v1
        self.v2 = v2
    }
    
    func next() -> Int {
        if index1 < v1.count && (index2 >= v2.count || turn == 0) {
            turn = 1
            return v1[index1++]
        } else if index2 < v2.count {
            turn = 0
            return v2[index2++]
        }
        return 0 // never reached
    }
    
    func hasNext() -> Bool {
        return index1 < v1.count || index2 < v2.count
    }
}
```

## 125. Valid Palindrome

**Условие задачи:**  
Фраза является палиндромом, если после преобразования всех заглавных букв в строчные и удаления всех неалфавитно-цифровых символов, она читается одинаково слева направо и справа налево. Алфавитно-цифровые символы — это буквы и цифры.

Данная строка `s`, верните `true`, если она является палиндромом, иначе `false`.

**Идея решения:**  
Используем два указателя: один с начала строки, другой с конца. Пропускаем все неалфавитно-цифровые символы и сравниваем символы в нижнем регистре. Если они не совпадают — строка не палиндром.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift):**  
```swift
func isPalindrome(_ s: String) -> Bool {
    var left = s.startIndex
    var right = s.index(s.endIndex, offsetBy: -1)
    
    while left < right {
        // Пропускаем неалфавитно-цифровые символы слева
        while left < right && !s[left].isLetter && !s[left].isNumber {
            left = s.index(after: left)
        }
        
        // Пропускаем неалфавитно-цифровые символы справа
        while left < right && !s[right].isLetter && !s[right].isNumber {
            right = s.index(before: right)
        }
        
        // Сравниваем символы в нижнем регистре
        if s[left].lowercased() != s[right].lowercased() {
            return false
        }
        
        left = s.index(after: left)
        right = s.index(before: right)
    }
    
    return true
}
```

---

## 161. One Edit Distance

**Условие задачи:**  
Даны две строки `s` и `t`. Определите, находятся ли они на расстоянии редактирования в одну операцию.

**Идея решения:**  
Сравниваем строки посимвольно. Если длины отличаются более чем на 1 — сразу возвращаем `false`. Если длины равны — проверяем количество различий (должно быть ровно 1). Если одна строка длиннее — пропускаем один символ в длинной строке и продолжаем сравнение.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift):**  
```swift
func isOneEditDistance(_ s: String, _ t: String) -> Bool {
    let lenS = s.count
    let lenT = t.count
    
    // Разница в длинах должна быть не более 1
    if abs(lenS - lenT) > 1 {
        return false
    }
    
    // Обеспечим, что s всегда короче или равна t
    if lenS > lenT {
        return isOneEditDistance(t, s)
    }
    
    let charsS = Array(s)
    let charsT = Array(t)
    
    var i = 0
    var j = 0
    var foundDiff = false
    
    while i < lenS && j < lenT {
        if charsS[i] != charsT[j] {
            if foundDiff {
                return false
            }
            foundDiff = true
            
            // Если строки разной длины, пропустим символ в более длинной
            if lenS != lenT {
                j += 1
            } else {
                i += 1
                j += 1
            }
        } else {
            i += 1
            j += 1
        }
    }
    
    // Если разница в длинах, то последний символ в более длинной строке должен быть единственным отличием
    return foundDiff || (lenS != lenT && !foundDiff)
}
```

---

## 560. Subarray Sum Equals K

**Условие задачи:**  
Дан массив целых чисел `nums` и целое число `k`. Верните общее количество подмассивов, сумма элементов которых равна `k`.

Подмассив — это непрерывная непустая последовательность элементов в массиве.

**Идея решения:**  
Используем префиксные суммы и хэш-таблицу для хранения частоты каждого префиксного суммы. Для текущей суммы `currentSum`, если `currentSum - k` уже встречался, значит, есть подмассив, сумма которого равна `k`.

**Оценка сложности:**  
Время: O(n), Память: O(n)

**Решение задачи (Swift):**  
```swift
func subarraySum(_ nums: [Int], _ k: Int) -> Int {
    var count = 0
    var currentSum = 0
    var sumCount: [Int: Int] = [0: 1] // Инициализируем с 0:1 для случая, когда подмассив начинается с начала
    
    for num in nums {
        currentSum += num
        
        if let prevCount = sumCount[currentSum - k] {
            count += prevCount
        }
        
        sumCount[currentSum, default: 0] += 1
    }
    
    return count
}
```

---

## 283. Move Zeroes

**Условие задачи:**  
Дан массив целых чисел `nums`. Переместите все нули в конец массива, сохраняя относительный порядок ненулевых элементов.

Обратите внимание: необходимо выполнить это *in-place*, без создания копии массива.

**Идея решения:**  
Используем двухуказательный метод. Один указатель (`writeIndex`) отслеживает позицию, куда нужно записать следующий ненулевой элемент. Проходим по массиву, и если элемент не ноль, перемещаем его на позицию `writeIndex` и увеличиваем `writeIndex`.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift):**  
```swift
func moveZeroes(_ nums: inout [Int]) {
    var writeIndex = 0
    
    for readIndex in 0..<nums.count {
        if nums[readIndex] != 0 {
            nums[writeIndex] = nums[readIndex]
            writeIndex += 1
        }
    }
    
    // Заполняем оставшиеся позиции нулями
    while writeIndex < nums.count {
        nums[writeIndex] = 0
        writeIndex += 1
    }
}
```


## 49. Group Anagrams

**Условие задачи:**  
Дан массив строк `strs`. Сгруппируйте анаграммы вместе.  
Вы можете вернуть ответ в любом порядке.

Анаграмма — это строка, которая может быть получена перестановкой букв другой строки.

**Идея решения:**  
Анаграммы имеют одинаковые буквы в одинаковых количествах. Поэтому можно использовать сортировку символов в строке как ключ для хэш-таблицы. Все строки, которые при сортировке дают одинаковый результат, являются анаграммами.

**Оценка сложности:**  
Время: O(n * m log m), где n — количество строк, m — средняя длина строки.  
Память: O(n * m)

**Решение задачи (Swift):**  
```swift
func groupAnagrams(_ strs: [String]) -> [[String]] {
    var map: [String: [String]] = [:]
    
    for str in strs {
        let sortedStr = String(str.sorted())
        map[sortedStr, default: []].append(str)
    }
    
    return Array(map.values)
}
```

---

## 380. Insert Delete GetRandom O(1)

**Условие задачи:**  
Реализуйте класс `RandomizedSet`, который поддерживает следующие операции со средней временной сложностью O(1):

- `RandomizedSet()` — инициализирует объект.
- `insert(int val)` — добавляет элемент `val` в множество, если он отсутствует. Возвращает `true`, если элемент был добавлен, иначе `false`.
- `remove(int val)` — удаляет элемент `val` из множества, если он существует. Возвращает `true`, если элемент был удалён, иначе `false`.
- `getRandom()` — возвращает случайный элемент из текущего множества. Каждый элемент должен иметь равную вероятность быть выбран.

**Идея решения:**  
Используем массив для быстрого доступа к случайному элементу (`getRandom`) и хэш-таблицу (словарь) для быстрого поиска и удаления. При удалении элемента меняем его местом с последним элементом в массиве, чтобы сохранить O(1) время удаления.

**Оценка сложности:**  
Время: O(1) для всех операций в среднем, Память: O(n)

**Решение задачи (Swift):**  
```swift
class RandomizedSet {
    private var nums: [Int] = []
    private var indexMap: [Int: Int] = [:] // значение -> индекс в массиве
    
    init() {}
    
    func insert(_ val: Int) -> Bool {
        if indexMap.contains(where: { $0.key == val }) {
            return false
        }
        
        nums.append(val)
        indexMap[val] = nums.count - 1
        return true
    }
    
    func remove(_ val: Int) -> Bool {
        guard let index = indexMap[val] else {
            return false
        }
        
        // Заменяем элемент на последний
        let lastVal = nums[nums.count - 1]
        nums[index] = lastVal
        indexMap[lastVal] = index
        
        // Удаляем последний элемент
        nums.removeLast()
        indexMap.removeValue(forKey: val)
        
        return true
    }
    
    func getRandom() -> Int {
        let randomIndex = Int.random(in: 0..<nums.count)
        return nums[randomIndex]
    }
}
```

---

## 146. LRU Cache

**Условие задачи:**  
Спроектируйте структуру данных, которая соответствует ограничениям кэша Least Recently Used (LRU).

Реализуйте класс `LRUCache`:

- `LRUCache(int capacity)` — инициализирует кэш заданной ёмкости.
- `int get(int key)` — возвращает значение по ключу, если оно существует, иначе возвращает -1.
- `void put(int key, int value)` — обновляет значение по ключу или добавляет новую пару. Если число ключей превышает ёмкость, удаляется наименее недавно использованный ключ.

Функции `get` и `put` должны выполняться за O(1) в среднем.

**Идея решения:**  
Используем двусвязный список для хранения порядка использования (последний элемент — наиболее недавно использованный) и хэш-таблицу для быстрого доступа по ключу. При каждом `get` или `put` перемещаем элемент в конец списка.

**Оценка сложности:**  
Время: O(1) для `get` и `put`, Память: O(capacity)

**Решение задачи (Swift):**  
```swift
class LRUCache {
    private class Node {
        var key: Int
        var value: Int
        var prev: Node?
        var next: Node?
        
        init(_ key: Int, _ value: Int) {
            self.key = key
            self.value = value
        }
    }
    
    private var cache: [Int: Node] = [:]
    private var head: Node = Node(0, 0)
    private var tail: Node = Node(0, 0)
    private var capacity: Int
    
    init(_ capacity: Int) {
        self.capacity = capacity
        head.next = tail
        tail.prev = head
    }
    
    func get(_ key: Int) -> Int {
        guard let node = cache[key] else {
            return -1
        }
        
        // Перемещаем узел в конец (наиболее недавно использованный)
        remove(node)
        appendToTail(node)
        return node.value
    }
    
    func put(_ key: Int, _ value: Int) {
        if let node = cache[key] {
            node.value = value
            remove(node)
            appendToTail(node)
        } else {
            let newNode = Node(key, value)
            cache[key] = newNode
            appendToTail(newNode)
            
            if cache.count > capacity {
                // Удаляем наименее недавно использованный (первый после head)
                let toRemove = head.next!
                remove(toRemove)
                cache.removeValue(forKey: toRemove.key)
            }
        }
    }
    
    private func remove(_ node: Node) {
        node.prev?.next = node.next
        node.next?.prev = node.prev
    }
    
    private func appendToTail(_ node: Node) {
        node.prev = tail.prev
        node.next = tail
        tail.prev?.next = node
        tail.prev = node
    }
}
```



## 22. Generate Parentheses

**Условие задачи:**  
Дано `n` пар скобок, напишите функцию для генерации всех возможных комбинаций корректно сформированных скобок.

**Идея решения:**  
Используем рекурсию (backtracking). В каждый момент времени можем добавить открывающую скобку, если количество открытых ещё меньше `n`, и закрывающую, если количество закрывающих меньше количества открывающих. Это гарантирует корректность скобочной последовательности.

**Оценка сложности:**  
Время: O(4^n / √n) — количество корректных скобочных последовательностей (числа Каталана), Память: O(n) — глубина рекурсии.

**Решение задачи (Swift):**  
```swift
func generateParenthesis(_ n: Int) -> [String] {
    var result: [String] = []
    
    func backtrack(_ current: String, _ open: Int, _ close: Int) {
        if current.count == 2 * n {
            result.append(current)
            return
        }
        
        if open < n {
            backtrack(current + "(", open + 1, close)
        }
        
        if close < open {
            backtrack(current + ")", open, close + 1)
        }
    }
    
    backtrack("", 0, 0)
    return result
}
```

---

## 206. Reverse Linked List

**Условие задачи:**  
Дан указатель на голову односвязного списка. Переверните список и верните перевёрнутый список.

**Идея решения:**  
Проходим по списку, сохраняя предыдущий узел. Для каждого узла меняем направление указателя `next` на предыдущий узел. Используем три указателя: `prev`, `curr`, `next`.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift):**  
```swift
class ListNode {
    var val: Int
    var next: ListNode?
    init(_ val: Int) {
        self.val = val
        self.next = nil
    }
}

func reverseList(_ head: ListNode?) -> ListNode? {
    var prev: ListNode? = nil
    var curr = head
    
    while curr != nil {
        let next = curr?.next
        curr?.next = prev
        prev = curr
        curr = next
    }
    
    return prev
}
```

---

## 567. Permutation in String

**Условие задачи:**  
Даны две строки `s1` и `s2`. Верните `true`, если `s2` содержит перестановку `s1`, иначе `false`.  
Другими словами, верните `true`, если одна из перестановок `s1` является подстрокой в `s2`.

**Идея решения:**  
Используем слайдинг-окно размером `s1.length`. Сравниваем частоту символов в окне с частотой символов в `s1`. Если совпадают — значит, есть перестановка.

**Оценка сложности:**  
Время: O(n), где n — длина `s2`, Память: O(1) (только 26 букв)

**Решение задачи (Swift):**  
```swift
func checkInclusion(_ s1: String, _ s2: String) -> Bool {
    let len1 = s1.count
    let len2 = s2.count
    guard len1 <= len2 else { return false }
    
    var count1 = [Int](repeating: 0, count: 26)
    var count2 = [Int](repeating: 0, count: 26)
    
    // Подсчитываем частоты символов в s1
    for char in s1 {
        count1[Int(char.asciiValue! - Character("a").asciiValue!)] += 1
    }
    
    // Слайдинг-окно
    var left = 0
    for right in 0..<len2 {
        let charRight = s2[s2.index(s2.startIndex, offsetBy: right)]
        count2[Int(charRight.asciiValue! - Character("a").asciiValue!)] += 1
        
        // Если окно достигло нужной длины
        if right - left + 1 == len1 {
            if count1 == count2 {
                return true
            }
            
            let charLeft = s2[s2.index(s2.startIndex, offsetBy: left)]
            count2[Int(charLeft.asciiValue! - Character("a").asciiValue!)] -= 1
            left += 1
        }
    }
    
    return false
}
```

---

## 23. Merge k Sorted Lists

**Условие задачи:**  
Дан массив `k` отсортированных односвязных списков. Объедините все списки в один отсортированный список и верните его.

**Идея решения:**  
Используем мин-кучу (priority queue). Добавляем первый элемент каждого списка в кучу. Затем извлекаем минимальный элемент, добавляем его в результат и помещаем следующий элемент из того же списка в кучу.

**Оценка сложности:**  
Время: O(N log k), где N — общее количество узлов, k — количество списков.  
Память: O(k) — для хранения k элементов в куче.

**Решение задачи (Swift):**  
```swift
import Foundation

class ListNode {
    var val: Int
    var next: ListNode?
    init(_ val: Int) {
        self.val = val
        self.next = nil
    }
}

func mergeKLists(_ lists: [ListNode?]) -> ListNode? {
    guard !lists.isEmpty else { return nil }
    
    var heap = MinHeap<ListNode>()
    
    // Добавляем головы всех списков в кучу
    for list in lists where list != nil {
        heap.insert(list!)
    }
    
    let dummy = ListNode(0)
    var current = dummy
    
    while !heap.isEmpty {
        let node = heap.extractMin()
        current.next = node
        current = node
        
        if let nextNode = node.next {
            heap.insert(nextNode)
        }
    }
    
    return dummy.next
}

// Простая реализация MinHeap для ListNode
struct MinHeap<T: Comparable> {
    private var heap: [T] = []
    
    mutating func insert(_ value: T) {
        heap.append(value)
        siftUp(heap.count - 1)
    }
    
    mutating func extractMin() -> T {
        guard !heap.isEmpty else { fatalError("Heap is empty") }
        let min = heap[0]
        heap[0] = heap.removeLast()
        if !heap.isEmpty {
            siftDown(0)
        }
        return min
    }
    
    private mutating func siftUp(_ index: Int) {
        var child = index
        while child > 0 {
            let parent = (child - 1) / 2
            if heap[parent] <= heap[child] {
                break
            }
            heap.swapAt(parent, child)
            child = parent
        }
    }
    
    private mutating func siftDown(_ index: Int) {
        var parent = index
        while true {
            let leftChild = 2 * parent + 1
            let rightChild = 2 * parent + 2
            var candidate = parent
            
            if leftChild < heap.count && heap[leftChild] < heap[candidate] {
                candidate = leftChild
            }
            if rightChild < heap.count && heap[rightChild] < heap[candidate] {
                candidate = rightChild
            }
            
            if candidate == parent {
                break
            }
            heap.swapAt(parent, candidate)
            parent = candidate
        }
    }
    
    var isEmpty: Bool { heap.isEmpty }
}
```

## 933. Number of Recent Calls

**Условие задачи:**  
У вас есть класс `RecentCounter`, который подсчитывает количество запросов за определённый временной интервал.

Реализуйте класс `RecentCounter`:
- `RecentCounter()` — инициализирует счётчик с нулевым количеством последних запросов.
- `int ping(int t)` — добавляет новый запрос в момент времени `t`, где `t` — время в миллисекундах, и возвращает количество запросов, произошедших за последние 3000 миллисекунд (включая новый запрос). Конкретно, верните количество запросов, которые произошли в диапазоне `[t - 3000, t]`.

Гарантируется, что каждый вызов `ping` использует значение `t`, строго большее предыдущего.

**Идея решения:**  
Используем очередь (deque), чтобы хранить временные метки запросов. При каждом вызове `ping(t)` добавляем `t` в конец очереди и удаляем все элементы из начала, которые меньше `t - 3000`. Размер очереди — это ответ.

**Оценка сложности:**  
Время: O(1) в среднем на операцию `ping`, Память: O(n), где n — количество запросов.

**Решение задачи (Swift, комментариями в коде):**  
```swift
class RecentCounter {
    private var queue: [Int] = [] // Очередь для хранения временных меток запросов
    
    init() {}
    
    func ping(_ t: Int) -> Int {
        // Добавляем текущую временную метку в конец очереди
        queue.append(t)
        
        // Удаляем все запросы, которые старше чем t - 3000
        while queue.first! < t - 3000 {
            queue.removeFirst()
        }
        
        // Возвращаем количество запросов в текущем окне
        return queue.count
    }
}
```

---

## 20. Valid Parentheses

**Условие задачи:**  
Дана строка `s`, содержащая только символы `'('`, `')'`, `'{'`, `'}'`, `'['` и `']'`. Определите, является ли входная строка корректной.

Строка считается корректной, если:
1. Открывающие скобки должны закрываться скобками того же типа.
2. Открывающие скобки должны закрываться в правильном порядке.
3. Каждая закрывающая скобка имеет соответствующую открывающую скобку того же типа.

**Идея решения:**  
Используем стек. Проходим по строке: при встрече открывающей скобки добавляем её в стек. При закрывающей — проверяем, совпадает ли она с верхним элементом стека. Если нет — строка некорректна. После прохода стек должен быть пуст.

**Оценка сложности:**  
Время: O(n), Память: O(n)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func isValid(_ s: String) -> Bool {
    var stack: [Character] = [] // Стек для хранения открывающих скобок
    let pairs: [Character: Character] = [
        ')': '(',
        '}': '{',
        ']': '['
    ]
    
    for char in s {
        // Если это открывающая скобка — добавляем в стек
        if char == "(" || char == "{" || char == "[" {
            stack.append(char)
        } else {
            // Это закрывающая скобка
            // Если стек пуст или верхняя скобка не соответствует — строка некорректна
            if stack.isEmpty || stack.popLast() != pairs[char] {
                return false
            }
        }
    }
    
    // Если стек пуст — все скобки были правильно закрыты
    return stack.isEmpty
}
```

---

## 487. Max Consecutive Ones II

**Условие задачи:**  
Дан двоичный массив. Найдите максимальное количество последовательных единиц в этом массиве, если вы можете перевернуть не более одного нуля.

**Идея решения:**  
Используем слайдинг-окно с одним разрешённым нулём. Поддерживаем два указателя: левый и правый. Увеличиваем правый пока не встретим второй ноль. Тогда двигаем левый, пока не уберём первый ноль. Сохраняем максимум длины окна.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func findMaxConsecutiveOnes(_ nums: [Int]) -> Int {
    var left = 0
    var zeroCount = 0
    var maxLength = 0
    
    for right in 0..<nums.count {
        // Если текущий элемент — ноль, увеличиваем счётчик нулей
        if nums[right] == 0 {
            zeroCount += 1
        }
        
        // Если больше одного нуля — сдвигаем левый указатель
        while zeroCount > 1 {
            if nums[left] == 0 {
                zeroCount -= 1
            }
            left += 1
        }
        
        // Обновляем максимальную длину
        maxLength = max(maxLength, right - left + 1)
    }
    
    return maxLength
}
```

---

## 849. Maximize Distance to Closest Person

**Условие задачи:**  
Дан массив, представляющий ряд мест, где `seats[i] = 1` означает, что человек сидит на i-м месте, а `seats[i] = 0` — что i-е место свободно (индексация с 0).

Существует хотя бы одно свободное место и хотя бы один человек, сидящий.

Алекс хочет сесть так, чтобы расстояние до ближайшего человека было максимально возможным.

Верните это максимальное расстояние до ближайшего человека.

**Идея решения:**  
Рассмотрим три случая:
1. Расстояние от первого свободного места до первого человека.
2. Расстояние между двумя соседними людьми — половина расстояния между ними.
3. Расстояние от последнего человека до последнего свободного места.

Максимальное из этих значений и будет ответом.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func maxDistToClosest(_ seats: [Int]) -> Int {
    var maxDistance = 0
    var lastPersonIndex = -1 // Индекс последнего человека
    
    for i in 0..<seats.count {
        if seats[i] == 1 {
            if lastPersonIndex == -1 {
                // Первый человек — расстояние от начала до него
                maxDistance = i
            } else {
                // Расстояние между двумя людьми
                // Максимальное расстояние до ближайшего — половина этого интервала
                let distance = (i - lastPersonIndex) / 2
                maxDistance = max(maxDistance, distance)
            }
            lastPersonIndex = i
        }
    }
    
    // Расстояние от последнего человека до конца
    maxDistance = max(maxDistance, seats.count - 1 - lastPersonIndex)
    
    return maxDistance
}
```

## 362. Design Hit Counter

**Условие задачи:**  
Спроектируйте счётчик запросов, который подсчитывает количество запросов, полученных за последние 5 минут.

Каждая функция принимает параметр временной метки (в секундах) и можно предположить, что вызовы поступают в хронологическом порядке (то есть временная метка монотонно возрастает). Можно предположить, что самая ранняя временная метка начинается с 1.

Возможно, что несколько запросов приходят примерно одновременно.

**Идея решения:**  
Используем массив из 300 элементов (по одному на каждую секунду в 5 минутах). Каждый элемент хранит количество запросов в соответствующую секунду. При `hit(t)` увеличиваем значение для секунды `t % 300`. При `getHits(t)` суммируем все значения за последние 300 секунд.

**Оценка сложности:**  
Время: O(1) для `hit` и `getHits`, Память: O(300) = O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
class HitCounter {
    private var hits: [Int] = Array(repeating: 0, count: 300) // 300 секунд в 5 минутах
    private var timestamps: [Int] = [] // Хранит временные метки для точного удаления старых данных
    
    init() {}
    
    func hit(_ timestamp: Int) {
        // Увеличиваем счётчик для текущей секунды
        let index = timestamp % 300
        hits[index] += 1
        
        // Добавляем временную метку
        timestamps.append(timestamp)
    }
    
    func getHits(_ timestamp: Int) -> Int {
        // Суммируем все запросы за последние 300 секунд
        var total = 0
        for i in 0..<300 {
            let time = timestamp - i
            if i < timestamps.count && timestamps[timestamps.count - 1 - i] >= time {
                total += hits[i]
            }
        }
        
        return total
    }
}
```

---

## 56. Merge Intervals

**Условие задачи:**  
Дан массив интервалов, где `intervals[i] = [start_i, end_i]`. Объедините все пересекающиеся интервалы и верните массив непересекающихся интервалов, которые покрывают все интервалы из входных данных.

**Идея решения:**  
Сортируем интервалы по началу. Затем проходим по отсортированному массиву и объединяем текущий интервал с предыдущим, если они пересекаются (текущее начало <= конец предыдущего).

**Оценка сложности:**  
Время: O(n log n) из-за сортировки, Память: O(1) — если не считать выходной массив.

**Решение задачи (Swift, комментариями в коде):**  
```swift
func merge(_ intervals: [[Int]]) -> [[Int]] {
    guard !intervals.isEmpty else { return [] }
    
    // Сортируем интервалы по началу
    var sortedIntervals = intervals.sorted { $0[0] < $1[0] }
    var result: [[Int]] = []
    
    // Первый интервал добавляем в результат
    result.append(sortedIntervals[0])
    
    // Проходим по остальным интервалам
    for i in 1..<sortedIntervals.count {
        let current = sortedIntervals[i]
        let last = result.last!
        
        // Если текущий интервал пересекается с последним в результате
        if current[0] <= last[1] {
            // Объединяем: обновляем конец последнего интервала
            last[1] = max(last[1], current[1])
        } else {
            // Нет пересечения — добавляем новый интервал
            result.append(current)
        }
    }
    
    return result
}
```

---

## 42. Trapping Rain Water

**Условие задачи:**  
Дано n неотрицательных целых чисел, представляющих карту высот, где ширина каждого столбца равна 1. Вычислите, сколько воды она может удержать после дождя.

**Идея решения:**  
Для каждой позиции количество воды, которое может удержаться, определяется как `min(leftMax, rightMax) - height[i]`, где `leftMax` — максимальная высота слева, а `rightMax` — справа. Используем два прохода: один для вычисления `leftMax`, другой — `rightMax`.

**Оценка сложности:**  
Время: O(n), Память: O(n)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func trap(_ height: [Int]) -> Int {
    guard height.count > 2 else { return 0 }
    
    let n = height.count
    var leftMax = Array(repeating: 0, count: n) // Максимальная высота слева от i
    var rightMax = Array(repeating: 0, count: n) // Максимальная высота справа от i
    
    // Проход слева направо: вычисляем leftMax
    leftMax[0] = height[0]
    for i in 1..<n {
        leftMax[i] = max(leftMax[i - 1], height[i])
    }
    
    // Проход справа налево: вычисляем rightMax
    rightMax[n - 1] = height[n - 1]
    for i in stride(from: n - 2, through: 0, by: -1) {
        rightMax[i] = max(rightMax[i + 1], height[i])
    }
    
    // Вычисляем общее количество воды
    var water = 0
    for i in 0..<n {
        water += min(leftMax[i], rightMax[i]) - height[i]
    }
    
    return water
}
```

---

## 1. Two Sum

**Условие задачи:**  
Дан массив целых чисел `nums` и целое число `target`. Верните индексы двух чисел так, чтобы их сумма была равна `target`.

Можно предположить, что каждый вход имеет ровно одно решение, и нельзя использовать один и тот же элемент дважды.

Вы можете вернуть ответ в любом порядке.

**Идея решения:**  
Используем хэш-таблицу для хранения значений и их индексов. Для каждого числа проверяем, есть ли `target - num` в таблице. Если есть — нашли пару.

**Оценка сложности:**  
Время: O(n), Память: O(n)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
    var map: [Int: Int] = [:] // значение -> индекс
    
    for (index, num) in nums.enumerated() {
        let complement = target - num // То, что нужно найти
        
        // Если комплемент уже в карте — значит, нашли пару
        if let complementIndex = map[complement] {
            return [complementIndex, index]
        }
        
        // Сохраняем текущее значение и его индекс
        map[num] = index
    }
    
    // Решение гарантировано, поэтому этот код никогда не выполнится
    fatalError("No solution found")
}
```


## 253. Meeting Rooms II

**Условие задачи:**  
Дан массив временных интервалов встреч, состоящих из времен начала и окончания `[s1,e1],[s2,e2],...` (где `si < ei`). Найдите минимальное количество конференц-залов, необходимых для проведения всех встреч.

**Идея решения:**  
Сортируем временные метки начала и окончания отдельно. Используем два указателя: один проходит по началам, другой — по окончаниям. При каждом шаге увеличиваем счётчик комнат при начале новой встречи и уменьшаем при окончании. Максимальное значение счётчика — ответ.

**Оценка сложности:**  
Время: O(n log n) из-за сортировки, Память: O(n)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func minMeetingRooms(_ intervals: [[Int]]) -> Int {
    guard !intervals.isEmpty else { return 0 }
    
    // Разделяем начала и окончания встреч
    var starts = [Int]()
    var ends = [Int]()
    
    for interval in intervals {
        starts.append(interval[0])
        ends.append(interval[1])
    }
    
    // Сортируем временные метки
    starts.sort()
    ends.sort()
    
    var rooms = 0 // Текущее количество занятых комнат
    var startIndex = 0
    var endIndex = 0
    
    // Проходим по всем событиям (началам и окончаниям)
    while startIndex < starts.count {
        if starts[startIndex] < ends[endIndex] {
            // Начинается новая встреча — увеличиваем комнаты
            rooms += 1
            startIndex += 1
        } else {
            // Заканчивается встреча — освобождаем комнату
            rooms -= 1
            endIndex += 1
        }
    }
    
    return rooms
}
```

---

## 438. Find All Anagrams in a String

**Условие задачи:**  
Даны две строки `s` и `p`. Верните массив всех начальных индексов анаграмм строки `p` в строке `s`. Вы можете вернуть ответ в любом порядке.

**Идея решения:**  
Используем слайдинг-окно размера `p.length`. Сравниваем частоту символов в окне с частотой в `p`. Если совпадают — добавляем индекс.

**Оценка сложности:**  
Время: O(n), где n — длина `s`, Память: O(1) — только 26 букв

**Решение задачи (Swift, комментариями в коде):**  
```swift
func findAnagrams(_ s: String, _ p: String) -> [Int] {
    let lenS = s.count
    let lenP = p.count
    guard lenS >= lenP else { return [] }
    
    // Подсчитываем частоту символов в p
    var pCount = Array(repeating: 0, count: 26)
    for char in p {
        pCount[Int(char.asciiValue! - Character("a").asciiValue!)] += 1
    }
    
    // Подсчитываем частоту в первом окне длины lenP
    var windowCount = Array(repeating: 0, count: 26)
    var left = 0
    
    for i in 0..<lenP {
        let char = s[s.index(s.startIndex, offsetBy: i)]
        windowCount[Int(char.asciiValue! - Character("a").asciiValue!)] += 1
    }
    
    var result: [Int] = []
    
    // Проверяем, совпадает ли текущее окно с p
    if windowCount == pCount {
        result.append(0)
    }
    
    // Сдвигаем окно
    for right in lenP..<lenS {
        let charRight = s[s.index(s.startIndex, offsetBy: right)]
        let charLeft = s[s.index(s.startIndex, offsetBy: left)]
        
        // Добавляем новый символ справа
        windowCount[Int(charRight.asciiValue! - Character("a").asciiValue!)] += 1
        
        // Удаляем старый символ слева
        windowCount[Int(charLeft.asciiValue! - Character("a").asciiValue!)] -= 1
        
        left += 1
        
        // Проверяем совпадение
        if windowCount == pCount {
            result.append(left)
        }
    }
    
    return result
}
```

---

## 470. Implement Rand10() Using Rand7()

**Условие задачи:**  
Дан API `rand7()`, который генерирует равномерно распределённое случайное целое число в диапазоне [1, 7]. Напишите функцию `rand10()`, которая генерирует равномерно распределённое случайное целое число в диапазоне [1, 10]. Можно использовать только `rand7()`, не используя другие API или встроенные функции случайных чисел.

Каждый тест будет иметь внутренний аргумент `n`, количество вызовов `rand10()` во время тестирования.

**Идея решения:**  
Используем `rand7()` дважды, чтобы получить число от 1 до 49. Если это число ≤ 40, то можем вычислить `rand10()` как `(num - 1) % 10 + 1`. В противном случае повторяем попытку.

**Оценка сложности:**  
Время: O(1) в среднем, Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
// Предполагается, что rand7() уже определена
func rand7() -> Int {
    // Это имитация API
    return Int.random(in: 1...7)
}

func rand10() -> Int {
    while true {
        // Генерируем число от 1 до 49
        let num = (rand7() - 1) * 7 + rand7() // Диапазон [1, 49]
        
        // Если число в допустимом диапазоне [1, 40], используем его
        if num <= 40 {
            return (num - 1) % 10 + 1 // Преобразуем в [1, 10]
        }
        // Иначе повторяем попытку
    }
}
```

---

## 101. Symmetric Tree

**Условие задачи:**  
Дан корень бинарного дерева. Проверьте, является ли оно зеркальным относительно себя (то есть симметричным относительно центра).

**Идея решения:**  
Рекурсивно сравниваем левое и правое поддерево. Для двух поддеревьев они являются зеркальными, если:
- Оба пусты.
- Оба не пусты, значения корней равны, и левое поддерево первого равно правому поддереву второго, и наоборот.

**Оценка сложности:**  
Время: O(n), Память: O(h), где h — высота дерева (из-за рекурсии)

**Решение задачи (Swift, комментариями в коде):**  
```swift
// Определение узла дерева
class TreeNode {
    var val: Int
    var left: TreeNode?
    var right: TreeNode?
    init(_ val: Int) {
        self.val = val
        self.left = nil
        self.right = nil
    }
}

func isSymmetric(_ root: TreeNode?) -> Bool {
    // Пустое дерево считается симметричным
    guard let root = root else { return true }
    
    // Рекурсивная проверка симметрии двух поддеревьев
    return isMirror(root.left, root.right)
}

private func isMirror(_ left: TreeNode?, _ right: TreeNode?) -> Bool {
    // Оба пусты — зеркальны
    if left == nil && right == nil {
        return true
    }
    
    // Один пуст, другой нет — не зеркальны
    if left == nil || right == nil {
        return false
    }
    
    // Значения должны быть равны, и поддеревья должны быть зеркальными
    return left!.val == right!.val &&
           isMirror(left!.left, right!.right) &&
           isMirror(left!.right, right!.left)
}
```

## 3. Longest Substring Without Repeating Characters

**Условие задачи:**  
Данная строка `s`, найдите длину самой длинной подстроки без повторяющихся символов.

**Идея решения:**  
Используем слайдинг-окно (двух указателей). Следим за текущей подстрокой без повторов, используя хэш-таблицу для хранения последнего индекса каждого символа. При встрече повторяющегося символа сдвигаем левый указатель до позиции после его предыдущего вхождения.

**Оценка сложности:**  
Время: O(n), Память: O(min(m,n)), где m — размер алфавита

**Решение задачи (Swift, комментариями в коде):**  
```swift
func lengthOfLongestSubstring(_ s: String) -> Int {
    var charIndex: [Character: Int] = [:] // Хранит последний индекс символа
    var left = 0 // Левая граница окна
    var maxLength = 0 // Максимальная длина
    
    for (right, char) in s.enumerated() {
        // Если символ уже был в окне, обновляем левую границу
        if let prevIndex = charIndex[char], prevIndex >= left {
            left = prevIndex + 1
        }
        
        // Обновляем индекс текущего символа
        charIndex[char] = right
        
        // Обновляем максимальную длину
        maxLength = max(maxLength, right - left + 1)
    }
    
    return maxLength
}
```

---

## 98. Validate Binary Search Tree

**Условие задачи:**  
Дан корень бинарного дерева. Определите, является ли оно корректным бинарным деревом поиска (BST).

Корректное BST определяется следующим образом:
- Левое поддерево узла содержит только узлы с ключами строго меньше ключа узла.
- Правое поддерево узла содержит только узлы с ключами строго больше ключа узла.
- Оба левое и правое поддеревья также должны быть бинарными деревьями поиска.

**Идея решения:**  
Рекурсивно проверяем каждое поддерево, передавая допустимые границы значений. Для левого поддерева значение должно быть < текущего узла, для правого — >. Границы обновляются при переходе в поддерево.

**Оценка сложности:**  
Время: O(n), Память: O(h), где h — высота дерева

**Решение задачи (Swift, комментариями в коде):**  
```swift
// Определение узла дерева
class TreeNode {
    var val: Int
    var left: TreeNode?
    var right: TreeNode?
    init(_ val: Int) {
        self.val = val
        self.left = nil
        self.right = nil
    }
}

func isValidBST(_ root: TreeNode?) -> Bool {
    return validate(root, Int.min, Int.max)
}

private func validate(_ node: TreeNode?, _ minVal: Int, _ maxVal: Int) -> Bool {
    guard let node = node else { return true }
    
    // Проверяем, что значение узла в допустимом диапазоне
    if node.val <= minVal || node.val >= maxVal {
        return false
    }
    
    // Рекурсивно проверяем левое и правое поддерево
    return validate(node.left, minVal, node.val) &&
           validate(node.right, node.val, maxVal)
}
```

---

## 200. Number of Islands

**Условие задачи:**  
Дан двумерный бинарный массив `grid` размера m x n, представляющий карту, где '1' — земля, а '0' — вода. Верните количество островов.

Остров окружён водой и образован соединением соседних земель горизонтально или вертикально. Можно считать, что все четыре края сетки окружены водой.

**Идея решения:**  
Проходим по каждой ячейке. Если находим '1', запускаем DFS или BFS, чтобы отметить весь остров как посещённый. Каждый запуск DFS/BFS увеличивает счётчик островов.

**Оценка сложности:**  
Время: O(m*n), Память: O(m*n) в худшем случае (стек рекурсии)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func numIslands(_ grid: [[Character]]) -> Int {
    guard !grid.isEmpty, !grid[0].isEmpty else { return 0 }
    
    let rows = grid.count
    let cols = grid[0].count
    var visited = Array(repeating: Array(repeating: false, count: cols), count: rows)
    var islands = 0
    
    // Функция DFS для обхода острова
    func dfs(_ row: Int, _ col: Int) {
        // Проверка границ и состояния ячейки
        if row < 0 || row >= rows || col < 0 || col >= cols || 
           visited[row][col] || grid[row][col] == "0" {
            return
        }
        
        // Отмечаем ячейку как посещённую
        visited[row][col] = true
        
        // Обходим соседние ячейки
        dfs(row + 1, col)
        dfs(row - 1, col)
        dfs(row, col + 1)
        dfs(row, col - 1)
    }
    
    // Проходим по всем ячейкам
    for i in 0..<rows {
        for j in 0..<cols {
            if grid[i][j] == "1" && !visited[i][j] {
                dfs(i, j)
                islands += 1
            }
        }
    }
    
    return islands
}
```

---

## 341. Flatten Nested List Iterator

**Условие задачи:**  
Дано вложенное список целых чисел `nestedList`. Каждый элемент — либо целое число, либо список, элементы которого могут быть числами или другими списками. Реализуйте итератор для его разворачивания.

Реализуйте класс `NestedIterator`:
- `NestedIterator(List<NestedInteger> nestedList)` — инициализирует итератор с вложенным списком.
- `int next()` — возвращает следующее целое число в вложенном списке.
- `boolean hasNext()` — возвращает `true`, если есть ещё целые числа, иначе `false`.

Ваш код будет тестироваться следующим псевдокодом:
```
initialize iterator with nestedList
res = []
while iterator.hasNext()
    append iterator.next() to the end of res
return res
```

Если `res` совпадает с ожидаемым развернутым списком, ваш код считается правильным.

**Идея решения:**  
Используем стек. В начале помещаем весь список в стек. На каждом шаге извлекаем элемент. Если это число — возвращаем. Если это список — добавляем его элементы в стек в обратном порядке (чтобы первый элемент шёл первым).

**Оценка сложности:**  
Время: O(n) для инициализации, O(1) для `next` и `hasNext`, Память: O(d), где d — глубина вложенности

**Решение задачи (Swift, комментариями в коде):**  
```swift
// Предполагается, что NestedInteger определен
public class NestedInteger {
    private var integer: Int?
    private var list: [NestedInteger]?

    public init(int value: Int) {
        self.integer = value
    }

    public init(list: [NestedInteger]) {
        self.list = list
    }

    public func isInteger() -> Bool {
        return integer != nil
    }

    public func getInteger() -> Int? {
        return integer
    }

    public func getList() -> [NestedInteger]? {
        return list
    }
}

class NestedIterator {
    private var stack: [NestedInteger] = []
    
    init(_ nestedList: [NestedInteger]) {
        // Добавляем все элементы списка в стек в обратном порядке
        for i in stride(from: nestedList.count - 1, through: 0, by: -1) {
            stack.append(nestedList[i])
        }
    }
    
    func next() -> Int {
        // Извлекаем верхний элемент
        let top = stack.removeLast()
        return top.getInteger()!
    }
    
    func hasNext() -> Bool {
        // Пока стек не пуст, продолжаем
        while !stack.isEmpty {
            let top = stack.last!
            
            if top.isInteger() {
                return true
            }
            
            // Если это список — добавляем его элементы в стек
            if let list = top.getList() {
                // Добавляем в обратном порядке, чтобы первый элемент шёл первым
                for i in stride(from: list.count - 1, through: 0, by: -1) {
                    stack.append(list[i])
                }
            } else {
                // Удаляем пустой список
                stack.removeLast()
            }
        }
        
        return false
    }
}
```


## 1446. Consecutive Characters

**Условие задачи:**  
Мощность строки — это максимальная длина непустой подстроки, содержащей только один уникальный символ.

Данная строка `s`, верните мощность строки `s`.

**Идея решения:**  
Проходим по строке, подсчитывая текущую длину последовательности одинаковых символов. При смене символа сравниваем текущую длину с максимальной и обновляем максимум.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func maxPower(_ s: String) -> Int {
    var maxLen = 1 // Минимальная длина — 1 (один символ)
    var currentLen = 1 // Текущая длина последовательности
    
    // Проходим по строке начиная со второго символа
    for i in 1..<s.count {
        let currentChar = s[s.index(s.startIndex, offsetBy: i)]
        let prevChar = s[s.index(s.startIndex, offsetBy: i - 1)]
        
        if currentChar == prevChar {
            // Увеличиваем текущую длину
            currentLen += 1
        } else {
            // Сравниваем с максимальной и сбрасываем
            maxLen = max(maxLen, currentLen)
            currentLen = 1
        }
    }
    
    // Не забываем проверить последнюю последовательность
    maxLen = max(maxLen, currentLen)
    
    return maxLen
}
```

---

## 986. Interval List Intersections

**Условие задачи:**  
Даны два списка замкнутых интервалов, `firstList` и `secondList`, где `firstList[i] = [start_i, end_i]` и `secondList[j] = [start_j, end_j]`. Каждый список интервалов попарно не пересекается и отсортирован.

Верните пересечение этих двух списков интервалов.

Замкнутый интервал `[a, b]` (где `a <= b`) обозначает множество вещественных чисел `x`, таких что `a <= x <= b`.

Пересечение двух замкнутых интервалов — это множество вещественных чисел, которое либо пусто, либо представляет собой замкнутый интервал. Например, пересечение `[1,3]` и `[2,4]` — это `[2,3]`.

**Идея решения:**  
Используем два указателя. Для каждого интервала из обоих списков находим их пересечение: `[max(start1, start2), min(end1, end2)]`. Если начало меньше или равно концу — добавляем в результат. Двигаем указатель к следующему интервалу, у которого конец меньше.

**Оценка сложности:**  
Время: O(m + n), где m и n — длины списков, Память: O(m + n)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func intervalIntersection(_ firstList: [[Int]], _ secondList: [[Int]]) -> [[Int]] {
    var result: [[Int]] = []
    var i = 0 // Указатель для firstList
    var j = 0 // Указатель для secondList
    
    while i < firstList.count && j < secondList.count {
        let firstStart = firstList[i][0]
        let firstEnd = firstList[i][1]
        let secondStart = secondList[j][0]
        let secondEnd = secondList[j][1]
        
        // Находим пересечение
        let intersectionStart = max(firstStart, secondStart)
        let intersectionEnd = min(firstEnd, secondEnd)
        
        // Если есть пересечение
        if intersectionStart <= intersectionEnd {
            result.append([intersectionStart, intersectionEnd])
        }
        
        // Двигаем указатель к интервалу с меньшим концом
        if firstEnd < secondEnd {
            i += 1
        } else {
            j += 1
        }
    }
    
    return result
}
```

---

## 232. Implement Queue using Stacks

**Условие задачи:**  
Реализуйте очередь FIFO (первым пришёл — первым вышел) с использованием только двух стеков. Реализованная очередь должна поддерживать все функции обычной очереди (push, peek, pop, empty).

Реализуйте класс `MyQueue`:
- `void push(int x)` — добавляет элемент x в конец очереди.
- `int pop()` — удаляет элемент из начала очереди и возвращает его.
- `int peek()` — возвращает элемент в начале очереди.
- `boolean empty()` — возвращает `true`, если очередь пуста, иначе `false`.

Примечания:
- Можно использовать только стандартные операции стека: push to top, peek/pop from top, size, is empty.
- В зависимости от языка, стек может не поддерживаться встроенно. Можно имитировать стек с помощью массива или deque, используя только стандартные операции стека.

**Идея решения:**  
Используем два стека: `input` для добавления элементов и `output` для извлечения. При `push` добавляем в `input`. При `pop` или `peek` переносим все элементы из `input` в `output`, если `output` пуст.

**Оценка сложности:**  
Время: O(1) в среднем, O(n) в худшем случае, Память: O(n)

**Решение задачи (Swift, комментариями в коде):**  
```swift
class MyQueue {
    private var input: [Int] = [] // Стек для добавления
    private var output: [Int] = [] // Стек для извлечения
    
    func push(_ x: Int) {
        input.append(x) // Добавляем в конец input
    }
    
    func pop() -> Int {
        // Переносим все элементы из input в output, если output пуст
        if output.isEmpty {
            while !input.isEmpty {
                output.append(input.removeLast())
            }
        }
        
        return output.removeLast() // Удаляем из вершины output
    }
    
    func peek() -> Int {
        // Аналогично pop, но не удаляем
        if output.isEmpty {
            while !input.isEmpty {
                output.append(input.removeLast())
            }
        }
        
        return output.last! // Возвращаем верхний элемент output
    }
    
    func empty() -> Bool {
        return input.isEmpty && output.isEmpty
    }
}
```

---

## 2. Add Two Numbers

**Условие задачи:**  
Даны два непустых односвязных списка, представляющих два неотрицательных целых числа. Цифры хранятся в обратном порядке, и каждый узел содержит одну цифру. Сложите два числа и верните сумму как односвязный список.

Можно предположить, что оба числа не содержат ведущих нулей, кроме самого числа 0.

**Идея решения:**  
Проходим по двум спискам одновременно, складываем цифры и переносим остаток. Создаём новый список для результата.

**Оценка сложности:**  
Время: O(max(m,n)), где m и n — длины списков, Память: O(max(m,n))

**Решение задачи (Swift, комментариями в коде):**  
```swift
// Определение узла списка
class ListNode {
    var val: Int
    var next: ListNode?
    init(_ val: Int) {
        self.val = val
        self.next = nil
    }
}

func addTwoNumbers(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
    var carry = 0 // Перенос
    var p1 = l1 // Указатель на первый список
    var p2 = l2 // Указатель на второй список
    let dummy = ListNode(0) // Заглушка для удобства
    var current = dummy // Указатель на текущий узел результата
    
    // Проходим по обоим спискам до тех пор, пока не закончатся оба
    while p1 != nil || p2 != nil || carry > 0 {
        let val1 = p1?.val ?? 0 // Если p1 == nil, используем 0
        let val2 = p2?.val ?? 0 // Если p2 == nil, используем 0
        
        let sum = val1 + val2 + carry // Сумма цифр и переноса
        carry = sum / 10 // Новый перенос
        let digit = sum % 10 // Цифра для текущего узла
        
        // Создаём новый узел и добавляем в результат
        current.next = ListNode(digit)
        current = current.next!
        
        // Переходим к следующему узлу в каждом списке
        p1 = p1?.next
        p2 = p2?.next
    }
    
    return dummy.next // Возвращаем голову результата
}
```


## 88. Merge Sorted Array

**Условие задачи:**  
Даны два целочисленных массива `nums1` и `nums2`, отсортированных в неубывающем порядке, и два целых числа `m` и `n`, представляющих количество элементов в `nums1` и `nums2` соответственно.

Объедините `nums1` и `nums2` в один массив, отсортированный в неубывающем порядке.

Итоговый отсортированный массив не должен возвращаться функцией, а должен храниться внутри массива `nums1`. Для этого `nums1` имеет длину `m + n`, где первые `m` элементов — это элементы, которые нужно объединить, а последние `n` элементов установлены в 0 и должны быть проигнорированы. `nums2` имеет длину `n`.

**Идея решения:**  
Используем трёхуказательный метод. Начинаем с конца обоих массивов и заполняем `nums1` с конца. Сравниваем элементы с конца `nums1` и `nums2`, помещаем больший в конец `nums1`.

**Оценка сложности:**  
Время: O(m + n), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func merge(_ nums1: inout [Int], _ m: Int, _ nums2: [Int], _ n: Int) {
    var i = m - 1 // Указатель на последний элемент nums1
    var j = n - 1 // Указатель на последний элемент nums2
    var k = m + n - 1 // Указатель на конец результата в nums1
    
    // Проходим по обоим массивам с конца
    while i >= 0 && j >= 0 {
        if nums1[i] > nums2[j] {
            // Берём из nums1
            nums1[k] = nums1[i]
            i -= 1
        } else {
            // Берём из nums2
            nums1[k] = nums2[j]
            j -= 1
        }
        k -= 1
    }
    
    // Если ещё есть элементы в nums2, копируем их
    while j >= 0 {
        nums1[k] = nums2[j]
        j -= 1
        k -= 1
    }
}
```

---

## 21. Merge Two Sorted Lists

**Условие задачи:**  
Даны головы двух отсортированных односвязных списков `list1` и `list2`.

Объедините два списка в один отсортированный список. Список должен быть составлен путём соединения узлов первых двух списков.

Верните голову объединённого односвязного списка.

**Идея решения:**  
Используем рекурсию или итерацию. Сравниваем значения голов двух списков, выбираем меньшее, и рекурсивно объединяем оставшиеся части.

**Оценка сложности:**  
Время: O(m + n), Память: O(1) для итеративного подхода

**Решение задачи (Swift, комментариями в коде):**  
```swift
// Определение узла списка
class ListNode {
    var val: Int
    var next: ListNode?
    init(_ val: Int) {
        self.val = val
        self.next = nil
    }
}

func mergeTwoLists(_ list1: ListNode?, _ list2: ListNode?) -> ListNode? {
    // Базовые случаи: если один из списков пустой — возвращаем другой
    if list1 == nil { return list2 }
    if list2 == nil { return list1 }
    
    // Выбираем узел с меньшим значением
    if list1!.val < list2!.val {
        list1!.next = mergeTwoLists(list1!.next, list2)
        return list1
    } else {
        list2!.next = mergeTwoLists(list1, list2!.next)
        return list2
    }
}
```

---

## 1004. Max Consecutive Ones III

**Условие задачи:**  
Дан двоичный массив `nums` и целое число `k`. Верните максимальное количество последовательных единиц в массиве, если вы можете перевернуть не более `k` нулей.

**Идея решения:**  
Используем слайдинг-окно. Поддерживаем окно, в котором количество нулей не превышает `k`. При увеличении количества нулей сдвигаем левый указатель.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func longestOnes(_ nums: [Int], _ k: Int) -> Int {
    var left = 0
    var zeros = 0
    var maxLength = 0
    
    for right in 0..<nums.count {
        // Если текущий элемент — ноль, увеличиваем счётчик
        if nums[right] == 0 {
            zeros += 1
        }
        
        // Если слишком много нулей, сдвигаем левый указатель
        while zeros > k {
            if nums[left] == 0 {
                zeros -= 1
            }
            left += 1
        }
        
        // Обновляем максимальную длину
        maxLength = max(maxLength, right - left + 1)
    }
    
    return maxLength
}
```

---

## 5. Longest Palindromic Substring

**Условие задачи:**  
Данная строка `s`, верните самую длинную палиндромную подстроку в `s`.

**Идея решения:**  
Для каждого символа (и между символами) рассматриваем его как центр палиндрома. Расширяем вокруг центра, пока строки симметричны. Ищем самый длинный палиндром.

**Оценка сложности:**  
Время: O(n²), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func longestPalindrome(_ s: String) -> String {
    guard s.count > 0 else { return "" }
    
    let chars = Array(s)
    var start = 0
    var maxLength = 1
    
    // Функция для расширения вокруг центра
    func expandAroundCenter(_ left: Int, _ right: Int) -> Int {
        var l = left
        var r = right
        
        while l >= 0 && r < chars.count && chars[l] == chars[r] {
            l -= 1
            r += 1
        }
        
        // Возвращаем длину палиндрома
        return r - l - 1
    }
    
    for i in 0..<chars.count {
        // Проверяем чётные и нечётные палиндромы
        let oddLength = expandAroundCenter(i, i) // Центр в одном символе
        let evenLength = expandAroundCenter(i, i + 1) // Центр между двумя символами
        
        let currentMax = max(oddLength, evenLength)
        
        if currentMax > maxLength {
            maxLength = currentMax
            start = i - (currentMax - 1) / 2
        }
    }
    
    return String(Array(s)[start..<start + maxLength])
}
```



## 771. Jewels and Stones

**Условие задачи:**  
Даны строки `jewels`, представляющие типы камней, которые являются драгоценностями, и `stones`, представляющая камни, которые у вас есть. Каждый символ в `stones` — это тип камня, который у вас есть. Вы хотите узнать, сколько из имеющихся камней также являются драгоценностями.

Буквы чувствительны к регистру, поэтому "a" считается другим типом камня, чем "A".

**Идея решения:**  
Создаём множество (set) из символов `jewels` для быстрого поиска. Затем проходим по каждому символу в `stones` и проверяем, является ли он драгоценностью.

**Оценка сложности:**  
Время: O(j + s), где j — длина `jewels`, s — длина `stones`, Память: O(j)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func numJewelsInStones(_ jewels: String, _ stones: String) -> Int {
    let jewelSet = Set(jewels) // Хранит уникальные драгоценности для быстрого доступа
    var count = 0
    
    // Проходим по каждому камню
    for stone in stones {
        if jewelSet.contains(stone) {
            count += 1
        }
    }
    
    return count
}
```

---

## 236. Lowest Common Ancestor of a Binary Tree

**Условие задачи:**  
Дано бинарное дерево, найдите наименьшего общего предка (LCA) двух заданных узлов в дереве.

Согласно определению LCA на Wikipedia: «Наименьший общий предок определяется между двумя узлами p и q как самый нижний узел в T, который имеет оба p и q в качестве потомков (где мы допускаем, что узел может быть потомком самого себя).»

**Идея решения:**  
Рекурсивно проходим дерево. Если один из узлов — это сам p или q, то текущий узел — возможный LCA. Если оба поддерева содержат один из узлов, то текущий узел — LCA.

**Оценка сложности:**  
Время: O(n), Память: O(h), где h — высота дерева

**Решение задачи (Swift, комментариями в коде):**  
```swift
// Определение узла дерева
class TreeNode {
    var val: Int
    var left: TreeNode?
    var right: TreeNode?
    init(_ val: Int) {
        self.val = val
        self.left = nil
        self.right = nil
    }
}

func lowestCommonAncestor(_ root: TreeNode?, _ p: TreeNode?, _ q: TreeNode?) -> TreeNode? {
    guard let root = root, let p = p, let q = q else { return nil }
    
    // Если текущий узел — один из искомых, он может быть LCA
    if root.val == p.val || root.val == q.val {
        return root
    }
    
    // Рекурсивно ищем в левом и правом поддереве
    let left = lowestCommonAncestor(root.left, p, q)
    let right = lowestCommonAncestor(root.right, p, q)
    
    // Если оба поддерева содержат один из узлов, текущий узел — LCA
    if left != nil && right != nil {
        return root
    }
    
    // Иначе возвращаем ненулевой результат
    return left ?? right
}
```

---

## 350. Intersection of Two Arrays II

**Условие задачи:**  
Даны два целочисленных массива `nums1` и `nums2`. Верните массив их пересечения. Каждый элемент в результате должен появляться столько раз, сколько он встречается в обоих массивах. Можно вернуть результат в любом порядке.

**Идея решения:**  
Используем хэш-таблицу для подсчёта частоты элементов в `nums1`. Затем проходим по `nums2` и добавляем элемент в результат, если его частота > 0.

**Оценка сложности:**  
Время: O(m + n), где m и n — длины массивов, Память: O(min(m,n))

**Решение задачи (Swift, комментариями в коде):**  
```swift
func intersect(_ nums1: [Int], _ nums2: [Int]) -> [Int] {
    var countMap: [Int: Int] = [:]
    var result: [Int] = []
    
    // Подсчитываем частоту элементов в nums1
    for num in nums1 {
        countMap[num, default: 0] += 1
    }
    
    // Проходим по nums2
    for num in nums2 {
        if let count = countMap[num], count > 0 {
            result.append(num)
            countMap[num] = count - 1
        }
    }
    
    return result
}
```

---

## 268. Missing Number

**Условие задачи:**  
Дан массив `nums`, содержащий `n` различных чисел в диапазоне `[0, n]`. Верните единственное число в этом диапазоне, которое отсутствует в массиве.

**Идея решения:**  
Используем сумму арифметической прогрессии: сумма чисел от 0 до n равна `n*(n+1)/2`. Вычитаем сумму массива из этой суммы.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func missingNumber(_ nums: [Int]) -> Int {
    let n = nums.count
    let expectedSum = n * (n + 1) / 2 // Сумма от 0 до n
    let actualSum = nums.reduce(0, +) // Сумма массива
    
    return expectedSum - actualSum
}
```



## 150. Evaluate Reverse Polish Notation

**Условие задачи:**  
Дан массив строк `tokens`, представляющий арифметическое выражение в обратной польской нотации (RPN). Вычислите значение выражения и верните целое число.

Примечания:
- Допустимые операторы: `'+'`, `'-'`, `'*'`, `'/'`.
- Каждый операнд может быть целым числом или другим выражением.
- Деление двух целых чисел всегда округляется к нулю.
- Не будет деления на ноль.
- Ввод представляет собой корректное арифметическое выражение в RPN.
- Ответ и все промежуточные вычисления могут быть представлены в 32-битном целом числе.

**Идея решения:**  
Используем стек. Проходим по токенам. Если токен — число, добавляем его в стек. Если оператор — извлекаем два последних числа, выполняем операцию и помещаем результат обратно в стек.

**Оценка сложности:**  
Время: O(n), Память: O(n)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func evalRPN(_ tokens: [String]) -> Int {
    var stack: [Int] = []
    
    for token in tokens {
        switch token {
        case "+":
            let b = stack.popLast()!
            let a = stack.popLast()!
            stack.append(a + b)
        case "-":
            let b = stack.popLast()!
            let a = stack.popLast()!
            stack.append(a - b)
        case "*":
            let b = stack.popLast()!
            let a = stack.popLast()!
            stack.append(a * b)
        case "/":
            let b = stack.popLast()!
            let a = stack.popLast()!
            stack.append(a / b) // Округление к нулю автоматически происходит в Swift
        default:
            // Это число — преобразуем и добавляем в стек
            if let num = Int(token) {
                stack.append(num)
            }
        }
    }
    
    return stack.last ?? 0
}
```

---

## 4. Median of Two Sorted Arrays

**Условие задачи:**  
Даны два отсортированных массива `nums1` и `nums2` размеров `m` и `n` соответственно. Верните медиану двух отсортированных массивов.

Общая временная сложность должна быть O(log(m+n)).

**Идея решения:**  
Используем двоичный поиск. Ищем разделение массивов так, что левая часть содержит `(m + n + 1) / 2` элементов, а правая — остальные. Медиана — это среднее значение между максимальным элементом слева и минимальным справа.

**Оценка сложности:**  
Время: O(log(min(m,n))), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func findMedianSortedArrays(_ nums1: [Int], _ nums2: [Int]) -> Double {
    // Убедимся, что nums1 — меньший массив
    if nums1.count > nums2.count {
        return findMedianSortedArrays(nums2, nums1)
    }
    
    let m = nums1.count
    let n = nums2.count
    let total = m + n
    let half = (total + 1) / 2
    
    var left = 0
    var right = m
    
    while left <= right {
        let partition1 = (left + right) / 2
        let partition2 = half - partition1
        
        let maxLeft1 = partition1 == 0 ? Int.min : nums1[partition1 - 1]
        let minRight1 = partition1 == m ? Int.max : nums1[partition1]
        
        let maxLeft2 = partition2 == 0 ? Int.min : nums2[partition2 - 1]
        let minRight2 = partition2 == n ? Int.max : nums2[partition2]
        
        if maxLeft1 <= minRight2 && maxLeft2 <= minRight1 {
            // Найдено правильное разделение
            if total % 2 == 1 {
                return Double(max(maxLeft1, maxLeft2))
            } else {
                return Double(max(maxLeft1, maxLeft2) + min(minRight1, minRight2)) / 2.0
            }
        } else if maxLeft1 > minRight2 {
            // Слишком много элементов слева в nums1
            right = partition1 - 1
        } else {
            // Слишком мало элементов слева в nums1
            left = partition1 + 1
        }
    }
    
    fatalError("No valid partition found")
}
```

---

## 71. Simplify Path

**Условие задачи:**  
Дан абсолютный путь для файловой системы Unix-стиля, который всегда начинается со слэша `/`. Ваша задача — преобразовать этот абсолютный путь в упрощённый канонический путь.

Правила Unix-стиля:
- Одиночный точка `.` означает текущий каталог.
- Двойная точка `..` означает предыдущий/родительский каталог.
- Последовательные слэши, такие как `//` и `///`, считаются одним слэшем `/`.
- Любая последовательность точек, не соответствующая вышеуказанным правилам, считается допустимым именем каталога или файла (например, `...` и `....`).

Правила упрощённого канонического пути:
- Путь должен начинаться с одного слэша `/`.
- Каталоги в пути должны разделяться ровно одним слэшем `/`.
- Путь не должен заканчиваться слэшем `/`, если это не корневой каталог.
- Путь не должен содержать одиночные или двойные точки (`.` и `..`) для обозначения текущего или родительского каталога.

Верните упрощённый канонический путь.

**Идея решения:**  
Разбиваем путь по слэшам, проходим по каждому компоненту. Если компонент — `..`, удаляем последний каталог из стека. Если компонент — `.` — пропускаем. Иначе добавляем в стек.

**Оценка сложности:**  
Время: O(n), Память: O(n)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func simplifyPath(_ path: String) -> String {
    var stack: [String] = []
    let components = path.split(separator: "/").map { String($0) }
    
    for component in components {
        if component == ".." {
            // Удаляем предыдущий каталог
            if !stack.isEmpty {
                stack.removeLast()
            }
        } else if component != "." && !component.isEmpty {
            // Добавляем каталог, если он не пуст и не точка
            stack.append(component)
        }
    }
    
    // Собираем путь
    return "/" + stack.joined(separator: "/")
}
```

---

## 392. Is Subsequence

**Условие задачи:**  
Даны две строки `s` и `t`. Верните `true`, если `s` является подпоследовательностью `t`, иначе `false`.

Подпоследовательность строки — это новая строка, образованная удалением некоторых (возможно, ни одного) символов из исходной строки без нарушения относительных позиций оставшихся символов. Например, "ace" является подпоследовательностью "abcde", а "aec" — нет.

**Идея решения:**  
Используем два указателя. Проходим по `t`, проверяя, совпадает ли текущий символ с символом из `s`. Если да — двигаем указатель `s`. Если прошли весь `s` — значит, он является подпоследовательностью.

**Оценка сложности:**  
Время: O(n), где n — длина `t`, Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func isSubsequence(_ s: String, _ t: String) -> Bool {
    var i = 0 // Указатель на s
    var j = 0 // Указатель на t
    
    while i < s.count && j < t.count {
        let charS = s[s.index(s.startIndex, offsetBy: i)]
        let charT = t[t.index(t.startIndex, offsetBy: j)]
        
        if charS == charT {
            i += 1
        }
        j += 1
    }
    
    return i == s.count
}
```

## 977. Squares of a Sorted Array

**Условие задачи:**  
Дан целочисленный массив `nums`, отсортированный в неубывающем порядке. Верните массив квадратов каждого числа, отсортированный в неубывающем порядке.

**Идея решения:**  
Используем двухуказательный метод. Поскольку квадраты больших по модулю чисел могут быть больше, чем квадраты малых, начинаем с концов массива и сравниваем квадраты элементов. Берём больший и помещаем в результат с конца.

**Оценка сложности:**  
Время: O(n), Память: O(1) (не считая выходного массива)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func sortedSquares(_ nums: [Int]) -> [Int] {
    let n = nums.count
    var result = Array(repeating: 0, count: n)
    var left = 0
    var right = n - 1
    var index = n - 1 // Заполняем результат с конца
    
    while left <= right {
        let leftSquare = nums[left] * nums[left]
        let rightSquare = nums[right] * nums[right]
        
        // Берём больший квадрат
        if leftSquare > rightSquare {
            result[index] = leftSquare
            left += 1
        } else {
            result[index] = rightSquare
            right -= 1
        }
        index -= 1
    }
    
    return result
}
```

---

## 19. Remove Nth Node From End of List

**Условие задачи:**  
Дано голова односвязного списка, удалите n-й узел с конца списка и верните его голову.

**Идея решения:**  
Используем два указателя. Первый указатель проходит n шагов вперёд. Затем оба указателя движутся вместе до конца. Второй указатель будет указывать на узел перед тем, который нужно удалить.

**Оценка сложности:**  
Время: O(L), где L — длина списка, Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
// Определение узла списка
class ListNode {
    var val: Int
    var next: ListNode?
    init(_ val: Int) {
        self.val = val
        self.next = nil
    }
}

func removeNthFromEnd(_ head: ListNode?, _ n: Int) -> ListNode? {
    let dummy = ListNode(0) // Заглушка для удобства
    dummy.next = head
    var fast = dummy
    var slow = dummy
    
    // Передвигаем fast на n шагов
    for _ in 0..<n {
        fast = fast.next!
    }
    
    // Двигаем оба указателя до конца
    while fast.next != nil {
        fast = fast.next!
        slow = slow.next!
    }
    
    // Удаляем n-й узел с конца
    slow.next = slow.next?.next
    
    return dummy.next
}
```

---

## 279. Perfect Squares

**Условие задачи:**  
Дано целое число `n`. Верните минимальное количество совершенных квадратов, сумма которых равна `n`.

Совершенный квадрат — это целое число, являющееся квадратом другого целого числа; другими словами, произведение некоторого целого числа на само себя. Например, 1, 4, 9 и 16 — совершенные квадраты, а 3 и 11 — нет.

**Идея решения:**  
Используем динамическое программирование. `dp[i]` — минимальное количество совершенных квадратов, сумма которых равна `i`. Для каждого `i`, пробуем все возможные совершенные квадраты меньше `i`.

**Оценка сложности:**  
Время: O(n√n), Память: O(n)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func numSquares(_ n: Int) -> Int {
    var dp = Array(repeating: Int.max, count: n + 1)
    dp[0] = 0 // 0 можно представить как 0 квадратов
    
    for i in 1...n {
        for j in 1...Int(sqrt(Double(i))) {
            let square = j * j
            if square <= i {
                dp[i] = min(dp[i], dp[i - square] + 1)
            }
        }
    }
    
    return dp[n]
}
```

---

## 716. Max Stack

**Условие задачи:**  
Спроектируйте стек максимума, который поддерживает операции:
1. `push(x)` — добавляет элемент x в стек.
2. `pop()` — удаляет элемент с вершины стека и возвращает его.
3. `top()` — возвращает элемент с вершины.
4. `peekMax()` — возвращает максимальный элемент в стеке.
5. `popMax()` — возвращает максимальный элемент и удаляет его. Если найдено несколько максимальных элементов, удаляется только самый верхний.

**Идея решения:**  
Используем два стека: один для хранения элементов, другой для хранения максимумов. При каждом `push` сохраняем текущий максимум. При `popMax` удаляем из обоих стеков.

**Оценка сложности:**  
Время: O(1) для всех операций, Память: O(n)

**Решение задачи (Swift, комментариями в коде):**  
```swift
class MaxStack {
    private var stack: [Int] = [] // Стек значений
    private var maxStack: [Int] = [] // Стек максимумов
    
    func push(_ x: Int) {
        stack.append(x)
        // Обновляем максимум
        if maxStack.isEmpty || x >= maxStack.last! {
            maxStack.append(x)
        }
    }
    
    func pop() -> Int {
        let value = stack.removeLast()
        if value == maxStack.last {
            maxStack.removeLast()
        }
        return value
    }
    
    func top() -> Int {
        return stack.last!
    }
    
    func peekMax() -> Int {
        return maxStack.last!
    }
    
    func popMax() -> Int {
        let maxValue = maxStack.removeLast()
        // Находим и удаляем последнее вхождение maxValue
        for i in stride(from: stack.count - 1, through: 0, by: -1) {
            if stack[i] == maxValue {
                stack.remove(at: i)
                break
            }
        }
        return maxValue
    }
}
```

## 1650. Lowest Common Ancestor of a Binary Tree III

**Условие задачи:**  
Даны два узла бинарного дерева `p` и `q`. Верните их наименьшего общего предка (LCA).

Каждый узел имеет ссылку на своего родительского узла. Определение узла:

```swift
class Node {
    public var val: Int
    public var left: Node?
    public var right: Node?
    public var parent: Node?
}
```

Согласно определению LCA на Wikipedia: «Наименьший общий предок двух узлов p и q в дереве T — это самый нижний узел, который имеет как p, так и q в качестве потомков (где мы допускаем, что узел может быть потомком самого себя).»

**Идея решения:**  
Поскольку у каждого узла есть указатель на родителя, можно найти путь от `p` до корня и от `q` до корня. Наименьший общий предок — это первый общий узел в этих путях. Используем множество для хранения узлов пути от `p`.

**Оценка сложности:**  
Время: O(h), где h — высота дерева, Память: O(h)

**Решение задачи (Swift, комментариями в коде):**  
```swift
// Определение узла дерева
class Node {
    var val: Int
    var left: Node?
    var right: Node?
    var parent: Node?
    
    init(_ val: Int) {
        self.val = val
        self.left = nil
        self.right = nil
        self.parent = nil
    }
}

func lowestCommonAncestor(_ p: Node?, _ q: Node?) -> Node? {
    guard let p = p, let q = q else { return nil }
    
    // Множество для хранения узлов пути от p к корню
    var path: Set<Node> = []
    var current = p
    
    // Строим путь от p к корню
    while current != nil {
        path.insert(current!)
        current = current!.parent
    }
    
    // Проходим по пути от q к корню, ищем пересечение
    current = q
    while current != nil {
        if path.contains(current!) {
            return current
        }
        current = current!.parent
    }
    
    return nil // Не должен сработать, если вход корректен
}
```

---

## 159. Longest Substring with At Most Two Distinct Characters

**Условие задачи:**  
Данная строка `s`, найдите длину самой длинной подстроки `t`, содержащей не более 2 различных символов.

**Идея решения:**  
Используем слайдинг-окно. Поддерживаем окно, в котором количество уникальных символов ≤ 2. При превышении — сдвигаем левый указатель.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func lengthOfLongestSubstringTwoDistinct(_ s: String) -> Int {
    guard !s.isEmpty else { return 0 }
    
    var charCount: [Character: Int] = [:]
    var left = 0
    var maxLength = 0
    
    for right in s.indices {
        let char = s[right]
        charCount[char, default: 0] += 1
        
        // Если больше 2 уникальных символов — сдвигаем левый указатель
        while charCount.count > 2 {
            let leftChar = s[left]
            charCount[leftChar]! -= 1
            if charCount[leftChar] == 0 {
                charCount.removeValue(forKey: leftChar)
            }
            left += 1
        }
        
        maxLength = max(maxLength, right.index(after: right).distance(to: s.startIndex) - left)
    }
    
    return maxLength
}
```

---

## 85. Maximal Rectangle

**Условие задачи:**  
Дан двумерный бинарный массив `matrix`, заполненный нулями и единицами. Найдите наибольший прямоугольник, содержащий только единицы, и верните его площадь.

**Идея решения:**  
Преобразуем задачу в задачу о наибольшем прямоугольнике в гистограмме. Для каждой строки вычисляем высоту столбцов. Затем применяем алгоритм для гистограммы.

**Оценка сложности:**  
Время: O(m * n), Память: O(n)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func maximalRectangle(_ matrix: [[String]]) -> Int {
    guard !matrix.isEmpty, !matrix[0].isEmpty else { return 0 }
    
    let rows = matrix.count
    let cols = matrix[0].count
    var heights = Array(repeating: 0, count: cols)
    var maxArea = 0
    
    for i in 0..<rows {
        // Обновляем высоты столбцов
        for j in 0..<cols {
            if matrix[i][j] == "1" {
                heights[j] += 1
            } else {
                heights[j] = 0
            }
        }
        
        // Вычисляем максимальную площадь прямоугольника в текущей гистограмме
        maxArea = max(maxArea, largestRectangleInHistogram(heights))
    }
    
    return maxArea
}

private func largestRectangleInHistogram(_ heights: [Int]) -> Int {
    var stack: [Int] = []
    var maxArea = 0
    var i = 0
    
    while i < heights.count {
        // Если стек пуст или текущая высота >= вершины стека
        if stack.isEmpty || heights[i] >= heights[stack.last!] {
            stack.append(i)
            i += 1
        } else {
            // Вычисляем площадь при удалении вершины стека
            let top = stack.removeLast()
            let width = stack.isEmpty ? i : i - stack.last! - 1
            let area = heights[top] * width
            maxArea = max(maxArea, area)
        }
    }
    
    // Обрабатываем оставшиеся элементы в стеке
    while !stack.isEmpty {
        let top = stack.removeLast()
        let width = stack.isEmpty ? i : i - stack.last! - 1
        let area = heights[top] * width
        maxArea = max(maxArea, area)
    }
    
    return maxArea
}
```

---

## 33. Search in Rotated Sorted Array

**Условие задачи:**  
Дан целочисленный массив `nums`, отсортированный по возрастанию (с уникальными значениями). Перед передачей вашей функции массив `nums` может быть повернут на неизвестный индекс `k` (1 <= k < nums.length), так что результат будет `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]`. Например, `[0,1,2,4,5,6,7]` может быть повернут на 3 позиции и стать `[4,5,6,7,0,1,2]`.

Дан массив `nums` после возможного поворота и целое число `target`, верните индекс `target`, если он есть в `nums`, или -1, если нет.

Вы должны написать алгоритм со временем выполнения O(log n).

**Идея решения:**  
Используем модифицированный двоичный поиск. На каждом шаге проверяем, в какой части массива находится `target`: в отсортированной или в повёрнутой.

**Оценка сложности:**  
Время: O(log n), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func search(_ nums: [Int], _ target: Int) -> Int {
    var left = 0
    var right = nums.count - 1
    
    while left <= right {
        let mid = (left + right) / 2
        
        if nums[mid] == target {
            return mid
        }
        
        // Проверяем, левая часть отсортирована
        if nums[left] <= nums[mid] {
            // Если target в левой отсортированной части
            if nums[left] <= target && target < nums[mid] {
                right = mid - 1
            } else {
                left = mid + 1
            }
        } else {
            // Правая часть отсортирована
            if nums[mid] < target && target <= nums[right] {
                left = mid + 1
            } else {
                right = mid - 1
            }
        }
    }
    
    return -1
}
```


## 167. Two Sum II - Input Array Is Sorted

**Условие задачи:**  
Дан 1-индексированный массив целых чисел `numbers`, уже отсортированный в неубывающем порядке. Найдите два числа, которые в сумме дают заданное число `target`. Пусть эти два числа будут `numbers[index1]` и `numbers[index2]`, где `1 <= index1 < index2 <= numbers.length`.

Верните индексы двух чисел, `index1` и `index2`, увеличенные на один, как целочисленный массив `[index1, index2]` длины 2.

Тесты сгенерированы так, что существует ровно одно решение. Вы не можете использовать один и тот же элемент дважды.

Ваше решение должно использовать только постоянное дополнительное пространство.

**Идея решения:**  
Используем два указателя: один сначала, другой с конца. Если сумма больше `target` — двигаем правый указатель. Если меньше — левый. Когда найдём пару — возвращаем индексы + 1.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func twoSum(_ numbers: [Int], _ target: Int) -> [Int] {
    var left = 0
    var right = numbers.count - 1
    
    while left < right {
        let sum = numbers[left] + numbers[right]
        
        if sum == target {
            // Индексы в задаче 1-индексированные
            return [left + 1, right + 1]
        } else if sum < target {
            left += 1
        } else {
            right -= 1
        }
    }
    
    // Решение гарантировано, поэтому этот код никогда не выполнится
    fatalError("No solution found")
}
```

---

## 26. Remove Duplicates from Sorted Array

**Условие задачи:**  
Дан целочисленный массив `nums`, отсортированный в неубывающем порядке. Удалите дубликаты *in-place* так, чтобы каждый уникальный элемент появлялся только один раз. Относительный порядок элементов должен оставаться прежним. Затем верните количество уникальных элементов в `nums`.

Предположим, что количество уникальных элементов равно `k`. Чтобы решение было принято, нужно:
- Изменить массив `nums` так, чтобы первые `k` элементов содержали уникальные элементы в том порядке, в котором они были изначально.
- Вернуть `k`.

**Идея решения:**  
Используем указатель `k` для хранения позиции следующего уникального элемента. Проходим по массиву и добавляем элемент в `nums[k]`, если он отличается от предыдущего.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func removeDuplicates(_ nums: inout [Int]) -> Int {
    guard nums.count > 0 else { return 0 }
    
    var k = 1 // Индекс следующего уникального элемента
    
    for i in 1..<nums.count {
        if nums[i] != nums[i - 1] {
            nums[k] = nums[i]
            k += 1
        }
    }
    
    return k
}
```

---

## 153. Find Minimum in Rotated Sorted Array

**Условие задачи:**  
Предположим, что массив длины `n`, отсортированный по возрастанию, был повернут от 1 до `n` раз. Например, массив `nums = [0,1,2,4,5,6,7]` может стать:
- `[4,5,6,7,0,1,2]` при повороте 4 раза.
- `[0,1,2,4,5,6,7]` при повороте 7 раз.

Поворот массива `[a[0], a[1], ..., a[n-1]]` на 1 раз приводит к `[a[n-1], a[0], a[1], ..., a[n-2]]`.

Дан отсортированный повернутый массив `nums` с уникальными элементами, верните минимальный элемент этого массива.

Вы должны написать алгоритм, который работает за время O(log n).

**Идея решения:**  
Используем двоичный поиск. На каждом шаге проверяем, в какой части находится минимум: левой или правой. Если `mid` > `right`, значит, минимум справа. Иначе — слева.

**Оценка сложности:**  
Время: O(log n), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func findMin(_ nums: [Int]) -> Int {
    var left = 0
    var right = nums.count - 1
    
    while left < right {
        let mid = (left + right) / 2
        
        if nums[mid] > nums[right] {
            // Минимум справа
            left = mid + 1
        } else {
            // Минимум слева или в mid
            right = mid
        }
    }
    
    return nums[left]
}
```

---

## 938. Range Sum of BST

**Условие задачи:**  
Дан корень бинарного дерева поиска и два целых числа `low` и `high`. Верните сумму значений всех узлов с значением в диапазоне `[low, high]`.

**Идея решения:**  
Рекурсивно проходим дерево. Если текущий узел в диапазоне — добавляем его значение. Если `low` > `root.val` — идём вправо. Если `high` < `root.val` — идём влево.

**Оценка сложности:**  
Время: O(h), где h — высота дерева, Память: O(h)

**Решение задачи (Swift, комментариями в коде):**  
```swift
// Определение узла дерева
class TreeNode {
    var val: Int
    var left: TreeNode?
    var right: TreeNode?
    init(_ val: Int) {
        self.val = val
        self.left = nil
        self.right = nil
    }
}

func rangeSumBST(_ root: TreeNode?, _ low: Int, _ high: Int) -> Int {
    guard let root = root else { return 0 }
    
    var sum = 0
    
    // Если текущий узел в диапазоне
    if root.val >= low && root.val <= high {
        sum += root.val
    }
    
    // Если low < root.val — идём влево
    if low < root.val {
        sum += rangeSumBST(root.left, low, high)
    }
    
    // Если high > root.val — идём вправо
    if high > root.val {
        sum += rangeSumBST(root.right, low, high)
    }
    
    return sum
}
```
## 763. Partition Labels

**Условие задачи:**  
Данная строка `s`. Мы хотим разделить строку на как можно больше частей так, чтобы каждая буква появлялась в не более чем одной части. Например, строка "abacc" может быть разделена на ["abab", "cc"], но разделения типа ["aba", "bcc"] или ["ab", "ab", "cc"] недопустимы.

Обратите внимание, что разделение выполняется так, чтобы после конкатенации всех частей в порядке следования результат был равен `s`.

Верните список целых чисел, представляющих размеры этих частей.

**Идея решения:**  
Сначала найдём последнее вхождение каждого символа. Затем проходим по строке, поддерживая текущий диапазон. Если текущий символ имеет последнее вхождение за пределами текущего диапазона — расширяем диапазон. Когда достигаем последнего вхождения — закрываем часть.

**Оценка сложности:**  
Время: O(n), Память: O(1) (поскольку только 26 букв)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func partitionLabels(_ s: String) -> [Int] {
    var lastOccurrence: [Character: Int] = [:]
    
    // Находим последнее вхождение каждого символа
    for (index, char) in s.enumerated() {
        lastOccurrence[char] = index
    }
    
    var result: [Int] = []
    var start = 0
    var end = 0
    
    for (i, char) in s.enumerated() {
        // Обновляем правую границу диапазона
        end = max(end, lastOccurrence[char]!)
        
        // Если достигли последнего вхождения — закрываем часть
        if i == end {
            result.append(end - start + 1)
            start = end + 1
        }
    }
    
    return result
}
```

---

## 238. Product of Array Except Self

**Условие задачи:**  
Дан целочисленный массив `nums`, верните массив `answer` такой, что `answer[i]` равен произведению всех элементов массива `nums`, кроме `nums[i]`.

Произведение любого префикса или суффикса `nums` гарантированно помещается в 32-битное целое число.

Вы должны написать алгоритм, который работает за время O(n) и без использования операции деления.

**Идея решения:**  
Используем два прохода. Первый — вычисление префиксов, второй — суффиксов. Для каждого элемента `answer[i] = prefix[i-1] * suffix[i+1]`.

**Оценка сложности:**  
Время: O(n), Память: O(1) (если не считать выходной массив)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func productExceptSelf(_ nums: [Int]) -> [Int] {
    let n = nums.count
    var result = Array(repeating: 1, count: n)
    
    // Первый проход: вычисляем префиксы
    for i in 1..<n {
        result[i] = result[i - 1] * nums[i - 1]
    }
    
    // Второй проход: умножаем на суффиксы
    var suffix = 1
    for i in stride(from: n - 1, through: 0, by: -1) {
        result[i] *= suffix
        suffix *= nums[i]
    }
    
    return result
}
```

---

## 124. Binary Tree Maximum Path Sum

**Условие задачи:**  
Путь в бинарном дереве — это последовательность узлов, где каждая пара соседних узлов соединена ребром. Узел может появляться в последовательности не более одного раза. Обратите внимание, что путь не обязан проходить через корень.

Сумма пути — это сумма значений узлов в пути.

Дан корень бинарного дерева, верните максимальную сумму пути любого непустого пути.

**Идея решения:**  
Используем рекурсию. Для каждого узла вычисляем максимум пути, проходящего через него. Этот путь может быть:
- Только сам узел.
- Узел + левое поддерево.
- Узел + правое поддерево.
- Узел + левое + правое поддерево (в этом случае путь не продолжается дальше).

**Оценка сложности:**  
Время: O(n), Память: O(h), где h — высота дерева

**Решение задачи (Swift, комментариями в коде):**  
```swift
// Определение узла дерева
class TreeNode {
    var val: Int
    var left: TreeNode?
    var right: TreeNode?
    init(_ val: Int) {
        self.val = val
        self.left = nil
        self.right = nil
    }
}

var maxSum = Int.min

func maxPathSum(_ root: TreeNode?) -> Int {
    maxSum = Int.min
    _maxPathSum(root)
    return maxSum
}

private func _maxPathSum(_ node: TreeNode?) -> Int {
    guard let node = node else { return 0 }
    
    // Рекурсивно получаем максимум для левого и правого поддеревьев
    let leftMax = max(0, _maxPathSum(node.left))
    let rightMax = max(0, _maxPathSum(node.right))
    
    // Вычисляем сумму пути, проходящего через текущий узел
    let currentSum = node.val + leftMax + rightMax
    
    // Обновляем глобальный максимум
    maxSum = max(maxSum, currentSum)
    
    // Возвращаем максимум пути, который может продолжаться вверх
    return node.val + max(leftMax, rightMax)
}
```

---

## 523. Continuous Subarray Sum

**Условие задачи:**  
Дан целочисленный массив `nums` и целое число `k`. Верните `true`, если `nums` содержит хорошую подмассив, иначе `false`.

Хороший подмассив — это подмассив, где:
- его длина не менее двух,
- сумма элементов подмассива кратна `k`.

Примечания:
- Подмассив — это непрерывная часть массива.
- Целое число `x` является кратным `k`, если существует целое `n`, такое что `x = n * k`. 0 всегда является кратным `k`.

**Идея решения:**  
Используем префиксные суммы и хэш-таблицу. Храним остатки от деления суммы на `k`. Если мы видели тот же остаток ранее, значит, между этими точками сумма кратна `k`.

**Оценка сложности:**  
Время: O(n), Память: O(min(n,k))

**Решение задачи (Swift, комментариями в коде):**  
```swift
func checkSubarraySum(_ nums: [Int], _ k: Int) -> Bool {
    var sum = 0
    var remainderToIndex: [Int: Int] = [0: -1] // Остаток -> индекс
    
    for i in 0..<nums.count {
        sum += nums[i]
        let remainder = k == 0 ? sum : sum % k
        
        if let prevIndex = remainderToIndex[remainder], i - prevIndex >= 2 {
            return true
        }
        
        // Добавляем остаток, если он ещё не встречался
        if remainderToIndex[remainder] == nil {
            remainderToIndex[remainder] = i
        }
    }
    
    return false
}
```


## 557. Reverse Words in a String III

**Условие задачи:**  
Данная строка `s`, переверните порядок символов в каждом слове внутри предложения, сохраняя пробелы и исходный порядок слов.

**Идея решения:**  
Разбиваем строку на слова, переворачиваем каждый отдельно и объединяем обратно.

**Оценка сложности:**  
Время: O(n), Память: O(n)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func reverseWords(_ s: String) -> String {
    return s.split(separator: " ")
        .map { String($0.reversed()) }
        .joined(separator: " ")
}
```

---

## 415. Add Strings

**Условие задачи:**  
Даны два неотрицательных целых числа, `num1` и `num2`, представленные как строки. Верните сумму `num1` и `num2` в виде строки.

Вы должны решить задачу без использования встроенных библиотек для обработки больших целых чисел (например, BigInteger). Также нельзя напрямую преобразовывать входные данные в целые числа.

**Идея решения:**  
Складываем цифры с конца, поддерживая перенос.

**Оценка сложности:**  
Время: O(max(m,n)), где m и n — длины строк, Память: O(max(m,n))

**Решение задачи (Swift, комментариями в коде):**  
```swift
func addStrings(_ num1: String, _ num2: String) -> String {
    var i = num1.count - 1
    var j = num2.count - 1
    var carry = 0
    var result: [String] = []
    
    while i >= 0 || j >= 0 || carry > 0 {
        let digit1 = i >= 0 ? Int(String(num1[num1.index(num1.startIndex, offsetBy: i)])! : 0
        let digit2 = j >= 0 ? Int(String(num2[num2.index(num2.startIndex, offsetBy: j)])! : 0
        
        let sum = digit1 + digit2 + carry
        carry = sum / 10
        let digit = sum % 10
        
        result.append(String(digit))
        
        i -= 1
        j -= 1
    }
    
    return result.reversed().joined()
}
```

---

## 234. Palindrome Linked List

**Условие задачи:**  
Дано начало односвязного списка, верните `true`, если он является палиндромом, иначе `false`.

**Идея решения:**  
Находим середину списка с помощью двух указателей, затем разворачиваем вторую половину и сравниваем с первой.

**Оценка сложности:**  
Время: O(n), Память: O(1)

**Решение задачи (Swift, комментариями в коде):**  
```swift
// Определение узла списка
class ListNode {
    var val: Int
    var next: ListNode?
    init(_ val: Int) {
        self.val = val
        self.next = nil
    }
}

func isPalindrome(_ head: ListNode?) -> Bool {
    guard let head = head else { return true }
    
    // Находим середину
    let mid = findMiddle(head)
    
    // Разворачиваем вторую половину
    let secondHalf = reverseList(mid.next)
    
    // Сравниваем первую и вторую половины
    let firstHalf = head
    var result = true
    
    var p1 = firstHalf
    var p2 = secondHalf
    
    while p2 != nil && result {
        if p1!.val != p2!.val {
            result = false
        }
        p1 = p1?.next
        p2 = p2?.next
    }
    
    // Восстанавливаем список
    mid.next = reverseList(secondHalf)
    
    return result
}

private func findMiddle(_ head: ListNode) -> ListNode {
    var slow = head
    var fast = head
    
    while fast.next != nil && fast.next?.next != nil {
        slow = slow.next!
        fast = fast.next!.next!
    }
    
    return slow
}

private func reverseList(_ head: ListNode?) -> ListNode? {
    var prev: ListNode? = nil
    var current = head
    
    while current != nil {
        let next = current?.next
        current?.next = prev
        prev = current
        current = next
    }
    
    return prev
}
```

---

## 387. First Unique Character in a String

**Условие задачи:**  
Данная строка `s`, найдите первый неповторяющийся символ в ней и верните его индекс. Если такого нет, верните -1.


**Идея решения:**  
Подсчитываем частоту каждого символа, затем проходим по строке и возвращаем первый символ с частотой 1.

**Оценка сложности:**  
Время: O(n), Память: O(1) (только 26 букв)

**Решение задачи (Swift, комментариями в коде):**  
```swift
func firstUniqChar(_ s: String) -> Int {
    var charCount: [Character: Int] = [:]
    
    // Подсчитываем частоту символов
    for char in s {
        charCount[char, default: 0] += 1
    }
    
    // Ищем первый символ с частотой 1
    for (index, char) in s.enumerated() {
        if charCount[char] == 1 {
            return index
        }
    }
    
    return -1
}
```




