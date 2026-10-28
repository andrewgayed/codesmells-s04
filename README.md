# Section Assignment #4: Code Smells
<span style="font-size: 18px;">Members: Anthony Rastellini, Andrew Gayed, Pius Castrillox</span>

### Introduction (Code Smells):

### **1. Comments:** 

<span style="font-size: 15.5px;">**What?:**</span> Comments in code usually point to a deeper problem of badly written code.

<span style="font-size: 15.5px;">**Causes?:**</span> Comes from programmers trying to cover up confusing code with comments.

<span style="font-size: 15.5px;">**Issues?:**</span> Comments mask bad code with a convincing but inccorect solution.

## Code Example:

<<<<<<< HEAD
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
=======
```javascript
// collision chekcing
for (let bullet of my.sprite.bullet) {
    if (enemyShip.visible && this.collides(enemyShip, bullet, 0.5)) {
        this.explosion = this.add.sprite(enemyShip.x, enemyShip.y, "deathResult").
        setScale(0.25).play("explosion");
        // clear the bullets out
        bullet.y = -100;
        enemyShip.visible = false;
        enemyShip.x = -100;
        ...
>>>>>>> 5d03dc79988d7e5d92b0d67db5cc63051dc250c4
```

## Refactored:

<<<<<<< HEAD
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

<span style="font-size: 15.5px;">To prevent this, fix unclear code instead of commenting it: use full descriptive names, replace magic numbers with named constants, and extract commented blocks into well-named single-purpose functions. Keep comments only for explaining why, and if deleting a comment loses no information, delete it.</span> 
=======
Fowler says Refactoring method **Extract Function** can be used:

```javascript
handleShipDestruction(enemyShip) {
    this.add.sprite(enemyShip.x, enemyShip.y, "deathResult")
            .setScale(0.25)
            .play("explosion");
            
    this.moveOffScreen(enemyShip);
}

recycleBullet(bullet) {
    this.moveOffScreen(bullet);
}

moveOffScreen(gameObject) {
    gameObject.y = -100; 
}
```

## Prevention 
>>>>>>> 5d03dc79988d7e5d92b0d67db5cc63051dc250c4

Extract Function: Whenever you feel the need to write comments to explain big functions or logic, you should instead break the code into separate smaller fuctions.

### **2. Long Function:**

<span style="font-size: 15.5px;">**What?: Function that is too long and performs multiple tasks making it difficult to understand.**</span>  

<span style="font-size: 15.5px;">**Causes?: It happens when a function handles a lot of responsibilities without breaking them inso smaller ones or reusable functions.**</span> 

<span style="font-size: 15.5px;">**Issues?: Long functions are harder to read and maintain and they are more likely to introduce bugs.**</span>

## Code Examples:

```lua
local function purchaseItem(player, item)
    -- validate
    if not item then
        warn("No item")
        return false
    end
    if player.Character == nil then
        return false
    end

    -- price
    local price = item.Price
    if player.VIP then
        price = price * 0.8
    end

    -- payment
    local coins = player.leaderstats.Coins
    if coins.Value < price then
        player.PlayerGui.Shop.ErrorLabel.Text = "Not enough coins"
        return false
    end
    coins.Value = coins.Value - price

    -- give item
    local clone = item.Tool:Clone()
    clone.Parent = player.Backpack

    -- update UI
    player.PlayerGui.Shop.CoinsLabel.Text = tostring(coins.Value)

    -- save
    local success, err = pcall(function()
        DataStore:SetAsync(player.UserId, coins.Value)
    end)
    if not success then
        warn(err)
    end

    return true
end
```

## Refactored:

```lua
local VIP_DISCOUNT_MULTIPLIER = 0.8

local function isValidPurchase(player, item)
    return item ~= nil and player.Character ~= nil
end

local function getPriceForPlayer(player, item)
    if player.VIP then
        return item.Price * VIP_DISCOUNT_MULTIPLIER
    end
    return item.Price
end

local function chargePlayer(player, price)
    local coins = player.leaderstats.Coins
    if coins.Value < price then
        return false
    end
    coins.Value -= price
    return true
end

local function giveItem(player, item)
    item.Tool:Clone().Parent = player.Backpack
end

local function updateShopUI(player)
    player.PlayerGui.Shop.CoinsLabel.Text = tostring(player.leaderstats.Coins.Value)
end

local function saveCoins(player)
    local success, err = pcall(function()
        DataStore:SetAsync(player.UserId, player.leaderstats.Coins.Value)
    end)
    if not success then
        warn(err)
    end
end

local function purchaseItem(player, item)
    if not isValidPurchase(player, item) then
        return false
    end
    if not chargePlayer(player, getPriceForPlayer(player, item)) then
        return false
    end
    giveItem(player, item)
    updateShopUI(player)
    saveCoins(player)
    return true
end
```

## Prevention:

<span style="font-size: 15.5px;">Give each function one job and name it after that job. If you add a comment to label a section inside a function, extract that section into its own function. If a function won't fit on one screen or you can't describe it without "and", split it. Have the top-level function only call the smaller ones in order.</span> 

