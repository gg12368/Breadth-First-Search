690. 员工的重要性
给定一个保存员工信息的数据结构，它包含了员工唯一的id，重要度 和 直系下属的id。

比如，员工1是员工2的领导，员工2是员工3的领导。他们相应的重要度为15, 10, 5。
那么员工1的数据结构是[1, 15, [2]]，员工2的数据结构是[2, 10, [3]]，员工3的数据结构是[3, 5, []]。
注意虽然员工3也是员工1的一个下属，但是由于并不是直系下属，因此没有体现在员工1的数据结构中。
现在输入一个公司的所有员工信息，以及单个员工id，返回这个员工和他所有下属的重要度之和。

示例 1:
输入: [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
输出: 11
解释:
员工1自身的重要度是5，他有两个直系下属2和3，而且2和3的重要度均为3。因此员工1的总重要度是 5 + 3 + 3 = 11。

不知道大🔥在做有关BFS和DFS的题的时候，有没有发现，其实就这两类：

题目明确给了一个逻辑结构。例如给一棵树，求它的左下角的值等等。这种问题不用抽象，直接干就完事了。
题目给了一个抽🐘问题，而这个问题所对应的的逻辑结构需要我们自己去抽🐘。例如排列组合子集N皇后问题，对应的其实都是一棵树。
我们将问题抽象后，其实就变成了搜索一棵树这样的问题。
这个题是属于第二类的，而且这抽象非常明显。一个领导对应多个员工，每个员工对应1个领导。
换句话说，领导是爸爸，员工是儿子。这tm不就是一棵树吗？所谓的重要性，不是就是root.val吗？

这样一抽🐘，这个问题就变成了给一个子树，求它的树上所有节点的总和，就变成了憨憨题了。

搜索树的问题，BFS和DFS都OK，看自己喜好吧，👨‍🦳附上👨‍🦳的DFS和BFS代码。


//DFS
class Solution {
    public int getImportance(List<Employee> employees, int id) {
        //根据id找到根节点
        Employee root = null;
        for(Employee e : employees) {
            if(e.id == id) {
                root = e;
                break;
            }
        }
        //累加它的子树和
        int ans = root.importance;
        for(int sub : root.subordinates) {
            ans += getImportance(employees, sub);
        }
        return ans;
    }
}

//BFS
class Solution {
    public int getImportance(List<Employee> employees, int id) {
        if(employees == null || employees.size() == 0) {
            return 0;
        }
        //这里可以用map先存一下，不然每次都得根据id去搜employee
        Map<Integer, Employee> map = new HashMap<>();
        for(Employee e : employees) {
            map.put(e.id, e);
        }
        //对于BFS，就维护一个队列，每次把出队的节点的孩子给丢进队列即可
        Deque<Employee> queue = new ArrayDeque<>();
        queue.offer(map.get(id));
        int ans = 0;
        while(!queue.isEmpty()) {
            Employee e = queue.poll();
            ans += e.importance;
            //把根节点孩子丢进队列里
            for(int subordinate : e.subordinates) {
                queue.offer(map.get(subordinate));
            }
        }
        return ans;
    }
}
