# 📘 1956

## 소요시간, 메모리
768ms, 66192KB

## 풀이 방법

## Code

```java
package backjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Q_1956 {

	static int INF = (int) 1e9;

	static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
	static StringTokenizer st;
	static int V, E;
	static int[][] map;
	static int ans = Integer.MAX_VALUE;

	public static void main(String[] args) throws IOException {
		st = new StringTokenizer(br.readLine());
		V = Integer.parseInt(st.nextToken());
		E = Integer.parseInt(st.nextToken());

		map = new int[V + 1][V + 1];

		for (int i = 1; i <= V; i++) {
			for (int j = 1; j <= V; j++) {
				if (i == j) {
					map[i][j] = 0;
				} else {
					map[i][j] = INF;
				}
			}
		}

		for (int i = 0; i < E; i++) {
			st = new StringTokenizer(br.readLine());
			int a = Integer.parseInt(st.nextToken());
			int b = Integer.parseInt(st.nextToken());
			int c = Integer.parseInt(st.nextToken());
			map[a][b] = c;
		}

		for (int k = 1; k <= V; k++) {
			for (int i = 1; i <= V; i++) {
				for (int j = 1; j <= V; j++) {
					map[i][j] = Math.min(map[i][j], map[i][k] + map[k][j]);
				}
			}
		}
		
		for(int i=1; i<=V; i++) {
			for(int j=1; j<=V; j++) {
				if(i==j) continue;
				if(map[i][j] != INF && map[j][i] != INF) {
					ans = Math.min(ans, map[i][j] + map[j][i]);
				}
			}
		}
		
		if(ans == Integer.MAX_VALUE) {
			System.out.println(-1);
			return;
		}
		
		System.out.println(ans);
	}
}
```