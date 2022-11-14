# 📘 1109 (김강호)

## 소요시간, 메모리

- 12304 KB
- 92 ms

## 풀이 방법

- BFS 활용해서 품. 담엔 DFS로 풀어봐야겠다.
- 비트마스킹을 조금더 공부해봐야겠다.

## Code

```java
import java.io.*;
import java.util.*;
public class Main {
    static int[] bit = {1,2,4,8};
    static int[] di = {0,-1,0,1};
    static int[] dj = {-1,0,1,0};
    static int N, M;
    static int[][] map, room;
    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        StringBuilder sb = new StringBuilder();
        M = Integer.parseInt(st.nextToken());
        N = Integer.parseInt(st.nextToken());
        map = new int[N][M];
        room = new int[N][M];
        for(int i=0; i<N; i++) {
            st = new StringTokenizer(br.readLine()," ");
            for(int j=0; j<M; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }
        int num = 1;
        int max = 0;
        int[] room_cnt = new int[N*M+1];
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                if(room[i][j]==0) {
                    room_cnt[num] = bfs(i,j,num);
                    max = Math.max(room_cnt[num++],max);
                }
            }
        }
        sb.append(--num).append("\n").append(max).append("\n");
        max = 0;
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                for(int d=0; d<4; d++) {
                    int ni = i + di[d];
                    int nj = j + dj[d];
                    if(ni<0 || N<=ni || nj<0 || M<=nj || room[i][j] == room[ni][nj]) continue;
                    max = Math.max(max, room_cnt[room[i][j]]+room_cnt[room[ni][nj]]);
                }
            }
        }
        sb.append(max);
        System.out.println(sb);
        br.close();
    }
    static int bfs(int x, int y, int num) {
        int cnt = 1;
        Queue<int[]> que = new ArrayDeque<>();
        que.offer(new int[]{x,y});
        room[x][y] = num;
        while(!que.isEmpty()) {
            int[] xy = que.poll();
            for(int d=0; d<4; d++) {
                if((map[xy[0]][xy[1]] & bit[d]) != 0) continue;
                int nx = xy[0] + di[d];
                int ny = xy[1] + dj[d];
                if(room[nx][ny]!=0) continue;
                room[nx][ny] = num;
                que.offer(new int[]{nx,ny});
                cnt++;
            }
        }
        return cnt;
    }
}
```
