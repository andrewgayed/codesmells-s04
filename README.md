# Section Assignment #4: Code Smells
<span style="font-size: 18px;">Members: Anthony Rastellini, Andrew Gayed, Pius Castrillox</span>

### Introduction (Code Smells):

### **1. Comments:** 

<span style="font-size: 15.5px;">**What?:**</span> Comments in code usually point to a deeper problem of badly written code.

<span style="font-size: 15.5px;">**Causes?:**</span> Comes from programmers trying to cover up confusing code with comments.

<span style="font-size: 15.5px;">**Issues?:**</span> Comments mask bad code with a convincing but inccorect solution.

## Code Example:

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
```

## Refactored:

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

Extract Function: Whenever you feel the need to write comments to explain big functions or logic, you should instead break the code into separate smaller fuctions.

### **2. Long Function:**

<span style="font-size: 15.5px;">**What?: Function that is too long and performs multiple tasks making it difficult to understand.**</span>  

<span style="font-size: 15.5px;">**Causes?: It happens when a function handles a lot of responsibilities without breaking them inso smaller ones or reusable functions.**</span> 

<span style="font-size: 15.5px;">**Issues?: Long functions are harder to read and maintain and they are more likely to introduce bugs.**</span>

## Code Examples:

## Refactored:

## Prevention:



