# 📘 1102 (박태이)

## 소요시간, 메모리
- 111720 KB
- 952 ms

## 풀이 방법
- 우선순위 큐를 활용하여 거리가 가장 짧은 노드 순으로 poll 될 수 있도록 구현하였습니다.
- 물론 기억이 하나도 나지 않아서 학습 + 구글링의 도움을 받아 풀었습니다!

## Code
```java
import java.io.*;
import java.util.*;

class Node implements Comparable<Node> {
    int end;
    int weight;

    public Node(int end, int weight) {
        this.end = end;
        this.weight = weight;
    }

    @Override
    public int compareTo(Node o) {
        return weight - o.weight; // 음수여야 앞에 배치
    }
}

public class DAY221102_BOJ1753_G4_최단경로 {
    static int V, E;
    static List<Node>[] list;

    static int[] dist;
    static boolean[] visited;
    static int INF = 987654321;

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        // 1. 입력
        V = Integer.parseInt(st.nextToken());
        E = Integer.parseInt(st.nextToken());

        int start = Integer.parseInt(br.readLine());

        // 2. 무한으로 초기화
        dist = new int[V + 1];
        Arrays.fill(dist, INF);

        // 3. 정보 세팅
        list = new ArrayList[V + 1];
        visited = new boolean[V + 1];
        for (int i = 1; i <= V; i++) {
            list[i] = new ArrayList<>();
        }

        for (int i = 0; i < E; i++) {
            st = new StringTokenizer(br.readLine());
            int s = Integer.parseInt(st.nextToken());
            int e = Integer.parseInt(st.nextToken());
            int w = Integer.parseInt(st.nextToken());

            list[s].add(new Node(e, w));
        }

        dijkstra(start);

        // dist[] 갱신 끝

        for (int i = 1; i <= V; i++) {
            if (dist[i] == INF) {
                System.out.println("INF");
            } else {
                System.out.println(dist[i]);
            }
        }

    }

    private static void dijkstra(int start) {
        // 1. 시작점은 0으로 세팅
        dist[start] = 0;

        // 2. 우선순위 큐
        PriorityQueue<Node> q = new PriorityQueue<>();
        // 시작 노드에서 시작 노드로 가는 최단 거리는 0이다 로 세팅하고 시작!
        q.add(new Node(start, 0));

        while (!q.isEmpty()) {
            Node node = q.poll(); // weight 가 제일 작은 친구부터 나옴
            int curr = node.end;

            // 방문했는지 체크
            if (visited[curr]) continue;
            visited[curr] = true;

            // curr이 시작점, n.end가 도착점
            for (Node n: list[curr]) {
                // 그래서 n.end의 거리가 curr의 거리 + n.weight 보다 크다면 갱신
                if (dist[n.end] > dist[curr] + n.weight) {
                    dist[n.end] = dist[curr] + n.weight;
                    q.add(new Node(n.end, dist[n.end]));
                }
            }
        }
    }
}

```
