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
