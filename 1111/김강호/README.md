# 📘 14503 (김강호)

## 소요시간, 메모리

80ms, 11836KB

## 풀이 방법

- dfs활용 구현 문제

## Code

```Java
import java.io.*;
import java.util.*;
public class Main {
    static int di[] = {-1,0,1,0};
    static int dj[] = {0,1,0,-1};
    static int N, M, cnt = 0;
    static int[][] map;
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        map = new int[N][M];
        st = new StringTokenizer(br.readLine()," ");
        int R = Integer.parseInt(st.nextToken());
        int C = Integer.parseInt(st.nextToken());
        int D = Integer.parseInt(st.nextToken());
        for(int i=0; i<N; i++) {
            st = new StringTokenizer(br.readLine()," ");
            for(int j=0; j<M; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }
        dfs(R,C,D);
        System.out.println(cnt);
        br.close();
    }
    static void dfs(int x, int y, int cur_d) {
        if(map[x][y]==1) return;
        else if(map[x][y]==0) {
            map[x][y]--;
            cnt++;
        }
        for(int d=cur_d+3; cur_d<=d; d--) {
            int nd = d % 4;
            int nx = x + di[nd];
            int ny = y + dj[nd];
            if(map[nx][ny]==0) {
                dfs(nx,ny,nd);
                return;
            }
        }
        dfs(x-di[cur_d],y-dj[cur_d],cur_d);
    }
}
```
