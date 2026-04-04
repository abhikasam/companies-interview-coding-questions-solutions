
Companies : #apple #amaxon #facebook #flipkart
________________________________________________________________________________________________________________________________________________

Problem : 
Given an array of points where points[i] = [xi, yi] represents a point on the X-Y plane and an integer k, return the k closest points to the origin (0, 0).

The distance between two points on the X-Y plane is the Euclidean distance (i.e., √(x1 - x2)2 + (y1 - y2)2).

You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).

Example 1:
<img width="939" height="939" alt="image" src="https://github.com/user-attachments/assets/1260ee55-492c-4d7d-8d92-4d6fd511e242" />

Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].
Example 2:

Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]
Explanation: The answer [[-2,4],[3,3]] would also be accepted.
 

Constraints:

1 <= k <= points.length <= 104
-104 <= xi, yi <= 104

______________________________________________________________________________________________________________________________________________

Solution :
<img width="2433" height="893" alt="image" src="https://github.com/user-attachments/assets/d7dc249d-0259-455b-8754-3f7ff2f56855" />

______________________________________________________________________________________________________________________________________________


class Solution {
    public record Point(Long distance,int[] point){}
    long distance(int[] point){
        return (long)(Math.pow(point[0],2)+Math.pow(point[1],2));
    }
    public int[][] kClosest(int[][] points, int k) {
        Point[] allpoints = new Point[points.length];
        for(int i=0;i<points.length;i++){
            allpoints[i] = new Point(distance(points[i]),points[i]);
        } 
        PriorityQueue<Point> pq = new PriorityQueue<>((a,b)->
            Long.compare(a.distance,b.distance)
        );
        for(var point : allpoints){
            pq.offer(point);
        }

        int[][] result = new int[k][2];
        for(int i=0;i<k;i++){
            result[i]=pq.poll().point;
        }

        return result;
    }
}
 
