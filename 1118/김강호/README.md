# 📘 2448 (김강호)

## 소요시간, 메모리

592 ms, 134644 KB

## 풀이 방법

- 재귀호출 활용하여 품

## Code

```Java
import java.io.*;
public class Main {
    static int N;
    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());
        StringBuilder[] sb = new StringBuilder[N];
        for(int i=0; i<N; i++) sb[i] = new StringBuilder();
        sb[0].append("  *  ");
        sb[1].append(" * * ");
        sb[2].append("*****");
        if(N!=3) sb = star(1,sb);
        for(int i=0; i<N; i++) System.out.println(sb[i]);
        br.close();
    }
    static StringBuilder[] star(int k, StringBuilder[] sb) {
        int n = (int)(3*Math.pow(2,k));
        if(n <= N) {
            int idx = 0;
            for(int i=n/2; i<n; i++) sb[i].append(sb[idx]).append(" ").append(sb[idx++]);
            for(int i=0; i<n/2; i++) {
                StringBuilder sbb = new StringBuilder();
                int nn = (int)(3*Math.pow(2,k-1));
                for(int j=0; j<nn; j++) sbb.append(" ");
                sb[i].insert(0,sbb);
                sb[i].append(sbb);
            }
            return star(k+1,sb);
        }
        else return sb;
    }
}
```
