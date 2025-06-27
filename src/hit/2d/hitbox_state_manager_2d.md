## Functionality
Provides a way to manage multiple hit states!

When a active hitbox is changed in any of FrayHitState (like, one gets active), all active hitboxes
of the other hit states gets disabled. This makes the other hitboxes invisible and it applys in
editor too.

### Code example for detecing only active states hitboxes (apply this to a Character2D)
```gdscript
func _on_fray_hit_state_manager_2d_hitbox_intersected(detector_hitbox: FrayHitbox2D, detected_hitbox: FrayHitbox2D) -> void:
	var state: FrayHitState2D = $FrayHitStateManager2D.get_current_state_obj()
	
	if state._is_active:
		print("it is..")
	
	print(state)
```
Screenshot of nodes example

![fray-learning/documentation/assets/character 2d fray hit state manager example.png](https://github.com/Lukaswbrr/fray-learning/blob/main/documentation/assets/character%202d%20fray%20hit%20state%20manager%20example.png?raw=true)


NOTE: maybe the state._is_active is unnecessary?

### Note
Apparently, this uses a metadata thing, which all FrayHitState uses, which seems to have the
source value (the variable source thing), investage this when it gets used. Probably in
Input or state management??????


## Used for
2D Hitboxes
2D Hitboxes states
