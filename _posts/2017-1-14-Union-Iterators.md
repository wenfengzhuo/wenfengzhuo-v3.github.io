---
layout: post
comments: true
categories: [algorithms, design]
title: Union Iterators
---

> **Input**:
>
> M iterators
>
> N tuples in each iterator
>
> K keys in each tuple
>
> **Output**:
>
> The ordered union of all tuples from each iterators.
>
> **Assumption**:
>
> 1 Tuples are different to each other in the same iterator
>
> 2 Each iterator has exact the same number of tuples




The question above could be simplified as: *merge M iterators into a single stream and keep those tuples sorted in the final stream*. As tuples in a single iterator are already in order, what we need to take care is to maintain the relative order among the different iterators during the merging process. With this objective, I have two solutions in mind.

### Priority Queue

**Algorithm**

Why does a priority queue help? The thinking process is pretty straightforward. As we want to make the final single stream as sorted, during the merging process if we always pick the smallest elements from those M iterators and put it to our final stream, then we will finally get a sorted single stream. To pick the smallest elements efficiently, priority queue (or heap) might be the most appropriate choice. We could maintain a min-heap, which has the smallest element in the root. The retrieval of smallest element and the addition of a element to the queue will both cost O(log(n)) time, n is the number of elements in the queue.

**Pseudo Code**

The following pseudo code elaborates the algorithm above. To make it simple, the code assumes that the final stream r (a list) will have a *add()* function, the function will append an element in this tail and also it will ignore those elements who are the same with the tail so that the final stream does not contain duplicates. For the priority queue, we will assume that it could compare the first element of a iterator.

```
union(Iterator[] A):
  r = []
  q = priority queue
  for i in A
    if i has next
      q.offer(i)  ====> (1) O(Klog(M)) * M
  while q has next
    i = q.poll()  ====> (2) O(Klog(M) * N * M
    r.add(i.next())
    if i has next
      q.offer(i)  ====> (3) O(Klog(M)) * (N*M - M)
  return r
```

**Complexity**

In the code above, we could see that we first add those iterators who have elements into the priority queue. Then we will retrieve the smallest element every time we poll the root from the priority queue. After we add the smallest element in our final stream, we also need to make sure that we will add the iterator back if it still has elements in it.

In line (1), we can see the program will execute M times if we have M iterators, for each addition into the queue, it will be O(log(M)) comparisons as of the property of priority queue. We have to notice that, for each comparison, it will cost O(K) time to compare a tuple since there are K keys in each tuple. So in total, the line (1) will contribute O(Klog(M)) \* M to the time complexity. In line (3), we know that there are still (N\*M - M) elements that are needed to be offered in the queue, so in total, it will be O(Klog(M))\*(N\*M - M). All in all, the total time complexity of this algorithm will be O(N * K * M * log(M)) in worst case.

To be more precise, as the number of comparisons matters if K is relatively not small, we could dive deeper to see more precise number of comparisons for this algorithm. From the code above, we could know that each tuple in those iterators will be offered in the queue and polled out of the queue for once separately. As a result, there will be 2 \* N \* M operations with the queue. For *poll* operation, by knowing the property of heap, we know that it will end up with 2 \* log(M) comparisons because it requires two comparisons to determine which key is smaller during the repairing of the heap. For *offer* operation, it will require another log(M). In total, there will be 3 \* N \* M \* log(M) comparisons, and hence the total time complexity will be O(3 \* N \* K \* M \* log(M)).

As for the space complexity, we know that during the merging process, there will always be M elements if all iterators still have remaining elements. So in worst case, it will be O(M) elements in queue, and each elements has O(K) keys, so in total, the space complexity will be O(M * K).

In conclusion:
Time Complexity: O(N * K * M * log(M))
Space Complexity: O(M * K)

### Divide-and-Conquer

**Algorithm**

Another way to think this problem is to find whether there is an approach to simplify the problem or convert the problem to an existing solved problem. We could start with a very simple case: if there are only two iterators to be merged. In this case, the situation is just like a the merging phase in merge-sort, so we could use the same logic to solve the problem in this case. If there are more iterators, we could always divide those iterators into halves, and solve it recursively until there are only two iterators. This logic is pretty much the same with merge-sort.

