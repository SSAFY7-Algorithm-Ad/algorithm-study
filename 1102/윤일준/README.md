# 📘 1753

## 소요시간, 메모리
1048ms, 109900KB
## 풀이 방법

## Code

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class Main {

    static int V, E, start;
    static int INF = (int) 1e9;
    static StringTokenizer st;
    static int[] arr;
    static boolean[] visit;
    static ArrayList<ArrayList<Node>> list = new ArrayList<ArrayList<Node>>();
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

    static class Node implements Comparable<Node> {

        int index;
        int distance;

        Node(int index, int distance) {
            this.index = index;
            this.distance = distance;
        }

        @Override
        public int compareTo(Node o) {
            if (this.distance < o.distance)
                return -1;
            return 1;
        }

    }

    public static void main(String[] args) throws IOException {
        st = new StringTokenizer(br.readLine());
        V = Integer.parseInt(st.nextToken());
        E = Integer.parseInt(st.nextToken());

        // 최단거리 테이블
        arr = new int[V + 1];
        start = Integer.parseInt(br.readLine());

        // 초기화
        for (int i = 0; i < V + 1; i++) {
            list.add(new ArrayList<Node>());
        }

        for (int i = 0; i < E; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            int c = Integer.parseInt(st.nextToken());

            list.get(a).add(new Node(b, c));
        }

        Arrays.fill(arr, INF);

        dijkstra(start);

        for (int i = 1; i <= V; i++) {
            if (arr[i] == INF) {
                System.out.println("INF");
                continue;
            }
            System.out.println(arr[i]);
        }
    }

    private static void dijkstra(int start) {
        PriorityQueue<Node> pq = new PriorityQueue<>();

        pq.offer(new Node(start, 0));
        arr[start] = 0;
        while (!pq.isEmpty()) {
            Node node = pq.poll();
            int dist = node.distance;
            int now = node.index;

            if (arr[now] < dist)
                continue;

            for (int i = 0; i < list.get(now).size(); i++) {
                int cost = arr[now] + list.get(now).get(i).distance;

                if (cost < arr[list.get(now).get(i).index]) {
                    arr[list.get(now).get(i).index] = cost;
                    pq.offer(new Node(list.get(now).get(i).index, cost));
                }
            }
        }

    }
}

```