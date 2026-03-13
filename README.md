# Simple-deletion-operation-an-AVL-tree-
Data structure practical program 
# AVL Tree Deletion

class Node:
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None
        self.height = 1

def height(node):
    if node is None:
        return 0
    return node.height

def get_balance(node):
    if node is None:
        return 0
    return height(node.left) - height(node.right)

def right_rotate(y):
    x = y.left
    t2 = x.right

    x.right = y
    y.left = t2

    y.height = max(height(y.left), height(y.right)) + 1
    x.height = max(height(x.left), height(x.right)) + 1

    return x

def left_rotate(x):
    y = x.right
    t2 = y.left

    y.left = x
    x.right = t2

    x.height = max(height(x.left), height(x.right)) + 1
    y.height = max(height(y.left), height(y.right)) + 1

    return y

def min_value_node(node):
    current = node
    while current.left:
        current = current.left
    return current

def delete(root, key):
    if not root:
        return root

    if key < root.key:
        root.left = delete(root.left, key)
    elif key > root.key:
        root.right = delete(root.right, key)
    else:
        if root.left is None:
            return root.right
        elif root.right is None:
            return root.left

        temp = min_value_node(root.right)
        root.key = temp.key
        root.right = delete(root.right, temp.key)

    root.height = 1 + max(height(root.left), height(root.right))
    balance = get_balance(root)

    # Balancing
    if balance > 1 and get_balance(root.left) >= 0:
        return right_rotate(root)

    if balance < -1 and get_balance(root.right) <= 0:
        return left_rotate(root)

    if balance > 1 and get_balance(root.left) < 0:
        root.left = left_rotate(root.left)
        return right_rotate(root)

    if balance < -1 and get_balance(root.right) > 0:
        root.right = right_rotate(root.right)
        return left_rotate(root)

    return root

def inorder(root):
    if root:
        inorder(root.left)
        print(root.key, end=" ")
        inorder(root.right)

# Example
root = Node(30)
root.left = Node(20)
root.right = Node(40)
root.left.left = Node(10)
root.left.right = Node(25)

print("Before deletion:")
inorder(root)

root = delete(root, 20)

print("\nAfter deletion:")
inorder(root)

output:
Before deletion:
10 20 25 30 40

After deletion:
10 25 30 40
