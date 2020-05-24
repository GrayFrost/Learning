# 链表

``` javascript
class Node {
  constructor(val) {
    this.val = val;
    this.next = null;
  }
}

class LinkedList {
  constructor() {
    this.head = null;
    this.length = 0;
  }
  append(val) {
    const node = new Node(val);
    if (this.head === null) {
      this.head = node;
    } else {
      let current = this.head;
      while (current.next) {
        current = current.next;
      }
      current.next = node;
    }
    this.length++;
  }
  insert(position, val) {
    if (position > -1 && position < this.length - 1) {
      let node = new Node(val);
      let index = 0;
      let prev, current;
      if (position === 0) {
        current = this.head;
        this.head = node;
        this.head.next = current;
      } else {
        current = this.head;
        while (index < position) {
          prev = current;
          current = current.next;
          index++;
        }
        prev.next = node;
        node.next = current;
      }
      this.length++;
    }
  }
  findIndex(val) {
    let index = 0;
    let current = this.head;
    while (index < this.length) {
      if (current.val === val) {
        return index;
      }
      current = current.next;
      index++;
    }
    return -1;
  }
  remove(val) {
    const position = this.findIndex(val);
    this.removeAt(position);
  }
  removeAt(position) {
    if (position > -1 && position < this.length - 1) {
      let index = 0;
      let prev, current;
      if (index === 0) {
        this.head = this.head.next;
      } else {
        current = this.head;
        while (index < position) {
          prev = current;
          current = current.next;
          index++;
        }
        prev.next = current.next;
      }
      this.length--;
    }
  }
  get size() {
    return this.length;
  }
  isEmpty() {
    return this.length === 0;
  }
  print() {
    let current = this.head;
    let str = current.val;
    while (current.next) {
      current = current.next;
      str = str + " " + current.val;
    }
    console.log(str);
  }
}
```