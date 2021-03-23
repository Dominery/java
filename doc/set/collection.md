# Collection接口方法



| Modifier and Type        | Method and Description                                       |
| ------------------------ | ------------------------------------------------------------ |
| `boolean`                | `add(E e)`  Ensures that this collection contains the specified element  (optional operation). |
| `boolean`                | `addAll(Collection<? extends E> c)`  Adds all of the elements in the specified collection to this  collection (optional operation). |
| `void`                   | `clear()`  Removes all of the elements from this collection (optional  operation). |
| `boolean`                | `contains(Object o)`  Returns `true` if this collection contains the specified  element. |
| `boolean`                | `containsAll(Collection<?> c)`  Returns `true` if this collection contains all of the  elements in the specified collection. |
| `boolean`                | `equals(Object o)`  Compares the specified object with this collection for  equality. |
| `int`                    | `hashCode()`  Returns the hash code value for this  collection. |
| `boolean`                | `isEmpty()`  Returns `true` if this collection contains no  elements. |
| `Iterator<E>`            | `iterator()`  Returns an iterator over the elements in this  collection. |
| `default Stream<E>`      | `parallelStream()`  Returns a possibly parallel `Stream` with this  collection as its source. |
| `boolean`                | `remove(Object o)`  Removes a single instance of the specified element from this  collection, if it is present (optional operation). |
| `boolean`                | `removeAll(Collection<?> c)`  Removes all of this collection's elements that are also  contained in the specified collection (optional operation). |
| `default boolean`        | `removeIf(Predicate<? super E> filter)`  Removes all of the elements of this collection that satisfy the  given predicate. |
| `boolean`                | `retainAll(Collection<?> c)`  Retains only the elements in this collection that are contained  in the specified collection (optional operation). |
| `int`                    | `size()`  Returns the number of elements in this  collection. |
| `default Spliterator<E>` | spliterator()  Creates a `Spliterator` over the  elements in this collection. |
| `default Stream<E>`      | `stream()`  Returns a sequential `Stream` with this collection  as its source. |
| `Object[]`               | `toArray()`  Returns an array containing all of the elements in this  collection. |
| `<T> T[]`                | `toArray(T[] a)`  Returns an array containing all of the elements in this  collection; the runtime type of the returned array is that of the specified  array. |

如果希望在自定义对象集合中调用`cotains`方法返回值是按照对象的属性判断，那么需要重写该对象的`equals`方法。

`Collection`调用`iterator`方法会返回一个迭代器对象。该对象可以通过`hasNext`、`next`方法进行对集合的遍历操作。

jdk5.0新增`forEach`循环，通过内部迭代器实现。

```java
for(Object obj:list){
    
}
```

