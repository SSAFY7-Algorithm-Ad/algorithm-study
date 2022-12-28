# 📘 16236 (이주희)

## 소요시간, 메모리
<img width="80%" alt="image" src="https://user-images.githubusercontent.com/83942393/209750055-0308c9b5-bfa2-45f8-bc91-15a8c4a5557b.png">

## 풀이 방법

## Code

```Java
import java.io.*;
import java.util.*;

public class Main_bj_16236_아기상어 {

	static int N;
	static int[] di = { -1, 0, 1, 0 };
	static int[] dj = { 0, 1, 0, -1 };

	public static void main(String[] args) throws Exception {
//		System.setIn(new FileInputStream("res/input_bj_16236.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

		N = Integer.parseInt(br.readLine());
		int[][] sea = new int[N][N];
		int[] shark = new int[4]; // [0]: x, [1]:y, [2]: 크기, [3]: 지금까지 먹은 물고기
		StringTokenizer st;
		for (int i = 0; i < N; i++) {
			st = new StringTokenizer(br.readLine(), " ");
			for (int j = 0; j < N; j++) {
				int fish = Integer.parseInt(st.nextToken());
				sea[i][j] = fish;
				if (fish == 9) {
					shark = new int[] {i, j, 2, 0};
					sea[i][j] = 0;
				}
			}
		}

		int ans = 0;
		int[][] dist;
		PriorityQueue<Fish> pq;
		while (true) {
			// 0. 상어와 물고기까지 거리 찾기
			dist = new int[N][N];
			getDistance(shark, sea, dist);

			// 1. 물고기의 위치와 거리 pq 에 넣기
			pq = new PriorityQueue<>();
			for (int i = 0; i < N; i++) {
				for (int j = 0; j < N; j++) {
					if (sea[i][j] != 0 && dist[i][j] != 0 && sea[i][j] < shark[2]) { // 크기가 작은 물고기
						pq.add(new Fish(dist[i][j], i, j));
					}
				}
			}

			// 2. 먹을 물고기가 없다면 종료
			if (pq.isEmpty())
				break;

			// 3. 물고기 먹기
			Fish fish = pq.poll();

			shark[3]++;
			if (shark[3] == shark[2]) { // 먹은 물고기수와 크기가 같아지면
				shark[3] = 0;
				shark[2]++;
			}

			ans += fish.dist - 1; 		// 방문처리를 위해 1부터 시작한 수 빼기
			sea[fish.x][fish.y] = 0; 	// 잡아먹힘
			shark[0] = fish.x;
			shark[1] = fish.y; // 상어 위치 업데이트
		}

		System.out.println(ans);
		br.close();

	}

	static class Fish implements Comparable<Fish> {
		private int dist;
		private int x;
		private int y;

		public Fish(int dist, int x, int y) {
			this.dist = dist;
			this.x = x;
			this.y = y;
		}

		@Override
		public int compareTo(Fish o) {
			if (dist == o.dist) {
				if (x == o.x) {
					return y - o.y;
				}
				return x - o.x;
			}
			return dist - o.dist;		// 크기가 작은 게 우선
		}
	}

	static void getDistance(int[] shark, int[][] sea, int[][] dist) {
		Queue<int[]> que = new ArrayDeque<>();
		int size = shark[2];
		que.offer(new int[] { shark[0], shark[1] });
		dist[shark[0]][shark[1]] = 1; // 방문처리하기 위해 0이 아닌 1로 설정

		while (!que.isEmpty()) {
			int[] now = que.poll();

			for (int d = 0; d < 4; d++) {
				int ni = now[0] + di[d];
				int nj = now[1] + dj[d];

				if (ni < 0 || ni >= N || nj < 0 || nj >= N || dist[ni][nj] != 0 || sea[ni][nj] > size)
					continue;
				dist[ni][nj] = dist[now[0]][now[1]] + 1;
				que.offer(new int[] { ni, nj });
			}
		}
	}

}

```
