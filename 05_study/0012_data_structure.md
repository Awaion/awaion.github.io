# Data Structure

- 数组(Array)
- 定义:一组具有相同类型的数据元素的集合,通过索引来访问元素.
- 特点:固定大小,存储相同类型的元素.
- 优点:随机访问元素效率高.
- 缺点:大小固定,插入和删除元素相对较慢.

```text
public static void main(String[] args) {
    // 静态初始化
    int[] numbers1 = {1, 2, 3, 4, 5};
    String[] words1 = {"Hello", "World", "Java", "Array"};

    // 动态初始化
    int[] numbers2 = new int[5];
    numbers2[0] = 1;
    numbers2[1] = 2;
    numbers2[2] = 3;
    numbers2[3] = 4;
    numbers2[4] = 5;

    String[] words2 = new String[4];
    words2[0] = "Hello";
    words2[1] = "World";
    words2[2] = "Java";
    words2[3] = "Array";

    // 静态初始化二维数组
    int[][] matrix1 = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
    };

    // 动态初始化二维数组
    int[][] matrix2 = new int[3][3];
    for (int i = 0; i < matrix2.length; i++) {
        for (int j = 0; j < matrix2[i].length; j++) {
            matrix2[i][j] = i * 3 + j;
        }
    }

    // 遍历二维数组
    for (int i = 0; i < matrix2.length; i++) {
        for (int j = 0; j < matrix2[i].length; j++) {
            System.out.print(matrix2[i][j] + " ");
        }
        System.out.println();
    }

    // 多维数组
}
```

- 链表(LinkedList):
- 定义:由若干个节点组成,每个节点包含数据和指向下一个节点的指针.
- 特点:双向链表,元素之间通过指针连接.
- 优点:插入和删除元素高效,迭代器性能好.
- 缺点:随机访问相对较慢.
- 常用链表:java.util.LinkedList

```text
// 创建简单链表
public class LinkedListDemo {
    Node head;

    public static void main(String[] args) {
        LinkedListDemo list = new LinkedListDemo();
        list.append(1).append(2).append(3).append(4);
        list.printList();
    }

    public LinkedListDemo append(int data) {
        if (head == null) {
            head = new Node(data);
            return this;
        }

        Node current = head;
        while (current.next != null) {
            current = current.next;
        }

        current.next = new Node(data);
        return this;
    }

    public void printList() {
        Node tempNode = head;
        while (tempNode != null) {
            System.out.print(tempNode.data + " -> ");
            tempNode = tempNode.next;
        }
    }
}

class Node {
    int data;
    Node next;

    Node(int d) {
        data = d;
        next = null;
    }
}


```

- 栈(Stack):
- 定义:一种后进先出(LIFO)的数据结构,只允许在栈顶进行插入和删除操作.
- 常用栈:java.util.Stack extends Vector
- 官方推荐使用 Deque 接口的实现类java.util.ArrayDeque 或 java.util.LinkedList 来实现栈的功能

```text
public class StackDemo<T> {
    private Deque<T> deque;

    public StackDemo() {
        deque = new ArrayDeque<>();
    }

    // 入栈操作
    public void push(T item) {
        deque.push(item);
    }

    // 出栈操作
    public T pop() {
        if (isEmpty()) {
            return null;
        }
        return deque.pop();
    }

    // 查看栈顶元素但不移除
    public T peek() {
        if (isEmpty()) {
            return null;
        }
        return deque.peek();
    }

    // 检查栈是否为空
    public boolean isEmpty() {
        return deque.isEmpty();
    }

    // 获取栈的大小
    public int size() {
        return deque.size();
    }

    public static void main(String[] args) {
        StackDemo<Integer> stack = new StackDemo<>();
        stack.push(1);
        stack.push(2);
        stack.push(3);

        System.out.println("栈顶元素:" + stack.peek()); // 输出:栈顶元素:3
        System.out.println("弹出栈顶元素:" + stack.pop()); // 输出:弹出栈顶元素:3
        System.out.println("栈的大小:" + stack.size()); // 输出:栈的大小:2

        // 遍历栈,栈通常不支持直接遍历,会破坏栈的LIFO特性,示例使用Deque的迭代功能
        for (Integer item :stack.deque) {
            System.out.println(item); // 输出:2, 1(从栈顶到底部)

        }
    }
}
```

