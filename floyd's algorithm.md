This algorithm detects a **loop** in a **path**.

It works on:
- Linked node lists (e.g., singly linked lists), and
- Recursive calls (e.g., function call stacks) that may form a cycle

1. The main idea is to execute the traversal with two pointers moving at different speeds.
2. Two pointers are used:
   - A fast pointer that moves two steps at a time
   - A slow pointer that moves one step at a time
3. If there is a loop, the fast pointer will eventually catch up to the slow pointer.
4. If there is no loop, the fast pointer will reach the end (`None`).
5. This approach only applies when there is a single path (i.e., not suitable for general graphs or multiple independent subroutines).

```python
def hasCycle(head):
    fast = head
    slow = head
    while fast and fast.next:  # Safely check fast and fast.next
        fast = fast.next.next  # Move fast two steps
        slow = slow.next       # Move slow one step
        if fast == slow:
            return True        # Cycle found
    return False               # No cycle
```
