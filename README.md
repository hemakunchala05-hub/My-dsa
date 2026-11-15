# My-dsa

Two Pointers:

1. The core idea
Two pointers means using two indices that move through the data according to a rule, so you avoid examining all pairs or subarrays.
The brute-force version of most of these problems checks every pair (i, j), which is O(n²). Two pointers works because at each step you can prove that a whole group of pairs can never be the answer, so you skip them without checking.
Each pointer move eliminates many candidates at once. That is the entire trick.
Why that’s legal: monotonicity
Moving a pointer must have a predictable effect on the thing you’re measuring. Examples:
In a sorted array, moving left right makes the sum bigger. Moving right left makes it smaller.
In a window of positive numbers, expanding makes the sum bigger. Shrinking makes it smaller.
If there is no such predictable direction (e.g., an unsorted array with negatives, looking for a subarray sum), two pointers fails. That is the most common mistake beginners make.


Variant A: Opposite ends (converging)
Setup: l = 0, r = n-1, move toward each other until l >= r.
Logic: Compute something from a[l] and a[r], then decide which side to move based on a comparison. The decision must be provably safe.

    Template:
    l, r = 0, n-1
    while l < r:
        evaluate(a[l], a[r])
        if need bigger:  l += 1
        elif need smaller: r -= 1
        else: record answer, move one or both


Time: O(n), because every iteration moves a pointer inward and the pointers cross after at most n steps. Space: O(1).



Variant B: Same direction (read/write, slow/fast)
Setup: slow marks where the next valid element goes (write position). fast scans every element (read position).
Logic: Invariant: a[0..slow-1] is the finished, valid output. fast explores. When it finds a keeper, copy it to slow and advance slow.


    Template:
    slow = 0
    for fast in range(n):
        if keep(a[fast]):
            a[slow] = a[fast]
            slow += 1
    return slow

    
Time: O(n). Space: O(1).




Variant C: Sliding window (two pointers defining a range)
Setup: left and right define the window [left, right]. right expands, and left shrinks when the window breaks a rule.
Requirement: the constraint must be monotonic. Adding elements can only push toward “invalid” (or only toward “valid”), and removing does the opposite. This is why windows work for positive numbers and break with negatives.

    Template (variable window):
    left = 0
    for right in range(n):
        add a[right] to window state
        while window is invalid:
            remove a[left] from state
            left += 1
        update answer using window [left, right]
        
Time: O(n), even though there’s a nested while. This is amortized analysis: left only moves forward and never exceeds n. right moves n times total, left moves at most n times total, so the combined work is ≤ 2n. Counting the inner loop as O(n) per outer iteration is the classic wrong answer.