- 队列(Queue):
- 定义:一种先进先出(FIFO)的数据结构,只允许在队头和队尾进行插入和删除操作.
- java.util.LinkedList 基于链表双端队列
- java.util.ArrayDeque 基于数组的双端队列
- java.util.PriorityQueue 基于优先级堆的无界队列
- java.util.concurrent.ConcurrentLinkedQueue 基于链接节点的无界线程安全队列
- java.util.concurrent.LinkedBlockingQueue 基于链接节点的范围任意的阻塞队列
- java.util.concurrent.ArrayBlockingQueue 由数组结构组成的有界阻塞队列


```text
public class QueueDemo<T> {
    private T[] items;
    private int size;
    private int front;
    private int rear;

    @SuppressWarnings("unchecked")
    public QueueDemo(int capacity) {
        items = (T[]) new Object[capacity];
        size = 0;
        front = 0;
        rear = -1; // 使用-1表示队列为空
    }

    // 入队操作
    public boolean enqueue(T item) {
        if (isFull()) {
            return false; // 队列已满,不能入队
        }
        if (rear == items.length - 1) {
            rear = 0;// 队列尾到达数组末尾,需要循环回到数组开始
        } else {
            rear++;
        }
        items[rear] = item;
        size++;
        return true;
    }

    // 出队操作
    public T dequeue() {
        if (isEmpty()) {
            return null; // 队列为空,不能出队
        }
        T item = items[front];
        items[front] = null; // 避免对象引用保持,帮助垃圾回收
        front = (front + 1) % items.length; // 循环回到数组开始
        size--;
        return item;
    }

    // 查看队首元素
    public T peek() {
        if (isEmpty()) {
            return null; // 队列为空
        }
        return items[front];
    }

    // 检查队列是否为空
    public boolean isEmpty() {
        return size == 0;
    }

    // 检查队列是否已满
    public boolean isFull() {
        return size == items.length;
    }

    // 获取队列大小
    public int size() {
        return size;
    }

    // 打印队列内容,用于调试
    public void printQueue() {
        if (isEmpty()) {
            System.out.println("Queue is empty.");
            return;
        }
        int k = front;
        while (k <= rear) {
            System.out.print(items[k] + " ");
            k = (k + 1) % items.length;
        }
        System.out.println();
    }

    public static void main(String[] args) {
        QueueDemo<Integer> queue = new QueueDemo<>(5);
        queue.enqueue(1);
        queue.enqueue(2);
        queue.enqueue(3);
        queue.printQueue(); // 输出:1 2 3
        System.out.println("Dequeued:" + queue.dequeue()); // 输出:Dequeued:1
        queue.printQueue(); // 输出:2 3
        System.out.println("Peek:" + queue.peek()); // 输出:Peek:2
    }
}
```

- 双端队列(Deque)
- 支持在两端插入和删除元素的线性数据结构
- java.util.LinkedList 基于链表双端队列
- java.util.ArrayDeque 基于数组的双端队列
- java.util.concurrent.LinkedBlockingDeque 基于链接节点的,范围任意的,线程安全的双端阻塞队列

