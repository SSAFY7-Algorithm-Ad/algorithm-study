# 📘 1130 (이주희)

## 소요시간, 메모리
<img width="90%" alt="image" src="https://user-images.githubusercontent.com/83942393/207781286-3a969902-0737-41cf-ad0f-d252ff31c5da.png">

## 풀이 방법
문제 유형 : 수학, 소수 판정

잘못된 방법)
완전 탐색으로 풀 경우 N개의 숫자가 차례로 N-1 (자신을 뺀 나머지) 개의 숫자를 탐색하면서 자신이 배수인지 확인한다면 시간 복잡도 O(N^2)
N의 최댓값은 10만이므로 N^2 라면 100억, 1억당 1초가 걸린다면 소요 시간 10초 -> 제한 시간 2초를 초과

해결 방법)
* 약수의 갯수만큼 머리 톡톡 하므로 약수의 갯수 구하기
* 계산 횟수를 줄이기 위해 제곱근까지만 탐색

## Code
```Java
import java.io.*;
import java.util.Arrays; 

public class Main_bj_1241_머리톡톡 {

	public static void main(String[] args) throws Exception {
//		System.setIn(new FileInputStream("res/input_bj_1241.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int N = Integer.parseInt(br.readLine());
		int[] numbers = new int[N];
		int[] count = new int[1000001];
		
		for(int i=0; i<N; i++) {
			numbers[i] = Integer.parseInt(br.readLine());
			count[numbers[i]]++;
		}
		
		StringBuilder sb = new StringBuilder();
		for(int i=0; i<N; i++) {
			int ans = 0;
			
			// 약수의 갯수만큼 머리 톡톡 -> 약수 구하기
			for(int j=1; j<=Math.sqrt(numbers[i]); j++) {
				if(numbers[i] % j == 0) {
					ans += count[j];
					
					if((numbers[i] / j) != j) {			// 제곱근인 경우 제외(ex)1*1=1)
						ans += count[numbers[i] / j];
					}
				}
			}
			
			ans--;		// 자기 자신 갯수 빼기
			sb.append(ans).append("\n");
		}
		
		System.out.println(sb.toString());
		br.close();
	}
}
```
