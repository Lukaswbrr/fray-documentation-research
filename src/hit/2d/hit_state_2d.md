## Functionality
Provides a way to manage multiple hitboxes in a state, useful in attacking states where there are multiple
states.

Specially useful for Hitbox state manager.

To use this on a Character2D, first, you create this node, add a fray hitbox to it and collision
shape to the hitbox 2d. To connect this, use the hitbox_intersected signal and connect it to the
character 2D. Every hitbox that is active in the hit state will emit the signal. (Check the active
hitboxes in the inspector of the hit state!)

## Used for
2D Hitboxes
src/hit/2d/hitbox_state_manager