```text
public class DequeDemo<T> {
    private Object[] items;
    private int front;
    private int rear;
    private int size;

    public DequeDemo(int capacity) {
        items = new Object[capacity];
        front = 0;
        rear = -1;
        size = 0;
    }

    // 在队尾添加元素
    public void addLast(T item) {
        if (isFull()) {
            throw new IllegalStateException("Deque is full");
        }
        rear = (rear + 1) % items.length;
        items[rear] = item;
        size++;
    }

    // 在队头添加元素
    public void addFirst(T item) {
        if (isFull()) {
            throw new IllegalStateException("Deque is full");
        }
        if (isEmpty()) {
            rear = 0;
        } else {
            front = (front - 1 + items.length) % items.length; // 循环数组
        }
        items[front] = item;
        size++;
    }

    // 从队头移除元素
    public T removeFirst() {
        if (isEmpty()) {
            throw new NoSuchElementException("Deque is empty");
        }
        T item = (T) items[front];
        front = (front + 1) % items.length;
        size--;
        return item;
    }

    // 从队尾移除元素
    public T removeLast() {
        if (isEmpty()) {
            throw new NoSuchElementException("Deque is empty");
        }
        rear = (rear - 1 + items.length) % items.length; // 循环数组
        T item = (T) items[rear + 1]; // 注意：因为rear已经减1,所以这里用rear+1
        size--;
        return item;
    }

    // 查看队头元素
    public T getFirst() {
        if (isEmpty()) {
            throw new NoSuchElementException("Deque is empty");
        }
        return (T) items[front];
    }

    // 查看队尾元素
    public T getLast() {
        if (isEmpty()) {
            throw new NoSuchElementException("Deque is empty");
        }
        return (T) items[rear];
    }

    // 检查Deque是否为空
    public boolean isEmpty() {
        return size == 0;
    }

    // 检查Deque是否已满
    public boolean isFull() {
        return size == items.length;
    }

    // 获取Deque的大小
    public int size() {
        return size;
    }

    public static void main(String[] args) {
        DequeDemo<Integer> deque = new DequeDemo<>(5);
        deque.addLast(1);
        deque.addLast(2);
        deque.addFirst(0);
        System.out.println("Deque:" + deque.getFirst() + ", " + deque.getLast()); // 输出:Deque:0, 2
        System.out.println("Size:" + deque.size()); // 输出:Size:3
        System.out.println("Removed:" + deque.removeLast()); // 输出:Removed:2
    }
}
```

- 集合(Set):
- 定义:一种不允许重复元素的数据结构.
- java.util.HashSet
- java.util.TreeSet
- HashSet特点:无序集合,基于HashMap实现.
- TreeSet特点:有序集合,基于红黑树实现,不允许重复元素.

```text
public class HashSetDemo<T> {
    private static final int DEFAULT_CAPACITY = 16;
    private static final float LOAD_FACTOR = 0.75f;

    private int capacity;
    private int size;
    private Node<T>[] buckets; // 哈希桶

    // 内部节点类用于存储数据和指向下一个节点的链接
    private static class Node<T> {
        T data;
        Node<T> next;

        public Node(T data) {
            this.data = data;
        }
    }

    // 构造函数
    public HashSetDemo() {
        this(DEFAULT_CAPACITY);
    }

    public HashSetDemo(int capacity) {
        this.capacity = capacity;
        buckets = new Node[capacity];
    }

    // 简单的哈希函数
    private int hash(T data) {
        return Math.abs(data.hashCode()) % capacity;
    }

    // 添加元素
    public boolean add(T data) {
        int index = hash(data);
        Node<T> current = buckets[index];

        // 检查是否存在重复元素
        while (current != null) {
            if (current.data.equals(data)) {
                return false; // 如果元素已存在,则不添加
            }
            current = current.next;
        }

        // 添加新元素到链表
        buckets[index] = new Node<>(data);
        size++;
        // 检查是否需要扩容(这里为了简化省略了扩容逻辑)

        return true;
    }

    // 检查元素是否存在
    public boolean contains(T data) {
        int index = hash(data);
        Node<T> current = buckets[index];

        while (current != null) {
            if (current.data.equals(data)) {
                return true;
            }
            current = current.next;
        }

        return false;
    }

    // 获取集合大小
    public int size() {
        return size;
    }

    public static void main(String[] args) {
        HashSetDemo<String> set = new HashSetDemo<>();
        set.add("apple");
        set.add("banana");
        set.add("apple"); // 这将不会添加,因为元素已存在

        System.out.println(set.contains("apple")); // 输出:true
        System.out.println(set.contains("orange")); // 输出:false
        System.out.println(set.size()); // 输出:2(注意：虽然添加了两次"apple",但只计算一次)
    }
}
```

