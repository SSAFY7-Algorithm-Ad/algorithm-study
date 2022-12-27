# 📘 1744 (이주희)

## 소요시간, 메모리
<img width="80%" alt="image" src="https://user-images.githubusercontent.com/83942393/209649509-34eb9f95-2c1c-4d23-a170-ea45983d0d76.png">

## 풀이 방법
문제 유형 : 그리디, 정렬 </br></br>
처음엔 정렬해서 큰 수를 두개씩 묶어서 푸는 방법을 생각했다. 그러다 걸리는 게 0 과 1과 음수들이었다. 음수인 경우엔 무조건 음수 또는 0 과 곱해서 상쇄시켜야 한다. 1인 경우는 곱하는 것보다
더하는 게 더 큰 수인 결과값이 나온다.
- 양수를 담는 리스트와 음수와 0을 함께 담는 리스트 두 개를 따로 준비해 담는다.

## Code

```Java
import java.io.*;
import java.util.*;

public class Main_bj_1744_수묶기 {

	public static void main(String[] args) throws Exception {
//		System.setIn(new FileInputStream("res/input_bj_1744.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int N = Integer.parseInt(br.readLine());
		ArrayList<Integer> positive = new ArrayList<>();		// 양수들만 모아놓은 리스트
		ArrayList<Integer> negative = new ArrayList<>();		// 음수, 0 을 모아놓은 리스트
		
		for(int i=0; i<N; i++) {
			int a = Integer.parseInt(br.readLine());
			
			if(a>0) positive.add(a);
			else negative.add(a);
		}
		
		// 내림차순 정렬
//		Collections.sort(positive, Collections.reverseOrder());				1. Collections.reverseOrder 사용
		Collections.sort(positive, ((Integer o1, Integer o2) -> {return -(o1-o2);}));	// 2. 람다식 사용
		
		Collections.sort(negative);		// 오름차순 정렬
		
		int ans = 0;
		
		for(int i=0; i<positive.size(); i++) {
			if((positive.get(i) == 1) || (i == positive.size()-1) || positive.get(i+1) == 1) {
				ans += positive.get(i);
			}
			else {
				ans += (positive.get(i) * positive.get(i+1));
				i++;
			}
		}
		
		for(int i=0; i<negative.size(); i++) {
			if(i == negative.size()-1) {
				ans += negative.get(i);
			}
			else {
				ans += (negative.get(i) * negative.get(i+1));
				i++;
			}
		}
		
		System.out.println(ans);
		br.close();
		
	}

}

```
