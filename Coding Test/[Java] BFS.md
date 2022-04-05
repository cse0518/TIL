## BFS - 배열
```java
import java.util.*;

public class Main {
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int[][] graph;
    static boolean[][] visited;
    static int lines;

    public static void main(String[] args) {
        final Scanner scanner = new Scanner(System.in);
        lines = scanner.nextInt();
        graph = new int[lines][lines];
        visited = new boolean[lines][lines];

        for (int i = 0; i < lines; i++) {
            final int source = scanner.nextInt();
            final int target = scanner.nextInt();
            for (int j = 0; j < lines; j++) {
                graph[source - 1][target - 1] = 1;
                graph[target - 1][source - 1] = 1;
            }
        }
        scanner.close();

        bfs(0, 0);
        for (final int answer : answers) {
            System.out.println(answer);
        }
    }

    // BFS 함수 정의
    public static void bfs(int startRow, int startColumn) {
        // 연결된 노드의 개수 count
        int count = 0;

        Queue<int[]> q = new LinkedList<>();

        // 시작 노드를 큐에 추가
        q.offer(new int[]{startRow, startColumn});
        count++;

        // 현재 노드를 방문 처리
        visited[startRow][startColumn] = true;

        // 큐가 빌 때까지 반복
        while(!q.isEmpty()) {
            // 큐에서 하나의 원소를 poll
            final int[] poll = queue.poll();
            final int nowX = poll[0];
            final int nowY = poll[1];

            // 주변 노드들을 체크
            for (int k = 0; k < 4; k++) {
                final int nextX = nowX + dx[k];
                final int nextY = nowY + dy[k];

                if (nextX < 0 | nextX >= lines | nextY < 0 | nextY >= lines) continue;
                if (graph[nextX][nextY] == 0 | visited[nextX][nextY]) continue;
                visited[nextX][nextY] = true;
                queue.add(new int[]{nextX, nextY});
                count++;
            }
        }

        // 연결된 노드 개수 출력
        System.out.println(count);
    }
}
```