```text
public class TreeSetDemo<T extends Comparable<T>> {
    private T[] elements;
    private int size;

    @SuppressWarnings("unchecked")
    public TreeSetDemo() {
        this.elements = (T[]) new Comparable[16]; // 初始容量
        this.size = 0;
    }

    // 添加元素(简单插入排序)
    public void add(T element) {
        if (size == elements.length) {
            // 如果数组满了,重新分配空间(这里简单翻倍,实际实现可能需要更复杂的逻辑)
            elements = Arrays.copyOf(elements, elements.length * 2);
        }

        // 找到合适的位置插入
        int i;
        for (i = 0; i < size; i++) {
            if (element.compareTo(elements[i]) < 0) {
                break; // 找到插入点
            }
        }

        // 插入元素并移动后续元素
        for (int j = size; j > i; j--) {
            elements[j] = elements[j - 1];
        }

        elements[i] = element;
        size++;
    }

    // 其他方法,如remove,contains等可以根据需要添加

    // 打印集合
    public void printSet() {
        for (int i = 0; i < size; i++) {
            System.out.print(elements[i] + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        TreeSetDemo<Integer> set = new TreeSetDemo<>();
        set.add(5);
        set.add(2);
        set.add(9);
        set.add(1);
        set.add(5); // 这个5会被忽略,因为集合中已存在
        set.printSet(); // 输出:1 2 5 9
    }
}
```

- 列表(List):
- 定义:一种有序的数据结构,允许重复元素.
- java.util.ArrayList
- java.util.LinkedList
- ArrayList特点:动态数组,可变大小,高效的随机访问和快速尾部插入.
- LinkedList特点:双向链表,插入和删除元素高效,迭代器性能好.

```text
public class ArrayListDemo<T> {
    private Object[] elements;
    private int size;

    public ArrayListDemo() {
        this.elements = new Object[10]; // 初始容量为10
        this.size = 0;
    }

    // 添加元素到列表末尾
    public void add(T element) {
        // 如果当前数组已满,则扩容
        if (size == elements.length) {
            elements = Arrays.copyOf(elements, elements.length * 2);
        }
        elements[size++] = element; // 将元素添加到当前末尾并增加大小
    }

    // 获取指定位置的元素
    public T get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index:" + index + ", Size:" + size);
        }
        return (T) elements[index]; // 强制类型转换并返回元素
    }

    // 删除指定位置的元素
    public T remove(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException("Index:" + index + ", Size:" + size);
        }
        T removed = (T) elements[index];
        // 将后续元素向前移动一个位置
        for (int i = index; i < size - 1; i++) {
            elements[i] = elements[i + 1];
        }
        elements[--size] = null; // 将最后一个位置置空以便垃圾回收
        return removed;
    }

    // 获取列表的大小
    public int size() {
        return size;
    }

    // 判断列表是否为空
    public boolean isEmpty() {
        return size == 0;
    }

    // 清空列表
    public void clear() {
        for (int i = 0; i < size; i++) {
            elements[i] = null; // 将所有元素置空以便垃圾回收
        }
        size = 0;
    }

    // 打印列表内容
    public void printList() {
        for (int i = 0; i < size; i++) {
            System.out.print(elements[i] + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        ArrayListDemo<String> list = new ArrayListDemo<>();
        list.add("Apple");
        list.add("Banana");
        list.add("Cherry");
        list.printList(); // 输出:Apple Banana Cherry
        System.out.println(list.get(1)); // 输出:Banana
        list.remove(1);
        list.printList(); // 输出:Apple Cherry
    }
}
```

