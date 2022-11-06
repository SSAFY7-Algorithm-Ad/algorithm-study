# 📘 1916

## 소요시간, 메모리
552ms, 51844KB

## 풀이 방법
- 하나의 시작점에서 모든 노드로의 최소 비용 경로 -> 다익스트라
- 모든 노드로 도착하는 경로 구한 후 결과 출력

⛔ 시간초과
- 현재 탐색할 노드의 방문 배열이 true인 것도 탐색하게 했었음
- 방문했던 노드는 탐색하지 못하도록 수정


## Code

```java
package BAEKJOON;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class gold5_1916_최소비용구하기 {

    static int N, M, start, end, result=Integer.MAX_VALUE;
    static ArrayList<Node>[] graph;
    static int[] dist;
    static boolean[] visit;

    public static void main(String[] args) throws NumberFormatException, IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        N = Integer.parseInt(br.readLine());
        M = Integer.parseInt(br.readLine());
        graph = new ArrayList[N+1];
        dist = new int[N+1];
        visit = new boolean[N+1];

        for (int i = 0; i <= N; i++) {
            graph[i] = new ArrayList<>();
            dist[i] = Integer.MAX_VALUE;
        }

        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());
            int iu = Integer.parseInt(st.nextToken());
            int iv = Integer.parseInt(st.nextToken());
            int iw = Integer.parseInt(st.nextToken());
            graph[iu].add(new Node(iv, iw));
        }

        st = new StringTokenizer(br.readLine());
        start = Integer.parseInt(st.nextToken());
        end = Integer.parseInt(st.nextToken());

        dijkstra();

        System.out.println(dist[end]);
    }

    private static void dijkstra() {
        PriorityQueue<Node> queue = new PriorityQueue<>();
        dist[start] = 0;
        queue.add(new Node(start, dist[start]));

        while(!queue.isEmpty()) {
            Node current = queue.poll();
            if(!visit[current.v]) {
                visit[current.v] = true;

                for (Node next : graph[current.v]) {
                    if(!visit[next.v] && dist[next.v] > current.w + next.w) {
                        dist[next.v] = current.w + next.w;
                        queue.add(new Node(next.v, dist[next.v]));
                    }
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