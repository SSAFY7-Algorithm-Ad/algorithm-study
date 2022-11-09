# 📘 1107 (김강호)

## 소요시간, 메모리

- 11668 KB
- 76 ms

## 풀이 방법

- BFS 응용하여 품 
- 한 쪽방향으로 탐색할때 while문 두개(빨간구슬,파란구슬) 사용 
- 빨간구슬, 파란구슬 동시에 움직이는동안 문제의 조건모두 적용

## Code

```java
import java.util.*;
import java.io.*;
public class Main {
    static int[] di = {-1,1,0,0};
    static int[] dj = {0,0,-1,1};
    static char[][] map;
    static int N, M;
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        map = new char[N][M];
        int rx = 0;
        int ry = 0;
        int bx = 0;
        int by = 0;
        for (int i = 0; i < N; i++) {
            map[i] = br.readLine().toCharArray();
            for (int j = 0; j < M; j++) {
                if(map[i][j] == 'R') {
                    rx = i;
                    ry = j;
                    map[i][j] = '.';
                }
                else if (map[i][j] == 'B') {
                    bx = i;
                    by = j;
                    map[i][j] = '.';
                }
            }
        }
        System.out.println(start(rx,ry,bx,by));
        br.close();
    }
    static int start(int x, int y, int bx, int by) {
        int dep = 0;
        Queue<int[]> q = new ArrayDeque<>();
        q.offer(new int[]{x,y,dep,bx,by,4});
        while(!q.isEmpty()) {
            int[] xy = q.poll();
            dep = xy[2];
            if(9<dep) return -1;
            for(int d = 0; d<4; d++) {
                if(xy[5]==d) continue;
                boolean chk = false;
                boolean end_b_chk = false;
                boolean end_r_chk = false;
                boolean r_chk = false;
                boolean b_chk = false;
                int nx = xy[0];
                int ny = xy[1];
                int nbx = xy[3];
                int nby = xy[4];
                while(true) {
                    nx += di[d];
                    ny += dj[d];
                    if(nx<0 || N<=nx || ny<0 || M<=ny || map[nx][ny] == '#') break;
                    if(map[nx][ny] == 'O') {
                        end_r_chk = true;
                        break;
                    }
                    if(nx == xy[3] && ny == xy[4]) b_chk = true;
                    chk = true;
                }
                while(true) {
                    nbx += di[d];
                    nby += dj[d];
                    if(nbx<0 || N<=nbx || nby<0 || M<=nby || map[nbx][nby] == '#') break;
                    if(map[nbx][nby] == 'O') end_b_chk = true;
                    if(xy[0] == nbx && xy[1] == nby) r_chk = true;
                    chk = true;
                }
                if(end_b_chk) continue;
                if(end_r_chk) return ++dep;
                if(chk) {
                    if(b_chk) {
                        nx-=di[d];
                        ny-=dj[d];
                    }
                    if(r_chk) {
                        nbx-=di[d];
                        nby-=dj[d];
                    }
                    int cd = xy[5];
                    if(d==0) cd = 1;
                    else if(d==1) cd = 0;
                    else if(d==2) cd = 3;
                    else if(d==3) cd = 2;
                    q.offer(new int[]{nx-di[d],ny-dj[d],dep+1,nbx-di[d],nby-dj[d],cd});
                }
            }
        }
        return -1;
    }
}
```
