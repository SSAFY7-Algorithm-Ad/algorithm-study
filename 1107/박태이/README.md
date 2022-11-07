# 📘 1107 (박태이)

## 소요시간, 메모리
- 14360 KB
- 128 ms

## 풀이 방법
- bfs를 사용하여 최소로 이동하는 cnt를 구하고자 했습니다.
- 빨간 구슬과 파란 구슬이 움직인 위치를 같이 저장하고자 클래스를 한꺼번에 구현했습니다.
- 절대 제 힘으로 풀 수 없는 문제라서 또 구글링을 해서 풀었습니다. 며칠 뒤에 다시 한번 풀어봐야겠습니다.
- 구현 문제를 많이 풀어야겠습니다..
- 도대체 이거 그냥 푸시는 분은 .. 어떤 분이신지.. 대단..

## Code
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class DAY221107_BOJ13460_G1_구슬탈출2 {
    static int N, M;
    static char[][] map;
    static Marble blue, red;
    static int hole_x, hole_y;
    static boolean[][][][] visited;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken()); // 세로
        M = Integer.parseInt(st.nextToken()); // 가로

        map = new char[N][M];
        visited = new boolean[N][M][N][M]; // 방문 처리할 배열을... 4차원으로...

        // 보드 세팅
        for (int i = 0; i < N; i++) {
            String s = br.readLine();
            for (int j = 0; j < M; j++) {
                char c = s.charAt(j);
                map[i][j] = c;
                if (c == 'B') {
                    blue = new Marble(0, 0, i, j, 0);
                } else if (c == 'R') {
                    red = new Marble(i, j, 0, 0, 0);
                } else if (c == 'O') {
                    hole_x = i;
                    hole_y = j;
                }
            }
        }

        System.out.println(bfs());

        br.close();
    }

    // 위 오 아 왼
    static int[] dr = {-1, 0, 1, 0};
    static int[] dc = {0, 1, 0, -1};

    private static int bfs() {
        Queue<Marble> queue = new LinkedList<>();
        queue.add(new Marble(red.red_x, red.red_y, blue.blue_x, blue.blue_y, 1)); // cnt를 1을 넣어주는 이유는?
        visited[red.red_x][red.red_y][blue.blue_x][blue.blue_y] = true;

        while (!queue.isEmpty()) {
            Marble marble = queue.poll();

            int curr_red_x = marble.red_x;
            int curr_red_y = marble.red_y;
            int curr_blue_x = marble.blue_x;
            int curr_blue_y = marble.blue_y;
            int curr_cnt = marble.cnt;

            if (curr_cnt > 10) return -1;

            for (int d = 0; d < 4; d++) {
                int new_red_x = curr_red_x;
                int new_red_y = curr_red_y;
                int new_blue_x = curr_blue_x;
                int new_blue_y = curr_blue_y;

                boolean isRedHole = false;
                boolean isBlueHole = false;

                // 빨간 구슬 이동
                // 이동한 곳이 #이 아니라면
                while (map[new_red_x + dr[d]][new_red_y + dc[d]] != '#') {
                    // 위치 갱신
                    new_red_x += dr[d];
                    new_red_y += dc[d];

                    // 근데 그 위치를 갱신한 곳이 구멍이라면?
//                    if (map[new_red_x][new_red_y] == 'O') {
                    if (new_red_x == hole_x && new_red_y == hole_y) {
                        isRedHole = true;
                        break;
                    }
                }

                // 파란 구슬도 동시에 이동할거니까
                while (map[new_blue_x + dr[d]][new_blue_y + dc[d]] != '#') {
                    new_blue_x += dr[d];
                    new_blue_y += dc[d];

//                    if (map[new_blue_x][new_blue_y] == 'O') {
                    if (new_blue_x == hole_x && new_blue_y == hole_y) {
                        isBlueHole = true;
                        break;
                    }
                }

                if (isBlueHole) continue;

                if (isRedHole) return curr_cnt;

                // 둘다 구멍에 빠지지 않고 이동했는데 그 자리가 겹치는 경우
                // 위치 조정이 필요함
                if(new_red_x == new_blue_x && new_red_y == new_blue_y) {
                    // 위
                    if (d == 0) {
                        // 이동하기 전에 더 위에 있던 게 더 위에 있도록 해야지
                        if (curr_blue_x < curr_red_x) {
                            new_red_x -= dr[d];
                        } else {
                            new_blue_x -= dr[d];
                        }
                    } else if (d == 1) { // 오른쪽
                        // y값이 더 큰게 더 오른쪽에 있어야 함
                        if (curr_blue_y > curr_red_y) {
                            new_red_y -= dc[d];
                        } else {
                            new_blue_y -= dc[d];
                        }
                    } else if (d == 2) { // 아래
                        // 값이 더 작은 게 더 위에 있어야 함
                        if (curr_red_x < curr_blue_x) {
                            new_red_x -= dr[d];
                        } else {
                            new_blue_x -= dr[d];
                        }
                    } else { // 왼쪽
                        // y이 값이 더 작은 게 더 왼쪽에 있어야함
                        // 큰 걸 빼야함
                        if (curr_red_y > curr_blue_y) {
                            new_red_y -= dc[d];
                        } else {
                            new_blue_y -= dc[d];
                        }
                    }
                }

                // 위치 조정 후 이동한 곳이 처음 방문한 곳이라면 큐에 넣기
                if (!visited[new_red_x][new_red_y][new_blue_x][new_blue_y]) {
                    visited[new_red_x][new_red_y][new_blue_x][new_blue_y] = true;
                    queue.add(new Marble(new_red_x, new_red_y, new_blue_x, new_blue_y, curr_cnt + 1));
                }

            }

        }
        // 이렇게 해도 아무 것도 안나와?
        return -1;
    }
}

// Marble 클래스 하나에 빨간 공, 파란 공의 좌표를 모두 담는 이유는 뭘까?
// 한방에 움직이려고
class Marble {
    int red_x;
    int red_y;
    int blue_x;
    int blue_y;
    int cnt;

    public Marble(int red_x, int red_y, int blue_x, int blue_y, int cnt) {
        this.red_x = red_x;
        this.red_y = red_y;
        this.blue_x = blue_x;
        this.blue_y = blue_y;
        this.cnt = cnt;
    }
}
```
