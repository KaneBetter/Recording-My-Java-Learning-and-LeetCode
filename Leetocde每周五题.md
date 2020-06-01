# Leetocde每周五题

## Two Sum（1月8日）

> Given an array of integers, return indices of the two numbers such that they add up to a specific target.
>
> You may assume that each input would have exactly one solution, and you may not use the same element twice.
>
> Example:
>
> Given nums = [2, 7, 11, 15], target = 9,
>
> Because nums[0] + nums[1] = 2 + 7 = 9,
> return [0, 1].
>
> 来源：力扣（LeetCode）
> 链接：https://leetcode-cn.com/problems/two-sum
> 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

- 解决方法

```python
#Python3
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashmap={}
        for index,num in enumerate(nums):
            another_num = target - num
            if another_num in hashmap:
                return [hashmap[another_num],index]
                hashmap[num] = index
                return None
```

```java
#Java
class Solution {
   public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>((int) ((float) nums.length / 0.75F + 1.0F));
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(target - nums[i])) {
                return new int[]{map.get(target - nums[i]), i};
            }
            map.put(nums[i], i);
        }
        throw new IllegalArgumentException("No two sum value");
    }
}
```

- hashmap