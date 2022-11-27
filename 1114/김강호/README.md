# 📘 16234 (김강호)

## 소요시간, 메모리

608ms, 294368KB

## 풀이 방법

- bfs활용하여 품.

## Code

```Java
import java.io.*;
import java.util.*;
public class Main {
	static int[] di= {-1,1,0,0};
	static int[] dj= {0,0,-1,1};
	public static void main(String[] args) throws Exception {
		BufferedReader br= new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st =new StringTokenizer(br.readLine()," ");
		int N = Integer.parseInt(st.nextToken());
		int L = Integer.parseInt(st.nextToken());
		int R = Integer.parseInt(st.nextToken());
		int[][] map = new int[N][N];
		for(int i=0; i<N; i++) {
			st =new StringTokenizer(br.readLine()," ");
			for(int j=0; j<N; j++) {
				map[i][j] = Integer.parseInt(st.nextToken());
			}
		}
		int day=0;
		fi:while(true) {
			boolean[][] v = new boolean[N][N];
			for(int x=0; x<N; x++) {
				for(int y=0; y<N; y++) {
					if(!v[x][y]) {
						b(x,y,map,v,N,L,R);
					}
				}
			}
			for(int x=0; x<N; x++) {
				for(int y=0; y<N; y++) {
					if(v[x][y]) {
						day++;
						continue fi;
					}
				}
			}
			break;
		}
		System.out.println(day);
		br.close();
	}
	static void b(int i, int j, int[][] map, boolean[][] v, int N, int L, int R) {
		Queue<int[]> q = new ArrayDeque<>();
		Queue<int[]> q2 = new ArrayDeque<>();
		boolean flag=false;
		int sum=0;
		int cnt=0;
		q.offer(new int[] {i,j});
		q2.offer(new int[] {i,j});
		v[i][j]=true;
		while(!q.isEmpty()) {
			int[] xy=q.poll();
			i=xy[0];
			j=xy[1];
			sum+=map[i][j];
			cnt++;
			for(int d=0; d<4; d++) {
				int ni=i+di[d];
				int nj=j+dj[d];
				if(0<=ni && ni<N && 0<=nj && nj<N && !v[ni][nj] && L<=Math.abs(map[i][j]-map[ni][nj]) && Math.abs(map[i][j]-map[ni][nj])<=R) {
					v[ni][nj]=true;
					q.offer(new int[] {ni,nj});
					q2.offer(new int[] {ni,nj});
					flag=true;
				}
			}
		}
		if(flag) {
			int n = sum/cnt;
			while(!q2.isEmpty()) {
				int[] ij=q2.poll();
				map[ij[0]][ij[1]]=n;
			}
		}
		else v[i][j]=false;
	}
}
```
