# 📘 1916 (이주희)

## 소요시간, 메모리
![3](https://user-images.githubusercontent.com/83942393/200139056-7cced138-8473-4484-bba7-a4244ab294e1.PNG)

## 풀이 방법
* 한 정점에서 다른 정점까지의 최단 거리를 구하는 문제 -> Dijkstra

## Code
```Java
import java.io.*;
import java.util.Arrays;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class Main_bj_1916_최소비용구하기 {
	static class Node {
		int vertex, weight;
		Node link;
		
		public Node(int vertex, int weight, Node link) {
			this.vertex = vertex;
			this.weight = weight;
			this.link = link;
		}
	}

	public static void main(String[] args) throws Exception{
		System.setIn(new FileInputStream("res/input_bj_1916.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int V = Integer.parseInt(br.readLine());
		int E = Integer.parseInt(br.readLine());
		
		Node[] adjList = new Node[V+1];
		
		StringTokenizer st;
		for(int i=0; i<E; i++) {
			st = new StringTokenizer(br.readLine(), " ");
			int from = Integer.parseInt(st.nextToken());
			int to = Integer.parseInt(st.nextToken());
			int w = Integer.parseInt(st.nextToken());
			
			adjList[from] = new Node(to, w, adjList[from]);
		}
		
		st = new StringTokenizer(br.readLine(), " ");
		int start = Integer.parseInt(st.nextToken());
		int end = Integer.parseInt(st.nextToken());
		
		// dijkstra
		int[] dist = new int[V+1];
		Arrays.fill(dist, Integer.MAX_VALUE);
		dist[start] = 0;
		PriorityQueue<int[]> pq = new PriorityQueue<>((o1, o2) -> (o1[1] - o2[1]));
		pq.offer(new int[] {start, 0});
		
		while(!pq.isEmpty()) {
			int[] now = pq.poll();
			if(dist[now[0]] < now[1]) continue;
			if(now[0] == end) break;
			
			for(Node temp = adjList[now[0]]; temp != null; temp = temp.link) {
				if(dist[temp.vertex] > dist[now[0]] + temp.weight) {
					dist[temp.vertex] = dist[now[0]] + temp.weight;
					pq.offer(new int[] {temp.vertex, dist[temp.vertex]});
				}
			}
		}
		
		System.out.println(dist[end]);
		br.close();
	}
}

```
