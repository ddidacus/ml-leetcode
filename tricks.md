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

### Trees
- Diameter of a binary tree: longest path between any two nodes
```python
	self.maxDiameter = 0
	def maxLen(node):
		if not node.left and not node.right: return 1
		if node.left: left = maxLen(node.left)
		else: left = 0
		if node.right: right = maxLen(node.right)
		else: right = 0
		self.maxDiameter = max(self.maxDiameter, left+right)
		return 1 + max(left, right)
	maxLen(root)
	return self.maxDiameter
```
- Depth-First Search:
```python
	def dfs(node):
		if not node: return
		# evaluate node.val
		for child in node.children:
			dfs(node.child)
	dfs(root)
```
- Breadth-First Search:
```python
	def bfs(node):
		stack = [node]
		while len(stack):
			curr = stack.pop(0)
			for child in curr.children:
				stack.append(child)
```
	
### Matrices
- standard matrix multiplication:
```python
	def matmul(mat1, mat2):
	    n1, m1 = len(mat1), len(mat1[0])
	    n2, m2 = len(mat2), len(mat2[0])
	    assert m1 == n2
	    res = [[0 for _ in range(m2)] for _ in range(n1)]
	    for i in range(n1):
	        for k in range(m2):
	            for j in range(m1):
	                res[i][k] += mat1[i][j] * mat2[j][k]
	    return res
```
- efficient matrix multiplication for sparse matrices:
```python
	def sparse_matmul(mat1, mat2):
	    n1, m1 = len(mat1), len(mat1[0])
	    n2, m2 = len(mat2), len(mat2[0])
	    assert m1 == n2
	    rm1 = []
	    rm2 = []
	    def compress(mat, rows):
	        n, m = len(mat), len(mat[0])
	        for i in range(n):
	            row = []
	            for j in range(m):
	                if mat[i][j] != 0:
	                    row.append((mat[i][j], j))
	            rows.append(row)
	        return rows
	    rm1 = compress(mat1, rm1)
	    rm2 = compress(mat2, rm2)
	    res = [[0 for _ in range(m2)] for _ in range(n1)]
	    # for each row in mat1
	    for r1 in range(len(rm1)):
	        # for each nonzero element
	        for el1 in rm1[r1]:
	            r2 = el1[1]
	            # col -> for each element in col-th row
	            # multiply and store in row1, col2
	            for el2 in rm2[r2]:
	                res[r1][el2[1]] += el1[0] * el2[0]
	    return res
```
