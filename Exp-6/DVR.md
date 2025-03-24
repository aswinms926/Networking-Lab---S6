# Distance Vector Routing Algorithm Implementation

This code implements the Distance Vector Routing algorithm (also known as the Bellman-Ford algorithm) for network routing. The algorithm calculates the shortest paths between nodes in a network.

## Code

```
#include<stdio.h>
struct node{
    unsigned dist[20];
    unsigned from[20];
}rt[20];

int main()
{
    int cost[20][20],i,j,k,nodes,count;
    
    printf("Enter the number of nodes : ");
    scanf("%d",&nodes);
    
    printf("Enter the costmatrix : ");
    for(i=0;i<nodes;i++)
    {
        for(j=0;j<nodes;j++)
        {
            scanf("%d",&cost[i][j]);
            if(i==j)
            {
               cost[i][i]=0; 
            }
            
            rt[i].dist[j]=cost[i][j];
            rt[i].from[j]=j;
        }
    }
    
    do{
        count=0;
        for(i=0;i<nodes;i++)
        {
            for(j=0;j<nodes;j++)
            {
                for(k=0;k<nodes;k++) // Note: Fixed 'anodes' to 'nodes'
                {
                    if(rt[i].dist[j]>rt[i].dist[k]+rt[k].dist[j])
                    {
                        rt[i].dist[j]=rt[i].dist[k]+rt[k].dist[j];
                        rt[i].from[j]=k;
                        count++;
                    }
                }
            }
        }
        
    }while(count!=0);
    
    for(i=0;i<nodes;i++)
    {
        printf("\nfor router %d\n",i+1);
        for(j=0;j<nodes;j++)
        {
            printf("node %d via %d distance %d\n",j+1,rt[i].from[j]+1,rt[i].dist[j]);
        }
    }
    return 0;
}
```

## Key Concepts

### Distance Vector Routing

Distance Vector Routing is a protocol where each router:
- Maintains a table (vector) of distances to all other nodes in the network
- Periodically shares its distance vector with neighboring routers
- Updates its own table when it finds a shorter path to a destination

### Data Structures

- `struct node`: Contains two arrays:
  - `dist[20]`: Stores the distance from a node to all other nodes
  - `from[20]`: Stores the next hop node in the shortest path to reach each destination

- `rt[20]`: Array of routing tables for each node
- `cost[20][20]`: Cost matrix representing direct link costs between nodes

### Algorithm Steps

1. **Initialization**:
   - The program asks for the number of nodes in the network
   - It then takes the cost matrix as input
   - Initialize the routing tables with direct costs

2. **Distance Vector Calculation**:
   - The algorithm iteratively looks for better paths
   - For each node i, it checks if going through node k to reach node j is better than the current path
   - If a better path is found, it updates the routing table and sets a flag (count)
   - The process continues until no better paths can be found (count=0)

3. **Output**:
   - For each router, it prints the shortest path to all other nodes
   - Shows the next hop and total distance

### Time Complexity

- The algorithm has a time complexity of O(n³), where n is the number of nodes
- In the worst case, it may take n-1 iterations to converge

### Common Uses

- Used in RIP (Routing Information Protocol)
- Suitable for small to medium-sized networks
- Distributed algorithm where each node only needs to know about its neighbors

### Limitations

- Count-to-infinity problem can occur in certain network topologies
- Slow convergence in large networks
- No knowledge of the entire network topology (unlike link-state algorithms)

## Note

There appears to be a typo in the original code where `anodes` is used instead of `nodes` in the innermost loop. The corrected version is shown in the code above.
