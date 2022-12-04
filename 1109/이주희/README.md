# 📘 1109 (이주희)

## 소요시간, 메모리
![3](https://user-images.githubusercontent.com/83942393/205477083-629a7c85-7631-47b5-b179-78a162318944.PNG)


## 풀이 방법
- 푸는데 몇일 걸렸다..
- 벽 깨부수는 문제는 3차원 배열로만 풀 수 있는 줄 알았는데, 인접한 방과 합치는 방법은 처음 봐서 좋은 공부가 된 것 같다.

## Code

```java
import java.io.*;
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Queue;
import java.util.Set;
import java.util.StringTokenizer;

public class Main_bj_2234_성곽2 {
	
	static int[][] wall, map;	// 벽의 정보를 담을 2차원 배열, 같은 방 번호를 담을 2차원 배열
	static int maxSize=0;
	static ArrayList<Integer> space = new ArrayList<>();	// 방 번호에 해당하는 넓이 리스트
	static HashMap<Integer, Set<Integer>> side = new HashMap<>();	// 방과 인접한 방의 리스트
	static Queue<int[]> que;
	static int[] di = {0, -1, 0, 1};
	static int[] dj = {-1, 0, 1, 0};
	static int[] dr = {1, 2, 4, 8};
	
	public static void main(String[] args) throws Exception {
		System.setIn(new FileInputStream("res/input_bj_2234.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		StringTokenizer st = new StringTokenizer(br.readLine(), " ");
		int M = Integer.parseInt(st.nextToken());
		int N = Integer.parseInt(st.nextToken());
		
		wall = new int[N][M];	// 벽의 정보를 담을 배열
		map = new int[N][M];	// 방의 번호를 담을 배열
		
		for(int i=0; i<N; i++) {
			st = new StringTokenizer(br.readLine(), " ");
			for(int j=0; j<M; j++) {
				wall[i][j] = Integer.parseInt(st.nextToken());
			}
		}
		que = new ArrayDeque<>();
		int num = 1; 	// 1번 방부터 번호 매기기
		for(int i=0; i<N; i++) {
			for(int j=0; j<M; j++) {
				if (map[i][j] == 0) {			// 아직 방문 안했다면
					bfs(i, j, num++, N, M); 	// 갔다 오면 번호가 1씩 증가
				}
			}
		}
		
		int roomNum = num-1;
		System.out.println(roomNum);
		System.out.println(maxSize);
		
		
		int sum = 0; // 벽 하나 허물고 합친 최대 넓이 
		for(int room=1; room<=roomNum; room++) {	// 방 한개씩 돌아보며
			if(side.get(room) != null) {	// 인접한 방이 있다면
				for(int j : side.get(room)) {	// 인접한 방 한개씩 받아오기
					sum = Math.max(sum, space.get(room-1) + space.get(j-1));
				}
			}
		}
		
		System.out.println(sum);
	}
	
	static void bfs(int i, int j, int num, int N, int M) {
		map[i][j] = num;
		que.add(new int[] {i, j});
		
		Set<Integer> set = new HashSet<>();		// 현재 방과 인접한 방의 번호, 겹치지 않기 위해 set 사용
		
		int cnt = 0;
		while(!que.isEmpty()) {
			int[] now = que.poll();
			cnt++;
			
			for(int d=0; d<4; d++) {
				int ni = now[0] + di[d];
				int nj = now[1] + dj[d];
				
				if(ni<0 || ni>=N || nj<0 || nj>=M) continue;
				if(map[ni][nj] != 0 && map[ni][nj] != num) {	// 방문을 했는데 지금 방 번호와 다르다면 인접한 방
					set.add(map[ni][nj]);
					continue;
				}
				if((wall[now[0]][now[1]] & dr[d]) == 0 && map[ni][nj] == 0) {	// 갈 수 있는 방이고, 방문하지 않았다면
					que.add(new int[] {ni, nj});
					map[ni][nj] = num; 	// num 번 방 할당
				}
			}
		}
		
		maxSize = Math.max(maxSize, cnt);
		space.add(cnt);
		side.put(num, set);
	}

}
```
