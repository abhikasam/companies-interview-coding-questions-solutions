
______________________________________________________________________________________________________________________________________________________
Companies :

#Alibaba #Amazon #Apple #Asana #Baidu #BlackRock #Booz Allen Hamilton #Cisco #Citadel #Coursera #Coinbase #DocuSign #DoorDash #Dropbox #eBay #Expedia #Facebook #GoDaddy #Goldman Sachs #Google #Grab #HBO #Intuit #JPMorgan Chase #LinkedIn #Lyft #Microsoft #Morgan Stanley #Nutanix #NVIDIA #Oracle #Otter.ai #PayPal #Pinterest #Roblox #Salesforce #SAP #Snapchat #Spotify #Tesla #Tripadvisor #Twilio #Twitch #X #Uber #Visa #VMware #Wish #Yahoo #Yandex #Zillow
______________________________________________________________________________________________________________________________________________________

Problem :

Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the LRUCache class:

LRUCache(int capacity) Initialize the LRU cache with positive size capacity.
int get(int key) Return the value of the key if the key exists, otherwise return -1.
void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.
The functions get and put must each run in O(1) average time complexity.

 

Example 1:

Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
 

Constraints:

1 <= capacity <= 3000
0 <= key <= 104
0 <= value <= 105
At most 2 * 105 calls will be made to get and put.
 
______________________________________________________________________________________________________________________________________________________
Explanation :

<img width="846" height="1136" alt="image" src="https://github.com/user-attachments/assets/e6114d67-848f-4f23-8f9b-e51a2459b176" />
______________________________________________________________________________________________________________________________________________________

Solution :

class Node{
    int key;
    int val;
    Node prev;
    Node next;
    Node(){}
    Node(int key,int val){
        this.key = key;
        this.val = val;
    }
}
class LRUCache {
    Node head;
    Node tail;
    int capacity;
    Map<Integer,Node> map;
    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.head = new Node();
        this.tail = new Node();
        this.head.next = this.tail;
        this.tail.prev = this.head;
        this.map = new HashMap<>();
    }
    
    void removeNode(Node node){
        Node next = node.next;
        Node prev = node.prev;
        prev.next = next;
        next.prev = prev;
    }

    void addNode(Node node){
        Node next = head.next;
        head.next = node;
        node.next = next;
        node.prev = head;
        next.prev = node;
    }

    public int get(int key) {
        if(map.containsKey(key)){
            Node node = map.get(key);
            removeNode(node);
            addNode(node);
            return node.val;
        }
        return -1;
    }
    
    public void put(int key, int value) {
        if(map.containsKey(key)){
            Node curr = map.get(key);
            curr.val = value;
            removeNode(curr);
            addNode(curr);
        }
        else{
            Node curr = new Node(key,value);
            if(map.size()>=capacity){
                Node end = tail.prev;
                removeNode(end);
                map.remove(end.key);
            }
            addNode(curr);
            map.put(key,curr);
        }
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
