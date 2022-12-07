# 📘 16236

## 소요시간, 메모리
136ms, 14676KB

## 풀이 방법
- bfs로 구현, pq 사용
- compare 함수에서 어이없는 실수... (this.r, o.r)

## Code

```java
package BAEKJOON;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.PriorityQueue;
import java.util.Queue;
import java.util.StringTokenizer;

public class gold3_16236_아기상어 {
	static int[] dr = {-1, 0, 1, 0};
	static int[] dc = {0, 1, 0, -1};
	static int N, result = 0;
	static Node baby;
	static int[][] map;
	static boolean[][] visit;

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st;
		N = Integer.parseInt(br.readLine());
		map = new int[N][N];

		for (int r = 0; r < map.length; r++) {
			st = new StringTokenizer(br.readLine());
			for (int c = 0; c < map[r].length; c++) {
				map[r][c] = Integer.parseInt(st.nextToken());
				if(map[r][c] == 9) {
					baby = new Node(r, c, 0);
					map[r][c] = 0;
				}
			}
		}

		bfs();
		System.out.println(result);
	}

	private static void bfs() {
		int size = 2; // 아기상어 크기
		int eat = 0; // 먹은 갯수

		while(true) {
			PriorityQueue<Node> pq = new PriorityQueue<Node>();
			visit = new boolean[N][N];

			pq.add(new Node(baby.r, baby.c, 0));
			visit[baby.r][baby.c] = true;

			boolean check = false;

			// 옮겨다니면서 물고기 먹기
			while(!pq.isEmpty()) {
				baby = pq.poll();

				// 먹이가 있고, 상어 사이즈보다 작으면
				if(map[baby.r][baby.c] != 0 && map[baby.r][baby.c] < size) {
					// 물고기 먹기
					map[baby.r][baby.c] = 0;
					eat++;
					// 움직인 거리 추가
					result += baby.dist;
					// 먹었으면 끝내고 다시 실행해야하니까 true
					check = true;
					break;
				}

				for (int d = 0; d < dc.length; d++) {
					int nr = baby.r + dr[d];
					int nc = baby.c + dc[d];

					if(nr>=0 && nr<N && nc>=0 && nc<N && !visit[nr][nc] && map[nr][nc] <= size) {
						pq.add(new Node(nr, nc, baby.dist+1));
						visit[nr][nc] = true;
					}
				}
			}

			// 먹을 물고기가 없다는 뜻~
			if(!check) {
				break;
			}

			// 상어 사이즈 변경~
			if(size == eat) {
				size++;
				eat = 0;
			}
		}

	}

	static class Node implements Comparable<Node>{
		int r, c, dist;

		public Node(int x, int y, int dist) {
			super();
			this.r = x;
			this.c = y;
			this.dist = dist;
		}

		@Override
		public int compareTo(Node o) {
			if(o.dist == this.dist) {
				if(o.r == this.r) {
					return Integer.compare(this.c, o.c);
				}
				return Integer.compare(this.r, o.r);
			}
			return Integer.compare(this.dist, o.dist);
		}
	}

}

```
