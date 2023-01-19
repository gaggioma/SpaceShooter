# Space Shooter  🎅 🧙‍♀️ 🔫

## Purpose
During these "boring" christams holidays i tried to study in the deep the new release of [Vue.js](https://vuejs.org/), but i had to find something on which both to test this new rewrited framework, see the performance and know if this new release could induce me to adopt it respect to its ancestor.<br/> 

For this purpose i have implemented a custom and coarse (but funny) copy of the legendary game [Space Invaders](https://en.wikipedia.org/wiki/Space_Invaders).<br/>

## Implementation

Mainly this game is composed by these elements:
- [Arena](#arena)
- [Spacecraft](#spacecraft)
- [Enemies](#enemies)
- [Shot](#enemies)

All these elments have been implemented like Vue components.

In the follow a brief description.

### Arena

This component works like a H.O.C. which controls:
- spacecraft, enemies and shot cartesian coords
- generates enemies every N ms
- check if shot impacts enemy
- check that all components stay inside a perimetral bounds
- create the listener for user interaction

### Spacecraft

Spacecraft has to:
- move in all 4 directions when left keyboard is pushed
- shot a rocket when spacebar is pushed

when a direction is pushed, every 10ms the coordinate changes using:

```
clearInterval(timerDirectionFunctionLeft.value);

timerDirectionFunctionLeft.value = window.setInterval(() => {
    if(left.value - movStep.value <= 0){
        left.value = 0
    }else{
        left.value = left.value - movStep.value;    
    }
}, 10);
        
```

### Enemies

Single enemy has been implemented like a component that contains a single image (randomically chosen) and by the props the Arena can change the cartesian coordinates to move image down.

### Shot

Shot is a component contains a single image, when user push spacebar button arena create new component with initial (x,y) coords (center of spacecraft).
Inside component, every 20ms, y coord is updated and an event is fired to the arena which check if shot impact enemy. 

## Results

Video below shows the final behaviour.
This could be a starting point for further improvements

https://user-images.githubusercontent.com/64643932/213450674-1f4c2b60-96db-4a07-b54d-be324f40130c.mp4

## Further improvements
