# Matching Engine
### Problem Statement
To write an exchange order matching engine.


Input will arrive from stdin, where each line shall be comprised of several columns separated by one space.
The first column of each line specifies the operation to be carried out. The supported operations are:
1. BUY
2. SELL
3. CANCEL
4. MODIFY
5. PRINT

If the first column is BUY or SELL, then this line will contain five columns in total, where:
1. The second column denotes the order type, which will be either: IOC or GFD.
2. The third column will denote the price you want to buy or sell.It will be a positive integer.
3. The fourth column denotes the quantity (or amount) of that will be either bought or sold. It, too, will be a positive 
integer.
4. The fifth column is the Order ID, it can be comprised of any sequence of printable characters.


If the first column is CANCEL, then this line will contain two columns in total, where:
1. The second column is the Order ID. The order associated with the Order ID must be deleted and can no longer be 
considered for trading.
2. If the Order ID does not exist simply do nothing.

If the first column is MODIFY, then this line will contain five columns in total, where:
1. The second column is the Order ID, it means that specific order needs to be modified.
2. The third column is either BUY or SELL.
3. The fourth column is the new price of that order.
4. The fifth column is the new quantity of that order.
5. If the Order ID does not exist simply do nothing.

Note: We cannot modify an IOC order type, as will be explained later.

The MODIFY is a powerful operation, e.g. a BUY order can be modified to become a SELL order and vice versa.

BUY GFD 1000 10 order1
   MODIFY order1 SELL 1000 10

For an input line where first column is PRINT, then there shall be no other columns in this line. The task to be 
carried out for the PRINT operation is to output the current price book according to the following output format.

Output Format:
   SELL:
   price1 qty1
   price2 qty2
   BUY:
   price1 qty1
   price2 qty2

The price for, SELL section must be decreasing (descending) order and correspondingly the price for BUY section must 
also be decreasing order.
   e.g.
   SELL:
   1010 10
   1000 20
   BUY:
   999 10
   800 20

Lets now discuss the order types:
   The GFD order (stands for "Good For Day") will stay in the order book until it has been completely traded.
   The IOC order (stands for "Insert Or Cancel") means if it cannot be traded immediately, it will be cancelled right 
   away. If it is only partially traded, the non-traded part is cancelled.

Matching Rule
   The rule for matching is simple:
   If there is a "price cross", meaning someone is willing to buy at a price higher than or equal to the lowest current 
   selling price, these two orders will be considered as having traded against each other.
   Furthermore, the matching engine is expected to print out information pertaining to the newly minted trade.

For example:
   BUY GFD 1000 10 order1
   SELL GFD 900 10 order2

After you process the second line, we know that these two orders can be traded against each other, so you matching 
engine's output should be:
   TRADE order1 1000 10 order2 900 10

The print-out displays the two order's trade information: Order ID followed by the trade's price and the traded 
quantities for each side. (Real matching engines will have only one traded price, but to make things simple, we output two prices by each.)

The sequence for order1 and order2 is decided by which order was processed by the matching engine first.

For example:
SELL GFD 900 10 order2
BUY GFD 1000 10 order1
Then the output is:
TRADE order2 900 10 order1 1000 10

Note: The "TRADE order1 price1 qty1 order2 price2 qty2" message should be output every time a trade occurs within 
the matching engine.

The overarching aim of the matching engine is to be fair. To achieve this we utilise price-time priority: Simply put 
orders that are equally priced are traded out in the order they are received at the matching engine.

For example:
BUY GFD 1000 10 order1
BUY GFD 1000 10 order2
SELL GFD 900 20 order3

The output will be:
TRADE order1 1000 10 order3 900 10
TRADE order2 1000 10 order3 900 10

There one more "property" to note of the MODIFY operation. When are order is modified will lose its original priority (place). So
BUY GFD 1000 10 order1
BUY GFD 1000 10 order2
MODIFY order1 BUY 1000 20
SELL GFD 900 20 order3

Because order1 is modified in the middle, now order2 is in the first place, order1 is in the second place, so the 
expected output from the matching engine will be:

TRADE order2 1000 10 order3 900 10
TRADE order1 1000 10 order3 900 10


Note: Order ID will always be unique for all active orders in the matching engine, otherwise the given operation
(Buy, Sell, Modify Cancel etc) should be ignored.


Example 1:
BUY GFD 1000 10 order1
PRINT

The output of above would be:
SELL:
BUY:
1000 10

Example 2:
BUY GFD 1000 10 order1
BUY GFD 1000 20 order2
PRINT

The output of above would be:
SELL:
BUY:
1000 30

Example 3:
BUY GFD 1000 10 order1
BUY GFD 1001 20 order2
PRINT
The output of above would be:
SELL: 
BUY:
1001 20
1000 10

Example 4:
BUY GFD 1000 10 order1
SELL GFD 900 20 order2
PRINT

The output of above would be:
TRADE order1 1000 10 order2 900 10
SELL:
900 10
BUY:

Example 5:
BUY GFD 1000 10 ORDER1
BUY GFD 1010 10 ORDER2
SELL GFD 1000 15 ORDER3
TRADE ORDER2 1010 10 ORDER3 1000 10
TRADE ORDER1 1000 5 ORDER3 1000 5

Any input with price <= 0 or quantity <= 0 or empty order ID is invalid and should be ignored by the matching engine.

Your solution should:

1. Be entirely of your own devising and not sourced from anyone or anywhere else
2. Apply SOLID principles where appropriate, applicable and practical
3. Be clear, concise and correct - demonstrating common sense software engineering and the application of modern C++ skills and principles.
4. Take into account usage in low latency scenarios
5. Be scalable, in short where applicable the overall performance should not degrade as the number of inputs or actions increase

Extended:
Implement multithreading.