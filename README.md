# Space Shooter  🎅 🧙‍♀️ 🔫

## Purpose
During these boring christams holidays i tried to study in the deep the new release of [Vue.js](https://vuejs.org/), but i had to find something on which both to test this new rewrited framework, see the performance and if the simplicity of write code could induce me to adopt it respect to its ancestor.<br/> 

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
- create listener for user interaction

### Spacecraft

Spacecraft has to:
- move left when left keyboard is pushed 
- move right when right keyborad is pushed
- shot a rocket when spacebar is pushed

### Enemies

Enemies has been implemented like a simple image that by props can be moved over cartesian coordinates.

### Shot

Shot has been implemented like Enemies.

## Results

This roughtly video shows final behaviour.
This is a starting point for further improvements

https://github.com/gaggioma/SpaceShooter/blob/main/video/ResultVideo.mp4