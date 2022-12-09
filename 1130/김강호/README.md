# 📘 1241 (김강호)

## 소요시간, 메모리

400 ms, 28708 KB

## 풀이 방법

- 소수를 판별할때 모든 수를 탐색하지 않고, 제곱근 값까지만 탐색하여 해결

## Code

```Java
import java.io.*;
public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();
        int N = Integer.parseInt(br.readLine());
        int[] arr = new int[N];
        int[] cnt = new int[1000001];
        int[] res = new int[N];
        for(int i=0; i<N; i++) {
            arr[i] = Integer.parseInt(br.readLine());
            cnt[arr[i]]++;
        }
        for(int i=0; i<N; i++) {
            for(int j=1; j*j<=arr[i]; j++) {
                    if(arr[i]%j==0) {
                        res[i] += cnt[j];
                        if(arr[i]/j!=j) res[i] += cnt[arr[i]/j];
                }
            }
        }
        for(int i=0; i<N; i++) sb.append(res[i]-1).append('\n');
        System.out.println(sb);
        br.close();
    }
}
```
