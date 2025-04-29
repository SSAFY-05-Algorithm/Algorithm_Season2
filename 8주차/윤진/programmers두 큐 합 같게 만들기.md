## 프로그래머스 두 큐 합 같게 만들기

(https://school.programmers.co.kr/learn/courses/30/lessons/118667)

### 문제분석

1. 주어진 두 큐의 합이 동일하게 맞추려면 몇번 poll하고 다른 queue 에 add 해야하는지 action의 횟수를 구하는 문제

### 문제조건

1.  처음 주어진 두 큐의 길이는 동일
2.  1 <= 각 큐의 길이 <= 300,000
3.  1 <= 각 큐의 원소 <= 10^9 => SUM을 구할때 long 필요

### 아이디어

1. 큰 큐에서 빼서 다른 큐에 넣는다.
2. 모든 수의 합이 홀수이거나, 한 큐가 비게되는 경우, 그리고 무한 루프에 빠졌을 것이라 생각되는 경우 -1 반환
3. action의 횟수를 세 반환

### 시간복잡도

1. 처음 Sum을 구하고 queue 생성 O(N);
2. 그후 최대 4N만큼 while 문 실행 O(4N);
3. 결국 O(N);

### 회고

1. 배열을 queue로 만들때 그냥 반복문 쓰기로 했습니다.
2. 무한 루프가 발생해 최대 횟수를 정해주는 조건을 다시 원래 자리로 돌아올 최악의 경우로 산정했습니다.

### Code

```java
import java.util.*;

class Solution {
    public int solution(int[] queue1, int[] queue2) {
        int length = queue1.length;
        Queue<Integer> q1 = new ArrayDeque<>(length);
        Queue<Integer> q2 = new ArrayDeque<>(length);

        long sum1 = 0L;
        long sum2 = 0L;

        for (int i = 0; i < length; i++) {
            q1.add(queue1[i]);
            sum1 += queue1[i];
            q2.add(queue2[i]);
            sum2 += queue2[i];
        }

        long totalSum = sum1 + sum2;
        System.out.println(totalSum);

        // 합이 홀수? 조건 X
        if (totalSum % 2 != 0) {
            return -1;
        }

        long target = totalSum / 2;
        int actionCount = 0;

        // 무한 루프 예방으로 대략 초기 길이의 4배
        // 첫 움직임 이후로 양쪽이 균등하게 움직이면 2N번 action
        // 다시 첫 queue로 추가된 뒤 제일 앞까지 움직이려면 다시 2N번 action
        int limit = 4 * length;

        while (sum1 != target) {
            if (actionCount > limit) {
                return -1;
            }
            if (sum1 > target) {
                if (q1.isEmpty()) {
                    return -1;
                }
                int temp = q1.poll();
                q2.add(temp);
                sum1 = sum1 - temp;
                sum2 = sum2 + temp;
            } else if (sum2 > target) {
                if (q2.isEmpty()) {
                    return -1;
                }
                int temp = q2.poll();
                q1.add(temp);
                sum1 = sum1 + temp;
                sum2 = sum2 - temp;
            }
            actionCount += 1;
        }
        return actionCount;
    }
}

/*
 * 테스트 1 〉 통과 (0.28ms, 87.5MB)
 * 테스트 2 〉 통과 (0.27ms, 83.7MB)
 * 테스트 3 〉 통과 (0.31ms, 72.4MB)
 * 테스트 4 〉 통과 (0.35ms, 73.2MB)
 * 테스트 5 〉 통과 (0.38ms, 74.4MB)
 * 테스트 6 〉 통과 (0.36ms, 80.3MB)
 * 테스트 7 〉 통과 (0.42ms, 81.3MB)
 * 테스트 8 〉 통과 (0.62ms, 79.2MB)
 * 테스트 9 〉 통과 (0.63ms, 78.4MB)
 * 테스트 10 〉 통과 (0.96ms, 70.2MB)
 * 테스트 11 〉 통과 (22.70ms, 102MB)
 * 테스트 12 〉 통과 (15.30ms, 101MB)
 * 테스트 13 〉 통과 (11.26ms, 98.9MB)
 * 테스트 14 〉 통과 (14.77ms, 82.3MB)
 * 테스트 15 〉 통과 (14.19ms, 110MB)
 * 테스트 16 〉 통과 (13.43ms, 113MB)
 * 테스트 17 〉 통과 (15.73ms, 114MB)
 * 테스트 18 〉 통과 (26.00ms, 133MB)
 * 테스트 19 〉 통과 (29.31ms, 122MB)
 * 테스트 20 〉 통과 (36.72ms, 146MB)
 * 테스트 21 〉 통과 (37.19ms, 136MB)
 * 테스트 22 〉 통과 (50.19ms, 120MB)
 * 테스트 23 〉 통과 (36.11ms, 128MB)
 * 테스트 24 〉 통과 (46.27ms, 131MB)
 * 테스트 25 〉 통과 (0.39ms, 74.3MB)
 * 테스트 26 〉 통과 (0.33ms, 70.4MB)
 * 테스트 27 〉 통과 (0.47ms, 89.9MB)
 * 테스트 28 〉 통과 (37.96ms, 115MB)
 * 테스트 29 〉 통과 (2.13ms, 90.9MB)
 * 테스트 30 〉 통과 (23.49ms, 121MB)
 */
```
