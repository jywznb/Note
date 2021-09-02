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



```java
import java.util.*;
// We have imported the necessary tool classes.
// If you need to import additional packages or classes, please import here.


public class Main {
  static int n;
  static int[] A, B;
  static int mi = (int)1e9;

  public static void dfs(int pos, int op, int a, int b) {
    if (pos > n) return;
    //System.out.println(pos + " " + op + " " + a + " " + b);

    if (pos == n)
    {
      if (mi > a + b) mi = a + b;
      return;
    }
    if (op == 1)
    {
      // use
      int zf1 = Math.min(a, A[pos + 1]);
      int res1 = Math.min(a - zf1, B[pos + 1]);
      res1 = Math.min(res1 + b , A[pos + 1]);
      //System.out.println(A[pos] + " " + B[pos + 1]);
      //System.out.println(zf1 + " " + res1);
      dfs(pos + 1, 1, zf1, res1);

      //nouse
      dfs(pos + 1, 0, a, b);


    }
    else{
      // use
      int zf1 = Math.min(a, A[pos + 1]);
      int res1 = Math.min(a - zf1, B[pos + 1]);
      res1 = Math.min(res1 + b, A[pos + 1]);
      dfs(pos + 1, 1, zf1, res1);

    }


  }


  public static void main(String[] args) {
    // please define the JAVA input here. For example: Scanner s = new Scanner(System.in);
    // please finish the function body here.
    // please define the JAVA output here. For example: System.out.println(s.nextInt());


    Scanner sc = new Scanner(System.in);
    n = sc.nextInt();
    A = new int[n + 1]; B = new int[n + 2];
    for (int i = 1; i <= n; i++)
    {
      String s = sc.next();
      String[] t = s.split(",");
      int x = Integer.parseInt(t[0]), y = Integer.parseInt(t[1]);
      A[i] = x;
      B[i] = y;
    }
    int val = sc.nextInt();
    dfs(0, 1, val, 0);
    System.out.println(mi);
      /*
    int mi = (int)1e9;
    if (n == 1)
    {
        n = 2;
        a[2] = a[1];
        b[2] = b[1];
    }

    for (int i = 1; i <= n; i++)
    {
      for (int j = i + 1; j <= n; j++)
      {
        int x = val, y = 0;
        int zf1 = Math.min(x, a[i]), res1 = Math.min(x - zf1, b[i]);
        int zf2 = Math.min(zf1, a[j]), res2 = Math.min(zf1 - zf2, b[j]);
        int zf3 = Math.min(res1, a[i]);
        int res3 = Math.min(zf3 + res2, a[j]);
        mi = Math.min(mi, res3 + zf2);
          //if (res3 + zf2 == 15) System.out.println(i + " " + j);
      }
    }
    System.out.println(mi);
   */
  }
}
/*
2
50,60 30,25
120
 */
```

