# 📘 14503

## 소요시간, 메모리
132ms, 14384KB

## 풀이 방법
dfs로 구현

## Code

```java
package bj;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class gold5_14503_로봇청소기 {
	// 북 동 남 서
	static int[] dr = { -1, 0, 1, 0 };
	static int[] dc = { 0, 1, 0, -1 };
	static int N, M, R, C, D, result;
	static int[][] map;

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
		N = Integer.parseInt(st.nextToken());
		M = Integer.parseInt(st.nextToken());
		map = new int[N][M];
		st = new StringTokenizer(br.readLine());
		R = Integer.parseInt(st.nextToken());
		C = Integer.parseInt(st.nextToken());
		D = Integer.parseInt(st.nextToken());

		for (int r = 0; r < map.length; r++) {
			st = new StringTokenizer(br.readLine());
			for (int c = 0; c < map[r].length; c++) {
				map[r][c] = Integer.parseInt(st.nextToken());
			}
		}

		map[R][C] = 2;
		result = 1;
		play(R, C, D);
		System.out.println(result);
	}

	private static void play(int r, int c, int dist) {
		for (int d = 0; d < dc.length; d++) {
			dist = (dist + 3) % 4;
			int nr = r + dr[dist];
			int nc = c + dc[dist];
			// 왼쪽방향이 청소하지 않았다면
			if (map[nr][nc] == 0 && nr >= 0 && nr < N && nc >= 0 && nc < M) {
				// 현재 위치 청소
				map[nr][nc] = 2;
				result++;

				// 한칸 전진 후 진행
				play(nr, nc, dist);
				return;
			}
		}

		int back = (dist + 2) % 4;
		int nr = r + dr[back];
		int nc = c + dc[back];
		if (nr >= 0 && nr < N && nc >= 0 && nc < M) {
			if (map[nr][nc] != 1) {
				// 3번 진행
				play(nr, nc, dist);
			}
		}
	}
}

```
