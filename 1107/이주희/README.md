# 📘 1107 (이주희)

## 소요시간, 메모리
<img width="90%" alt="image" src="https://user-images.githubusercontent.com/83942393/207598590-865b4cc4-5137-4cee-956e-2cd79f9e7448.png">

## 풀이 방법
* BFS, 시뮬레이션

## Code
```Java
import java.io.*;
import java.util.ArrayDeque;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main_bj_13460_구슬탈출2 {
	
	static int N, M;
	static char[][] map;
	static boolean[][][][] visited;
	static int[] dx = {-1, 0, 1, 0};	// 상, 우, 하, 좌
	static int[] dy = {0, 1, 0, -1};

	public static void main(String[] args) throws Exception {
//		System.setIn(new FileInputStream("res/input_bj_13460.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine(), " ");
		
		int N = Integer.parseInt(st.nextToken());
		int M = Integer.parseInt(st.nextToken());
		
		map = new char[N][M];
		visited = new boolean[N][M][N][M]; 		// 방문 여부 체크
		
		int redX=0, redY=0, blueX=0, blueY=0;	// 빨간 공, 파란 공 위치 저장
		
		for(int i=0; i<N; i++) {
			map[i] = br.readLine().toCharArray();
			for(int j=0; j<M; j++) {
				if(map[i][j] == 'B') {
					blueX = i;
					blueY = j;
				}
				if(map[i][j] == 'R') {
					redX = i;
					redY = j;
				}
			}
		}
		
		System.out.println(bfs(redX, redY, blueX, blueY));
		br.close(); 
	}
	
	static class Position {
		private int redX;
		private int redY;
		private int blueX;
		private int blueY;
		private int cnt;
		
		Position(int redX, int redY, int blueX, int blueY, int cnt) {
			this.redX = redX;
			this.redY = redY;
			this.blueX = blueX;
			this.blueY = blueY;
			this.cnt = cnt;
		}
	}
	
	static int bfs(int redX, int redY, int blueX, int blueY) {
		Queue<Position> que = new ArrayDeque<>();
		que.add(new Position(redX, redY, blueX, blueY, 1));
		visited[redX][redY][blueX][blueY] = true;
		
		while(!que.isEmpty()) {
			Position now = que.poll();
			
			if(now.cnt > 10) return -1;
			
			// 4방 탐색
			outer: for(int i=0; i<4; i++) {
				// 구슬 이동
				int nrx = now.redX;
				int nry = now.redY;
				int nbx = now.blueX;
				int nby = now.blueY;
				
				// 파란 공 이동
				while(map[nbx + dx[i]][nby + dy[i]] != '#') {
					nbx += dx[i];
					nby += dy[i]; 
					
					if(map[nbx][nby] == 'O') {	// 파란 공이 구멍에 들어가면 무조건 다음 경로
						continue outer;
					}
				}
				
				while(map[nrx + dx[i]][nry + dy[i]] != '#') {
					nrx += dx[i];
					nry += dy[i]; 
					
					if(map[nrx][nry] == 'O') {	// 파란 공이 구멍에 들어가지 않고, 빨간 공이 들어가면 답 return
						return now.cnt;
					}
				}
				
				// 빨간 공과 파란 공 둘 다 구멍에 들어가지 않고
				// 빨간 공과 파란 공의 위치가 겹치는 경우 -> 상,우,하,좌 방향에 따라 뒤에 있을 공 위치 재조정
				if(nrx == nbx && nry == nby) {
					
					// 상
					if(i == 0) {
						if(now.redX > now.blueX) {
							nrx -= dx[i];
						}
						else {
							nbx -= dx[i];
						}
					}
					
					// 우
					if(i == 1) {
						if(now.redY > now.blueY) {
							nby -= dy[i];
						}
						else {
							nry -= dy[i];
						}
					}
					
					// 하
					if(i == 2) {
						if(now.redX > now.blueX) {
							nbx -= dx[i];
						}
						else {
							nrx -= dx[i];
						}
					}
					
					// 좌
					if(i == 3) {
						if(now.redY > now.blueY) {
							nry -= dy[i];
						}
						else {
							nby -= dy[i];
						}
					}
				}
				
				// 구슬 이동 종료
				if(!visited[nrx][nry][nbx][nby]) {
					visited[nrx][nry][nbx][nby] = true;
					que.offer(new Position(nrx, nry, nbx, nby, now.cnt+1));
				}
				
			}
		}
		return -1;
	}

}

```
