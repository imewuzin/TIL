# 이진 탐색 트리 Binary Search Tree

: Data들을 빠르게 검색할 수 있도록 체계적으로 저장을 해 두고, 최대 `O(logn)`의 빠른 속도로 값을 검색할 수 있는 자료 구조

- 빠르게 검색될 수 있도록, 특정 규칙을 갖는 이진트리 형태로 값을 저장

- 모든 원소는 서로 다른 유일한 키를 가짐

- <mark>`key(왼쪽 서브트리)` < `key(루트 노드)` < `key(오른쪽 서브트리)`</mark>

- 파이썬 공식 Library에는 Binary Search Tree 자료구조가 내장되어 있지 않아 직접 구현해 사용해야 함

- List vs BST
  
  - BST는 List보다 더 빠른 삽입, 삭제, 탐색 가능
  
  - 리스트 성능
    
    - 삽입 : `O(n)`, 단 맨 끝 삽입은 `O(1)`
    
    - 삭제 : `O(n)`, 단 맨 끝 삭제는 `O(1)`
    
    - 탐색 : `O(n)`
  
  - BST 성능
    
    - 삽입 : 평균 `O(logN)`
    
    - 삭제 : 평균 `O(logN)`
    
    - 탐색 : 평균 `O(logN)`
    
    - 최악 : 한쪽으로 치우친 경사 이진트리의 경우. `O(n)` (순차 탐색과 시간복잡도 같음)
  
  ```python
  class Node:
      def __init__(self, data):
          self.data = data
          self.left = None
          self.right = None
  
  class BSTree:
      def __init__(self):
          self.root = None
  ```

---

## 동작 원리

