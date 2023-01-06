# Space Shooter  🎅 🧙‍♀️ 🔫

## Purpose
During these boring christams holidays i tried to study in the deep the new release of [Vue.js](https://vuejs.org/), but i had to find something on which both to test this new rewrited framework and if the simplicity of write code could induce me to adopt it respect to its ancestor.<br/> 

For this purpose i have implemented a custom and rudimental (but funny) of the legendary game [Space Invaders](https://en.wikipedia.org/wiki/Space_Invaders).<br/>

## Implementation

Mainly this game is composed by these elements/components:
- [Arena](#arena)
- [Spacecraft](#spacecraft)
- [Enemies](#enemies)
- [Shot](#enemies)

### Arena

This component works like a H.O.C. which controls:
- spacecraft, enemies and shot cartesian coords
- generates enemies
- check if shot impacts enemy
- check that all components stay inside a perimetral bounds
- create the listener for user interaction

### Spacecraft

Spacecraft has to:
- move left when left keyboard is pushed 
- move right when right keyborad is pushed
- shot a rocket when spacebar is pushed

### Enemies

Single enemy has been implemented like a component that contains a single image (randomically chosen) and by the props the Arena can change the cartesian coordinates to move image down.

### Shot

Shot is a component contains a single image, when user push spacebar button arena create new component with initial (x,y) coords.
Inside component every 20ms y coord is updated and an event is fire to the arena which check if shot impact enemy. 

## Results

This roughtly video (handmade by smartphone 😂) shows final behaviour.
This is a starting point for further improvements

https://github.com/gaggioma/SpaceShooter/blob/main/video/ResultVideo.mp4

## Further improvements