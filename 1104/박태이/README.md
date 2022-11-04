# 📘 1104 (박태이)

## 소요시간, 메모리
- 51172 KB
- 548 ms

## 풀이 방법
- 11월 2일에 풀었던 문제처럼 우선순위 큐를 활용하여 거리가 가장 짧은 노드 순으로 poll 될 수 있도록 구현하였습니다.
- 스스로의 힘으로 풀면서 구현이 되지 않는 부분에 있어 조금 더 이해할 수 있는 계기가 되었습니다. 다익스트라 문제를 계속 풀면서 감을 잡아야겠어요!

## Code
```java
import java.io.*;
import java.util.*;

class Node implements Comparable<Node>{
    int end;
    int weight;

    public Node(int end, int weight) {
        this.end = end;
        this.weight = weight;
    }

    @Override
    public int compareTo(Node node) {
        return this.weight - node.weight;
    }
}

public class DAY221104_BOJ1916_G5_최소비용구하기 {
    public static int V, E;
    public static List<Node>[] map;
    public static int[] dist;
    public static boolean[] visited;
    public static int INF = 987654321;
    public static int start, end;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        V = Integer.parseInt(st.nextToken());
        E = Integer.parseInt(br.readLine());

        map = new ArrayList[V + 1];
        for (int i = 1; i <= V; i++) {
            map[i] = new ArrayList<>();
        }

        for (int i = 0; i < E; i++) {
            st = new StringTokenizer(br.readLine());
            int s = Integer.parseInt(st.nextToken());
            int e = Integer.parseInt(st.nextToken());
            int d = Integer.parseInt(st.nextToken());

            // s에서 출발하는 버스는 e까지 d만큼의 거리가 걸림
            map[s].add(new Node(e, d));
        }

        st = new StringTokenizer(br.readLine());
        start = Integer.parseInt(st.nextToken());
        end = Integer.parseInt(st.nextToken());

        // TODO 배열의 인덱스와 도시 번호를 동일하게 가져갈 거라서 +1로 생성해야하는 것을 놓침
        dist = new int[V + 1];
        visited = new boolean[V + 1];
        Arrays.fill(dist, INF);

        // 최소 거리로 갱신시키고 오기
        dijkstra(start);

        System.out.println(dist[end]);
    }

    public static void dijkstra(int start) {
        // 시작점을 0으로 초기화
        dist[start] = 0;

        // 거리가 가장 짧은 곳부터 시작해서 우선순위 큐에 담기
        PriorityQueue<Node> q = new PriorityQueue<>();
        // 시작점에서 시작점으로 가는 게 제일 가까우니까 담는 거! 까먹지 말기
        q.add(new Node(start, 0));

        while (!q.isEmpty()) {
            Node node = q.poll();
            int curr = node.end;
            if (visited[curr]) continue;
            visited[curr] = true;

            // 갱신
            for (Node n : map[curr]) {
                if (dist[n.end] > dist[curr] + n.weight) {
                    dist[n.end] = dist[curr] + n.weight;
                }
                q.add(new Node(n.end, dist[n.end]));
            }
        }
    }
}
```