- 映射(Map):
- 定义:一种键值对的数据结构,以键作为唯一标识符来存储和访问元素.
- java.util.HashMap
- java.util.TreeMap
- HashMap特点:基于哈希表实现的键值对存储结构,高效的查找,插入和删除操作.
- TreeMap特点:基于红黑树实现的有序键值对存储结构,支持按照键的顺序遍历.

```text
public class HashMapDemo<K, V> {
    private static final int DEFAULT_CAPACITY = 16;
    private static final float DEFAULT_LOAD_FACTOR = 0.75f;

    private Entry<K, V>[] table;
    private int size;
    private int threshold;
    private float loadFactor;

    private static class Entry<K, V> {
        final K key;
        V value;
        Entry<K, V> next;

        Entry(K key, V value, Entry<K, V> next) {
            this.key = key;
            this.value = value;
            this.next = next;
        }
    }

    public HashMapDemo() {
        this.loadFactor = DEFAULT_LOAD_FACTOR;
        this.threshold = (int) (DEFAULT_CAPACITY * DEFAULT_LOAD_FACTOR);
        this.table = new Entry[DEFAULT_CAPACITY];
    }

    // 简单的哈希函数
    private int hash(K key) {
        return key.hashCode() & (table.length - 1);
    }

    // 添加键值对
    public void put(K key, V value) {
        if (key == null) {
            throw new NullPointerException("key cannot be null");
        }

        if (size >= threshold) {
            resize();
        }

        int index = hash(key);
        Entry<K, V> entry = table[index];
        if (entry == null) {
            table[index] = new Entry<>(key, value, null);
        } else {
            while (entry.next != null && !entry.key.equals(key)) {
                entry = entry.next;
            }
            if (entry.key.equals(key)) {
                entry.value = value; // 更新值
            } else {
                entry.next = new Entry<>(key, value, entry.next); // 添加到链表
            }
        }
        size++;
    }

    // 获取值
    public V get(K key) {
        if (key == null) {
            return null;
        }

        int index = hash(key);
        Entry<K, V> entry = table[index];
        while (entry != null) {
            if (entry.key.equals(key)) {
                return entry.value;
            }
            entry = entry.next;
        }
        return null;
    }

    // 扩容
    private void resize() {
        Entry<K, V>[] oldTable = table;
        int oldCapacity = oldTable.length;
        if (oldCapacity == Integer.MAX_VALUE) {
            // 如果已经达到最大容量,则不再扩容
            threshold = Integer.MAX_VALUE;
            return;
        }

        int newCapacity = oldCapacity * 2;
        threshold = (int) (newCapacity * loadFactor);
        table = new Entry[newCapacity];

        for (Entry<K, V> e :oldTable) {
            if (e != null) {
                doResize(e);
            }
        }
    }

    // 辅助方法,用于在扩容时重新哈希并放置元素
    private void doResize(Entry<K, V> e) {
        while (e != null) {
            Entry<K, V> next = e.next;
            int index = hash(e.key);
            e.next = table[index];
            table[index] = e;
            e = next;
        }
    }

    public static void main(String[] args) {
        HashMapDemo<String, Integer> map = new HashMapDemo<>();
        map.put("one", 1);
        map.put("two", 2);
        map.put("three", 3);
        System.out.println(map.get("one"));
    }
}
```

