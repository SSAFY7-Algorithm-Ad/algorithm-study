# 📘 10775 (김강호)

## 소요시간, 메모리

204 ms, 22104 KB

## 풀이 방법

- 그리디한 방법으로 가능한 큰번호의 공항부터 접근함.
- 처음엔 번호를 하나씩 줄여가며 모든 경우의 수를 접근해서 시간초과 발생.
- Union & Find 활용하여 접근할 수 있는 공항을 합쳐서 경우의 수를 줄임
- 첫번째 공항처리 및 마지막으로 접근가능한 공항을 처리하는 부분에서 틀림.
  - 단순히 root배열이 바라보는 값이 아닌 find메서드를 사용하여 최상단 값을 찾아서 비교해야함

## Code

```Java
import java.io.*;
public class Main {
    static int[] root;
    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int G = Integer.parseInt(br.readLine());
        int P = Integer.parseInt(br.readLine());
        root = new int[G+1];
        for(int i=0; i<=G; i++) root[i] = i;
        int cnt = 0;
        for(int i=0; i<P; i++) {
            int g = Integer.parseInt(br.readLine());
            if(find(g) == 0) break;
            union(find(g),root[g]-1);
            cnt++;
        }
        System.out.println(cnt);
        br.close();
    }
    static void union(int a, int b) {
        int root_a = find(a);
        int root_b = find(b);
        if(root_a == root_b) return;
        root[a] = root_b;
    }
    static int find(int a) {
        if(root[a]==a) return a;
        return root[a] = find(root[a]);
    }
}
```
