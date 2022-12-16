# 📘 1976. 여행 가자 (이주희)

## 소요시간, 메모리
**Floyd** </br>
<img width="80%" alt="image" src="https://user-images.githubusercontent.com/83942393/208032249-e1bcc4c2-4cbb-4fe8-87c1-b566a027150d.png">

**Union-Find** </br>
<img width="80%" alt="image" src="https://user-images.githubusercontent.com/83942393/208032051-72999950-4d53-4c35-9390-15dc771b85c5.png">
</br>

## 풀이 방법
* 처음엔 Floyd 를 사용해서 경로 여부를 판단했다.
* Floyd 에서 출발지와 도착지가 같다면 여행이 가능 하다는 것을 파악하지 못해서 틀렸다.
* 검색해보니 Union-Find 를 사용해서 많이 푼 것 같아 Union-Find 로도 풀었다.
* union 할 때 parent[x] = y; 에서 x 를 x의 부모로 해야 하는 것에서 조금 헤맸다.

## Code
**Floyd** </br>
```Java
import java.io.*;
import java.util.*;

public class Main_bj_1976_여행가자 {

	public static void main(String[] args) throws Exception {
//		System.setIn(new FileInputStream("res/input_bj_1976.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int N = Integer.parseInt(br.readLine());
		int tripN = Integer.parseInt(br.readLine());
		
		int[][] graph = new int[N+1][N+1];
		
		final int INF = 987654321;
		
		StringTokenizer st;
		for(int i=1; i<=N; i++) {
			st = new StringTokenizer(br.readLine(), " ");
			for(int j=1; j<=N; j++) {
				graph[i][j] = Integer.parseInt(st.nextToken());
				if(i==j) graph[i][j] = 1; 		// 출발점과 도착점이 같다면 항상 여행이 가능함
				if(graph[i][j] == 0) graph[i][j] = INF;
			}
		}
		
		// Floyd
		for(int k=1; k<=N; k++) {			// 경
			for(int i=1; i<=N; i++) {		// 출
				for(int j=1; j<=N; j++) {	// 도
					if(graph[i][j] > graph[i][k] + graph[k][j]) {
						graph[i][j] = graph[i][k] + graph[k][j];
					}
				}
			}
		}
		
		st = new StringTokenizer(br.readLine(), " ");
		int from = Integer.parseInt(st.nextToken());	// 첫 번째 출발 도시
		
		boolean isPossible = true;
		for(int i=1; i<tripN; i++) {
			int to = Integer.parseInt(st.nextToken());
			
			if(graph[from][to] >= INF) {
				isPossible = false;
				break;
			}
			from = to;
		}
		
		if(isPossible)
			System.out.println("YES");
		else
			System.out.println("NO");
		
		br.close();
	}

}

```
</br>

**Union-Find** </br>
```Java
import java.io.*;
import java.util.*;

public class Main_bj_1976_여행가자_sol2 {
	
	static int[] parent;

	public static void main(String[] args) throws Exception {
		System.setIn(new FileInputStream("res/input_bj_1976.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int N = Integer.parseInt(br.readLine());
		int tripN = Integer.parseInt(br.readLine());
		
		StringTokenizer st;
		parent = new int[N+1];		// 부모 노드를 저장할 배열
		for(int i=1; i<=N; i++)
			parent[i] = i;			// 자기 자신으로 초기화
		
		for(int i=1; i<=N; i++) {
			st = new StringTokenizer(br.readLine(), " ");
			for(int j=1; j<=N; j++) {
				int value = Integer.parseInt(st.nextToken());
				
				if(value == 1) {
					union(i, j);
				}
			}
		}
		
		st = new StringTokenizer(br.readLine(), " ");
		int from = Integer.parseInt(st.nextToken());
		
		boolean isPossible = true;
		for(int i=1; i<tripN; i++) {
			int to = Integer.parseInt(st.nextToken());
			
			if(findParent(from) != findParent(to)) {
				isPossible = false;
				break;
			}
			
			from = to;
		}
		
		if(isPossible) System.out.println("YES");
		else System.out.println("NO");
		
		br.close();
	}
	
	static int findParent(int n) {
		if(parent[n] == n) return n;
		return parent[n] = findParent(parent[n]);
	}
	
	static void union(int from, int to) {
		int fromParent = findParent(from);
		int toParent = findParent(to);
		
		if(fromParent == toParent) return;
		
		if(fromParent < toParent) parent[toParent] = fromParent;
		else parent[fromParent] = toParent;
	}
}

```
