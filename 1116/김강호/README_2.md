# 📘 14500 (김강호)

## 소요시간, 메모리

800 ms, 73980 KB

## 풀이 방법

- 

## Code

```Java
import java.io.*;
import java.util.*;
public class Main {
	static int[] di = {-1,1,0,0};	//상 하 좌 우 
	static int[] dj = {0,0,-1,1};
	static int max=0;
	public static void main(String[] args) throws Exception {
		BufferedReader br= new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine()," ");
		int N = Integer.parseInt(st.nextToken());
		int M = Integer.parseInt(st.nextToken());
		int[][] map = new int[N][M];
		for(int i=0; i<N; i++) {
			st = new StringTokenizer(br.readLine()," ");
			for(int j=0; j<M; j++) {
				map[i][j]=Integer.parseInt(st.nextToken());
			}
		}
        boolean[][] v = new boolean[N][M];
		for(int i=0; i<N; i++) {
			for(int j=0; j<M; j++) {
				d(map,v,i,j,1,N,M,map[i][j]); 
				b(map,i,j,N,M);
			}
		}
		System.out.println(max);
		br.close();
	}
	static void b(int[][] map, int i, int j, int N, int M) {
		List<Integer> arr = new LinkedList<>();
		int sum=map[i][j];
		int count=0;
		for(int d=0; d<4; d++) {
			int ni=i+di[d];
			int nj=j+dj[d];
			if(0<=ni && ni<N && 0<=nj && nj<M) {
				arr.add(map[ni][nj]);
				count++;
			}
		}
		if(count<3)return;
		Collections.sort(arr);
		for(int q=0; q<3; q++) sum+=arr.remove(arr.size()-1);
        max=Math.max(max, sum);
	}
	static void d(int[][]map, boolean[][] v, int i, int j, int cnt, int N, int M, int sum) {
		if(cnt==4) {
			max=Math.max(max, sum);
			return;
		}
        v[i][j]=true;
		for(int d=0; d<4; d++) {
			int ni=i+di[d];
			int nj=j+dj[d];
			if(0<=ni && ni<N && 0<=nj && nj<M && !v[ni][nj]) {
				v[ni][nj]=true;
				d(map,v,ni,nj,cnt+1,N,M,sum+map[ni][nj]);
				v[ni][nj]=false;
			}
		}
        v[i][j]=false;
	}
}
```
