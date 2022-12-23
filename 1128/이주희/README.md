# 📘 10775 (이주희)

## 소요시간, 메모리
<img width="80%" alt="image" src="https://user-images.githubusercontent.com/83942393/209319767-5f982f83-f667-4573-8ed5-3e77b581598a.png">

## 풀이 방법
문제 유형 : 그리디, 분리 집합
- 최대한 많은 비행기가 게이트에 도킹하기 위해선 도킹할 수 있는 게이트들 중 큰 번호에 도킹해야 함 -> 그리디
- 도킹할 수 있는 게이트를 찾기 위해서 반복적으로 뒤에서부터 탐색하면 시간 복잡도 증가 -> 비어져 있는 게이트 번호를 parent 배열에 업데이트 -> union-Find 이용

## Code

```Java
import java.io.*;
import java.util.*;

public class Main_bj_10775_공항 {
	
	static int[] parent;

	public static void main(String[] args) throws Exception {
//		System.setIn(new FileInputStream("res/input_bj_10775.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int G = Integer.parseInt(br.readLine());
		int P = Integer.parseInt(br.readLine());
		
		parent = new int[G+1];
		for(int i=1; i<=G; i++)
			parent[i] = i;			// 자기 자신으로 초기화
		
		int ans = 0;
		for(int i=0; i<P; i++) {
//			for(int j=1; j<=G; j++) {
//				System.out.print(parent[j] + " ");
//			}
//			System.out.println();
			
			int g = Integer.parseInt(br.readLine());
			int p = findParent(g);
			
			if(p == 0) break;
			else {
				union(p-1, p);
				ans++;
			}
		}
		
		System.out.println(ans);
		br.close();
	}
	
	static int findParent(int a) {
		if(parent[a] == a) return a;
		return parent[a] = findParent(parent[a]);
	}
	
	static void union(int a, int b) {
		int pa = findParent(a);
		int pb = findParent(b);
		
		parent[pb] = pa;
	}

}

```
