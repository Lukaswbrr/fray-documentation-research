## Functionality
Wraps FrayCompoundState and uses SceneTrees for hiearchry thing

### Initializing
When making custom states, it's possible to provide them read-only context, like, setting their
variations when starting up the state.
--- state machine
state_machine.initialize({
	player_max_health=100
}, ...)

--- custom state script file (i think this is wrong since it doesnt extend State)
class_name CustomState
extends State 

var player_max_health: float

func _ready(context: Dictionary) -> void:
	player_max_health = context.get("player_max_health", 0.0)

## Used for
States
