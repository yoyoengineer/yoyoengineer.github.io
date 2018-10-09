---
layout: post
title:  "Segment Tree"
date:   2018-10-08 11:26:00 +0800
featured-img: segment_tree
categories: segment_tree
---

This article is Only about the range minimum query.  Of course, the segment tree can be used to solve other problems. But I won't talk about them here. The following websites explained them:

1. [https://www.geeksforgeeks.org/segment-tree-set-1-sum-of-given-range/](https://www.geeksforgeeks.org/segment-tree-set-1-sum-of-given-range/)
2. [https://www.geeksforgeeks.org/segment-tree-set-1-range-minimum-query/](https://www.geeksforgeeks.org/segment-tree-set-1-range-minimum-query/)
3. [https://www.geeksforgeeks.org/lazy-propagation-in-segment-tree/](https://www.geeksforgeeks.org/lazy-propagation-in-segment-tree/)

The example of the range minimum query is shown below:

![](/assets/img/posts/segment_tree/Snipaste_2018-10-08_13-56-47.png)

The minimum from 2 to 4 in this array is 0, the minimum from 0 to 3 in this array is -1 ...

And with the segment tree, first what we need to do is to split the array into two halves.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-08_14-11-41.png)

So -1, 3 and 4 on one side, and 0, 2, and 1 on the other side. And again, I split them as before.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-08_14-16-22.png)

Next, we can construct the segment tree

![](/assets/img/posts/segment_tree/Snipaste_2018-10-08_14-25-31.png)

![](/assets/img/posts/segment_tree/Snipaste_2018-10-08_14-32-27.png)

Then, we can query the minimum within the specified interval. Before that, we have three rules:

1. partial overlap
2. total overlap
3. no overlap

That means if  the specified interval partially overlapped the interval of one node,  then we go on both sides. if  the specified interval totally overlapped the interval of one node(the interval of the node is inside the specified interval),  then I'm going to stop. And if there is no overlap, I'm going to stop.

So, Let's take an example:

The specified interval is [2,4]

The procedure begins its search at the root, [2,4] does not completely overlap [0,5]. So,  there is a partial overlap. The search continues in the sub-trees. For the left sub-tree, [2,4] again is a partial overlap on [0,2]. So we go to the forth node. [0,1] does not have overlap on [2,4]. So this node is not going to contribute to our answer. So we are going to return a really big number Max from here. Next, [2,4] totally overlaps [2,2]. So we return 4 from the fifth node. the minimum between Max and 4 is 4, so we return 4 from the second node. Then for the third node, [2,4] and [3,5] have a partial overlap. So the search continue. For the sixth node, [2,4] totally overlaps [3,4], so this return 0. Next [5,5] and [2,4] have no overlap, so we return Max. The minimum between 0 and Max is 0, so we return 0 from the third node. At last, the minimum between 0 and 4 is 0. So, the result is 0.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-08_17-01-11.png)

**Representation of Segment trees**

Suppose the input array is -1, 0, 3, 6.

Then we can represent the corresponding segment tree in another array.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-08_18-28-13.png)

The rest of the content is well explained in the [website](https://www.geeksforgeeks.org/segment-tree-set-1-range-minimum-query/) I mentioned earlier, so I won't go into details.

**Lazy Propagation Segment Tree**

lazy propagation is an optimization technique of segment tree.

Let's take an example, first we create a minimum segment tree.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-09_09-49-38.png)

for lazy propagation, we created an another array of the same size as the segment tree.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-09_09-58-57.png)

Next, what we are going to do is applying these operations and showing how lazy propagation works.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-09_10-43-45.png)

For the first operation, first we come to the root node. and we check whether the value of it is up-to-date. because the value of the lazy tree's root node is 0. so it means the value of the root node of the segment tree is up-to-date. And owing to [0,3] does not totally overlap [0,7], it's a partial overlap. so the procedure continues in sub-trees.

[0,3] has an total overlap on the interval of the second node [0,3], so now what we are going to do is incrementing the value of this node by 3. And it becomes 2. Next Instead of going down,  we just stop here and update the value of the corresponding children of the lazy tree for the lazy propagation. So we increment the value of the children by 3.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-09_11-31-58.png)

