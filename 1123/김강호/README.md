# 📘 1744 (김강호)

## 소요시간, 메모리

220 ms, 18240 KB

## 풀이 방법

- 우선순위 큐 활용

## Code

```Java
import java.util.*;
import java.io.*;
public class Main {
    public static void main(String[] agrs) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        PriorityQueue<Integer> pq_plus = new PriorityQueue<>((o1,o2)-> {return o2-o1;});
        PriorityQueue<Integer> pq_minus = new PriorityQueue<>();
        int zero_cnt = 0;
        int one_cnt = 0;
        for(int i=0; i<N; i++) {
            int n = Integer.parseInt(br.readLine());
            if(1<n) pq_plus.offer(n);
            else if(n<0) pq_minus.offer(n);
            else if(n==1) one_cnt++;
            else zero_cnt++;
        }
        int sum = 0;
        while(pq_minus.size()>1) sum += pq_minus.poll() * pq_minus.poll();
        if(pq_minus.size()==1 && zero_cnt>0) pq_minus.poll();
        while(pq_plus.size()>1) sum += pq_plus.poll() * pq_plus.poll();
        while(!pq_minus.isEmpty()) sum += pq_minus.poll();
        while(!pq_plus.isEmpty()) sum+= pq_plus.poll();
        sum+=one_cnt;
        System.out.println(sum);
        br.close();
    }
}
```
