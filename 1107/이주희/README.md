# 📘 1107 (이주희)

## 소요시간, 메모리
<img width="90%" alt="image" src="https://user-images.githubusercontent.com/83942393/207598590-865b4cc4-5137-4cee-956e-2cd79f9e7448.png">

## 풀이 방법

## Code
```Java
import java.io.*;
import java.util.ArrayDeque;
import java.util.Queue;
import java.util.StringTokenizer;

/*
 * 최소 몇 번만에 빨간 구슬을 빼낼 수 있는지
 * 
 * 파란 구슬을 먼저 꺼내면 X
 * 10 이하로 빼낼 수 없다면 -1 출력
 */

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
		
		N = Integer.parseInt(st.nextToken());
		M = Integer.parseInt(st.nextToken());
		
		int[] redBall = new int[2];
		int[] blueBall = new int[2];
		
		map = new char[N][M];
		visited = new boolean[N][M][N][M];
		
		for(int i=0; i<N; i++) {
			String s = br.readLine();
			for(int j=0; j<M; j++) {
				map[i][j] = s.charAt(j);
				
				if(map[i][j] == 'B') {
					blueBall[0] = i;
					blueBall[1] = j;
				}
				if(map[i][j] == 'R') {
					redBall[0] = i;
					redBall[1] = j;
				}
			}
		}
		
		System.out.println(bfs(redBall, blueBall));
		br.close();
	}
	
	static int bfs(int[] redBall, int[] blueBall) {
		Queue<Position> que = new ArrayDeque<>();
		que.offer(new Position(redBall[0], redBall[1], blueBall[0], blueBall[1], 1));
		visited[redBall[0]][redBall[1]][blueBall[0]][blueBall[1]] = true;
		
		while(!que.isEmpty()) {
			Position now = que.poll();
			
			if(now.cnt > 10) return -1;
			
			// 상, 우, 하, 좌
			for(int i=0; i<4; i++) {
				
				int nrx = now.redBall[0];
				int nry = now.redBall[1];
				int nbx = now.blueBall[0];
				int nby = now.blueBall[1];
				
				boolean isRedHole = false;
				boolean isBlueHole = false;
				
				// # 만날 때까지 옮기기
				while(map[nrx + dx[i]][nry + dy[i]] != '#') {
					nrx += dx[i];
					nry += dy[i];
					
					if(map[nrx][nry] == 'O') {
						isRedHole = true;
						break;
					}
				}
				while(map[nbx + dx[i]][nby + dy[i]] != '#') {
					nbx += dx[i];
					nby += dy[i];
					
					if(map[nbx][nby] == 'O') {
						isBlueHole = true;
						break;
					}
				}
				
				// 파란 공이 구멍에 빠지면 이 경우는 버리기
				if(isBlueHole) continue;
				
				// 빨간 공이 구멍에 빠지면 종료
				if(isRedHole) return now.cnt;
				
				// 빨간 공과 파란 공이 같은 위치라면
				if((nrx == nbx) && (nry == nby)) {
					
					// 옮기기 전 더 뒤에 있던 공을 뒤로 이동
					
					// 상
					if(i == 0) {
						if(now.redBall[0] > now.blueBall[0]) {
							nrx -= dx[i];
						} else {
							nbx -= dx[i];
						}
					}
					
					// 우
					if(i == 1) {
						if(now.redBall[1] > now.blueBall[1]) {
							nby -= dy[i];
						} else {
							nry -= dy[i];
						}
					}
					
					// 하
					if(i == 2) {
						if(now.redBall[0] > now.blueBall[0]) {
							nbx -= dx[i];
						} else {
							nrx -= dx[i];
						}
						
					}
					
					// 좌
					if(i == 3) {
						if(now.redBall[1] > now.blueBall[1]) {
							nry -= dy[i];
						} else {
							nby -= dy[i];
						}
					}
					
				}
				// 공 기울이기 종료
				if(!visited[nrx][nry][nbx][nby]) {	// 두 공이 방문하지 않은 위치라면
					visited[nrx][nry][nbx][nby] = true;
					que.offer(new Position(nrx, nry, nbx, nby, now.cnt+1));
				}
			}
		}
		
		return -1;
		
	}
	
	static class Position {
		private int[] redBall = new int[2];
		private int[] blueBall = new int[2];
		private int cnt;			// 몇번 기울였는지
		
		Position(int rx, int ry, int bx, int by, int cnt) {
			redBall[0] = rx;
			redBall[1] = ry;
			blueBall[0] = bx;
			blueBall[1] = by;
			this.cnt = cnt;
		}
	}

}
```
