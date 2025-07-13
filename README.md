# -LCA-of-three-Nodes
This is a python(3.10) code. Code360 by coding ninjas
: this code is down here ✌✔

from collections import deque
import sys
sys.setrecursionlimit(10**6)  # Prevent RecursionError on large trees

class TreeNode:
    def __init__(self, val: int):
        self.data = val
        self.left = None
        self.right = None

def build_tree(level_order: list[int]) -> TreeNode:
    if not level_order or level_order[0] == -1:
        return None

    root = TreeNode(level_order[0])
    queue = deque([root])
    index = 1
    n = len(level_order)

    while queue and index < n:
        current = queue.popleft()

        # Left child
        if index < n and level_order[index] != -1:
            current.left = TreeNode(level_order[index])
            queue.append(current.left)
        index += 1

        # Right child
        if index < n and level_order[index] != -1:
            current.right = TreeNode(level_order[index])
            queue.append(current.right)
        index += 1

    return root

def find_lca(root: TreeNode, n1: int, n2: int) -> TreeNode:
    if not root:
        return None
    if root.data == n1 or root.data == n2:
        return root

    left = find_lca(root.left, n1, n2)
    right = find_lca(root.right, n1, n2)

    if left and right:
        return root
    return left if left else right

def lca_of_three_nodes(root: TreeNode, n1: int, n2: int, n3: int) -> int:
    lca1 = find_lca(root, n1, n2)
    lca2 = find_lca(root, lca1.data, n3)
    return lca2.data

# ------------ INPUT PROCESSING -------------
T = int(input())
for _ in range(T):
    n1, n2, n3 = map(int, input().split())
    level_order = list(map(int, input().split()))
    root = build_tree(level_order)
    print(lca_of_three_nodes(root, n1, n2, n3))