**Pseudo Code**

*mergeIterator* is a function that merges two sorted iterators into a single iterable object. The function assumes that the iterator has a *peek* function that returns the first elements in the iterator.

```
mergeIterator(Iterator l, Iterator r)
  r = []
  while l has next && r has next
    kl = l.peek()
    kr = r.peek()
    if kl <= kr ====> O(K)
      r.add(l.next())
    else
      r.add(r.next())
  while l has next
    r.add(l.next())
  while r has next
    r.add(r.next())
  return r
```
*union* is the major function in this algorithm. It adopts the same idea with merge-sort. It divides the iterator array into halves recursively, and then use *mergeIterator* function to merge the iterators that are returned by the two recursive call.

```
union(Iterator[] A)
  len = len of A
  if len == 0
    return []
  if len == 1
    return [](A[0])
  mid = len / 2
  l = union(A[:mid]) ====> (1) f(M/2)
  r = union(A[mid+1:]) ====> (2) f(M/2)
  return mergeIterator(l, r) ====> (3) O(N * M * K) => O(N * k * M)
```
**Complexity**

As we can see in the code, the time complexity follows the formula of a typical divide-and-conquer solution:
> T(n) = 2*T(n/2) + f(n)

In the line (3), we can see it calls *mergeIterator* function. The *mergeIterator* function has time complexity proportional to the size of the two iterators, because there will be the same number of comparisons with the number of elements in these two iterators. As a result, line (3) indicates that f(n) = O(N*M\*K)=O(NK * M). According to the master theorem, the final time complexity of this algorithm will be O(N \* K \* M \* log(M)).

Let's see the number of comparisons in this algorithm. In *mergeIterator* function, the maximum number of comparison is equal to the sum of the number of elements in the two given iterators (for example, if the two iterators are [1, 3, 5], [2, 4, 5], the number of comparison will be 3 + 3 - 1). The number of comparisons follows the divide-and-conquer formula:

> C(n) = 2 \* C(n/2) + f(n)

The f(n) = N * M, when n = M, then the exact maximum number of comparison will be O(N \* M * log(M)), hence the total time complexity will be O(N \* K \* M \* log(M)).

For space complexity, as you can see from the code, there will be no extra space needed if we do not count the stack space for recursive call. With the space for recursive call considered, the space complexity will be O(log(M)).

In conclusion:
Time Complexity: O(N * K * M * log (M))
Space Complexity: O(log(M))

### Comparison between queue and divide-and-conquer

**Time Complexity**

From the time complexity perspective, both of the two functions will have the same time complexity, which means that theoretically, both of the two algorithms will have nearly same running time for the same input. While taking the constant factor into consideration, the divide-and-conquer has smaller constant factor due to the fewer comparison during the merging process. In general, the divide-and-conquer solution performs better than the queue solution.

**Space Complexity**

As we could tell clear from the descriptions above, the divide-and-conquer solution will requires much less space than the priority queue solution. In practice, if we adopt divide-and-conquer solution, it means we only need few extra memory to achieve the same time complexity with the priority queue solution. In fact, in practice more extra memory might have impact on the total running time. When request more memory, new memory will be allocated, and this process will cost some time. What is more, if the data is too large to fit in memory, then virtual memory will be involved, which has significantly lower performance than the RAM memory. In this perspective, I will prefer divide-and-conquer solution.

**Parallel capability**

By using divide-and-conquer method, we know that the second approach could parallelize very well. To simply put, we could run the two halves of the iterator arrays at the same time, and this applies to every recursive call. By using parallelism, in assumption that we have enough processors, the time complexity will be reduced to O(N*M\*K). Further discussion will be covered in next section.
For priority queue solution, since we need to get the smallest tuples every time we poll from the queue, it is hard for us to parallelize the algorithm.

### Further Optimization

**Parallelism**

As we already mentioned in previous section, we could parallelize divide-and-conquer solution in a very natural way. The following pseudo code explains how could achieve parallelism:

