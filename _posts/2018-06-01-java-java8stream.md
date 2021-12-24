---
title: Java8的lamada的常用操作方法
tags: Java8
layout: post
---

## 1、List&lt;Map>中的的某个key字段值转换成以逗号分隔的字符串
```java
    public static void convertListMapToStringJoiningComma(){
        List<Map<String,String>> list = new ArrayList<>();
        Map name1 = new HashMap();
        name1.put("name","张三");
        Map name2 = new HashMap();
        name2.put("name","李四");
        list.add(name1);
        list.add(name2);
        list.stream().map(name->name.get("name")).collect(Collectors.joining(","));
        // 输出结果：张三,李四
    }

```

## 2. 将List&lt;Map>中的某个key字段值相同的Map去重
### 方法1：
```java
    public static void distinctListMapByKey(){
        List<Map<String, String>> list = new ArrayList<>() ;
        Map<String,String> map1 = new HashMap<>();
        map1.put("key1", "val1");
        Map<String,String> map2 = new HashMap<>();
        map2.put("key1", "val1");
        Map<String,String> map3 = new HashMap<>();
        map3.put("key1", "val2");
        list.add(map1);
        list.add(map2);
        list.add(map3);
        List<Map<String, String>> data = list.stream().collect(
                Collectors.collectingAndThen(Collectors.toCollection(
                        () -> new TreeSet<>(Comparator.comparing(m -> m.get("key1")))), ArrayList::new));
        System.out.println(data.toString());
        // 输出结果：[{key1=val1}, {key1=val2}]
    }

```
### 方法2：
```java
    public static void distinctListMapByKey(){
        List<Map<String, String>> list = new ArrayList<>() ;
        Map<String,String> map1 = new HashMap<>();
        map1.put("key1", "val1");
        Map<String,String> map2 = new HashMap<>();
        map2.put("key1", "val1");
        Map<String,String> map3 = new HashMap<>();
        map3.put("key1", "val2");
        list.add(map1);
        list.add(map2);
        list.add(map3);
        List<Map<String, String>> data = list.stream().filter(distinctByKey(map->map.get("key1"))).collect(Collectors.toList());
        System.out.println(data.toString());
        // 输出结果：[{key1=val1}, {key1=val2}]
    }
    private static <T> Predicate<T> distinctByKey(Function<? super T, ?> keyExtractor) {
        Set<Object> seen = ConcurrentHashMap.newKeySet();
        return t -> seen.add(keyExtractor.apply(t));
    }
```

## 3. 将List&lt;Map>中的某个key字段值相同的Map对象分组
```java
    public static void groupListMapByKey(){
        List<Map<String, String>> list = new ArrayList<>() ;
        Map<String,String> map1 = new HashMap<>();
        map1.put("key1", "val1");
        map1.put("key2", "val2");
        map1.put("key3", "val3");
        Map<String,String> map2 = new HashMap<>();
        map2.put("key1", "val2");
        map2.put("key2", "val2");
        map2.put("key3", "val3");
        Map<String,String> map3 = new HashMap<>();
        map3.put("key1", "val2");
        map3.put("key2", "val4");
        map3.put("key3", "val4");
        list.add(map1);
        list.add(map2);
        list.add(map3);
        Map<String,List<Map>> mapGroup= list.stream().collect(Collectors.groupingBy(e -> e.get("key1").toString()));
        System.out.println(mapGroup.toString()); 
        // 输出结果：{val2=[{key1=val2, key2=val2, key3=val3}, {key1=val2, key2=val4, key3=val4}], val1=[{key1=val1, key2=val2, key3=val3}]}
    }
```

## 4. 提取List&lt;Map>中的某个key字段值转换成List&lt;String>并去重
```java
    public static void convertListMapToListWithDistinct (){
        List<Map<String,String>> list = new ArrayList<>();
        Map name1 = new HashMap();
        name1.put("name","张三");
        Map name2 = new HashMap();
        name2.put("name","李四");
        Map name3 = new HashMap();
        name3.put("name","张三");
        list.add(name1);
        list.add(name2);
        list.add(name3);
        List<String> listResult = list.stream().map(project->project.get("name")).distinct().collect(Collectors.toList());
        System.out.println(listResult);
        // 输出结果：[张三, 李四]
    }
```

## 5. 对List&lt;Map>中的某个key字段值求和
```java
    public static void listMapKeySum (){
        List<Map<String,Integer>> list = new ArrayList<>();
        Map name1 = new HashMap();
        name1.put("sum",4);
        Map name2 = new HashMap();
        name2.put("sum",5);
        Map name3 = new HashMap();
        name3.put("sum",6);
        list.add(name1);
        list.add(name2);
        list.add(name3);
        int sum =  list.stream().mapToInt(map->map.get("sum")).sum();
        System.out.println(sum);
        // 输出结果：15
    }
```