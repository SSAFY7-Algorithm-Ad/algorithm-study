# 📘 1753

## 소요시간, 메모리

2536ms, 115252KB

## 풀이 방법

## Code

```java
import java.io.*;
import java.util.*;
public class Main {
	public static void main(String[] args)throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringBuilder sb = new StringBuilder();
		StringTokenizer st = new StringTokenizer(br.readLine()," ");
		int V = Integer.parseInt(st.nextToken());
		int E = Integer.parseInt(st.nextToken());
		int K = Integer.parseInt(br.readLine());

		List<int[]>[] g = new List[V+1];
		for(int i=0; i<V+1; i++) g[i]=new ArrayList<int[]>();
		int[] distance = new int[V+1];
		for(int i=1; i<V+1; i++) distance[i]=Integer.MAX_VALUE;
		boolean[] v = new boolean[V+1];
		for(int i=0; i<E; i++) {
			st = new StringTokenizer(br.readLine()," ");
			int from = Integer.parseInt(st.nextToken());
			int to = Integer.parseInt(st.nextToken());
			int w = Integer.parseInt(st.nextToken());
			g[from].add(new int[] {to,w});
		}
		distance[K]=0;
		for(int i=1; i<V+1; i++) {
			int min=Integer.MAX_VALUE;
			int cur=1;
			for(int j=1; j<V+1; j++) {
				if(!v[j] && min>distance[j]) {
					min=distance[j];
					cur=j;
				}
			}
			v[cur]=true;
			for (int[] j:g[cur]) {
				if(!v[j[0]] && distance[j[0]]>distance[cur]+j[1]) {
							   distance[j[0]]=distance[cur]+j[1];
				}
			}
		}
		for(int i=1; i<V+1; i++) {
			if(distance[i]==Integer.MAX_VALUE) sb.append("INF").append("\n");
			else sb.append(distance[i]).append("\n");
		}
		System.out.println(sb);
		br.close();
	}
}
```
