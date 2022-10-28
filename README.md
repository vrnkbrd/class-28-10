# class-28-10
Task 6kyu A runner, who runs with base speed s with duration t will cover a distances d: d = s * t
However, this runner can sprint for one unit of time with double speed s * 2
After sprinting, base speed s will permanently reduced by 1, and for next one unit of time runner will enter recovery phase and can't sprint again.

Your task, given base speed s and time t, is to find the maximum possible distance d.

Input:
1 <= s < 1000
1 <= t < 1000

My solution:
public class Kata {
  public static int solution(int speed, int time) {
     int boost = Math.min(speed / 3 + 1, (time + 1) / 2);
    return (time + boost) * speed - boost * (boost - 1) * 3 / 2;
  }
}
### Fav solution: 
public class Kata {
  private static final int[][] table = tabulate(1001, 1001);
  
  public static int solution(int speed, int time) {
    return table[speed][time];
  }
  
  private static int[][] tabulate(final int s, final int t) {
    int[][] table = new int[s][t];
    
    for (int speed = 1; speed < s; speed++) {
      for (int time = 1; time < t; time++) {
        // The recurrence relation is given by our decision
        // to either run or sprint at this time.  We choose
        // the max of:
        // 1. s + dist(s, t - 1)
        // 2. 2s, if t == 1
        // 3. 3s, if t == 2
        // 4. 3s - 1 + dist(s - 1, t - 2), if t > 2
        int runDist = speed + table[speed][time - 1];
        int sprintDist;
        
        if (time == 1) {
          sprintDist = 2 * speed;
        } else if (time == 2) {
          sprintDist = 3 * speed;
        } else {
          sprintDist = 3 * speed - 1 + table[speed - 1][time - 2];
        }
        
        table[speed][time] = Math.max(runDist, sprintDist);
      }
    }
    
    return table;
  }
}
This one is exactly what I expected to do in the beginning. But I found the way to make it shorter. 

Task 2 6 kyu 
Write a simple parser that will parse and run Deadfish.

Deadfish has 4 commands, each 1 character long:

i increments the value (initially 0)
d decrements the value
s squares the value
o outputs the value into the return array
Invalid characters should be ignored.

Deadfish.parse("iiisdoso") =- new int[] {8, 64};

My solution: 
public class DeadFish {
    public static int[] parse(String data) {
        int value = 0;
        int pointer = 0;
        int[] result = new int[(int) data.chars().filter(c -> c == 'o').count()];
        for (char c : data.toCharArray()) {
            if (c == 'i') value += 1;
            if (c == 'd') value -= 1;
            if (c == 's') value *= value;
            if (c == 'o') {
                result[pointer] = value;
                pointer++;
            }
        }
        return result;
    }
}
Fav solution: 
import java.util.ArrayList;
import java.util.List;
public class DeadFish {
    public static int[] parse(String data) {
        int res = 0;
        List<Integer> list = new ArrayList<>();
        String[]  split_data =data.split("");

        for (String i : split_data ){

            if(i.equals("i")){
                res++;
            }
            if(i.equals( "o")){
                list.add(res);
            }
            if(i.equals( "s")){
                res*= res;
            }
            if(i.equals( "d")){
                res -= 1;
            }


        }
        System.out.println(list.size());
        int[] outputarray = new int[list.size()];

        for (int i = 0; i < list.size(); i++) {
            System.out.println(i);
            outputarray[i] = list.get(i);
        }
        return outputarray;



    }
}
