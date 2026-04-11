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
1611 - minimum one bit operation to make integers zero <br>

solution<br>
class Solution {
  public int minimumOneBitOperations(int n) {
    // Observation: e.g. n = 2^2
    //        100 (2^2 needs 2^3 - 1 ops)
    // op1 -> 101
    // op2 -> 111
    // op1 -> 110
    // op2 -> 010 (2^1 needs 2^2 - 1 ops)
    // op1 -> 011
    // op2 -> 001 (2^0 needs 2^1 - 1 ops)
    // op1 -> 000
    //
    // So 2^k needs 2^(k + 1) - 1 ops. Note this is reversible, i.e., 0 -> 2^k
    // also takes 2^(k + 1) - 1 ops.

    // e.g. n = 1XXX, our first goal is to change 1XXX -> 1100.
    //   - If the second bit is 1, you only need to consider the cost of turning
    //     the last 2 bits to 0.
    //   - If the second bit is 0, you need to add up the cost of flipping the
    //     second bit from 0 to 1.
    // XOR determines the cost minimumOneBitOperations(1XXX^1100) accordingly.
    // Then, 1100 -> 0100 needs 1 op. Finally, 0100 -> 0 needs 2^3 - 1 ops.
    if (n == 0)
      return 0;
    // x is the largest 2^k <= n.
    // x | x >> 1 -> x >> 1 needs 1 op.
    //     x >> 1 -> 0      needs x = 2^k - 1 ops.
    int x = 1;
    while (x * 2 <= n)
      x <<= 1;
    return minimumOneBitOperations(n ^ (x | x >> 1)) + 1 + x - 1;
  }
}

