## Tricks

### Linked lists
- Check palindrome: split in half, reverse second half, check two halves match element by element
- Reverse a linked list in-place:
```python
	prev = None
	curr = head
	while curr:
		next_node = curr.next
		curr.next = prev
		prev = curr
		curr = next_node
	return prev
```
- Check for cycles in a linked list:
```python
	slow = head
	fast = head.next
	while slow != fast:
		if not fast or not fast.next: return False
		slow = slow.next
		fast = fast.next.next
	return True
```

### Heaps
- Get k-th largest of a stream:
```python
	# nums is a stream, k is another input
	minHeap = nums[:k]
	heapq.heapify(minHeap)
	for n in nums[k:]:
		if n > heap[0]:
			heapq.heappop(heap)
			heapq.heappush(heap, n)
	# return kth largest (not strictly ordered)
	return minHeap
	# return kth largest in descending order
	res = []
	while minHeap: res.append(heapq.heappop(minHeap))
	return reversed(res)
```
- Get k-th smallest of a stream:
```python
	# nums is a stream, k is another input
	maxHeap = nums[:k]
	heapq.heapify_max(maxHeap)
	for n in nums[k:]:
		if n < maxHeap[0]:
			heapq.heappop(maxHeap)
			heapq.heappush(maxHeap, n)
	# return kth largest (not strictly ordered)
	return minHeap
	# return kth largest in descending order
	res = []
	while maxHeap: res.append(heapq.heappop(maxHeap))
	return reversed(res)
	
```
- Treat a minHeap as a maxHeap:
```python
	maxHeap = [-n for n in nums]
	heapq.heapify(maxHeap) # largest will be negated at the top
	return -heapq.heappop(maxHeap)
```
