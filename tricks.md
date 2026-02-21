## Tricks

### Two Pointers
- same start (sliding window)
```python
	def minSubArrayLen(target, nums):
		left = 0
		current_sum = 0
		min_length = float('inf') # Start with "infinity"
		
		for right in range(len(nums)):
			# 1. Add the current number to the sum
			current_sum += nums[right]
			
			# 2. While the sum is big enough, try to shrink from the left
			while current_sum >= target:
				# Update min_length (current window size is right - left + 1)
				min_length = min(min_length, right - left + 1)
				
				# Subtract the element at 'left' and move 'left' forward
				current_sum = current_sum - nums[left]
				left += 1
				
		return min_length if min_length != float('inf') else 0
```
- same start (slow writer, fast scouter)
```python
	def moveZeros(nums):
		if len(nums) <= 1: return nums
		slow = -1
		fast = 0
		while fast < len(nums):
			if nums[fast] > 0:
				slow += 1
				nums[slow] = nums[fast]
				if slow != fast: nums[fast] = 0
			fast += 1
		return nums
	# nums = [1, 0, 2, 0, 3]
	# nums = [1, 2, 3, 0, 0]
```
- opposite ends (two sums problem)
```python
	def twoSums(nums, target):
		left = 0
		right = len(nums)-1
		while left < right:
			curr_sum = nums[left] + nums[right]
			if curr_sum == target:
				return (left+1, right+1)
			elif curr_sum < target:
				left += 1
			else:
				right -= 1
		return -1
```

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
