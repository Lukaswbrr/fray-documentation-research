## Tags
Used for tagging states in a category, which allows to make global transitions between states via
tag.

### Example
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

A global transitions can make every state tagged with normal go to special!

## Transitions
A transition in which a state can go to.

## Global Transitions

## Conditions
Adds conditions to states that only makes them possible to goto if the condition is true.
Useful for on hit situations where the player hits the opponent and is able to chain into
light > medium > heavy

Only root states can have conditions.