```
union(Iterator[] A)
  len = len of A
  if len == 0
    return []
  if len == 1
    return [](A[0])
  mid = len / 2
  fork l = union(A[:mid]) ====> (1)
  r = union(A[mid+1:])
  join                    ====> (2)
  return mergeIterator(l, r)
```

In line (1), we create a new thread (or process) to run the recursive call for the left half of the array in parallel. In line(2), we will wait the completion of the two processes. So the time complexity could be calculated with the formula:
> T(M) = T(M/2) + O(M * N * K)

As a result, the final time complexity will be O(N * K * M).

**Tuple Comparison**

For a tuple, assign a hash value for it. The hash value is determined by the keys in each tuple. The large tuple will have large hash value. Then for most of keys, just compare their hash value, we will know their order. For the tuples who have the same hash value, we compare them by comparing the keys in each tuple. The following picture shows a way of hashing a tuple.

![hash](http://i.imgur.com/LJs9a5U.png)

Let's assume the range of keys in those tuples is 0~1023, and there are 3 keys in each tuples. We divide the whole range with 2^3 = 8 parts, each part will be labeled as 0, 1, 2, ..., 7. The following code explains how we compute the hash for a tuple: t (k1, k2, k3).

```
hash(tuple)
  h = 0
  i = 0
  while i < 3
	if tuple[i] >= 512
	  h |= 1 << (2-i)
	i ++
  return h
```

Let's assume each tuple now has a function *hashCode* which returns a pre-computed hash. If luckily, all the tuples in those iterators have different hash value, then we could directly reduce the time complexity of our algorithm to O(N\*M\*log(M)). In reality, some tuples might have the same hash value. So we need to do an amortized analysis of the algorithm. If all the tuples distributed normally in the picture above, then we could calculate the how many tuples might end up with having the same hash values. For two tuples, the possibility to have the same hash value will be 1 / (2 ^ k). We know that there will N\*M comparisons in total, so for those comparisons which need full comparisons of each keys, the total number will be (1/(2^k)) * (N\*M). Let's assume there is **c** percentage of comparison which could directly use hash values. So the average time for each comparison will be
> ((1/(2^k)*(N\*M)) * K + cN\*M)/(M\*N) = K/(2^K) + c

As c is lower than 1 (because not all comparisons could directly use hash values), so the average time for each comparison will less than 2. The time complexity now will be from O(N*K\*M\*log(M)) to O(N\*M\*log(M)） because the average time for comparison becomes constant time.

**Combination**
Theoretically, if we combine above two optimizations, we could achieve O(N * M) time

### Benchmark
As to demonstrate the algorithms described above and its performance, I implemented them with Java. The source code could be found here:

> [Union Iterators](https://github.com/wenfengzhuo/union-iterator)

The following pictures show the results from the benchmark tests for those two algorithms.

![M](http://i.imgur.com/OwDe7tz.png)

**Picture 1**: The running time for different number of iterations to be merged. (M changed)

![N](http://i.imgur.com/hiKMXvs.png)

**Picture 2**: The running time for different number of elements in each iterator. (N changed).

For the **picture 1**, we could see that when number of iterators to be merged grows, divide-and-conquer solution will perform better than priority queue solution. This is what we expected from the analysis of the algorithm. Although there are some difference in running time, the two solutions differ only in a small scale, which might indicates the constant factor might not cause huge performance gap between these two solutions.

For the **picture 2**,  an interesting thing to note that is the priority queue solution outperforms than the divide-and-conquer solution when number of elements in each iterator grows. This could be interpreted as the the M is small so it will not gain too much from the benefits of divide-and-conquer solutions, and the function call during the recursive process might cost some overhead, which make it slower than the priority queue solution.

From the two pictures above, we could conclude that, in real-world practice, when M is usually large, we might choose divide-and-conquer solution, and when N is usually larger than M, we could choose priority queue solution.

In the two pictures, we could see that there is a third line called *ParallelMerger*. It is the parallelized version of divide-and-conquer solution. Unfortunately, it does not meet our expectation. One explanation could be that there is thread-switching overhead that makes it slower than non-parallelized algorithm. Or maybe I could implement in a different way for *ParallelMerger*.
