# Dynamic Programming

The key for dp is to find the variables to represent the states and deduce the transition function. \(aka find state -&gt; find relation -&gt; find constraints\)

Take [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) as an example:

#### Find state

The natural states for this problem is the 3 possible transactions : **buy, sell, rest.**

Because the transaction sequences can end with any of these three states. For each of them we make an array, **buy\[n\], sell\[n\] and rest\[n\].**, which represents the maxProfit for any sequence end with buy/sell/rest.

#### Find relation

Then we want to deduce the transition functions for buy sell and rest. By definition we have:

* buy\[i\] = max\(rest\[i-1\]-price, buy\[i-1\]\)
* sell\[i\] = max\(buy\[i-1\]+price, sell\[i-1\]\)
* rest\[i\] = max\(sell\[i-1\], buy\[i-1\], rest\[i-1\]\) Where price is the price of day i. All of these are very straightforward. They simply represents :
* We have to `rest` before we `buy` and
* we have to `buy` before we `sell`

#### Find constraints

One tricky point is how do you make sure you sell before you buy, since from the equations it seems that \[buy, rest, buy\] is entirely possible.

Well, the answer lies within the fact that buy\[i\] &lt;= rest\[i\] which means rest\[i\] = max\(sell\[i-1\], rest\[i-1\]\). That made sure \[buy, rest, buy\] is never occurred.

A further observation is that and rest\[i\] &lt;= sell\[i\] is also true therefore

* rest\[i\] = sell\[i-1\] Substitute this in to buy\[i\] we now have 2 functions instead of 3:
* buy\[i\] = max\(sell\[i-2\]-price, buy\[i-1\]\)
* sell\[i\] = max\(buy\[i-1\]+price, sell\[i-1\]\)

This is better than 3, but, we can do even better

Since states of day i relies only on i-1 and i-2 we can reduce the O\(n\) space to O\(1\). And here we are at our final solution:

```text
public int maxProfit(int[] prices) {
    int sell = 0, prev_sell = 0, buy = Integer.MIN_VALUE, prev_buy;
    for (int price : prices) {
        prev_buy = buy;
        buy = Math.max(prev_sell - price, prev_buy);
        prev_sell = sell;
        sell = Math.max(prev_buy + price, prev_sell);
    }
    return sell;
}
```

