# 📘 2448 (이주희)

## 소요시간, 메모리
<img width="70%" alt="image" src="https://user-images.githubusercontent.com/83942393/208051673-96f2ea7d-b79d-4d20-8465-2b78b2040be5.png">

## 풀이 방법
- 강호님꺼 보고 풀었습니다^-^b
- k=0 (N=3) 일 땐 기본 블록
```
  *  
 * *
*****
```
- 규칙을 찾자면 크게 2줄로 구분됨
- 첫 번째 줄은 중앙에 한 개의 블록, 두 번째 줄은 빈칸을 사이에 둔 두 개의 블록

```
     *        
    * *        
   *****     
  *     *    
 * *   * *  
***** *****
```
- 두 번째 줄은 블록을 한 개 삽입하고, 빈칸 삽입하고, 블록 한 개 삽입
- 첫 번쨰 줄은 블록을 중앙에 놓기 위해 블록 앞과 뒤에 빈칸을 삽입 (빈칸의 수 = N/2개)
- 현재 만들어진 전체 그림이 다음 재귀에서 한 개의 부분 불록이 된다.

## Code

```Java
import java.io.*;
import java.util.*;

public class Main_bj_2448_별찍기11 {
	static int N;
	
	public static void main(String[] args) throws Exception {
		Scanner sc = new Scanner(System.in);
		N = Integer.parseInt(sc.next());
		StringBuilder[] sb = new StringBuilder[N];
		for(int i=0; i<N; i++)
			sb[i] = new StringBuilder();
		sb[0].append("  *  ");
		sb[1].append(" * * ");
		sb[2].append("*****");
		
		if(N>3) sb = star(1, sb);
		
		for(int i=0; i<N; i++)
			System.out.println(sb[i]);
		sc.close();
	}
	
	static StringBuilder[] star(int k, StringBuilder[] sb) {
		int n = (int) (3 * Math.pow(2, k));
		if(n<=N) {
			int idx = 0;
			for(int i=n/2; i<n; i++) {
				sb[i].append(sb[idx]).append(" ").append(sb[idx++]);
			}
			
			StringBuilder sbb = new StringBuilder();
			for(int i=0; i<n/2; i++)
				sbb.append(" ");
			
			for(int i=0; i<n/2; i++) {
				sb[i].insert(0, sbb);
				sb[i].append(sbb);
			}
			
			return star(k+1, sb);
		}
		
		return sb;
	}

}
```
