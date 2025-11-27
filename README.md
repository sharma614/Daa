# Daa
Here are the C codes for each algorithm individually, formatted for clarity and direct use:

***

### 1. Insertion Sort (Iterative)

```c
#include <stdio.h>
void insertionSort(int arr[], int n) {
    int i, key, j;
    for (i = 1; i < n; i++) {
        key = arr[i];
        j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
}
int main() {
    int arr[] = {12, 11, 13, 5, 6};
    int n = sizeof(arr)/sizeof(arr[0]);
    insertionSort(arr, n);
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    return 0;
}
```
***

### 2. Merge Sort (Divide & Conquer)

```c
#include <stdio.h>
void merge(int arr[], int l, int m, int r) {
    int n1 = m - l + 1, n2 = r - m;
    int L[n1], R[n2];
    for (int i = 0; i < n1; i++)
        L[i] = arr[l + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[m + 1 + j];
    int i = 0, j = 0, k = l;
    while (i < n1 && j < n2)
        arr[k++] = (L[i] <= R[j]) ? L[i++] : R[j++];
    while (i < n1)
        arr[k++] = L[i++];
    while (j < n2)
        arr[k++] = R[j++];
}
void mergeSort(int arr[], int l, int r) {
    if (l < r) {
        int m = l + (r - l) / 2;
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);
        merge(arr, l, m, r);
    }
}
int main() {
    int arr[] = {12, 11, 13, 5, 6, 7};
    int n = sizeof(arr)/sizeof(arr[0]);
    mergeSort(arr, 0, n - 1);
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    return 0;
}
```
***

### 3. Quick Sort (Divide & Conquer)

```c
#include <stdio.h>
void swap(int* a, int* b) { int t = *a; *a = *b; *b = t; }
int partition(int arr[], int low, int high) {
    int pivot = arr[high], i = (low - 1);
    for (int j = low; j <= high - 1; j++)
        if (arr[j] < pivot)
            swap(&arr[++i], &arr[j]);
    swap(&arr[i + 1], &arr[high]);
    return (i + 1);
}
void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
int main() {
    int arr[] = {10, 7, 8, 9, 1, 5};
    int n = sizeof(arr)/sizeof(arr[0]);
    quickSort(arr, 0, n - 1);
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    return 0;
}
```
***

### 4. Fractional Knapsack (Greedy)

```c
#include <stdio.h>
typedef struct { int weight, value; } Item;
void fractionalKnapsack(Item items[], int n, int W) {
    float ratio[n], total = 0;
    int i, j;
    for (i = 0; i < n; i++)
        ratio[i] = (float)items[i].value / items[i].weight;
    for (i = 0; i < n-1; i++)
        for (j = i+1; j < n; j++)
            if (ratio[i] < ratio[j]) {
                Item temp = items[i]; items[i]=items[j]; items[j]=temp;
                float t = ratio[i]; ratio[i]=ratio[j]; ratio[j]=t;
            }
    for (i = 0; i < n; i++) {
        if (W >= items[i].weight) {
            total += items[i].value;
            W -= items[i].weight;
        } else {
            total += ratio[i] * W;
            break;
        }
    }
    printf("Maximum value = %.2f\n", total);
}
int main() {
    Item items[] = {{10, 60}, {20, 100}, {30, 120}};
    int W = 50;
    int n = sizeof(items)/sizeof(items[0]);
    fractionalKnapsack(items, n, W);
    return 0;
}
```
***

### 5. Job Sequencing (Greedy)

```c
#include <stdio.h>
#define MAX 20
typedef struct { int id, deadline, profit; } Job;
void jobSequencing(Job jobs[], int n) {
    int result[MAX], slot[MAX]={0};
    for (int i=0; i<n-1; i++)
        for (int j=i+1; j<n; j++)
            if (jobs[i].profit < jobs[j].profit) {
                Job temp = jobs[i]; jobs[i]=jobs[j]; jobs[j]=temp;
            }
    for (int i = 0; i < n; i++) {
        for (int j = jobs[i].deadline-1; j >= 0; j--) {
            if (!slot[j]) {
                result[j] = jobs[i].id;
                slot[j] = 1;
                break;
            }
        }
    }
    printf("Job sequence: ");
    for (int i = 0; i < n; i++)
        if (slot[i]) printf("%d ", result[i]);
    printf("\n");
}
int main() {
    Job jobs[] = {{1,2,100},{2,1,19},{3,2,27},{4,1,25},{5,3,15}};
    int n = sizeof(jobs)/sizeof(jobs[0]);
    jobSequencing(jobs, n);
    return 0;
}
```
***

### 6. Prim’s Algorithm (Greedy)

