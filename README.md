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

class | ordr | uniq | sort | rand | thrd | init | load | impl.
----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | -----
ArrayList | Yes | - | - | Yes | - | 10 | 0.75 | dynamic array
LinkedList | Yes | - | - |  | - | 0 |  | doubly linked list
Vector | Yes | - | - |  | Yes | 10 |  | dynamic array
Stack | Yes | - | - |  | Yes | 10 |  | dynamic array
PriorityQueue | - | - | Yes |  | - | 11 |  | dynamic array
ArrayDeque | Yes | - | - |  | - | 16 |  | dynamic array
HashSet | - | Yes | - |  | - | 16 | 0.75 | 
LinkedHashSet | Yes | Yes | - |  | - |  |  | 
TreeSet | - | Yes | Yes |  | - |  |  | 
Hashtable | - | Yes | - |  | Yes | 11 | 0.75 | inherits Dictionary
HashMap | - | Yes | - |  | - | 16 | 0.75 | inherits AbstractMap
LinkedHashMap | Yes | Yes | - |  | - | 16 | 0.75 | 
TreeMap | - | Yes | Yes |  | - |  |  | 

### Speed
* LinkedList faster than ArrayList faster than Vector
* HashMap faster than Hashtable

### ArrayList
--- | ---
--- | ---
manipulation is little bit slower than LinkedList <br> because a lot of shifting needs to occur if any element is removed from the array list. | ![](https://github.com/shamy1st/java-collections/blob/main/images/arraylist.png)

**capacity** = floor(old-capacity * 3/2)
10 | 15 | 22 | 33 | 49 | 73 | 109 | 163 | 244 | 366 | 549 | 823 | 1234

### LinkedList
--- | ---
--- | ---
--- | ![](https://github.com/shamy1st/java-collections/blob/main/images/linkedlist.png)

### Vector

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
