# 阿里

![image-20210901102622116](/Users/jiayangwei/Library/Application Support/typora-user-images/image-20210901102622116.png)

![image-20210901103008076](/Users/jiayangwei/Library/Application Support/typora-user-images/image-20210901103008076.png)

![image-20210901101526711](/Users/jiayangwei/Library/Application Support/typora-user-images/image-20210901101526711.png)

```java
import java.util.*;

public class Main{
  static HashMap<Integer, ArrayList<Integer>> mp, edge;
  static int n, m;


  static int[] sg;
  public static int getsg(int x){
    if (sg[x] != -1) return sg[x];
    if (x == n) return sg[x] = 0;
    int ok = 0;
    if (edge.containsKey(x)){
      for (int i : edge.get(x)){
        int ans = getsg(i);
        if (ans == 0) ok = 1;
      }
    }
    return sg[x] = ok;
  }


  public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    int t = sc.nextInt();
    while (t-- > 0)
    {
      n = sc.nextInt();
      m = sc.nextInt();
      edge = new HashMap<Integer, ArrayList<Integer>>();
      sg = new int[(int)(1e5) + 10];
      for (int i = 1; i <= n; i++) sg[i] = -1;
      for (int i = 1; i <= m; i++)
      {
        int a = sc.nextInt(), b = sc.nextInt();
        if (edge.containsKey(a))
        {
          edge.get(a).add(b);
        }
        else
        {
          ArrayList<Integer> list = new ArrayList<Integer>();
          list.add(b);
          edge.put(a, list);
        }
      }
      String s = sc.next();
      int ans = getsg(1);
      if (ans == 1)
      {
        if ("Bob".equals(s)) System.out.println("Alice");
        else System.out.println("Bob");
      }
      else System.out.println(s);

          /*if ((d[1] - d[n]) % 2 == 0) System.out.println(s);
          else if ("Alice".equals(s)) System.out.println("Bob");
          else System.out.println("Alice");*/
    }

  }
}
```

# 美团

# 京东

# 小米

# 华为
