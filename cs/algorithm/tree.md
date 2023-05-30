### 层级遍历：levelorder 
- 先进先出Queue
- queuesize记录当前层节点数
```sql
public List<List<Integer>> levelOrder(TreeNode root) {
    if(root == null)
        return new ArrayList<>();
    List<List<Integer>> res = new ArrayList<>();
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.offer(root);
    while(!queue.isEmpty()){
        int count = queue.size();
        List<Integer> levelRes = new ArrayList<Integer>();
        while(count > 0){
            TreeNode node = queue.poll();
            levelRes.add(node.val);
            if(node.left != null)
                queue.add(node.left);
            if(node.right != null)
                queue.add(node.right);
            count--;
        }
        res.add(levelRes);
    }
    return res;
}
```

### 先序遍历递归方式：preoder
根结点->左子树->右子树，左子树和右子树递归这个过程

#### preorder-by-recursion
```sql
class Solution {
    private List<Integer> result = new ArrayList();
    public List<Integer> preorderTraversal(TreeNode root) {
        if (root == null) {
            return new ArrayList();
        }
        result.add(root.val);
        preorderTraversal(root.left);
        preorderTraversal(root.right);
        return result;
    }
}
```
#### preorder-by-iteration1
```sql
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList();
        if (root == null) {
            return new ArrayList();
        }
        Stack<TreeNode> stack = new Stack();
        while(!stack.isEmpty() || root != null) {
            while(root != null) {
                result.add(root.val);
                stack.push(root);
                root = root.left;
            }
            root = stack.pop().right;
        }
        return result;
    }
}
```

#### preorer-by-iteration2
- stack 先进后出

```sql
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList();
        if (root == null) {
            return new ArrayList();
        }
        Stack<TreeNode> stack = new Stack();
        stack.push(root);
        while(!stack.isEmpty()) {
            TreeNode curNode = stack.pop();
            result.add(curNode.val);
            if (curNode.right != null) {
                stack.push(curNode.right);
            }
            if (curNode.left != null) {
                stack.push(curNode.left);
            }
        }
        return result;
    }
}
```

### 后续遍历：postOrder
左子树->右子树->根结点，左子树和右子树递归这个过程

#### 递归postorder-by-recursion
```sql
class Solution {
    private List<Integer> result = new ArrayList();
    public List<Integer> postorderTraversal(TreeNode root) {
        if (root == null) {
            return new ArrayList();
        }
        postorderTraversal(root.left);
        postorderTraversal(root.right);
        result.add(root.val);
        return result;
    }
}
``` 

```sql
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if (root == null) {
            return res;
        }

        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        TreeNode prev = null;
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            if (root.right == null || root.right == prev) {
                res.add(root.val);
                prev = root;
                root = null;
            } else {
                stack.push(root);
                root = root.right;
            }
        }
        return res;
    }
}
```

### 中序遍历：inorder
左子树->根结点->右子树，左子树和右子树递归这个过程
#### inorder-by-recursion
```sql
class Solution {
    private List<Integer> res = new ArrayList();
    public List<Integer> inorderTraversal(TreeNode root) {
        if (root == null) {
            return new ArrayList();
        }
        inorderTraversal(root.left);
        res.add(root.val);
        inorderTraversal(root.right);
        return res;
    }
}
```

#### inorder-by-iteration
```sql
class Solution {
   
    public List<Integer> inorderTraversal(TreeNode root) {
        if (root == null) {
            return new ArrayList();
        }
        List<Integer> res = new ArrayList();
        Stack<TreeNode> stack = new Stack();
        while(root != null) {
            stack.push(root);
            root = root.left;
        }

        while(!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            res.add(cur.val);
            root = cur.right;
            while(root != null) {
                stack.push(root);
                root = root.left;
            }
        }
        return res;
    }
}
```