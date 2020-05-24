# 树

``` javascript
class Node {
  constructor(key) {
    this.key = key;
    this.left = null;
    this.right = null;
  }
}

// 二叉搜索树，所谓的BST
class BinarySearchTree {
  constructor() {
    this.root = null;
  }
  insert(key) {
    const node = new Node(key);
    if (this.root === null) {
      this.root = node;
    } else {
      _insert(this.root, node);
    }
    function _insert(node, newNode) {
      if (newNode.key < node.key) {
        if (node.left === null) {
          node.left = newNode;
        } else {
          _insert(node.left, newNode);
        }
      } else {
        if (node.right === null) {
          node.right = newNode;
        } else {
          _insert(node.right, newNode);
        }
      }
    }
  }
  // 中序遍历。即顺序为 左 -> 根 -> 右
  inOrderTraverse(callback) {
    const _inOrderTraverse = (node, callback) => {
      if (node !== null) {
        _inOrderTraverse(node.left, callback);
        callback(node.key);
        _inOrderTraverse(node.right, callback);
      }
    };
    _inOrderTraverse(this.root, callback);
  }
  // 先序遍历。即顺序为 根 -> 左 -> 右
  preOrderTraverse(callback){
    const _preOrderTraverse = (node, callback) => {
      if(node !== null){
        callback(node.key);
        _preOrderTraverse(node.left, callback);
        _preOrderTraverse(node.right, callback);
      }
    }
    _preOrderTraverse(this.root, callback);
  }
  // 后序遍历。即顺序为 左 -> 右 -> 根
  postOrderTraverse(callback){
    const _postOrderTraverse = (node, callback) => {
      if(node !== null){
        _postOrderTraverse(node.left, callback);
        _postOrderTraverse(node.right, callback);
        callback(node);
      }
    }
    _postOrderTraverse(this.root, callback);
  }
}

const tree = new BinarySearchTree();

// test
tree.insert(11);
tree.insert(7);
tree.insert(5);
tree.insert(3);
tree.insert(6);
tree.insert(9);
tree.insert(8);
tree.insert(10);
tree.insert(15);
tree.insert(13);
tree.insert(12);
tree.insert(14);
tree.insert(20);
tree.insert(18);
tree.insert(25);

tree.inOrderTraverse(value => {
  console.log(value); // 中序遍历打印搜索二叉树的值，应该是从小到大排列
});

// 深度优先遍历
// 广度优先遍历
```

找到某个节点的所有父级元素，类似功能面包屑

树形结构转成数组