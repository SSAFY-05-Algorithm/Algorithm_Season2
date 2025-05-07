## 프로그래머스 길 찾기 게임

(https://school.programmers.co.kr/learn/courses/30/lessons/42892)

### 문제분석

1. 조건에 맞게 트리를 만들고 전위순회, 후위순회 결과를 반환하는 문제

### 문제조건

1. 입력으로 제공되는 nodeinfo는 이진트리를 구성하는 각 노드의 좌표가 1번부터 순차적으로 들어 있는 2차원 배열
2. 1 <= nodeinfo 길이 <= 10,000
3. nodeinfo[i]는 i+1번 노드의 좌표, [x좌표, y좌표] 순서로 들어있다.
4. 0 <= 모든 노드의 좌표값 <= 100,000 정수
5. 트리의 깊이는 1,000 이하인 경우만
6. 모든 노드의 좌표는 규칙을 따르고 잘못된 노드 위치는 주어지지 않는다.

### 아이디어

1. 조건에 맞춰 입력을 정렬하여 root node 찾기 (Y값 내림차순으로 정렬 시 0번이 root)
2. Linked List 형태로 Tree 를 생성
3. 조건에 맞게 전위순회, 후위순회 List 생성 및 반환 (DFS)

### 시간복잡도

1. 첫 노드 리스트 생성 O(N)
2. 노드 리스트 정렬 O(N log N)
3. 이진 트리 생성 O(N^2) (편향 트리 최악 경우 시)
4. dfs O(N)
5. 결론 1+2+3+4 -> O(N^2)

### 회고

1. 링크드 리스트를 손으로 만들어 본게 얼마만인지
2. Did you Know? Programmers에서 `int[][]` 를 반환하지 않고 `List<List<Integer>>`를 반환해도 된다는 것을?
3. 처음에는 전위순회 한번 후위순회 한번 2번 돌았으나, 한번에 둘다 해결
4. DFS도 이제는 재귀 말고 Stack을 사용하는걸로 개선하는게 더 나을까?

### Code

```java
/*
 * MST인가?
 * 아니네 레벨이 주어진다 이를 바탕으로 트리를 만들고 순회할 것인가?
 * 후위 순회는 DFS 빠져 나오면서 추가하면 될 것 같고
 * 전위순회는 DFS 들어오면서
 * 결국 트리가 있다면 DFS 한번이면 둘다 가능
 * 트리를 어떻게 만들것인가? 이진트리라고 무조건 배열 규칙으로 놓는거는 별로일지도?
 * 링크드 리스트로 좌 우를 넣는가?
 * 레벨에 따라서 키 밸류로 저장해 놨다가 트리 구성?
 */

import java.util.*;

class Solution {
    class Node {
        int idx, x, y;
        Node left, right;
        Node(int idx, int x, int y){
            this.idx = idx;
            this.x = x;
            this.y = y;
        }
    }
    public List<List<Integer>> solution(int[][] nodeinfo) {
        List<Node> nodes = new ArrayList<>();
        for(int i = 0; i < nodeinfo.length; i++){
            nodes.add(new Node(i+1,nodeinfo[i][0],nodeinfo[i][1]));
        }

        nodes.sort((a, b) -> {
            if (a.y != b.y) {
                return Integer.compare(b.y, a.y);
            } else {
                return Integer.compare(a.x, b.x);
            }
        });
        Node root = mkTree(nodes);
        List<Integer> preorderList = new ArrayList<>();
        List<Integer> postorderList = new ArrayList<>();

        // preorderSearch(root, preorderList);
        // postorderSearch(root, postorderList);

        dfs(root, preorderList, postorderList);

        List<List<Integer>> answer = new ArrayList<>();
        answer.add(preorderList);
        answer.add(postorderList);
        return answer;

//         int[][] answer = new int[2][nodeinfo.length];
//         for(int i = 0; i < nodeinfo.length; i++){
//             answer[0][i] = preorderList.get(i);
//             answer[1][i] = postorderList.get(i);
//             // 어떻게 toList 하냐...
//         }

//         return answer;
    }
    public static void dfs(Node node, List<Integer> preorderList, List<Integer> postorderList){
        if(node == null) {
            return;
        }
        preorderList.add(node.idx);
        dfs(node.left, preorderList, postorderList);
        dfs(node.right, preorderList, postorderList);
        postorderList.add(node.idx);
    }
//     public static void preorderSearch(Node node, List<Integer> list){
//         if (node == null) {
//             return;
//         }
//         list.add(node.idx);
//         preorderSearch(node.left, list);
//         preorderSearch(node.right, list);
//     }

//     public static void postorderSearch(Node node, List<Integer> list){
//         if (node == null) {
//             return;
//         }
//         postorderSearch(node.left, list);
//         postorderSearch(node.right, list);
//         list.add(node.idx);
//     }

    public static Node mkTree(List<Node> nodes) {
        Node root = nodes.get(0);
        for(int i = 1; i < nodes.size() ; i++){
            insertNode(root, nodes.get(i));
        }
        return root;
    }

    public static void insertNode(Node parent, Node child){
        if (parent.x > child.x){
            if (parent.left == null){
                parent.left = child;
            } else {
                insertNode(parent.left, child);
            }
        } else {
            if (parent.right == null){
                parent.right = child;
            } else {
                insertNode(parent.right, child);
            }
        }
    }
}

// 테스트 1 〉 통과 (1.28ms, 71.8MB)
// 테스트 2 〉 통과 (1.05ms, 87.3MB)
// 테스트 3 〉 통과 (1.04ms, 79.3MB)
// 테스트 4 〉 통과 (1.00ms, 74.6MB)
// 테스트 5 〉 통과 (1.04ms, 86.9MB)
// 테스트 6 〉 통과 (7.93ms, 77.2MB)
// 테스트 7 〉 통과 (9.25ms, 89.6MB)
// 테스트 8 〉 통과 (8.91ms, 99MB)
// 테스트 9 〉 통과 (25.71ms, 95.3MB)
// 테스트 10 〉 통과 (7.36ms, 87.2MB)
// 테스트 11 〉 통과 (25.48ms, 88.9MB)
// 테스트 12 〉 통과 (23.06ms, 91.8MB)
// 테스트 13 〉 통과 (2.84ms, 71.7MB)
// 테스트 14 〉 통과 (3.09ms, 86.7MB)
// 테스트 15 〉 통과 (14.30ms, 102MB)
// 테스트 16 〉 통과 (21.23ms, 96.9MB)
// 테스트 17 〉 통과 (4.60ms, 80.8MB)
// 테스트 18 〉 통과 (15.23ms, 103MB)
// 테스트 19 〉 통과 (4.08ms, 93.9MB)
// 테스트 20 〉 통과 (7.38ms, 96.5MB)
// 테스트 21 〉 통과 (10.02ms, 101MB)
// 테스트 22 〉 통과 (14.44ms, 91.1MB)
// 테스트 23 〉 통과 (20.16ms, 108MB)
// 테스트 24 〉 통과 (0.83ms, 81.8MB)
// 테스트 25 〉 통과 (0.73ms, 69.2MB)
// 테스트 26 〉 통과 (10.98ms, 91.3MB)
// 테스트 27 〉 통과 (0.98ms, 80.9MB)
// 테스트 28 〉 통과 (0.90ms, 89.2MB)
// 테스트 29 〉 통과 (0.69ms, 81.7MB)
```
