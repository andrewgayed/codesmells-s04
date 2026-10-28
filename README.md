# Section Assignment #4: Code Smells
<span style="font-size: 18px;">Members: Anthony Rastellini, Andrew Gayed, Pius Castrillox</span>

### **1. Comments:** 

<span style="font-size: 15.5px;">**What?:**</span> Comments in code usually point to a deeper problem of badly written code.

<span style="font-size: 15.5px;">**Causes?:**</span> Comes from programmers trying to cover up confusing code with comments.

<span style="font-size: 15.5px;">**Issues?:**</span> Comments mask bad code with a convincing but inccorect solution.

## Code Example:
```lua
-- check if player can buy item
local function check(p, i)
    -- get coins
    local c = p.leaderstats.Coins.Value
    -- get price and apply discount if vip
    local x = i.Price
    if p.VIP then
        x = x * 0.8 -- 20% off
    end
    -- return true if enough coins
    if c >= x then
        return true
    else
        return false
    end
end
```

## Refactored:

```lua
local VIP_DISCOUNT_MULTIPLIER = 0.8

local function getPriceForPlayer(player, item)
    if player.VIP then
        return item.Price * VIP_DISCOUNT_MULTIPLIER
    end
    return item.Price
end

local function canAffordItem(player, item)
    local coins = player.leaderstats.Coins.Value
    return coins >= getPriceForPlayer(player, item)
end
```

## Prevention:

To prevent this, fix unclear code instead of commenting it: use full descriptive names, replace magic numbers with named constants, and extract commented blocks into well-named single-purpose functions. Keep comments only for explaining why, and if deleting a comment loses no information, delete it.


## Terms:

Extract Methods: Whenever you feel the need to write comments to explain big functions or logic, you should instead break the code into separate smaller fuctions.

Replacing Magic Numbers with Constants: When random numbers are in code, it could be better to assign that number to a constant so the purpose of the number is obvious.

### **2. Long Function:**

<span style="font-size: 15.5px;">**What?: Function that is too long and performs multiple tasks making it difficult to understand.**</span>  

<span style="font-size: 15.5px;">**Causes?: It happens when a function handles a lot of responsibilities without breaking them inso smaller ones or reusable functions.**</span> 

<span style="font-size: 15.5px;">**Issues?: Long functions are harder to read and maintain and they are more likely to introduce bugs.**</span>

## Code Examples:

```javascript
// processing an order
void processindOrder()
{
   cout << "Processing the order ... " << endl;


   //calculating
   double total = 100.0;
   total =+ total * 0.08;


   cout << "Processing payment of $" << total << endl;
   cout << "Printing receipt..." << endl;


}
```


## Refactored:

Fowler says Refactoring method **Extract Function** can be used:


```javascript
void validateOrder() {
   cout << "Validating order..." << endl;
}


double calculateTotal(double price) {
   return price + price * 0.08;
}


void processPayment(double total) {
   cout << "Processing payment of $" << total << endl;
}


void printReceipt() {
   cout << "Printing receipt..." << endl;
}


void processOrder() {
   validateOrder();
   double total = calculateTotal(100.0);
   processPayment(total);
   printReceipt();
}
```


## Prevention:

<span style="font-size: 15.5px;">Break down long functions into smaller functions while having each function perform one specific task. Also it is a good idea to name the function meaningful names as they describe what they do.</span> 
