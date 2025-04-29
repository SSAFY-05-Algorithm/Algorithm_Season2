## 프로그래머스 합승택시요금

(https://school.programmers.co.kr/learn/courses/30/lessons/72413)

### 문제분석

1. 시작 지점에서 두 지점에 도착하는 비용을 최소화 할 수 있는 비용 찾기

### 문제조건

1.  3 <= 지점 수 n <= 200
2.  1 <= 시작(s), 도착 지점(a,b) <= n (각자 다른 값)
3.  2 <= 주어지는 시작, 도착점과 비용의 배열 fares.length <= n \* (n-1) / 2
4.  fares의 각 원소는 [시작, 도착, 비용]
5.  fares 시작, 도착은 n이하 자연수, 각기 다른 값
6.  비용은 100,000 이하 자연수
7.  fares 시작, 도착은 양방향
8.  s 에서 a,b를 갈 수 있는 경로가 존재하는 경우만 입력

### 아이디어

1. 인접리스트로 그래프
2. 플루이드 워셜로 모든 경로 구하기
3. 모든 경로를 경유지로 사용하는 경우에 대해 검사해 최소 비용 찾기

### 시간복잡도

1. graph생성 O(N^2);
2. 간선 값 설정 O(N^2/2);
3. 플루이드 워셜 모든 최단거리 구하기 O(N^3);
4. 모든 노드를 경유지로 사용하는 경우에 최소값 구하기 O(N);
5. 최종 O(N^2 + N^2/2 + N^3 + N) => O(N^3)

### 회고

1. 플루이드 워셜 코드가 기억이 안나 플루이드 워셜 수도 코드를 찾아봐야 했습니다.
2. 오버플로우를 신경써야합니다.

### Code

```java
class Solution {
    public int solution(int n, int s, int a, int b, int[][] fares) {
        int answer = 0;
        s -= 1;
        a -= 1;
        b -= 1;
        int[][] graph = new int[n][n];

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i == j) {
                    graph[i][j] = 0;
                } else {
                    graph[i][j] = Integer.MAX_VALUE;
                }
            }
        }

        for (int i = 0; i < fares.length; i++) {
            int start = fares[i][0] - 1;
            int end = fares[i][1] - 1;
            int dist = fares[i][2];
            graph[start][end] = dist;
            graph[end][start] = dist;
        }

        for (int k = 0; k < n; k++) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (graph[i][k] != Integer.MAX_VALUE && graph[k][j] != Integer.MAX_VALUE) {
                        if (graph[i][j] > graph[i][k] + graph[k][j]) {
                            graph[i][j] = graph[i][k] + graph[k][j];
                        }
                    }
                }
            }
        }

        answer = graph[s][a] + graph[s][b];

        for (int k = 0; k < n; k++) {
            if (graph[s][k] != Integer.MAX_VALUE &&
                    graph[k][a] != Integer.MAX_VALUE &&
                    graph[k][b] != Integer.MAX_VALUE) {
                int temp = graph[s][k] + graph[k][a] + graph[k][b];
                if (temp < answer) {
                    answer = temp;
                }
            }
        }
        return answer;
    }
}
/*
 * 테스트 1 〉 통과 (0.03ms, 87MB)
 * 테스트 2 〉 통과 (0.04ms, 96.9MB)
 * 테스트 3 〉 통과 (0.03ms, 80.5MB)
 * 테스트 4 〉 통과 (0.08ms, 71.7MB)
 * 테스트 5 〉 통과 (0.15ms, 76.1MB)
 * 테스트 6 〉 통과 (0.26ms, 76.5MB)
 * 테스트 7 〉 통과 (0.19ms, 81.3MB)
 * 테스트 8 〉 통과 (0.34ms, 70.6MB)
 * 테스트 9 〉 통과 (0.31ms, 82.4MB)
 * 테스트 10 〉 통과 (0.35ms, 84.2MB)
 *
 * 테스트 1 〉 통과 (15.02ms, 55.5MB)
 * 테스트 2 〉 통과 (87.89ms, 73.9MB)
 * 테스트 3 〉 통과 (28.50ms, 54.3MB)
 * 테스트 4 〉 통과 (30.48ms, 54.5MB)
 * 테스트 5 〉 통과 (30.94ms, 54.1MB)
 * 테스트 6 〉 통과 (28.51ms, 54.4MB)
 * 테스트 7 〉 통과 (60.63ms, 66.5MB)
 * 테스트 8 〉 통과 (51.62ms, 67.4MB)
 * 테스트 9 〉 통과 (63.57ms, 66.6MB)
 * 테스트 10 〉 통과 (48.76ms, 67.5MB)
 * 테스트 11 〉 통과 (62.32ms, 66.9MB)
 * 테스트 12 〉 통과 (92.24ms, 64.7MB)
 * 테스트 13 〉 통과 (100.97ms, 66.5MB)
 * 테스트 14 〉 통과 (102.61ms, 65MB)
 * 테스트 15 〉 통과 (52.71ms, 62.5MB)
 * 테스트 16 〉 통과 (39.95ms, 54.8MB)
 * 테스트 17 〉 통과 (28.05ms, 54.6MB)
 * 테스트 18 〉 통과 (27.62ms, 55.1MB)
 * 테스트 19 〉 통과 (34.55ms, 55MB)
 * 테스트 20 〉 통과 (93.28ms, 73.7MB)
 * 테스트 21 〉 통과 (40.95ms, 59.2MB)
 * 테스트 22 〉 통과 (52.86ms, 62.7MB)
 * 테스트 23 〉 통과 (45.96ms, 61.7MB)
 * 테스트 24 〉 통과 (78.83ms, 65MB)
 * 테스트 25 〉 통과 (26.17ms, 55.3MB)
 * 테스트 26 〉 통과 (25.04ms, 54.7MB)
 * 테스트 27 〉 통과 (45.99ms, 55.1MB)
 * 테스트 28 〉 통과 (70.66ms, 59.1MB)
 * 테스트 29 〉 통과 (16.03ms, 54.2MB)
 * 테스트 30 〉 통과 (26.01ms, 73.9MB)
 */
```
