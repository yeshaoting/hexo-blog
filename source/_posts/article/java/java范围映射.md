title: java范围映射
tags:
  - java
categories:
  - 后端开发
date: 2016-08-11 15:13:00
---

<img src="/asserts/images/logo/javaee.png" class="img-logo img-center" />


## 一、背景
近期一个功能开发涉及到一个核心业务规则为：根据消费额实行阶梯计价，额度处于的阶梯档位不同，对应的单价不同，需要根据给定的消费额找到对应的消费单价。


## 二、问题分析
阶梯计价翻译成编码逻辑，先容易想到是一个映射，即：Map。
不过特殊的地方在于，这是一个范围映射，将一个范围的键值映射到一个固定的值。
下来我们来看一下这种范围映射逻辑如何使用java程序来表现。


## 三、范围映射分类
范围映射按其连续性，可以简单分为如下两类情形：

### 1. 连续的范围映射
例如：计价阶梯就是连续的范围映射。
大多数情况下，计价阶梯都是连续的情况。

#### 场景举例
举个大家都熟悉的场景：北京移动套餐外流量资费。这一流量资费在某个时期就是阶梯计价的，消费阶梯是连续的。
具体的阶梯计价映射关系如下：

| 流量消费阶梯 | 流量单价 |
|---|---|
| [0,100MB) | 0.15元/MB |
| [100MB,500MB)  | 0.08元/MB|
| [500MB,1GB) | 0.05元/MB |
| [1GB,2GB) | 0.04元/MB|
| [2GB,15GB) | 0.03元/MB |
| [15GB,∞) | 0.01元/MB |

<!-- more -->


#### 实现代码
``` java
package cn.yeshaoting.demo;

import java.util.List;
import java.util.Map.Entry;
import java.util.NavigableMap;

import org.junit.Before;
import org.junit.Test;

import com.google.common.collect.Lists;
import com.google.common.collect.Maps;

/**
 * @description 阶梯计价
 * @author yeshaoting
 * @date Aug 8, 2016 12:26:39 PM
 */
public class SteppedCalculateTest {

    // 流量下界阶梯
    private NavigableMap<Integer, Double> steppedFloorFlows = Maps.newTreeMap();
    
    private final static int GB = 1000;
    
    @Before
    public void init() {
        steppedFloorFlows.put(0, 0.15);
        steppedFloorFlows.put(100, 0.08);
        steppedFloorFlows.put(500, 0.05);
        steppedFloorFlows.put(GB * 1, 0.04);
        steppedFloorFlows.put(GB * 2, 0.03);
        steppedFloorFlows.put(GB * 15, 0.01);
    }
    
    @Test
    public void continuousStepTest() {
        List<Integer> flows = Lists.newArrayList(70, 100, 200, 500, GB * 20);
        calculate(flows);
    }
    
    private void calculate(List<Integer> flows) {
        for (Integer flow : flows) {
            Entry<Integer, Double> entry = steppedFloorFlows.floorEntry(flow);
            System.out.println("消费额:" + flow + "MB\t -> 档位:" + entry.getKey() + "\t单价:" + entry.getValue());
        }
    }
}
```

#### 执行结果
``` bash
消费额:70	 -> 档位:0	单价:0.15
消费额:100	 -> 档位:100	单价:0.08
消费额:200	 -> 档位:100	单价:0.08
消费额:500	 -> 档位:500	单价:0.05
消费额:20000	 -> 档位:15000	单价:0.03
```


### 2. 不连续的范围映射
某些特殊的业务场景需要实现不连续的范围映射。

#### 范围映射
| 范围 | 值 |
|---|---|
| [0,3) | 0 |
| [5,10)  | 1|
| [100,200) | 2 |

#### 实现代码
``` java
package cn.yeshaoting.demo;

import java.util.List;
import java.util.Map.Entry;
import java.util.NavigableMap;

import org.apache.commons.lang3.tuple.Pair;
import org.junit.Before;
import org.junit.Test;

import com.google.common.collect.Lists;
import com.google.common.collect.Maps;

/**
 * @description 范围映射
 * @author yeshaoting
 * @date Aug 8, 2016 12:26:39 PM
 */
public class RangeCalculateTest {

    // 范围映射
    private NavigableMap<Integer, Pair<Integer, Integer>> ranges = Maps.newTreeMap();
    
    @Before
    public void init() {
        ranges.put(0, Pair.of(3, 0));       // 0..3     => 0
        ranges.put(5, Pair.of(10, 1));      // 5..10    => 1
        ranges.put(100, Pair.of(200, 2));   // 100..200 => 2
    }
    
    @Test
    public void uncontinuousStepTest() {
        List<Integer> values = Lists.newArrayList(2, 3, 7, 10, 100, 150, 300);
        calculate(values);
    }
    
    private void calculate(List<Integer> values) {
        for (Integer value : values) {
            Entry<Integer, Pair<Integer, Integer>> entry = ranges.floorEntry(value);
            Pair<Integer, Integer> pair = entry.getValue();
            if (value < pair.getKey()) {
                System.out.println("值:" + value + "\t -> 范围:[" + entry.getKey() + "," + pair.getKey()  + ")\t值:" + pair.getValue());
            } else {
                System.out.println("值:" + value + "\t -> 未包含的范围");
            }
        }
    }
}
```

