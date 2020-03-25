# take

# 1. take

Extracts the first n items of the RDD and returns them as an array.
>(Note: This sounds very easy, but it is actually quite a tricky problem for the implementors of Spark because the items in question can be in many different partitions.)

```scala
def take(num: Int): Array[T]
```

```scala
val b = sc.parallelize(List("dog", "cat", "ape", "salmon", "gnu"), 2)
b.take(2)
res18: Array[String] = Array(dog, cat)

val b = sc.parallelize(1 to 10000, 5000)
b.take(100)
res6: Array[Int] = Array(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 
13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 
29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 
45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 
61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 
77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 
93, 94, 95, 96, 97, 98, 99, 100)
```

# 2. takeOrdered

Orders the data items of the RDD using their **inherent implicit ordering function** and returns the first n items as an array.

```scala
//排序函数
def takeOrdered(num: Int)(implicit ord: Ordering[T]): Array[T]

val b = sc.parallelize(List("dog", "cat", "ape", "salmon", "gnu"), 2)
b.takeOrdered(2)
res19: Array[String] = Array(ape, cat)
```

# 3. takeSample

抽样

Behaves different from sample in the following respects:
* It will return an exact number of samples (Hint: 2nd parameter)
* It returns an Array instead of RDD.
* It internally randomizes the order of the items returned.

```scala
def takeSample(withReplacement: Boolean, num: Int, seed: Int): Array[T]

val x = sc.parallelize(1 to 1000, 3)
x.takeSample(true, 100, 1)
res3: Array[Int] = Array(339, 718, 810, 105, 71, 268, 333, 360,
 341, 300, 68, 848, 431, 449, 773, 172, 802, 339, 431, 285, 937,
  301, 167, 69, 330, 864, 40, 645, 65, 349, 613, 468, 982, 314, 
  160, 675, 232, 794, 577, 571, 805, 317, 136, 860, 522, 45, 
  628, 178, 321, 482, 657, 114, 332, 728, 901, 290, 175, 876, 
  227, 130, 863, 773, 559, 301, 694, 460, 839, 952, 664, 851, 
  260, 729, 823, 880, 792, 964, 614, 821, 683, 364, 80, 875, 
  813, 951, 663, 344, 546, 918, 436, 451, 397, 670, 756, 512, 
  391, 70, 213, 896, 123, 858)
```


