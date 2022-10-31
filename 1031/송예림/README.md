# 📘 1956

## 소요시간, 메모리
728ms, 53296KB

## 풀이 방법
모든 간선 탐색해 두 노드 간 최소 비용 출력 -> 플로이드 워셜

## Code

```java
package BAEKJOON;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class gold4_1956_운동 {

    static int V, E, result;
    static int[][] dist;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        st = new StringTokenizer(br.readLine());
        V = Integer.parseInt(st.nextToken());
        E = Integer.parseInt(st.nextToken());
        dist = new int[V+1][V+1];
        result = Integer.MAX_VALUE;

        // 자기자신 = 0
        for (int r = 1; r < dist.length; r++) {
            for (int c = 1; c < dist[r].length; c++) {
//				if(r != c) -> 자기자신 max로 두고 갱신
                dist[r][c] = Integer.MAX_VALUE;
            }
        }

        for (int i = 0; i < E; i++) {
            st = new StringTokenizer(br.readLine());
            int s = Integer.parseInt(st.nextToken());
            int e = Integer.parseInt(st.nextToken());
            int w = Integer.parseInt(st.nextToken());
            dist[s][e] = w;
        }

        // i : 중간, j : 시작, k : 끝
        for (int i = 1; i < dist.length; i++) {
            for (int j = 1; j < dist.length; j++) {
                // 자기자신 제외
                if(i == j)
                    continue;
                for (int k = 1; k < dist.length; k++) {
                    if(dist[j][i] == Integer.MAX_VALUE || dist[i][k] == Integer.MAX_VALUE)
                        continue;
                    dist[j][k] = Math.min(dist[j][k], dist[j][i] + dist[i][k]);
                }
            }
        }

        // 사이클 -> 출발, 도착 노드 같음
        for (int i = 1; i < dist.length; i++) {
            result = Math.min(result, dist[i][i]);
        }

        result = (result == Integer.MAX_VALUE) ? -1 : result;

        System.out.println(result);
    }
}

```