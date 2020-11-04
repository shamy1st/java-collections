# Java Collections
**Iterable Interface** contains only one abstract method. [ Iterator<T> iterator() ]

**Collection Interface** acts as container for other objects, with common operations add(), remove(), contains()

**List Interface** store the inserted order of elements. (allow duplicate values)

**Queue Interface** orders the elements in FIFO (First In First Out).

**Dequeue Interface** is a double ended queue.

**Set Interface** unordered set which doesn't allow to store duplicate items, allow at most one null value.

**SortedSet Interface** elements sorted in the increasing (ascending) order.

**Map Interface** key and value pair, keys are unique.

**SortedMap Interface**

### Hierarchy

--- | ---
--- | ---
![](https://github.com/shamy1st/java-collections/blob/main/images/collection-hierarchy.png) | ![](https://github.com/shamy1st/java-collections/blob/main/images/map-hierarchy.png)

### Properties

class | order | unique | sorted | random | thread | init | load factor
----- | ----- | ------ | ------ | ------ | ------ | ---- | -----------
ArrayList | Yes | - | - | Yes | - | 0 | -
LinkedList | Yes | - | - | - | - | 0 | -
Vector | Yes | - | - |  | Yes | 10 | -
Stack | Yes | - | - |  | Yes | 10 | -
PriorityQueue | - | - | Yes |  | - | 11 |  
ArrayDeque | Yes | - | - |  | - | 16 | 
HashSet | - | Yes | - |  | - | 16 | 0.75
LinkedHashSet | Yes | Yes | - |  | - |  | 
TreeSet | - | Yes | Yes |  | - |  | 
Hashtable | - | Yes | - |  | Yes | 11 | 0.75
HashMap | - | Yes | - |  | - | 16 | 0.75
LinkedHashMap | Yes | Yes | - |  | - | 16 | 0.75
TreeMap | - | Yes | Yes |  | - |  | 

### Complexity

list | add() | get() | remove() | contains() | Data Structure
----- | ----- | ----- | -------- | --------- | --------------
ArrayList | O(1) | O(1) | O(n) | O(n) | dynamic array
LinkedList | O(1) | O(n) | O(1) | O(n) | doubly linked list
Vector | - | - | - |  | dynamic array
Stack | - | - | - |  | dynamic array

queue | add() | get() | remove() | contains()
----- | ----- | ----- | -------- | ----------
PriorityQueue | - | - | - |  
ArrayDeque | - | - | - |  

set | add() | get() | remove() | contains()
----- | ----- | ----- | -------- | ----------
HashSet | - | - | - |  
LinkedHashSet | - | - | - |  
TreeSet | - | - | - |  

map | add() | get() | remove() | contains()
----- | ----- | ----- | -------- | ----------
Hashtable | - | - | - |  
HashMap | - | - | - |  | - 
LinkedHashMap | - | - | - |  
TreeMap | - | - | - |  

### Speed
* LinkedList faster than ArrayList faster than Vector
* HashMap faster than Hashtable

### ArrayList
* like an array, but no size limit.
* slower than LinkedList because a lot of shifting while remove.
* better than LinkedList for storing and accessing data.

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

            public class Vector<E> extends AbstractList<E>
                implements List<E>, RandomAccess, Cloneable, java.io.Serializable
            {
                ...
            }

* **capacity** = oldCapacity + ((capacityIncrement > 0) ? capacityIncrement : oldCapacity)

    size     | 0  | 11 | 21 | 41 | 81  | 161 | 321 | 641  | ...
    -------- | -- | -- | -- | -- | --- | --- | --- | ---- | ---
    capacity | 10 | 20 | 40 | 80 | 160 | 320 | 640 | 1280 | ...

### Stack

### PriorityQueue

### ArrayDeque

### HashSet

### LinkedHashSet

### TreeSet

### Hashtable

### HashMap

### LinkedHashMap

### TreeMap

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