Then we go to the third node of the segment tree, the value 1 of it is up-to-date, because the corresponding value of the lazy tree is 0. And [0,3] as well as [0,7] don't have overlap, so we go back here. Next we update the value of the parent node(root node) with the minimum of 2 and 1.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-09_11-51-56.png)

After the second operation is performed, the two trees become the same as shown in the figure below.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-09_11-57-32.png)

Then we are going to perform the third operation. First because [0,0] partially overlaps [0,7] and it's up-to-date, so we go on. And the situation of the second node is the same as the root, so we reach the children of the second node. But the values of it's children is not up-to-date, because the values of the corresponding node on the lazy tree is not 0. So what we are going to do is first updating the values of the two nodes, we'll add 4 to them. Thence they'll become 3 and 5 respectively. Moreover, the 4 is propagated to the children of the forth node and fifth node on the lazy tree.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-09_13-39-29.png)

For the forth node, [0,0] partially overlaps [0,1], so we continue to go down. And the value of left child is not up-to-date, so we update it.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-09_14-31-30.png)

Then the third operation is to increment [0,0] by 2. So, we add 2 to the node.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-09_14-29-07.png)

After that, we go back and to the right child, and update the value of it.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-09_14-45-50.png)

Then [0,0] does not overlap [1,1], therefore we will not increment it by 2. Next we return the minimum of 5 and 6 and update the parent value.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-09_14-54-48.png)

Then, let's go back and to the fifth node. first we update it's value. Next, Because [0,0] does not overlap [2,3], we go back and return the minimum of 5 and 5. And we update the parent value. [0,0] also does not overlap the interval of the third node, so we also just go back. the minimum of 1 and 5 is 1, so the root value is still 1.

![](/assets/img/posts/segment_tree/Snipaste_2018-10-09_15-09-44.png)

![](/assets/img/posts/segment_tree/Snipaste_2018-10-09_15-25-37.png)

For the query, the difference is also that it will check if there is a pending update and if there is, then updates the node. Once it makes sure that pending update is done, it works same as the previous. So I won't go into details.

Finally, I will attach the corresponding Java code.

```java
public class NextPowerOf2 {
    public int nextPowerOf2(int num){
        if(num ==0){
            return 1;
        }
        if(num > 0 && (num & (num-1)) == 0){
            return num;
        }
        while((num & (num-1)) > 0){
            num = num & (num-1);
        }
        return num<<1;
    }
    public static void main(String args[]){
        NextPowerOf2 np = new NextPowerOf2();
        System.out.println(np.nextPowerOf2(4));
    }
}
```