```c
#include <stdio.h>
#define V 5
#define INF 99999
int minKey(int key[], int mstSet[]) {
    int min = INF, min_index;
    for (int v = 0; v < V; v++)
        if (!mstSet[v] && key[v] < min)
            min = key[v], min_index = v;
    return min_index;
}
void primMST(int graph[V][V]) {
    int parent[V], key[V], mstSet[V];
    for (int i = 0; i < V; i++)
        key[i] = INF, mstSet[i] = 0;
    key[0] = 0, parent[0] = -1;
    for (int count = 0; count < V-1; count++) {
        int u = minKey(key, mstSet);
        mstSet[u] = 1;
        for (int v = 0; v < V; v++)
            if (graph[u][v] && !mstSet[v] && graph[u][v] < key[v])
                parent[v] = u, key[v] = graph[u][v];
    }
    printf("Edge \tWeight\n");
    for (int i = 1; i < V; i++)
        printf("%d - %d \t%d\n", parent[i], i, graph[i][parent[i]]);
}
int main() {
    int graph[V][V] = {
        {0, 2, 0, 6, 0},
        {2, 0, 3, 8, 5},
        {0, 3, 0, 0, 7},
        {6, 8, 0, 0, 9},
        {0, 5, 7, 9, 0}
    };
    primMST(graph);
    return 0;
}
```
***

### 7. Kruskal’s Algorithm (Greedy)

```c
#include <stdio.h>
#include <stdlib.h>
#define V 4
#define E 5
typedef struct { int u, v, w; } Edge;
int find(int parent[], int i) {
    while (parent[i] != i)
        i = parent[i];
    return i;
}
void uni(int parent[], int x, int y) {
    parent[x] = y;
}
void kruskal(Edge edges[], int n) {
    int parent[V];
    for (int i = 0; i < V; i++)
        parent[i] = i;
    int count = 0;
    for (int i = 0; i < n && count < V-1; i++) {
        int u = find(parent, edges[i].u);
        int v = find(parent, edges[i].v);
        if (u != v) {
            uni(parent, u, v);
            printf("%d - %d: %d\n", edges[i].u, edges[i].v, edges[i].w);
            count++;
        }
    }
}
int main() {
    Edge edges[E] = { {0,1,10},{0,2,6},{0,3,5},{1,3,15},{2,3,4} };
    for (int i = 0; i < E-1; i++)
        for (int j = i+1; j < E; j++)
            if (edges[i].w > edges[j].w) {
                Edge temp = edges[i]; edges[i]=edges[j]; edges[j]=temp;
            }
    kruskal(edges, E);
    return 0;
}
```
***

### 8. 0-1 Knapsack Problem (Dynamic Programming)

```c
#include <stdio.h>
int max(int a, int b) { return (a > b)? a : b; }
int knapSack(int W, int wt[], int val[], int n) {
    int dp[n+1][W+1];
    for (int i = 0; i <= n; i++) {
        for (int w = 0; w <= W; w++) {
            if (i==0 || w==0)
                dp[i][w] = 0;
            else if (wt[i-1] <= w)
                dp[i][w] = max(val[i-1] + dp[i-1][w-wt[i-1]], dp[i-1][w]);
            else
                dp[i][w] = dp[i-1][w];
        }
    }
    return dp[n][W];
}
int main() {
    int val[] = {60, 100, 120};
    int wt[] = {10, 20, 30};
    int W = 50;
    int n = sizeof(val)/sizeof(val[0]);
    printf("Max value: %d\n", knapSack(W, wt, val, n));
    return 0;
}
```
***

### 9. Floyd–Warshall Algorithm (All-Pairs Shortest Path, DP)

```c
#include <stdio.h>
#define V 4
#define INF 99999
void floydWarshall(int graph[V][V]) {
    int dist[V][V], i, j, k;
    for (i = 0; i < V; i++)
        for (j = 0; j < V; j++)
            dist[i][j] = graph[i][j];
    for (k = 0; k < V; k++)
        for (i = 0; i < V; i++)
            for (j = 0; j < V; j++)
                if (dist[i][k] + dist[k][j] < dist[i][j])
                    dist[i][j] = dist[i][k] + dist[k][j];
    for (i = 0; i < V; i++) {
        for (j = 0; j < V; j++)
            if (dist[i][j] == INF)
                printf("INF ");
            else
                printf("%d ", dist[i][j]);
        printf("\n");
    }
}
int main() {
    int graph[V][V] = {
        {0, 5, INF, 10},
        {INF, 0, 3, INF},
        {INF, INF, 0, 1},
        {INF, INF, INF, 0}
    };
    floydWarshall(graph);
    return 0;
}
```
***

### 10. 8-Queens Problem (Backtracking)

```c
#include <stdio.h>
#define N 8
int board[N];
int isSafe(int row, int col) {
    for (int i = 0; i < row; i++)
        if (board[i] == col || board[i] - i == col - row || board[i] + i == col + row)
            return 0;
    return 1;
}
void solve(int row) {
    if (row == N) {
        for (int i = 0; i < N; i++)
            printf("%d ", board[i]);
        printf("\n");
        return;
    }
    for (int col = 0; col < N; col++) {
        if (isSafe(row, col)) {
            board[row] = col;
            solve(row + 1);
            board[row] = -1;
        }
    }
}
int main() {
    solve(0);
    return 0;
}
```
