### 121. Best Time to Buy and Sell Stock

2/200

design an algorithm to find the maximum profit.

##### Exmples:

```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```



##### 思路:

保存最小值，然后再算之后项的最大值。

```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length < 2){return 0;} 
        int max_dif = 0;
        int low = prices[0];
        for(int i=0; i<prices.length; i++){
            if(prices[i] < low){
                low = prices[i];
            }else{
                int dif = prices[i] - low; //Notice the dif calc should be in this case.
                if(dif > max_dif){
                    max_dif = dif;
                }
            }
        }        
        return max_dif;
    }
}
```