```java
/**
 * A segment tree is a tree data structure for storing intervals, or segments. It allows
 * for faster querying (e.g sum or min) in these intervals. Lazy propagation is useful
 * when there are high number of updates in the input array.

 * Write a program to support mininmum range query
 * createSegmentTree(int arr[]) - create segment tree
 * query(int segment[], int startRange, int endRange) - returns minimum between startRange and endRange
 * update(int input[], int segment[], int indexToBeUpdated, int newVal) - updates input and segmentTree with newVal at index indexToBeUpdated;
 * updateRange(int input[], int segment[], int lowRange, int highRange, int delta) - updates all the values in given range by
 * adding delta to them
 * queryLazy(int segment[], int startRange, int endRange) - query based off lazy propagation
 *
 * Time complexity to create segment tree is O(n) since new array will be at max 4n size
 * Space complexity to create segment tree is O(n) since new array will be at max 4n size
 * Time complexity to search in segment tree is O(logn) since you would at max travel 4 depths
 * Time complexity to update in segment tree is O(logn)
 * Time complexity to update range in segment tree is O(range)
 *
 * References
 * http://www.geeksforgeeks.org/segment-tree-set-1-sum-of-given-range/
 * http://www.geeksforgeeks.org/segment-tree-set-1-range-minimum-query/
 * https://www.topcoder.com/community/data-science/data-science-tutorials/range-minimum-query-and-lowest-common-ancestor/
 */
public class SegmentTreeMinimumRangeQuery {
    /**
     * Creates a new segment tree based off input array.
     */
    public int[] createSegmentTree(int input[]){
        NextPowerOf2 np2 = new NextPowerOf2();
        //if input len is pow of 2 then size of
        //segment tree is 2*len - 1, otherwise
        //size of segment tree is next (pow of 2 for len)*2 - 1.
        int nextPowOfTwo = np2.nextPowerOf2(input.length);
        int segmentTree[] = new int[nextPowOfTwo*2 -1];

        for(int i=0; i < segmentTree.length; i++){
            segmentTree[i] = Integer.MAX_VALUE;
        }
        constructMinSegmentTree(segmentTree, input, 0, input.length - 1, 0);
        return segmentTree;

    }

    /**
     * Updates segment tree for certain index by given delta
     */
    public void updateSegmentTree(int input[], int segmentTree[], int index, int delta){
        input[index] += delta;
        updateSegmentTree(segmentTree, index, delta, 0, input.length - 1, 0);
    }

    /**
     * Updates segment tree for given range by given delta
     */
    public void updateSegmentTreeRange(int input[], int segmentTree[], int startRange, int endRange, int delta) {
        for(int i = startRange; i <= endRange; i++) {
            input[i] += delta;
        }
        updateSegmentTreeRange(segmentTree, startRange, endRange, delta, 0, input.length - 1, 0);
    }

    /**
     * Queries given range for minimum value.
     */
    public int rangeMinimumQuery(int []segmentTree,int qlow,int qhigh,int len){
        return rangeMinimumQuery(segmentTree,0,len-1,qlow,qhigh,0);
    }

    /**
     * Updates given range by given delta lazily
     */
    public void updateSegmentTreeRangeLazy(int input[], int segmentTree[], int lazy[], int startRange, int endRange, int delta) {
        updateSegmentTreeRangeLazy(segmentTree, lazy, startRange, endRange, delta, 0, input.length - 1, 0);
    }

    /**
     * Queries given range lazily
     */
    public int rangeMinimumQueryLazy(int segmentTree[], int lazy[], int qlow, int qhigh, int len) {
        return rangeMinimumQueryLazy(segmentTree, lazy, qlow, qhigh, 0, len - 1, 0);
    }

    private void constructMinSegmentTree(int segmentTree[], int input[], int low, int high,int pos){
        if(low == high){
            segmentTree[pos] = input[low];
            return;
        }
        int mid = (low + high)/2;
        constructMinSegmentTree(segmentTree, input, low, mid, 2 * pos + 1);
        constructMinSegmentTree(segmentTree, input, mid + 1, high, 2 * pos + 2);
        segmentTree[pos] = Math.min(segmentTree[2*pos+1], segmentTree[2*pos+2]);
    }

    private void updateSegmentTree(int segmentTree[], int index, int delta, int low, int high, int pos){

        //if index to be updated is less than low or higher than high just return.
        if(index < low || index > high){
            return;
        }

        //if low and high become equal, then index will be also equal to them and update
        //that value in segment tree at pos
        if(low == high){
            segmentTree[pos] += delta;
            return;
        }
        //otherwise keep going left and right to find index to be updated
        //and then update current tree position if min of left or right has
        //changed.
        int mid = (low + high)/2;
        updateSegmentTree(segmentTree, index, delta, low, mid, 2 * pos + 1);
        updateSegmentTree(segmentTree, index, delta, mid + 1, high, 2 * pos + 2);
        segmentTree[pos] = Math.min(segmentTree[2*pos+1], segmentTree[2*pos + 2]);
    }

    private void updateSegmentTreeRange(int segmentTree[], int startRange, int endRange, int delta, int low, int high, int pos) {
        if(low > high || startRange > high || endRange < low ) {
            return;
        }

        if(low == high) {
            segmentTree[pos] += delta;
            return;
        }

        int middle = (low + high)/2;
        updateSegmentTreeRange(segmentTree, startRange, endRange, delta, low, middle, 2 * pos + 1);
        updateSegmentTreeRange(segmentTree, startRange, endRange, delta, middle + 1, high, 2 * pos + 2);
        segmentTree[pos] = Math.min(segmentTree[2*pos+1], segmentTree[2*pos+2]);
    }

    private int rangeMinimumQuery(int segmentTree[],int low,int high,int qlow,int qhigh,int pos){
        if(qlow <= low && qhigh >= high){
            return segmentTree[pos];
        }
        if(qlow > high || qhigh < low){
            return Integer.MAX_VALUE;
        }
        int mid = (low+high)/2;
        return Math.min(rangeMinimumQuery(segmentTree, low, mid, qlow, qhigh, 2 * pos + 1),
                rangeMinimumQuery(segmentTree, mid + 1, high, qlow, qhigh, 2 * pos + 2));
    }

    private void updateSegmentTreeRangeLazy(int segmentTree[],
                                            int lazy[], int startRange, int endRange,
                                            int delta, int low, int high, int pos) {
        if(low > high) {
            return;
        }

        //make sure all propagation is done at pos. If not update tree
        //at pos and mark its children for lazy propagation.
        if (lazy[pos] != 0) {
            segmentTree[pos] += lazy[pos];
            if (low != high) { //not a leaf node
                lazy[2 * pos + 1] += lazy[pos];
                lazy[2 * pos + 2] += lazy[pos];
            }
            lazy[pos] = 0;
        }

        //no overlap condition
        if(startRange > high || endRange < low) {
            return;
        }

        //total overlap condition
        if(startRange <= low && endRange >= high) {
            segmentTree[pos] += delta;
            if(low != high) {
                lazy[2*pos + 1] += delta;
                lazy[2*pos + 2] += delta;
            }
            return;
        }

        //otherwise partial overlap so look both left and right.
        int mid = (low + high)/2;
        updateSegmentTreeRangeLazy(segmentTree, lazy, startRange, endRange,
                delta, low, mid, 2*pos+1);
        updateSegmentTreeRangeLazy(segmentTree, lazy, startRange, endRange,
                delta, mid+1, high, 2*pos+2);
        segmentTree[pos] = Math.min(segmentTree[2*pos + 1], segmentTree[2*pos + 2]);
    }

    private int rangeMinimumQueryLazy(int segmentTree[], int lazy[], int qlow, int qhigh,
                                      int low, int high, int pos) {

        if(low > high) {
            return Integer.MAX_VALUE;
        }

        //make sure all propagation is done at pos. If not update tree
        //at pos and mark its children for lazy propagation.
        if (lazy[pos] != 0) {
            segmentTree[pos] += lazy[pos];
            if (low != high) { //not a leaf node
                lazy[2 * pos + 1] += lazy[pos];
                lazy[2 * pos + 2] += lazy[pos];
            }
            lazy[pos] = 0;
        }

        //no overlap
        if(qlow > high || qhigh < low){
            return Integer.MAX_VALUE;
        }

        //total overlap
        if(qlow <= low && qhigh >= high){
            return segmentTree[pos];
        }

        //partial overlap
        int mid = (low+high)/2;
        return Math.min(rangeMinimumQueryLazy(segmentTree, lazy, qlow, qhigh,
                low, mid, 2 * pos + 1),
                rangeMinimumQueryLazy(segmentTree, lazy,  qlow, qhigh,
                        mid + 1, high, 2 * pos + 2));

    }

    public static void main(String args[]){
        SegmentTreeMinimumRangeQuery st = new SegmentTreeMinimumRangeQuery();

        int input[] = {0,3,4,2,1,6,-1};
        int segTree[] = st.createSegmentTree(input);

        //non lazy propagation example
        assert 0 == st.rangeMinimumQuery(segTree, 0, 3, input.length);
        assert 1 == st.rangeMinimumQuery(segTree, 1, 5, input.length);
        assert -1 == st.rangeMinimumQuery(segTree, 1, 6, input.length);
        st.updateSegmentTree(input, segTree, 2, 1);
        assert 2 == st.rangeMinimumQuery(segTree, 1, 3, input.length);
        st.updateSegmentTreeRange(input, segTree, 3, 5, -2);
        assert -1 == st.rangeMinimumQuery(segTree, 5, 6, input.length);
        assert 0 == st.rangeMinimumQuery(segTree, 0, 3, input.length);

        //lazy propagation example
        int input1[] = {-1,2,4,1,7,1,3,2};
        int segTree1[] = st.createSegmentTree(input1);
        int lazy1[] =  new int[segTree.length];
        st.updateSegmentTreeRangeLazy(input1, segTree1, lazy1, 0, 3, 1);
        st.updateSegmentTreeRangeLazy(input1, segTree1, lazy1, 0, 0, 2);
        assert 1 == st.rangeMinimumQueryLazy(segTree1, lazy1, 3, 5, input1.length);
    }
}
```