```text
public class TreeMapDemo<K extends Comparable<K>, V> {
    private static class Node<K, V> {
        K key;
        V value;
        Node<K, V> left;
        Node<K, V> right;
        Node<K, V> parent;

        Node(K key, V value) {
            this.key = key;
            this.value = value;
        }
    }

    private Node<K, V> root;
    private int size;

    // 插入节点
    private Node<K, V> insert(Node<K, V> node, K key, V value) {
        if (node == null) {
            return new Node<>(key, value);
        }

        int cmp = key.compareTo(node.key);
        if (cmp < 0) {
            node.left = insert(node.left, key, value);
            node.left.parent = node;
        } else if (cmp > 0) {
            node.right = insert(node.right, key, value);
            node.right.parent = node;
        } else {
            // 如果键已存在,则更新值
            node.value = value;
        }

        // 这里简化了红黑树的平衡操作
        // 在实际TreeMap实现中,需要维护红黑树的性质

        return node;
    }

    // 获取节点
    private Node<K, V> getNode(Node<K, V> node, K key) {
        if (node == null) {
            return null;
        }

        int cmp = key.compareTo(node.key);
        if (cmp < 0) {
            return getNode(node.left, key);
        } else if (cmp > 0) {
            return getNode(node.right, key);
        } else {
            return node;
        }
    }

    // 插入键值对
    public void put(K key, V value) {
        root = insert(root, key, value);
        size++;
    }

    // 获取值
    public V get(K key) {
        Node<K, V> node = getNode(root, key);
        return node == null ? null :node.value;
    }

    // 获取大小
    public int size() {
        return size;
    }

    public static void main(String[] args) {
        TreeMapDemo<Integer, String> map = new TreeMapDemo<>();
        map.put(3, "three");
        map.put(1, "one");
        map.put(2, "two");
        System.out.println(map.get(2)); // 输出:two
    }
}
```

- 堆(Heap):
- 定义:一种可以快速找到最大值(或最小值)的数据结构,常用于优先队列的实现.
- java.util.PriorityQueue 基于优先级堆的无界队列

```text
public class HeapDemo {
    private int[] heap;
    private int size;

    // 构造函数,初始化堆的大小
    public HeapDemo(int capacity) {
        heap = new int[capacity];
        size = 0;
    }

    // 父节点索引
    private int parent(int i) {
        return (i - 1) / 2;
    }

    // 左孩子节点索引
    private int leftChild(int i) {
        return 2 * i + 1;
    }

    // 右孩子节点索引
    private int rightChild(int i) {
        return 2 * i + 2;
    }

    // 堆化一个子树,从给定的节点开始
    private void heapify(int i) {
        int smallest = i;
        int left = leftChild(i);
        int right = rightChild(i);

        // 如果左孩子存在且小于当前节点
        if (left < size && heap[left] < heap[smallest]) {
            smallest = left;
        }

        // 如果右孩子存在且小于当前最小的节点
        if (right < size && heap[right] < heap[smallest]) {
            smallest = right;
        }

        // 如果找到更小的节点,则交换
        if (smallest != i) {
            swap(i, smallest);
            // 递归地堆化受影响的子树
            heapify(smallest);
        }
    }

    // 交换两个元素
    private void swap(int i, int j) {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }

    // 插入元素
    public void insert(int key) {
        if (size == heap.length) {
            // 堆已满,可以根据需要扩展堆的大小
            throw new IllegalStateException("Heap is full");
        }

        heap[size] = key;
        size++;

        // 从插入位置开始向上堆化
        int currentIndex = size - 1;
        while (heap[currentIndex] < heap[parent(currentIndex)]) {
            swap(currentIndex, parent(currentIndex));
            currentIndex = parent(currentIndex);
        }
    }

    // 提取最小元素
    public int extractMin() {
        if (size == 0) {
            throw new NoSuchElementException("Heap is empty");
        }

        int min = heap[0];
        heap[0] = heap[--size];
        heapify(0); // 堆化根节点

        return min;
    }

    // 获取堆的大小
    public int getSize() {
        return size;
    }

    public static void main(String[] args) {
        HeapDemo heapDemo = new HeapDemo(10);
        heapDemo.insert(3);
        heapDemo.insert(1);
        heapDemo.insert(4);
        heapDemo.insert(1);
        heapDemo.insert(5);
        heapDemo.insert(9);
        heapDemo.insert(2);
        heapDemo.insert(6);
        heapDemo.insert(5);
        heapDemo.insert(3);

        while (heapDemo.getSize() > 0) {
            System.out.println(heapDemo.extractMin());
        }
    }
}
```

