# Java Collections

### Generic List Iterator

    public class Main {
        public static void main(String[] args) {
            GenericList<Integer> list = new GenericList<>();
            list.add(5);
            list.add(10);
            list.add(20);
            Iterator iterator = list.iterator();
            while(iterator.hasNext()) {
                System.out.println(iterator.next());
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
    
###

