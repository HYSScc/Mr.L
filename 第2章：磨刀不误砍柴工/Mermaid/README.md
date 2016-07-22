```
graph TB 
    subgraph 
    one a1-->a2 
    end 
    subgraph two 
    b1-->b2 
    end 
    subgraph three 
    c1-->c2 
    end 
    c1-->a2
```

```mermaid 

    graph TD; 
    A-->B; 
    A-->C; 
    B-->D; 
    C-->D; 

\```