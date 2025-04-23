## 프로그래머스 입국심사

(https://school.programmers.co.kr/learn/courses/30/lessons/43236)

### 문제분석

1.  출발점, 도착점의 거리, 바위들의 위치, 제거할 바위의 수가 주어질 때, 바위를 제거하여 남은 바위들 사이의 거리의 최솟값 중 가장 큰 값을 구하는 문제.

### 문제조건

1.  1 <= distance <= 1,000,000,000 자연수
2.  1 <= rocks 총갯수 <= 50,000 자연연수
3.  1 <= rocks 원소 < distance 자연수 중복없음
4.  1 <= 제거할 바위 수 <= rocks 총 갯수 자연수

### 아이디어

1.  바위를 제거하는 모든 경우의 수를 탐색하는 것은 시간 초과
2.  최소 거리가 `x` 이상이 되도록 만들 수 있는가?
    - 만약 최소 거리를 `x`로 만들기 위해 제거해야 하는 바위의 수가 `n`개 이하라면, `x`는 가능한 최소 거리이다. 더 큰 `x`를 시도해볼 수 있다.
    - 만약 제거해야 하는 바위의 수가 `n`개보다 많다면, `x`는 불가능한 최소 거리이다. 더 작은 `x`를 시도해야 한다.
    - 이 결정 문제는 `x`에 대해 단조성(monotonicity)을 가지므로, `x`의 범위(`1` ~ `distance`)에 대해 이분 탐색을 적용할 수 있다.
3.  **결정 문제 해결 방법:**
    - 바위 위치 배열 `rocks`를 정렬한다. (시작점 0과 도착점 `distance` 포함하여 거리 계산)
    - 현재 기준 최소 거리 `mid`를 설정한다.
    - 시작점(0)부터 순차적으로 바위들을 확인하며, 이전 바위(또는 시작점)와의 거리가 `mid`보다 작으면 현재 바위를 제거한다 (제거 카운트 증가). 거리가 `mid` 이상이면 현재 바위를 남기고 기준 위치를 업데이트한다.
    - 마지막 남은 바위와 도착점(`distance`) 사이의 거리도 `mid` 이상인지 확인한다. (작으면 제거 카운트 증가)
    - 총 제거한 바위 수가 `n` 이하인지 반환한다.

### 시간복잡도

1.  rocks 정렬: O(N log N) (N 바위갯수)
2.  이분 탐색: O(log D) (D ditance).
3.  각 이분 탐색 바위 제거: O(N) (N 바위갯수)
4.  총: O(N log N + N log D)

### 회고

### Code

```java
import java.util.Arrays;
public class Solution {

    public static int solution(int distance, int[] rocks, int n) {
        Arrays.sort(rocks);

        int answer = 0;
        long left = 1;
        long right = distance;
        long mid;

        while (left <= right) {
            mid = left + (right - left) / 2;

            int removedRocks = 0;
            int lastPosition = 0;

            for (int rock : rocks) {
                if (rock - lastPosition < mid) {
                    removedRocks+=1;
                } else {
                    lastPosition = rock;
                }
            }

            if (distance - lastPosition < mid) {
                removedRocks+=1;
            }

            if (removedRocks <= n) {
                answer = (int) mid;
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return answer;
    }
}

// 테스트 1 〉	통과 (0.45ms, 75.8MB)
// 테스트 2 〉	통과 (0.41ms, 86.8MB)
// 테스트 3 〉	통과 (0.41ms, 83.8MB)
// 테스트 4 〉	통과 (4.74ms, 90.1MB)
// 테스트 5 〉	통과 (4.75ms, 81.7MB)
// 테스트 6 〉	통과 (31.28ms, 90MB)
// 테스트 7 〉	통과 (29.76ms, 79.6MB)
// 테스트 8 〉	통과 (30.19ms, 81.1MB)
// 테스트 9 〉	통과 (0.46ms, 91.8MB)
// 테스트 10 〉	통과 (0.38ms, 76.8MB)
// 테스트 11 〉	통과 (0.67ms, 76.4MB)
// 테스트 12 〉	통과 (0.42ms, 86.6MB)
// 테스트 13 〉	통과 (0.45ms, 85.4MB)
// 테스트 14 〉	통과 (0.37ms, 74.5MB)
// 테스트 15 〉	통과 (0.35ms, 84.8MB)
// 테스트 16 〉	통과 (0.39ms, 88.9MB)
// 테스트 17 〉	통과 (0.41ms, 81.4MB)
// 테스트 18 〉	통과 (0.35ms, 74.3MB)
// 테스트 19 〉	통과 (0.47ms, 86.3MB)
// 테스트 20 〉	통과 (24.21ms, 94.5MB)
// 테스트 21 〉	통과 (6.47ms, 80.9MB)
// 테스트 22 〉	통과 (19.03ms, 90.6MB)
// 테스트 23 〉	통과 (16.63ms, 98.5MB)
// 테스트 24 〉	통과 (26.84ms, 113MB)
// 테스트 25 〉	통과 (4.00ms, 89.9MB)
// 테스트 26 〉	통과 (11.31ms, 76.8MB)
// 테스트 27 〉	통과 (25.59ms, 76.8MB)
// 테스트 28 〉	통과 (3.13ms, 90MB)
// 테스트 29 〉	통과 (6.57ms, 75.7MB)
// 테스트 30 〉	통과 (10.96ms, 93MB)
// 테스트 31 〉	통과 (33.42ms, 88.9MB)
// 테스트 32 〉	통과 (2.95ms, 77.8MB)
// 테스트 33 〉	통과 (14.53ms, 94.7MB)
// 테스트 34 〉	통과 (35.36ms, 79.4MB)
// 테스트 35 〉	통과 (3.90ms, 84.6MB)
// 테스트 36 〉	통과 (13.54ms, 82.7MB)
// 테스트 37 〉	통과 (1.19ms, 80.6MB)
// 테스트 38 〉	통과 (2.64ms, 78.1MB)
// 테스트 39 〉	통과 (23.28ms, 95MB)
```