- #### 삽입 `Insert`
  
  - 삽입을 위해 `root`부터 바닥 노드까지 탐색을 하며 자기 위치 찾음
    
    1. 처음 등장하는 값은 `root`에 저장됨
    
    2. 비교할 노드 값보다 `target`값이 더 큰 경우 오른쪽 자식 노드로 배정, 그렇기 않으면 왼쪽 자식 노드로 배정
  
  - 트리의 높이 `h`만큼 탐색 시간 걸림
  
  - 완벽하게 균형잡힌 이진트리인 경우, 삽입 시간복잡도는 `O(logN)`
  
  > ```python
  > ```python
  > def __contains__(self, data):
  >     node = self.root
  >     while node:
  >         if node.data == data:
  >             return True
  >         elif node.data > data:
  >             node = node.left
  >         else:
  >             node = node.right
  >     return False
  > ```

- #### 순회
  
  - BST에서 **DFS 중위순회(`LVR`)** 를 하게 되면 Key값이 작은 순서대로 탐색 가능
  
  > ```python
  > def inorder(self):
  >     def _inorder(node):
  >         if not node:
  >             return
  >         _inorder(node.left)
  >         res.append(node.data)
  >         _inorder(node.right)
  >     res = []
  >     _inorder(self.root)
  >     return res
  > ```

- #### 삭제
  
  - 시간 복잡도 : `O(logN)` (탐색 후 삭제해야하지 때문에)
  
  - 리프노드 삭제 : 그냥 삭제
  
  - 자식 1개 : 자식을 부모로 연결 후 삭제
  
  - 자식 2개 : 삭제할 노드의 왼쪽 서브트리의 가장 큰 값 or 오른쪽 서브트리의 가장 작은 값을 넣고 삭제
  
  > ```python
  > #삭제할 노드가 리프 노드이거나 자식이 하나일 때 처리한다.
  >     #왼쪽 자식이 있으면 반환하고, 그렇지 않으면 오른쪽 자식을 반환한다.
  >     #리프 노드이면 왼쪽과 오른쪽이 모두 None이므로, None을 반환한다.
  >     def _del_node1(self, del_node):
  >         if del_node.left:
  >             return del_node.left
  >         return del_node.right
  > 
  >     #자식이 둘인 노드를 삭제한다.
  >     def _del_node2(self, del_node):
  >         #삭제할 노드의 오른쪽에서 가장 왼쪽 노드와 부모 노드를 찾는다.
  >         parent = del_node
  >         leftmost = del_node.right
  >         while leftmost.left:
  >             parent = leftmost
  >             leftmost = leftmost.left
  > 
  >         #삭제 노드의 값을 가장 왼쪽 노드의 값으로 대체한다.
  >         del_node.data = leftmost.data
  > 
  >         #가장 왼쪽 노드를 삭제한다.
  >         #가장 왼쪽 노드는 리프 노드이거나 오른쪽에 자식이 있다.
  >         if del_node == parent:
  >             parent.right = self._del_node1(leftmost)
  >         else:
  >             parent.left = self._del_node1(leftmost)
  >         parent.left = self._del_node1(leftmost)
  > 
  >     #삭제시 실제 사용할 메서드
  >     def delete(self, data):
  >         #루트가 None일 때와 삭제 대상일 때를 처리한다.
  >         if self.root is None:
  >             return
  >         if self.root.data == data:
  >             if self.root.left and self.root.right:
  >                 self._del_node2(self.root)
  >             else:
  >                 self.root = self._del_node1(self.root)
  >             return
  > 
  >         #삭제할 노드와 그 부모 노드를 찾는다.
  >         del_node = self.root
  >         while del_node and del_node.data != data:
  >             parent = del_node
  >             if del_node.data > data:
  >                 del_node = del_node.left
  >             else:
  >                 del_node = del_node.right
  > 
  >         if del_node is None: #삭제할 노드가 없으면 종료
  >             return
  > 
  >         #삭제할 노드의 자식이 둘일 때
  >         if del_node.left and del_node.right:
  >             self._del_node2(del_node)
  >             return
  >         #삭제할 노드가 리프 노드이거나 자식이 하나일 때
  >         if parent.left == del_node:
  >             parent.left = self._del_node1(del_node)
  >         else:
  >             parent.right = self._del_node1(del_node)
  > ```
  > 
  > ```python
  > #node의 가장 왼쪽 노드를 찾아 반환한다.
  >     def get_leftmost(self, node):
  >         while node.left:
  >             node = node.left
  >         return node   
  > 
  >     #재귀적으로 삭제 처리할 함수
  >     def _delete(self, node, data):
  >         if node is None:
  >             return
  > 
  >         #삭제할 노드가 현재 노드보다 작으면 왼쪽에서 찾아서 삭제
  >         #삭제할 노드가 현재 노드보다 크면 오른쪽에서 찾아서 삭제
  >         if node.data > data:
  >             node.left = self._delete(node.left, data)
  >         elif node.data < data:
  >             node.right = self._delete(node.right, data)
  >         else: 
  >             #삭제할 노드를 찾았고, 자식노드가 둘이면
  >             #삭제할 노드의 오른쪽에서 가장 작은(왼쪽) 노드를 찾는다.
  >             #삭제할 노드의 값을 가장 왼쪽 노드의 값으로 대체한다.
  >             #가장 왼쪽 노드를 삭제하고, 결과를 반환한다.
  >             if node.left and node.right:
  >                 leftmost = self.get_leftmost(node.right)
  >                 node.data = leftmost.data
  >                 node.right = self._delete(node.right, leftmost.data)
  >                 return node
  > 
  >             #자식이 하나이거나 리프 노드이면 해당 자식을 반환
  >             if node.left:
  >                 return node.left
  >             else:
  >                 return node.right
  >         return node
  > 
  >     def delete(self, data):
  >         self.root = self._delete(self.root, data)
  > ```
* 총 코드 예시
  
  > ```python
  > '''
  > 7
  > 3 5 1 2 7 4 -5
  > '''
  > 
  > class Node:
  >     def __init__(self, key):
  >         self.key = key
  >         self.left = None
  >         self.right = None
  > 
  > 
  > class BinarySearchTree:
  >     def __init__(self):
  >         self.root = None
  > 
  >     def insert(self, key):
  >         if self.root is None:
  >             self.root = Node(key)
  >         else:
  >             self._insert(self.root, key)
  > 
  >     def _insert(self, node, key):
  >         if key < node.key:
  >             if node.left is None:
  >                 node.left = Node(key)
  >             else:
  >                 self._insert(node.left, key)
  >         else:
  >             if node.right is None:
  >                 node.right = Node(key)
  >             else:
  >                 self._insert(node.right, key)
  > 
  >     def search(self, key):
  >         return self._search(self.root, key)
  > 
  >     def _search(self, node, key):
  >         if node is None or node.key == key:
  >             return node
  >         if key < node.key:
  >             return self._search(node.left, key)
  >         else:
  >             return self._search(node.right, key)
  > 
  >     def inorder_traversal(self):
  >         self._inorder_traversal(self.root)
  > 
  >     def _inorder_traversal(self, node):
  >         if node:
  >             self._inorder_traversal(node.left)
  >             print(node.key, end=' ')
  >             self._inorder_traversal(node.right)
  > 
  > N = int(input())
  > arr = list(map(int, input().split()))
  > 
  > bst = BinarySearchTree()
  > 
  > for num in arr:
  >     bst.insert(num)
  > 
  > print("중위 순회 결과:", end=' ')
  > bst.inorder_traversal()  # 중위 순회: -5 1 2 3 4 5 7
  > 
  > # 탐색 예제
  > key = 5
  > result = bst.search(key)
  > if result:
  >     print(f"\n키 {key} 찾음.")
  > else:
  >     print(f"\n키 {key} 못 찾음.")
  > ```

---

## 힙 Heap

: 완전 이진 트리에 있는 노드 중에서 키값이 가장 큰 노드나 키값이 가장 작은 노드를 찾기 위해 만든 자료 구조

- 힙의 키를 우선순위로 활용해 우선순위 큐 구현 가능

- 힙에서는 루트 노드의 원소만을 삭제할 수 있음

- 루트 노드의 원소를 삭제해 반환

- #### 삽입 과정
  
  1. 일단 맨 뒤에 삽입
  
  2. 자리 바꾸며 내 위치 찾기

- ##### 삭제 과정
  
  1. 루트 노드의 원소 삭제
  
  2. 마지막 노드 삭제하고 루트 노드에 넣기
  
  3. 자리 바꾸며 위치 찾기 (max 자식과 자리 바꾸기)

- #### 최대 힙(max heap)
  
  : 키 값이 가장 큰 노드를 찾기 위한 완전 이진 트리
  
  - `부모노드의 키 값` > `자식 노드의 키 값`
  
  - 루트 노드 : 키 값이 가장 큰 노드
  
  > ```python
  > '''
  > 7
  > 20 15 19 4 13 11 17
  > 
  > 7
  > 20 15 19 4 13 11 23
  > '''
  > 
  > 
  > # 최대힙
  > def enq(n):
  >     global last
  >     last += 1   # 마지막 노드 추가(완전이진트리 유지)
  >     h[last] = n # 마지막 노드에 데이터 삽입
  >     c = last    # 부모>자식 비교를 위해
  >     p = c//2    # 부모번호 계산
  >     while p >= 1 and h[p] < h[c]:   # 부모가 있는데, 더 작으면
  >         h[p], h[c] = h[c], h[p]  # 교환
  >         c = p
  >         p = c//2
  > 
  > 
  > def deq():
  >     global last
  >     tmp = h[1]   # 루트의 키값 보관
  >     h[1] = h[last]
  >     last -= 1
  >     p = 1           # 새로 옮긴 루트
  >     c = p*2
  >     while c <= last:  # 자식이 있으면
  >         if c+1 <= last and h[c] < h[c+1]: # 오른쪽자식이 있고 더 크면
  >             c += 1
  >         if h[p] < h[c]:
  >             h[p], h[c] = h[c], h[p]
  >             p = c
  >             c = p*2
  >         else:
  >             break
  >     return tmp
  > 
  > 
  > N = int(input())          # 필요한 노드 수
  > arr = list(map(int, input().split()))
  > 
  > h = [0]*(N+1)   # 최대힙
  > last = 0        # 힙의 마지막 노드 번호
  > 
  > for num in arr:
  >     enq(num)
  > 
  > print(h)
  > 
  > while last > 0:
  >     print(deq(), end=' ')
  > ```

- #### 최소 힙(min heap)
  
  : 키 값이 가장 작은 노드를  찾기 위한 완전 이진 트리
  
  - `부모노드의 키 값` < `자식 노드의 키 값`
  
  - 루트 노드 : 키 값이 가장 작은 노드
  
  > ```python
  > '''
  > 7
  > 20 15 19 4 13 11 17
  > 
  > 7
  > 20 15 19 4 13 11 23
  > '''
  > 
  > 
  > # 최소힙
  > def enq(n):
  >     global last
  >     last += 1   # 마지막 노드 추가(완전이진트리 유지)
  >     h[last] = n # 마지막 노드에 데이터 삽입
  >     c = last    # 부모>자식 비교를 위해
  >     p = c//2    # 부모번호 계산
  >     while p >= 1 and h[p] > h[c]:   # 부모가 있는데, 더 크면
  >         h[p], h[c] = h[c], h[p]  # 교환
  >         c = p
  >         p = c//2
  > 
  > 
  > def deq():
  >     global last
  >     tmp = h[1]   # 루트의 키값 보관
  >     h[1] = h[last]
  >     last -= 1
  >     p = 1           # 새로 옮긴 루트
  >     c = p*2
  >     while c <= last:  # 자식이 있으면
  >         if c+1 <= last and h[c] > h[c+1]:  # 오른쪽자식이 있고 더 작으면
  >             c += 1
  >         if h[p] > h[c]:
  >             h[p], h[c] = h[c], h[p]
  >             p = c
  >             c = p*2
  >         else:
  >             break
  >     return tmp
  > 
  > 
  > N = int(input())          # 필요한 노드 수
  > arr = list(map(int, input().split()))
  > 
  > h = [0]*(N+1)   # 최대힙
  > last = 0        # 힙의 마지막 노드 번호
  > 
  > for num in arr:
  >     enq(num)
  > 
  > print(h)
  > 
  > while last > 0:
  >     print(deq(), end=' ')
  > ```

- heap 라이브러리 사용
  
  > ```python
  > 
  > ```

---

#### cf. 균형잡힌 이진 탐색 트리로 만들기

```python
def balancing(root):

    #중위 순회한 결과를 반환한다.
    #중위 순회한 결과에는 노드의 값이 아니라 노드 자체가 들어간다.
    def _sort_nodes(node):
        if node is not None:
            _sort_nodes(node.left)
            nodes.append(node)
            _sort_nodes(node.right)

    #균형 잡기 위한 재귀 함수
    #중위 순회한 결과 노드들의 연결 관계를 알맞게 수정한다.
    def _balancing(left, right):
        if left > right:
            return None
        mid = (left + right) // 2
        node = nodes[mid]
        node.left = _balancing(left, mid-1)
        node.right = _balancing(mid+1, right)
        return node

    nodes = []
    _sort_nodes(root)
    root = _balancing(0, len(nodes)-1)   
```
