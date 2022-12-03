# 📘 16236 (김강호)

## 소요시간, 메모리

232 ms, 18640 KB

## 풀이 방법

- bfs 탐색 사용

## Code

```Java
import java.io.*;
import java.util.*;
public class Main {
    static int[] di = {-1,0,0,1};
    static int[] dj = {0,-1,1,0};
    static int[][] map;
    static int N, shark = 2, size_cnt = 0, res_time = 0;
    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());
        map = new int[N][N];
        StringTokenizer st;
        int x = 0;
        int y = 0;
        for(int i=0; i<N; i++) {
            st = new StringTokenizer(br.readLine()," ");
            for (int j = 0; j < N; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
                if(map[i][j]==9) {
                    map[i][j] = 0;
                    x = i;
                    y = j;
                }
            }
        }
        start(x,y,0);
        System.out.println(res_time);
        br.close();
    }
    static void start(int x, int y, int sum) {
        boolean[][] visit = new boolean[N][N];
        Queue<int[]> que = new ArrayDeque<>();
        PriorityQueue<int[]> pq = new PriorityQueue<>((o1, o2) -> {
            if(o1[2]==o2[2]) {
                if(o1[0]==o2[0]) return o1[1]-o2[1];
                return o1[0]-o2[0];
            }
            return o1[2]-o2[2];
        });
        pq.offer(new int[]{x,y,0});
        visit[x][y] = true;
        while(!pq.isEmpty()) {
            int[] xy = pq.poll();
            int time = xy[2];
            if(map[xy[0]][xy[1]]!=0 && map[xy[0]][xy[1]]<shark) {
                map[xy[0]][xy[1]] = 0;
                if(shark==++size_cnt) {
                    size_cnt=0;
                    shark++;
                }
                start(xy[0],xy[1],sum+time);
                return;
            }
            for(int d=0; d<4; d++) {
                int nx = xy[0] + di[d];
                int ny = xy[1] + dj[d];
                if(nx<0 || N<=nx || ny<0 || N<=ny || visit[nx][ny] || shark<map[nx][ny]) continue;
                visit[nx][ny] = true;
                pq.offer(new int[]{nx,ny,time+1});
            }
        }
        res_time = sum;
    }
}
```