- 树(Tree):
- 定义:一种层次关系的数据结构,包含根节点,子节点和叶子节点等.
- 二叉树(Binary Tree),二叉搜索树(Binary Search Tree),平衡二叉搜索树(如AVL树,红黑树),B树(B-Tree),B+树(B+-Tree),N叉树(N-ary Tree),堆(Heap)
- java.util.TreeMap
- java.util.TreeSet
- java.util.PriorityQueue

```text
class TreeNode<T> {
    T data;
    TreeNode<T> left;
    TreeNode<T> right;

    public TreeNode(T data) {
        this.data = data;
        this.left = null;
        this.right = null;
    }
}

public class TreeDemo<T> {
    private TreeNode<T> root;

    public TreeDemo() {
        this.root = null;
    }

    // 添加节点的方法(这里只提供了一个简单的添加方式,实际可能需要更复杂的逻辑)
    public void add(T data) {
        root = addRecursive(root, data);
    }

    private TreeNode<T> addRecursive(TreeNode<T> current, T data) {
        if (current == null) {
            return new TreeNode<>(data);
        }

        // 假设这里简单地按照数据大小决定左子树还是右子树(实际中可能根据其他规则)
        if (((Comparable<T>) data).compareTo(current.data) < 0) {
            current.left = addRecursive(current.left, data);
        } else {
            current.right = addRecursive(current.right, data);
        }

        return current;
    }

    // 前序遍历(根-左-右)
    public void preOrderTraversal(TreeNode<T> node) {
        if (node != null) {
            System.out.print(node.data + " ");
            preOrderTraversal(node.left);
            preOrderTraversal(node.right);
        }
    }

    // 中序遍历(左-根-右)
    public void inOrderTraversal(TreeNode<T> node) {
        if (node != null) {
            inOrderTraversal(node.left);
            System.out.print(node.data + " ");
            inOrderTraversal(node.right);
        }
    }

    // 后序遍历(左-右-根)
    public void postOrderTraversal(TreeNode<T> node) {
        if (node != null) {
            postOrderTraversal(node.left);
            postOrderTraversal(node.right);
            System.out.print(node.data + " ");
        }
    }

    public static void main(String[] args) {
        TreeDemo<Integer> tree = new TreeDemo<>();

        tree.add(5);
        tree.add(3);
        tree.add(7);
        tree.add(2);
        tree.add(4);
        tree.add(6);
        tree.add(8);

        System.out.println("Pre-order traversal:");
        tree.preOrderTraversal(tree.root);

        System.out.println("\nIn-order traversal:");
        tree.inOrderTraversal(tree.root);

        System.out.println("\nPost-order traversal:");
        tree.postOrderTraversal(tree.root);
    }
}
```

- 图(Graph):
- 定义:由节点和节点之间的关系(边)组成的数据结构,常用于描述网络等复杂关系.
- 有向图(Directed Graph)和无向图(Undirected Graph)
- 加权图(Weighted Graph)和非加权图(Unweighted Graph)
- 深度优先搜索,广度优先搜索,最短路径算法
- JGraphT,JGraphX,Apache Commons Graph,Guava Graphs

```text
public class GraphDemo {
    private Map<Integer, List<Integer>> adjacencyList;

    public GraphDemo() {
        adjacencyList = new HashMap<>();
    }

    // 添加节点,如果节点已存在则不执行任何操作
    public void addVertex(int vertex) {
        adjacencyList.putIfAbsent(vertex, new ArrayList<>());
    }

    // 添加边,如果节点不存在则先添加节点
    public void addEdge(int source, int destination) {
        addVertex(source);
        addVertex(destination);
        adjacencyList.get(source).add(destination);
        adjacencyList.get(destination).add(source); // 对于无向图,两边都要添加
    }

    // 打印图的邻接列表表示
    public void printGraph() {
        for (Map.Entry<Integer, List<Integer>> entry :adjacencyList.entrySet()) {
            int vertex = entry.getKey();
            List<Integer> neighbors = entry.getValue();
            System.out.println("Vertex " + vertex + " is connected to " + neighbors);
        }
    }

    public static void main(String[] args) {
        GraphDemo graphDemo = new GraphDemo();

        // 添加节点
        graphDemo.addVertex(1);
        graphDemo.addVertex(2);
        graphDemo.addVertex(3);
        graphDemo.addVertex(4);

        // 添加边
        graphDemo.addEdge(1, 2);
        graphDemo.addEdge(1, 3);
        graphDemo.addEdge(2, 3);
        graphDemo.addEdge(3, 4);

        // 打印图
        graphDemo.printGraph();
    }
}
```

