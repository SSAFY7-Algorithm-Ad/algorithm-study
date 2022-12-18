# 📘 16234 (이주희)

## 메모리, 소요시간
방법 1) 296900KB, 592ms </br>
방법 2) 298600KB, 620ms

## 풀이 방법
- BFS

## Code
방법 1)

```Java
import java.io.*;
import java.util.*;

public class Main {
	static int[] di = {-1, 0, 1, 0};
	static int[] dj = {0, 1, 0, -1};
	
	static int N, L, R;
	static int[][] map, open, nmap;
	
	public static void main(String[] args) throws Exception{
		//System.setIn(new FileInputStream("res/input_bj_16234.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine(), " ");
		
		N = Integer.parseInt(st.nextToken());
		L = Integer.parseInt(st.nextToken());
		R = Integer.parseInt(st.nextToken());
		
		map = new int[N][N];
		
		for(int i=0; i<N; i++) {
			st = new StringTokenizer(br.readLine(), " ");
			for(int j=0; j<N; j++) {
				map[i][j] = Integer.parseInt(st.nextToken());
			}
		}
		
		int ans=0;
		while(true) {
			open = new int[N][N];
			nmap = new int[N][N];
			for(int i=0; i<N; i++) {
				nmap[i] = Arrays.copyOf(map[i], N);
			}
			
			
			int cnt=0, index=1;
			for(int i=0; i<N; i++) {
				for(int j=0; j<N; j++) {
					if(open[i][j]==0) {
						cnt += bfs(i, j, index++);
					}
				}
			}
			
			if(cnt==0) break;
			for(int i=0; i<N; i++) {
				map[i] = Arrays.copyOf(nmap[i], N);
			}
			ans++;
		}
		System.out.println(ans);
		br.close();
	}
	
	static int bfs(int i, int j, int index) {
		Queue<int[]> que = new LinkedList<>();
		que.offer(new int[] {i, j});
		open[i][j] = index;
		
		int cnt=1, total=map[i][j];
		
		while(!que.isEmpty()) {
			int[] cur = que.poll();
			for(int k=0; k<4; k++) {
				int ni = cur[0]+di[k];
				int nj = cur[1]+dj[k];
				
				if(ni<0 || ni>=N || nj<0 || nj>=N || open[ni][nj]!=0) continue;
				int d = Math.abs(map[ni][nj] - map[cur[0]][cur[1]]);
				
				if(d>=L && d<=R) {
					open[ni][nj] = index;
					que.offer(new int[] {ni, nj});
					cnt++;
					total += map[ni][nj];
				}
			}
		}
		
		if(cnt>1) {
			int value = total/cnt;
			for(int x=0; x<N; x++) {
				for(int y=0; y<N; y++) {
					if(open[x][y]==index) {
						nmap[x][y] = value;
					}
				}
			}
			
			return 1;
		}
		return 0;
		
		
	}

}
```

방법 2)
```Java
import java.io.*;
import java.util.*;

public class Main_bj_16234_인구이동 {
	
	static int N, L, R;
	static int[][] map;
	static boolean[][] visited;
	static int[] dx = {-1, 0, 1, 0};	//상, 우, 하, 좌
	static int[] dy = {0, 1, 0, -1};
	static ArrayList<int[]> union;		// 연합국에 포함되서 인구수가 변경된 나라들

	public static void main(String[] args) throws Exception {
//		System.setIn(new FileInputStream("res/input_bj_16234.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		StringTokenizer st = new StringTokenizer(br.readLine(), " ");
		N = Integer.parseInt(st.nextToken());
		L = Integer.parseInt(st.nextToken());
		R = Integer.parseInt(st.nextToken());
		
		map = new int[N][N];
		
		for(int i=0; i<N; i++) {
			st = new StringTokenizer(br.readLine(), " ");
			for(int j=0; j<N; j++) {
				map[i][j] = Integer.parseInt(st.nextToken());
			}
		}
		
		int ans = 0;
		while(true) {
			visited = new boolean[N][N];
			union = new ArrayList<>();
			
			for(int i=0; i<N; i++) {
				for(int j=0; j<N; j++) {
					if(!visited[i][j]) {
						bfs(i, j);
					}
				}
			}
			
			if(union.size() == 0) break;			// 인구이동된 나라가 없음
			
			for(int i=0; i<union.size(); i++) {		// 재계산된 인구수 반영
				int[] city = union.get(i);
				map[city[0]][city[1]] = city[2];
			}
			
			ans++; 
		}
		
		System.out.println(ans);
		br.close();
	}
	
	static void bfs(int x, int y) {
		visited[x][y] = true;
		Queue<int[]> que = new ArrayDeque<>();
		que.offer(new int[] {x, y});
		
		int cnt = 1; 					// 연합국에 속한 나라의 갯수
		int sum = map[x][y];			// 연합국에 포함된 인구수의 총합
		int size = union.size();		// 연합국에 이미 포함되어 있던 나라 수
		
		while(!que.isEmpty()) {
			int[] now = que.poll();
			
			for(int i=0; i<4; i++) {
				int nx = now[0] + dx[i];
				int ny = now[1] + dy[i];
				
				if(nx<0 || nx>=N || ny<0 || ny>=N || visited[nx][ny]) continue;
				
				int diff = Math.abs(map[nx][ny] - map[now[0]][now[1]]);
				if(L <= diff && diff <= R) {
					que.offer(new int[] {nx, ny});
					sum += map[nx][ny];
					visited[nx][ny] = true; 
					cnt++;
					union.add(new int[] {nx, ny, 0});
				}
			}
		}
		
		if(cnt >= 2) {								// 인구이동된 나라들의 있음
			int newValue = sum / cnt;
			for(int i=size; i<union.size(); i++) {	// 새롭게 추가된 나라들
				union.get(i)[2] = newValue; 		// 새 인구수 할당
			}
			
			union.add(new int[] {x, y, newValue});	// 탐색을 시작한 나라 추가
		}
	}

}
```
