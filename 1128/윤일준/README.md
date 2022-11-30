# 📘 10775

## 소요시간, 메모리
244ms, 23028KB

## 풀이 방법

그리디, 완전탐색으로 풀면 10^10 계산이라 100초가 걸릴 수 있어 union-find를 이용해서 탐색 시간을 줄여야 함.

## Code

```java
/**
 * Union-find를 이용한 풀이
 */

package backjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Q_10775_2 {
    public static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    public static int G, P, ans;
    public static int[] arr;

    public static void main(String[] args) throws NumberFormatException, IOException {
        G = Integer.parseInt(br.readLine());
        P = Integer.parseInt(br.readLine());
        arr = new int[G + 1];
        for (int i = 0; i < G + 1; i++) {
            arr[i] = i;
        }

        // 비행기 한대씩 들어와
        for (int i = 0; i < P; i++) {
            int g = Integer.parseInt(br.readLine());
            // 만약 더 이상 댈 수 있는 곳이 없다.
            int emptyGate = find(g);
            if (emptyGate == 0) {
                break;
            }

            // 댈 수 있다.
            ans++;
            // 값 최신화
            union(emptyGate, emptyGate - 1);
        }

        System.out.println(ans);
    }

    private static int find(int x) {
        if (arr[x] == x) {
            return x;
        }

        return arr[x] = find(arr[x]);
    }

    private static void union(int x, int y) {
        int a = find(x);
        int b = find(y);

        if (a != b) {
            arr[x] = y;
        }
    }
}

```