- 数组时间复杂度:访问元素O(1),插入元素(末尾)：O(1),插入元素(中间或开始)：O(n)
- 链表时间复杂度:访问元素：O(n),插入元素(末尾)：O(n),插入元素(开始)：O(1)
- 栈时间复杂度:访问元素(栈顶)：O(1),插入元素(栈顶)：O(1)
- 双端队列时间复杂度:访问元素(队首/队尾)：O(1)插入元素(队尾)：O(1)
- 哈希表时间复杂度:平均O(1),最坏O(n)
- 跳表时间复杂度为O(log n)

MySQL索引数据结构
- 范围查找索引 B+Tree
  - 每个节点可以存放多个索引值及对应的指针,且叶子节点间通过链表相连
  - 减少树的深度和大小,从而减少IO的次数,提升查询效率
  - InnoDB的聚簇索引叶子节点包含行数据,而非聚簇索引的叶子节点存储主键值,根据主键查询效率极高
  - MyISAM的索引叶子节点存储的是指向行数据的指针
- 精确匹配索引 Hash
  - 在InnoDB中由存储引擎自动优化创建
- 全文搜索 Full-text
  - 基于倒排索引实现

- 常见NoSQL
- 键值存储(Key-Value Store) Redis
- 文档型数据库(Document-Oriented Database) MongoDB
- 图形数据库(Graph Database) Neo4J,Infinite Graph
- 时间序列数据库(Time Series Database) 

Redis 数据结构

String(字符串)
- 唯一的key字符串作为名称,通过这个唯一key值来获取相应的value数据
- 应用场景:热点数据缓存,对象缓存,全页缓存,以及用于实现分布式锁(SETNX + EXPIRE)等
- 分布式锁,在del释放锁之前加一个判断,验证当前的锁是不是自己加的锁
- 分布式锁,为获取锁的线程增加守护线程,为将要过期但未释放的锁增加有效时间

List(列表)
- 链表结构,允许在链表的两端插入和删除元素,也可以获取指定位置的元素
- 应用场景:发布与订阅,消息队列等

Set(集合)
- 集合中的元素是无序的且唯一
- 应用场景:用于实现交集,并集,差集等集合运算

Hash(哈希)
- 哈希表结构,类似于字典,可以存储键值对,且值只能是字符串
- 应用场景:存储用户信息,配置信息等

Zset(有序集合)
- 跳跃列表(Skip List)的数据结构
- 应用场景:排行榜,带权重的集合运算等

SDS(简单动态字符串)
- 解决了C语言字符串的一些问题,如获取字符串长度的时间复杂度,不能存储'\0'字符等

HyperLogLog
- 通过概率计算的方式,用较小的内存空间来完成对大数据集的基数统计
- 即使在输入元素的数量或体积非常大时,计算基数所需的空间总是固定的,并且很小,花费12 KB内存,就可以计算接近2^64个不同元素的基数
- 用于估计数据集中不重复元素的数量(基数),统计注册IP数,每日访问IP数,页面实时UV数,在线用户数等

Bitmap(位图)
- 是一种基于String类型实现的特殊数据结构,它利用字符串类型键(key)来存储一系列连续的二进制位(bits)
- 每个位可以独立地表示一个布尔值(0或1),非常适合用于存储和操作大量二值状态的数据
- 应用场景:用户在线状态跟踪,用户签到系统,数据去重,数据统计


