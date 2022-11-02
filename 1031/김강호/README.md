# 📘 1956

## 소요시간, 메모리

1600ms, 58044KB

## 풀이 방법

## Code

```java
import java.util.*;
import java.io.*;
public class Main {
    static boolean[] visited;
    static int V,res=Integer.MAX_VALUE;
    static int[][] edge;
    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");
        V = Integer.parseInt(st.nextToken());
        int E = Integer.parseInt(st.nextToken());
        edge = new int[V][V];
        for(int i=0; i<E; i++) {
            st = new StringTokenizer(br.readLine()," ");
            int f = Integer.parseInt(st.nextToken())-1;
            int t = Integer.parseInt(st.nextToken())-1;
            int w = Integer.parseInt(st.nextToken());
            edge[f][t] = w;
        }
        for(int i=0; i<V; i++) {
            visited = new boolean[V];
            start(i,i,0);
        }
        if(res==Integer.MAX_VALUE) System.out.println(-1);
        else System.out.println(res);
    }
    static void start(int st, int cur, int sum) {
        if(visited[cur]) {
            if(st == cur) {
                res = Math.min(res, sum);
                return;
            }
            return;
        }
        visited[cur] = true;
        for(int i=0; i<V; i++) {
            if(edge[cur][i]>0) {
                if(res<=sum+edge[cur][i]) continue;
                start(st,i,sum+edge[cur][i]);
            }
        }
        visited[cur]=false;
    }
}
```
