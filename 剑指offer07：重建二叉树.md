## 题目

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

## 思路

前序遍历性质： 节点按照 `[ 根节点 | 左子树 | 右子树 ]` 排序。
中序遍历性质： 节点按照 `[ 左子树 | 根节点 | 右子树 ]` 排序。

所以我们可以有以下步骤

1. 前序遍历的首元素 为 树的根节点 `node` 的值。

2. 在中序遍历中搜索根节点 node 的索引 ，可将 中序遍历 划分为 `[ 左子树 | 根节点 | 右子树 ]` 。

3. 根据中序遍历中的左 / 右子树的节点数量，可将 前序遍历 划分为 `[ 根节点 | 左子树 | 右子树 ]` 。

   

* 递推参数： 根节点在前序遍历的索引 `root` 、子树在中序遍历的左边界 `left` 、子树在中序遍历的右边界 `right`；

* 当 `left > right` ，代表已经越过叶节点，此时返回 `null`

* **递推工作：**

  * **建立根节点 `node` ：** 节点值为 `preorder[root]` ；
  * **划分左右子树：** 查找根节点在中序遍历 `inorder` 中的索引 `i` ；
  * **构建左右子树：** 开启左右子树递归；

  

  ### 代码

  ```java
  /**
   * Definition for a binary tree node.
   * public class TreeNode {
   *     int val;
   *     TreeNode left;
   *     TreeNode right;
   *     TreeNode(int x) { val = x; }
   * }
   */
  class Solution {
      private Map<Integer, Integer> indexMap;
  
      public TreeNode myBuildTree(int[] preorder, int[] inorder, int preorder_left, int preorder_right, int inorder_left, int inorder_right) {
          if (preorder_left > preorder_right) {
              return null;
          }
  
          // 前序遍历中的第一个节点就是根节点
          int preorder_root = preorder_left;
          // 在中序遍历中定位根节点
          int inorder_root = indexMap.get(preorder[preorder_root]);
          
          // 先把根节点建立出来
          TreeNode root = new TreeNode(preorder[preorder_root]);
          // 得到左子树中的节点数目
          int size_left_subtree = inorder_root - inorder_left;
          // 递归地构造左子树，并连接到根节点
          // 先序遍历中「从 左边界+1 开始的 size_left_subtree」个元素就对应了中序遍历中「从 左边界 开始到 根节点定位-1」的元素
          root.left = myBuildTree(preorder, inorder, preorder_left + 1, preorder_left + size_left_subtree, inorder_left, inorder_root - 1);
          // 递归地构造右子树，并连接到根节点
          // 先序遍历中「从 左边界+1+左子树节点数目 开始到 右边界」的元素就对应了中序遍历中「从 根节点定位+1 到 右边界」的元素
          root.right = myBuildTree(preorder, inorder, preorder_left + size_left_subtree + 1, preorder_right, inorder_root + 1, inorder_right);
          return root;
      }
  
      public TreeNode buildTree(int[] preorder, int[] inorder) {
          int n = preorder.length;
          // 构造哈希映射，帮助我们快速定位根节点
          indexMap = new HashMap<Integer, Integer>();
          for (int i = 0; i < n; i++) {
              indexMap.put(inorder[i], i);
          }
          return myBuildTree(preorder, inorder, 0, n - 1, 0, n - 1);
      }
  }
  ```

  

  