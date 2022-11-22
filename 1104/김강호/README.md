# 📘 1916 (김강호)

## 소요시간, 메모리

460ms, 51852KB

## 풀이 방법

다익스트라 알고리즘 활용해서 품.

## Code

```Java
import java.io.*;
import java.util.*;
public class Main {
    static class Node implements Comparable<Node> {
        int idx;
        int cost;
        public Node(int i, int c) {
            idx = i;
            cost = c;
        }

        @Override
        public int compareTo(Node o) {
            return cost-o.cost;
        }
    }
    static List<Node>[] bus;
    static int[] dist;
    static int N;
    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        N = Integer.parseInt(br.readLine());
        int M = Integer.parseInt(br.readLine());
        bus = new ArrayList[N+1];
        dist = new int[N+1];
        for(int i=0; i<=N; i++) {
            bus[i] = new ArrayList<>();
            dist[i] = Integer.MAX_VALUE;
        }
        for(int i=0; i<M; i++) {
            st = new StringTokenizer(br.readLine()," ");
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int cost = Integer.parseInt(st.nextToken());
            bus[a].add(new Node(b,cost));
        }
        st = new StringTokenizer(br.readLine()," ");
        int ST = Integer.parseInt(st.nextToken());
        int END = Integer.parseInt(st.nextToken());
        dijkstra(ST);
        System.out.println(dist[END]);
        br.close();
    }
    static void dijkstra(int ST) {
        PriorityQueue<Node> que = new PriorityQueue<>();
        boolean[] visit = new boolean[N+1];
        que.offer(new Node(ST,0));
        visit[ST] = true;
        dist[ST] = 0;
        while (!que.isEmpty()) {
            Node bs = que.poll();
            if(dist[bs.idx]<bs.cost) continue;
            if(!visit[bs.idx]) visit[bs.idx] =true;
            for(Node next: bus[bs.idx]) {
                if(!visit[next.idx] && dist[next.idx] > next.cost + bs.cost) {
                    dist[next.idx] = next.cost + bs.cost;
                    que.offer(new Node(next.idx,dist[next.idx]));
                }
            }
        }
    }
}

```
