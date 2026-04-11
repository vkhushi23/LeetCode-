# LeetCode-
my solution 

Hard qusetion :
 3651 - Minimum Cost Path with Teleporations
 solution in java 

 class Solution {

    static class State {
        int r, c, t;
        long cost;

        State(int r, int c, int t, long cost) {
            this.r = r;
            this.c = c;
            this.t = t;
            this.cost = cost;
        }
    }

    public int minCost(int[][] grid, int k) {   // return type FIXED
        int m = grid.length, n = grid[0].length;

        long INF = Long.MAX_VALUE / 4;
        long[][][] dist = new long[m][n][k + 1];

        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                Arrays.fill(dist[i][j], INF);

        PriorityQueue<State> pq = new PriorityQueue<>(
            (a, b) -> Long.compare(a.cost, b.cost)
        );

        dist[0][0][0] = 0;
        pq.offer(new State(0, 0, 0, 0));

        List<int[]> cells = new ArrayList<>();
        for (int i = 0; i < m; i++)
            for (int j = 0; j < n; j++)
                cells.add(new int[]{grid[i][j], i, j});

        cells.sort(Comparator.comparingInt(a -> a[0]));

        while (!pq.isEmpty()) {
            State cur = pq.poll();
            if (cur.cost > dist[cur.r][cur.c][cur.t]) continue;

            if (cur.r == m - 1 && cur.c == n - 1)
                return (int) cur.cost;   //  safe cast

            // Right
            if (cur.c + 1 < n) {
                long nc = cur.cost + grid[cur.r][cur.c + 1];
                if (nc < dist[cur.r][cur.c + 1][cur.t]) {
                    dist[cur.r][cur.c + 1][cur.t] = nc;
                    pq.offer(new State(cur.r, cur.c + 1, cur.t, nc));
                }
            }

            // Down
            if (cur.r + 1 < m) {
                long nc = cur.cost + grid[cur.r + 1][cur.c];
                if (nc < dist[cur.r + 1][cur.c][cur.t]) {
                    dist[cur.r + 1][cur.c][cur.t] = nc;
                    pq.offer(new State(cur.r + 1, cur.c, cur.t, nc));
                }
            }

            // Teleport
            if (cur.t < k) {
                for (int[] cell : cells) {
                    if (cell[0] > grid[cur.r][cur.c]) break;
                    int nr = cell[1], nc = cell[2];
                    if (cur.cost < dist[nr][nc][cur.t + 1]) {
                        dist[nr][nc][cur.t + 1] = cur.cost;
                        pq.offer(new State(nr, nc, cur.t + 1, cur.cost));
                    }
                }
            }
        }

        return -1;
    }
}

<br>add new qustion 
