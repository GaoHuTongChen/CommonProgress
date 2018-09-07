
1、ArrayList  优点
1）支持自动改变大小的功能
2）可以灵活的插入元素
3）可以灵活的删除元素

二、局限性

跟一般的数组比起来，速度上差些。
因为它是动态数组，初始化大小容量4，当数据存满时扩容是以当前数组容量大小的2倍扩容，之后再把数组元素一个一个的存入，数组在扩容时浪费一定的内存空间，和存储时间，而且，元素添加是一个装箱的过程，所以说，跟一般的数组比起来，速度上差些

总结一下：就是一个动态的数组。做了一层封装.
![这里写图片描述](https://img-blog.csdn.net/20170519105333354?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzMwOTg3MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

> @since   1.2
> public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable

看源码初始化的长度是10

> /**
     * Constructs an empty list with an initial capacity of ten.
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
    
    自定义初始化
```
   public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    } 
```

传入别的可以向下转化
```
public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // c.toArray might (incorrectly) not return Object[] (see 6260652)
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // replace with empty array.
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }
```


扩容每次1.5倍
```
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```
这里最大为int最大值就是4位 2^32次，  可以看到还是保留了8位  为什么好保留呢？
```
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
```
接下来看一段源码的注释  vm的数组保留一些标题。
`/**
     * The maximum size of array to allocate.
     * Some VMs reserve some header words in an array.
     * Attempts to allocate larger arrays may result in
     * OutOfMemoryError: Requested array size exceeds VM limit
     */
     `
