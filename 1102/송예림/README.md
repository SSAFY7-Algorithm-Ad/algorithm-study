# 📘 1753

## 소요시간, 메모리
1048ms, 112188KB

## 풀이 방법
하나의 시작점에서 모든 노드로의 최소 비용 경로 -> 다익스트라

Arrays.fill(graph, new ArrayList<>()); -> 이거 왜...? 좀 더 찾아보기ㅠㅠ

## Code

```java
package BAEKJOON;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class gold4_1753_최단경로 {

    static int V, E, start;
    static ArrayList<Node>[] graph;
    static int[] dist;
    static boolean[] visit;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        V = Integer.parseInt(st.nextToken());
        E = Integer.parseInt(st.nextToken());
        start = Integer.parseInt(br.readLine());
        graph = new ArrayList[V+1];
        dist = new int[V+1];
        visit = new boolean[V+1];

        for (int i = 1; i <= V; i++) {
            graph[i] = new ArrayList<>();
        }
        Arrays.fill(dist, Integer.MAX_VALUE);

        for (int i = 0; i < E; i++) {
            st = new StringTokenizer(br.readLine());
            int iu = Integer.parseInt(st.nextToken());
            int iv = Integer.parseInt(st.nextToken());
            int iw = Integer.parseInt(st.nextToken());
            graph[iu].add(new Node(iv, iw));
        }

        dijkstra();

        for (int i = 1; i < dist.length; i++) {
            System.out.println(dist[i]==Integer.MAX_VALUE?"INF":dist[i]);
        }
    }

    private static void dijkstra() {
        PriorityQueue<Node> queue = new PriorityQueue<>();
        dist[start] = 0;
        queue.add(new Node(start, dist[start]));

        while(!queue.isEmpty()) {
            Node current = queue.poll();
            // 현재꺼 방문처리
            visit[current.v] = true;

            for (int i = 0; i < graph[current.v].size(); i++) {
                Node next = graph[current.v].get(i);
                // 다음 노드를 방문하지 않았고, 현재 노드를 거치고 다음 노드로 이동하는 경우가 더 작을 경우
                if(!visit[next.v] && dist[next.v] > current.w + next.w) {
                    dist[next.v] = current.w + next.w;
                    queue.add(new Node(next.v, dist[next.v]));
                }
            }
        }
    }

    static class Node implements Comparable<Node>{
        int v, w;

        public Node(int v, int w) {
            this.v = v;
            this.w = w;
        }

        @Override
        public int compareTo(Node o) {
            return Integer.compare(this.w, o.w);
        }
    }
}


```