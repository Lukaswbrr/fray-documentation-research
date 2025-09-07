## Tags
Used for tagging states in a category, which allows to make global transitions between states via
tag.

### Example
```
normal {
	light,
	medium,
	heavy,
}
special {
	236p,
	214p,
	623p
}
```

A global transitions can make every state tagged with normal go to special!

## Transitions
A transition in which a state can go to.

## Global Transitions

## Root state
Root state is the main state that the state machines uses.

Keep in mind, if you want to get the current state, you need to use something like
state_machine.get_root().get_current_state()
since the compoundstate is has tons of fraystaets (thats where it occurs the transitions, etc)

## Conditions
Adds conditions to states that only makes them possible to goto if the condition is true.
Useful for on hit situations where the player hits the opponent and is able to chain into
light > medium > heavy

Only root states can have conditions.

### Multiple conditions
If using multiple conditions, always put the transition with most conditions top first,
otherwise, it will not trigger!

#### Jump to crouch or idle example
```gdscript
	state_machine.initialize({}, FrayCompoundState.builder()
		.register_conditions({
			"is_crouched_pressed" = func(): return FrayInput.is_pressed("down"),
			"is_on_ground" = func():
				return is_on_floor()})
		})
		.transition_press("idle", "jump", {input="up"})
		# NOTE: for higher priorities transitions, like is crouched pressed and on ground
		# put it on top of previous conditions transitions
		# otherwise it wouldnt trigger!
		.transition("jump", "crouch", {
			auto_advance=true,
			advance_conditions=["is_on_ground", "is_crouched_pressed"]
		})
		.transition("jump", "idle", {
			auto_advance=true,
			advance_conditions=["is_on_ground"]
		})
	)
```

### Prereqs
Consist of conditions that must be satisfied before a transition is allowed to occur.

### Advance Conditions
conditions that, if satisfied and with auto-advance enabled, trigger a transition.

### register_conditions
Function for FrayCompoundState builder which register conditions.

The argument is a Dictionary with a list of conditions.

#### prereqs and advance conditions examples

##### Idle to Block (Prereqs)
In this example, to go to blocking, the player must press left and enemy 
attacking condition must be true.

Reason this uses prereqs is because the condition must be satisfied to allow
this transition. If it was advance_condition, it would go even if true or false
because advance_condition automatically advances the state if the condition
is true

```gdscript
func _ready() -> void:
	FrayInputMap.add_bind_action("left", "ui_left")

	state_machine.initialize({}, FrayCompoundState.builder()
		.register_conditions({
			# condition that checks if enemy is attacking
			"is_able_block" = func(): 
				var enemy_attacking = false
				return enemy_attacking })
		
		
		.transition_press("idle", "block", {
			input="left",
			# NOTE: prereqs are the requirement for the transition to happen.
			# in this case, only occurs the transition when enemy is attacking
			prereqs=["is_able_blocking"]
		})
		
		.transition("block", "idle", {
				# NOTE: if auto_advance is false, it will not use the advance_conditions, skipping it
				# and immediatly causing transition.
				auto_advance=true,
				# NOTE: ! at the start of the condition, inverts the condition.
				# in this case, causes the transition if the enemy is not attacking
				advance_conditions=["!is_able_block"]
			})
```

##### Auto transition to idle if crouch button is not pressed
```gdscript
func _ready() -> void:
	FrayInputMap.add_bind_action("down", "ui_down")
	
	state_machine.initialize({}, FrayCompoundState.builder()
		.register_conditions({
			# condition that checks if down is pressed
			"is_crouched_pressed" = func(): return FrayInput.is_pressed("down")
		})
	
		.transition_press("idle", "crouch", {input="down"})
		.transition("crouch", "idle", 
			{
				# NOTE: if auto_advance is false, it will not use the advance_conditions, skipping it
				# and immediatly causing transition.
				auto_advance=true,
				# NOTE: ! at the start of the condition, inverts the condition.
				# in this case, only returns true if crouch is NOT pressed
				advance_conditions=["!is_crouched_pressed"]
		})
	)
```

##### Auto transition jump to idle
```gdscript
func _ready() -> void:
	FrayInputMap.add_bind_action("up", "ui_up")
	
	state_machine.initialize({}, FrayCompoundState.builder()
		# TODO: make it so it transitions globally back to idle if animation
		# ends? not sure if you make this way or just make it via the
		# animation player signal
		.register_conditions({
			# condition that checks if FrayInput is pressed down
			# NOTE: testing to see if it works so vars wouldnt be necessary if possible
			"is_on_ground" = func():
				return is_on_floor()})
		

		.transition_press("idle", "jump", {input="up"})
		.transition("jump", "idle", {
			auto_advance=true,
			advance_conditions=["is_on_ground"]
		})
```
