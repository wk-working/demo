---
title: Map 只会 put、get？快来学这几个“新”方法
date: '2024-12-26 16:34:54'
head: []
outline: deep
sidebar: false
prev: false
next: false
---

# Map 只会 put、get？快来学这几个“新”方法

---

* Map 只会 put、get？快来学这几个“新”方法 - 微信公众平台
* [https://mp.weixin.qq.com/s/VnWoD4gM5cIDx6OjEsaiEQ](https://mp.weixin.qq.com/s/VnWoD4gM5cIDx6OjEsaiEQ)
* 2024-12-26 16:34:54

---

​![cover_image](https://raw.githubusercontent.com/wk-working/demo/main/images/0-20241226163454-ep4zjdo.jpg)​

```

关注后，回复“面试”
即可获取一份大厂面试的大礼包
2024年度金九银十限量版pdf来源：juejin.cn/post/7342420969879093299
上一篇干货：Spring Boot 插件化开发模式，真香！
```

## **引子**

　　Map的数据操作，你是不是还只会put、get？

　　Map是我们日常编程中十分常用的数据接口，的在JDK8中，Map引入了几个新方法，可以简化我们对Map中数据的操作。

　　目前JDK的最新LTS版本已经更新到21了，这几个在JDK8引入的Map”新“方法其实也是”老“方法了，还没熟练使用也太out了，快来看看你都”学废“了吗？

​![图片](https://raw.githubusercontent.com/wk-working/demo/main/images/640-20241226163454-mkkrpld.webp)​

## **getOrDefault**

　　这个方法名很直观，见名知意：尝试获取key对应的值，如果未获取到，就返回默认值。

　　看一个使用的例子，新写法会比老写法更加简洁：

```
private static void testGetOrDefault() {
    Map<String, String> map = new HashMap<>(4);
    map.put("123", "123");
    String key = "key";
    String defaultValue = "defaultValue";

    // 老写法
    String oldValue = defaultValue;
    if (map.containsKey(key)) {
        oldValue = map.get(key);
    }
    System.out.println("oldValue = " + oldValue);

    // 新写法
    String newValue = map.getOrDefault(key, defaultValue);
    System.out.println("newValue = " + newValue);
}
```

## **foreach**

　　看方法名也可以知道，这个方法是遍历map的数据使用的。

　　如果没有foreach，我们遍历map的时候一般是使用增强for循环，有了这个方法后，可以更加方便使用entry中的key和val：

```
private static void testForeach() {
    Map<String, String> map = new HashMap<>(4);
    map.put("123", "123");

    // 老写法
    for (Map.Entry<String, String> entry : map.entrySet()) {
        System.out.printf("老写法 key = %s, value = %s%n", entry.getKey(), entry.getValue());
    }

    // 新写法
    map.forEach((key, value) -> System.out.printf("新写法 key = %s, value = %s%n", key, value));
}
```

## **merge**

　　从名字可以想到，是合并entry使用的，但是具体是怎么合并呢？

　　看一下日常最常用的Map实现类HashMap对merge方法的实现

```
@Override
public V merge(K key, V value,
               BiFunction<? super V, ? super V, ? extends V> remappingFunction) {
    if (value == null || remappingFunction == null)
        throw new NullPointerException();
    int hash = hash(key);
    Node<K,V>[] tab; Node<K,V> first; int n, i;
    int binCount = 0;
    TreeNode<K,V> t = null;
    Node<K,V> old = null;
    if (size > threshold || (tab = table) == null ||
        (n = tab.length) == 0)
        n = (tab = resize()).length;
    if ((first = tab[i = (n - 1) & hash]) != null) {
        if (first instanceof TreeNode)
            old = (t = (TreeNode<K,V>)first).getTreeNode(hash, key);
        else {
            Node<K,V> e = first; K k;
            do {
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k)))) {
                    old = e;
                    break;
                }
                ++binCount;
            } while ((e = e.next) != null);
        }
    }
    if (old != null) {
        V v;
        if (old.value != null) {
            int mc = modCount;
            v = remappingFunction.apply(old.value, value);
            if (mc != modCount) {
                throw new ConcurrentModificationException();
            }
        } else {
            v = value;
        }
        if (v != null) {
            old.value = v;
            afterNodeAccess(old);
        }
        else
            removeNode(hash, key, null, false, true);
        return v;
    } else {
        if (t != null)
            t.putTreeVal(this, tab, hash, key, value);
        else {
            tab[i] = newNode(hash, key, value, first);
            if (binCount >= TREEIFY_THRESHOLD - 1)
                treeifyBin(tab, hash);
        }
        ++modCount;
        ++size;
        afterNodeInsertion(true);
        return value;
    }
}
```

　　代码比较长，但是实现的效果比较容易描述：这个方法接收3个参数：key、value、function。

* 如果key存在，将value按照function做1次计算后，更新到Map中
* 如果key不存在，将key-value放入Map中

　　这个方法在某些场景中挺好用的，代码简洁易懂，例如：我们有1个List，要统计List中每个元素出现的次数。我们要实现的逻辑是，遍历List中的每个元素，如果这个元素在Map中存在，Map中的值+1；如果不存在，则放入Map中，次数（值）为1。

```
private static void testMerge() {
    Map<String, Integer> cntMap = new HashMap<>(8);
    List<String> list = Arrays.asList("apple", "orange", "banana", "orange");

    // 老写法
    for (String item : list) {
        if (cntMap.containsKey(item)) {
            cntMap.put(item, cntMap.get(item) + 1);
        } else {
            cntMap.put(item, 1);
        }
    }

    // 新写法
    for (String item : list) {
        cntMap.merge(item, 1, Integer::sum);
    }
}
```

　　可以看到我们使用merge方法的话，只用1行就简洁实现了这个逻辑。

## **putIfAbsent**

　　也是一个见名知意的方法：不存在key或者值为null时，才将键值对放入Map。跟put方法相比，这个方法不会直接覆盖已有的值，在不允许覆盖旧值的场景使用起来会比较简洁。

```
private static void testPutIfAbsent() {
    Map<String, Integer> scoreMap = new HashMap<>(4);
    scoreMap.put("Jim", 88);
    scoreMap.put("Lily", 90);

    // 老写法
    if (!scoreMap.containsKey("Lily")) {
        scoreMap.put("Lily", 98);
    }

    // 新写法
    scoreMap.putIfAbsent("Lily", 98);
}
```

## **computer**

　　computer方法需要传入2个参数：key、function。主要有3步操作

* 获取到key对应的oldValue，可能为null
* 经过function计算获取newValue
* put(key, newValue)

　　还是以刚刚统计单次次数需求为例，看一下computer的写法：

```
private static void testComputer() {
    Map<String, Integer> cntMap = new HashMap<>(8);
    List<String> list = Arrays.asList("apple", "orange", "banana", "orange");

    // 老写法
    for (String item : list) {
        if (cntMap.containsKey(item)) {
            cntMap.put(item, cntMap.get(item) + 1);
        } else {
            cntMap.put(item, 1);
        }
    }

    // 新写法
    for (String item : list) {
        cntMap.compute(item, (k, v) -> {
            if (v == null) {
                v = 1;
            } else {
                v += 1;
            }
            return v;
        });
    }
}
```

## **computeIfAbsent**

　　看名字就知道是compute方法衍生出来的方法，这个方法只在key不存在的时候，执行computer计算，如果说key对应的value存在，就直接返回这个value。

　　例如，我们需要计算斐波那锲数列的时候，可以使用这个方法来简化代码：

```
private static void testComputerIfAbsent() {
    Map<Integer, Integer> fabMap = new ConcurrentHashMap<>(16);
    fabMap.put(0, 1);
    fabMap.put(1, 1);
    System.out.println(fab(5, fabMap));
}

private static Integer fab(Integer index, Map<Integer, Integer> fabMap) {
    return fabMap.computeIfAbsent(index, i -> fab(i - 2, fabMap) + fab(i - 1, fabMap));
}
```

## **computeIfPresent**

　　这个是computeIfAbsent的姊妹方法，区别在于，这个方法是只有key存在的时候，才去执行computer计算和值的更新。

## **replace**

　　这个方法的效果是：

* 如果key存在，则更新值
* 如果key不存在，什么也不做

## **总结**

　　可以看到，这些JDK8引入的Map的方法，都可以在某些特定场景下简化我们的代码，虽然不嫌麻烦的话，put、get等方法都可以搞定，让我想起一张远古的图

​![图片](https://raw.githubusercontent.com/wk-working/demo/main/images/640-20241226163454-gg3qc82.webp)​

　　不过在不同的场景使用不同的方法，尽量把代码写的简洁和优雅，才是一个程序猿不断追求的目标吧。

---

　　另外，年关将近，开春又是一波面试跳槽的好时机，想要Java年度面试宝典的，扫码关注，进来公众号对话框免费领

```
扫码关注二维码回复“面试”，即可领取。点分享点收藏点在看点点赞
```

​![](https://raw.githubusercontent.com/wk-working/demo/main/images/qrcode-20241226163454-wc24xv4.bmp)​

　　微信扫一扫  
关注该公众号