#### 执行结果
``` bash
值:2	 -> 范围:[0,3)	值:0
值:3	 -> 未包含的范围
值:7	 -> 范围:[5,10)	值:1
值:10	 -> 未包含的范围
值:100	 -> 范围:[100,200)	值:2
值:150	 -> 范围:[100,200)	值:2
值:300	 -> 未包含的范围
```


## 四、小结
范围映射实现的核心类：java.util.NavigableMap

### 1. 常用方法
| 方法 | 说明 |
|---|---|
| floorEntry | 返回小于等于指定key中的所有key中最大的一个key |
| ceilingEntry | 返回大于等于指定key中的所有key中最小的一个key |
| lowerEntry | 返回小于指定key中的所有key总最大的一个key |
| higherEntry | 返回大于指定key中的所有key中最小的一个key |

### 2. 详解说明
``` java
public interface NavigableMap<K, V> extends SortedMap<K, V> {
    // 获取小于指定key的第一个节点对象
    Map.Entry<K, V> lowerEntry(K key);

    // 获取小于指定key的第一个key
    K lowerKey(K key);

    // 获取小于或等于指定key的第一个节点对象
    Map.Entry<K, V> floorEntry(K key);

    // 获取小于或等于指定key的第一个key
    K floorKey(K key);

    // 获取大于或等于指定key的第一个节点对象
    Map.Entry<K, V> ceilingEntry(K key);

    // 获取大于或等于指定key的第一个key
    K ceilingKey(K key);

    // 获取大于指定key的第一个节点对象
    Map.Entry<K, V> higherEntry(K key);

    // 获取大于指定key的第一个key
    K higherKey(K key);

    // 获取Map的第一个（最小的）节点对象
    Map.Entry<K, V> firstEntry();

    // 获取Map的最后一个（最大的）节点对象
    Map.Entry<K, V> lastEntry();

    // 获取Map的第一个节点对象，并从Map中移除改节点
    Map.Entry<K, V> pollFirstEntry();

    // 获取Map的最后一个节点对象，并从Map中移除改节点
    Map.Entry<K, V> pollLastEntry();

    // 返回当前Map的逆序Map集合
    NavigableMap<K, V> descendingMap();

    // 返回当前Map中包含的所有key的Set集合
    NavigableSet<K> navigableKeySet();

    // 返回当前map的逆序Set集合，Set由key组成
    NavigableSet<K> descendingKeySet();

    // 返回当前map中介于fromKey（fromInclusive是否包含）和toKey（toInclusive是否包含） 之间的子map
    NavigableMap<K, V> subMap(K fromKey, boolean fromInclusive, K toKey, boolean toInclusive);

    // 返回介于map第一个元素到toKey（inInclusive是否包含）之间的子map
    NavigableMap<K, V> headMap(K toKey, boolean inclusive);

    // 返回当前map中介于fromKey（inInclusive是否包含） 到map最后一个元素之间的子map
    NavigableMap<K, V> tailMap(K fromKey, boolean inclusive);

    // 返回当前map中介于fromKey（包含）和toKey（不包含）之间的子map
    SortedMap<K, V> subMap(K fromKey, K toKey);

    // 返回介于map第一个元素到toKey（不包含）之间的子map
    SortedMap<K, V> headMap(K toKey);

    // 返回当前map中介于fromKey（包含） 到map最后一个元素之间的子map
    SortedMap<K, V> tailMap(K fromKey);
}
```


## 五、参考文档
1. [Using java map for range searches](http://stackoverflow.com/questions/1314650/using-java-map-for-range-searches/1314708#1314708)
2. [给jdk写注释系列之jdk1.6容器(8)：TreeSet、NavigableMap、NavigableSet源码解析](http://www.importnew.com/17620.html)
3. [Java集合<10> (NavigableMap)](http://my.oschina.net/kevinair/blog/191242)

