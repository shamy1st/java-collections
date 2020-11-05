# Java Collections
**Iterable Interface** contains only one abstract method. [ Iterator<T> iterator() ]

**Collection Interface** acts as container for other objects, with common operations add(), remove(), contains()

**List Interface** store the inserted order of elements. (allow duplicate values)

**Queue Interface** orders the elements in FIFO (First In First Out).

**Dequeue Interface** is a double ended queue.

**Set Interface** unordered set which doesn't allow to store duplicate items, allow at most one null value.

**SortedSet Interface** elements sorted in the increasing (ascending) order.

**Map Interface** key and value pair, keys are unique.

**SortedMap Interface** provides a total ordering of its elements (elements can be traversed in sorted order of keys).

### Hierarchy

--- | ---
--- | ---
![](https://github.com/shamy1st/java-collections/blob/main/images/collection-hierarchy.png) | ![](https://github.com/shamy1st/java-collections/blob/main/images/map-hierarchy.png)

### Properties

class         | order | unique | null | sorted | random | thread | init | load factor |
------------- | ----- | ------ | ---- | ------ | ------ | ------ | ---- | ----------- |
ArrayList     | Yes   | -      | Yes  | -      | Yes    | -      | 0    | -           |
LinkedList    | Yes   | -      | Yes  | -      | -      | -      | 0    | -           |
Vector        | Yes   | -      | Yes  | -      | Yes    | Yes    | 10   | -           |
Stack         | Yes   | -      | Yes  | -      | Yes    | Yes    | 10   | -           |
PriorityQueue | -     | -      |  ?   | Yes    | -      | -      | 11   | -           |
ArrayDeque    | Yes   | -      | Yes  | -      | -      | -      | 16   | -           |
Hashtable     | -     | Yes    |  -   | -      | -      | Yes    | 11   | 0.75        |
HashSet       | -     | Yes    | Yes  | -      | -      | -      | 16   | 0.75        |
LinkedHashSet | Yes   | Yes    | Yes  | -      | -      | -      | 16   | 0.75        |
TreeSet       | -     | Yes    |  -   |Yes  ASC| ?      | -      | 0    | -           |
HashMap       | -     | Yes    | Yes  | -      | -      | -      | 16   | 0.75        |
LinkedHashMap | Yes   | Yes    | Yes  | -      | -      | -      | 16   | 0.75        |
TreeMap       | -     | Yes    |  -   |Yes  ASC| ?      | -      | 0    | -           |

### Complexity

List                 | Add  | Remove | Get  | Contains | Next | Data Structure
---------------------|------|--------|------|----------|------|-------------------
ArrayList            | O(1) |  O(n)  | O(1) |   O(n)   | O(1) | Dynamic Array
LinkedList           | O(1) |  O(1)  | O(n) |   O(n)   | O(1) | Doubly Linked List
Vector               |      |        |      |          |      | Dynamic Array
Stack                |      |        |      |          |      | Dynamic Array

Queue                   |  Offer   | Peak |   Poll   | Remove | Size | Data Structure
------------------------|----------|------|----------|--------|------|---------------------
PriorityQueue           | O(log n) | O(1) | O(log n) |  O(n)  | O(1) | Balanced Binary Heap
LinkedList              | O(1)     | O(1) | O(1)     |  O(1)  | O(1) | Doubly Linked List
ArrayDeque              | O(1)     | O(1) | O(1)     |  O(n)  | O(1) | Dynamic Array

Set                   |    Add   |  Remove  | Contains |   Next   | Size | Data Structure
----------------------|----------|----------|----------|----------|------|-------------------------
HashSet               | O(1)     | O(1)     | O(1)     | O(h/n)   | O(1) | Hash Table
LinkedHashSet         | O(1)     | O(1)     | O(1)     | O(1)     | O(1) | Hash Table + Linked List
TreeSet               | O(log n) | O(log n) | O(log n) | O(log n) | O(1) | Red-black tree

Map                   |   Get    | ContainsKey |   Next   | Data Structure
----------------------|----------|-------------|----------|-------------------------
HashMap               | O(1)     |   O(1)      | O(h / n) | Hash Table
LinkedHashMap         | O(1)     |   O(1)      | O(1)     | Hash Table + Linked List
TreeMap               | O(log n) |   O(log n)  | O(log n) | Red-black tree

### Capacity

class         | capacity
--------------|-------------------------------------------------------------------------------
ArrayList     | oldCapacity + (oldCapacity >> 1)
LinkedList    | -
Vector        | 2 * oldCapacity
Stack         | 2 * oldCapacity
PriorityQueue | oldCapacity + [ (oldCapacity < 64) ? (oldCapacity + 2) : (oldCapacity / 2) ]
ArrayDeque    | oldCapacity + [ (oldCapacity < 64) ? (oldCapacity + 2) : (oldCapacity / 2) ]
Hashtable     | 2 * oldCapacity + 1, load-factor = 0.75
HashSet       | 2 * oldCapacity, load-factor = 0.75
LinkedHashSet | 2 * oldCapacity, load-factor = 0.75
TreeSet       | -
HashMap       | 2 * oldCapacity, load-factor = 0.75
LinkedHashMap | 2 * oldCapacity, load-factor = 0.75
TreeMap       | -

### ArrayList
* like an array, but no size limit.
* slower than LinkedList because a lot of shifting while remove.
* better than LinkedList for storing and accessing data.
* increments 50% of current array if the number of elements exceeds from its capacity.

        public class ArrayList<E> extends AbstractList<E>
                implements List<E>, RandomAccess, Cloneable, java.io.Serializable
        {
            ...
        }

* **capacity** = oldCapacity + (oldCapacity >> 1)

    size     | 0 | 1  | 11 | 16 | 23 | 34 | 50 | 74  | 110 | 164 | 245 | 367 | 550 | ...
    -------- | - | -- | -- | -- | -- | -- | -- | --- | --- | --- | --- | --- | --- | ---
    capacity | 0 | 10 | 15 | 22 | 33 | 49 | 73 | 109 | 163 | 244 | 366 | 549 | 823 | ...

        public class Main {
            public static void main(String[] args) throws Exception {
                ArrayList<Integer> list = new ArrayList<>();
                int capacity = -1;

                for(int i=0;i<=1000;i++) {
                    if(getCapacity(list) != capacity) {
                        System.out.println("size: " + list.size() 
                        + ", capacity: " + getCapacity(list));
                        capacity = getCapacity(list);
                    }
                    list.add(i);
                }
            }

            private static int getCapacity(ArrayList<Integer> list) throws Exception {
                Field arrayField = ArrayList.class.getDeclaredField("elementData");
                arrayField.setAccessible(true);
                Object[] internalArray = (Object[]) arrayField.get(list);
                return internalArray.length;
            }
        }

### LinkedList
* provides a linked-list data structure.
* uses a doubly linked list to store the elements.
* acts as list and queue.
* faster than ArrayList.
* better than ArrayList for manipulating data.

            public class LinkedList<E> extends AbstractSequentialList<E>
                implements List<E>, Deque<E>, Cloneable, java.io.Serializable
            {
                transient Node<E> first;
                transient Node<E> last;
                
                ...
            }

### Vector
* same as ArrayList but thread-safe
* increments 100% of current array if the number of elements exceeds from its capacity.
* slow because it is synchronized.

            public class Vector<E> extends AbstractList<E>
                implements List<E>, RandomAccess, Cloneable, java.io.Serializable
            {
                ...
            }

* **capacity** = 2 * oldCapacity (if capacityIncrement <= 0)

    size     | 0  | 11 | 21 | 41 | 81  | 161 | 321 | 641  | ...
    -------- | -- | -- | -- | -- | --- | --- | --- | ---- | ---
    capacity | 10 | 20 | 40 | 80 | 160 | 320 | 640 | 1280 | ...

### Stack
* based on FILO (First In Last Out) or LIFO (Last In First Out).

            class Stack<E> extends Vector<E> {
                ...
            }

* **capacity** = 2 * oldCapacity (if capacityIncrement <= 0)

    size     | 0  | 11 | 21 | 41 | 81  | 161 | 321 | 641  | ...
    -------- | -- | -- | -- | -- | --- | --- | --- | ---- | ---
    capacity | 10 | 20 | 40 | 80 | 160 | 320 | 640 | 1280 | ...

### PriorityQueue
* provides the facility of using queue, but it does not orders the elements in FIFO.
* the elements in PriorityQueue must be of Comparable type.
* String and Wrapper classes are Comparable by default.
* To add user-defined objects in PriorityQueue, you need to implement **Comparable** interface.
* order elements according to **compareTo()** method in Comparable interface.

            public class PriorityQueue<E> extends AbstractQueue<E>
                implements java.io.Serializable 
            {
                transient Object[] queue; //balanced binary heap
                ...
            }

* **capacity** = oldCapacity + [ (oldCapacity < 64) ? (oldCapacity + 2) : (oldCapacity >> 1) ]

    size     | 0  | 12 | 25 | 51  | 103 | 154 | 230 | 344 | 515 | 772  | ...
    -------- | -- | -- | -- | --- | --- | --- | --- | --- | --- | ---- | ---
    capacity | 11 | 24 | 50 | 102 | 153 | 229 | 343 | 514 | 771 | 1156 | ...

### ArrayDeque
* Deque is an acronym for "double ended queue".
* supports element insertion and removal at both ends.
* Null elements are not allowed.
* no capacity restrictions.
* faster than LinkedList and Stack.

            public class ArrayDeque<E> extends AbstractCollection<E>
                    implements Deque<E>, Cloneable, Serializable
            {
                transient Object[] elements;
                ...
            }
            
* **capacity** = oldCapacity + [ (oldCapacity < 64) ? (oldCapacity + 2) : (oldCapacity >> 1) ]

    size     | 0  | 16 | 34 | 70  | 105 | 157 | 235 | 352 | 528 | 792  | ...
    -------- | -- | -- | -- | --- | --- | --- | --- | --- | --- | ---- | ---
    capacity | 16 | 34 | 70 | 105 | 157 | 235 | 352 | 528 | 792 | 1188 | ...
    
### Hashtable
* maps keys to values.
* Hashtable is an array of a list.
* each list is known as a bucket.
* the position of the bucket is identified by calling the hashcode() method.
* a Hashtable contains values based on the key.
* Hashtable class doesn't allow null key or value.

            public class Hashtable<K,V> extends Dictionary<K,V>
                implements Map<K,V>, Cloneable, java.io.Serializable 
            {
                private transient Entry<?,?>[] table;
                ...
            }
            
* **capacity** = (oldCapacity << 1) + 1, **load-factor** = 0.75

    size     | 0  | 9  | 18 | 36 | 72  | 144 | 288 | 576  | ...
    -------- | -- | -- | -- | -- | --- | --- | --- | ---- | ---
    capacity | 11 | 23 | 47 | 95 | 191 | 383 | 767 | 1535 | ...

### HashSet
* uses a hash table for storage.
* HashSet allows null value.
* HashSet class is non synchronized.

        public class HashSet<E> extends AbstractSet<E>
            implements Set<E>, Cloneable, java.io.Serializable 
        {
            private transient HashMap<E,Object> map;
            ...
        }
        
* **capacity** = (oldCapacity << 1), **load-factor** = 0.75

    size     | 0  | 12 | 24 | 48  | 96  | 192 | 384  | 768  | ...
    -------- | -- | -- | -- | --- | --- | --- | ---- | ---- | ---
    capacity | 16 | 32 | 64 | 128 | 256 | 512 | 1024 | 2048 | ...

### LinkedHashSet
* LinkedHashSet class is a Hashtable and Linked list implementation of the set interface.
* allows null elements.
* LinkedHashSet class is non synchronized.
* maintains insertion order.

        public class LinkedHashSet<E> extends HashSet<E>
            implements Set<E>, Cloneable, java.io.Serializable 
        {
            ...
        }
        
* **capacity** = (oldCapacity << 1), **load-factor** = 0.75

    size     | 0  | 12 | 24 | 48  | 96  | 192 | 384  | 768  | ...
    -------- | -- | -- | -- | --- | --- | --- | ---- | ---- | ---
    capacity | 16 | 32 | 64 | 128 | 256 | 512 | 1024 | 2048 | ...

### TreeSet
* uses a tree for storage.
* access and retrieval times are quiet fast.
* doesn't allow null element.
* non synchronized.
* maintains ascending order.

        public class TreeSet<E> extends AbstractSet<E>
            implements NavigableSet<E>, Cloneable, java.io.Serializable
        {
            ...
        }

### HashMap
* store key and value pair.
* non synchronized.
* allows only one null key, and multiple null values.

        public class HashMap<K,V> extends AbstractMap<K,V>
            implements Map<K,V>, Cloneable, Serializable 
        {
            ...
        }

* **capacity** = (oldCapacity << 1), **load-factor** = 0.75

    size     | 0  | 12 | 24 | 48  | 96  | 192 | 384  | 768  | ...
    -------- | -- | -- | -- | --- | --- | --- | ---- | ---- | ---
    capacity | 16 | 32 | 64 | 128 | 256 | 512 | 1024 | 2048 | ...

### LinkedHashMap
* maintains insertion order.
* non synchronized.
* allows only one null key, and multiple null values.

        public class LinkedHashMap<K,V> extends HashMap<K,V>
                implements Map<K,V>
        {
            ...
        }

* **capacity** = (oldCapacity << 1), **load-factor** = 0.75

    size     | 0  | 12 | 24 | 48  | 96  | 192 | 384  | 768  | ...
    -------- | -- | -- | -- | --- | --- | --- | ---- | ---- | ---
    capacity | 16 | 32 | 64 | 128 | 256 | 512 | 1024 | 2048 | ...

### TreeMap
* TreeMap class is a red-black tree based implementation.
* storing key-value pairs in sorted order.
* cannot have a null key but can have multiple null values.
* non synchronized.
* maintains ascending order.

        public class TreeMap<K,V> extends AbstractMap<K,V>
            implements NavigableMap<K,V>, Cloneable, java.io.Serializable
        {
            private transient Entry<K,V> root;

            ...
        }

### List Iterator

    public class Main {
        public static void main(String[] args) {
            GenericList<Integer> list = new GenericList<>();
            list.add(5);
            list.add(10);
            list.add(20);
            for(var item : list) {
                System.out.println(item);
            }
        }
    }

    public class GenericList<T> implements Iterable<T> {
        private T[] items = (T[]) new Object[10];
        private int count;

        public void add(T item) {
            items[count++] = item;
        }

        public T get(int index) {
            return items[index];
        }

        @Override
        public Iterator<T> iterator() {
            return new ListIterator(this);
        }

        public class ListIterator implements Iterator<T> {
            private GenericList<T> list;
            private int index;

            public ListIterator(GenericList<T> list) {
                this.list = list;
            }

            @Override
            public boolean hasNext() {
                return (index < list.count);
            }

            @Override
            public T next() {
                return list.items[index++];
            }
        }
    }

### Sort using Comparable Interface

    public class Main {
        public static void main(String[] args) {
            List<Customer> customers = new ArrayList<>();
            customers.add(new Customer("2"));
            customers.add(new Customer("3"));
            customers.add(new Customer("1"));
            Collections.sort(customers);
            System.out.println(customers);
        }
    }

    public class Customer implements Comparable<Customer> {
        private String name;

        public Customer(String name) {
            this.name = name;
        }

        @Override
        public int compareTo(Customer other) {
            return this.name.compareTo(other.name);
        }

        @Override
        public String toString() {
            return this.name;
        }
    }
    
### Sort using Comparator Interface

    public class Main {
        public static void main(String[] args) {
            List<Customer> customers = new ArrayList<>();
            customers.add(new Customer("2", "e3"));
            customers.add(new Customer("3", "e2"));
            customers.add(new Customer("1", "e1"));
            Collections.sort(customers, new EmailComparator());
            System.out.println(customers);
        }
    }

    public class EmailComparator implements Comparator<Customer> {
        @Override
        public int compare(Customer o1, Customer o2) {
            return o1.getEmail().compareTo(o2.getEmail());
        }
    }

    public class Customer implements Comparable<Customer> {
        private String name;
        private String email;

        public Customer(String name, String email) {
            this.name = name;
            this.email = email;
        }

        public String getEmail() {
            return email;
        }

        @Override
        public int compareTo(Customer other) {
            return this.name.compareTo(other.name);
        }

        @Override
        public String toString() {
            return this.name;
        }
    }

### Ref
* https://www.javatpoint.com/collections-in-java
* https://www.javatpoint.com/java-arraylist
* https://stackoverflow.com/questions/39871216/load-factor-of-arraylist-and-vector/44042394
* https://www.javatpoint.com/java-linkedlist
* https://www.baeldung.com/java-collections-complexity
* https://gist.github.com/psayre23/c30a821239f4818b0709
* https://www.javatpoint.com/java-vector
* https://www.javatpoint.com/java-stack
* https://www.javatpoint.com/java-priorityqueue
* https://www.javatpoint.com/java-deque-arraydeque
* https://www.baeldung.com/java-array-deque
* https://www.javatpoint.com/java-hashtable
* https://www.javatpoint.com/java-hashset
* https://www.javatpoint.com/java-linkedhashset
* https://www.javatpoint.com/java-treeset
* https://www.javatpoint.com/java-hashmap
* https://www.javatpoint.com/java-linkedhashmap
* https://www.javatpoint.com/java-treemap
* 
