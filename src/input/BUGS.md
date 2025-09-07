## Virtual pressed causes error
If using .is_virtual for inputs, _decompose_impl causes a error.

Probably affected due to the StringName static typing, but definetely affected
due to the if else condition at the end of the return.

### Error
group_input.gd

Trying to return an array of type "Array" where expected return type is "Array[StringName]".
```gdscript
func _decompose_impl(device: int) -> Array[StringName]:
	return _last_inputs_by_device[device] if _last_inputs_by_device.has(device) else []
```

#### Potential Solution
Replacing the code with this fixes the issue. (not sure if should be a pull request since it doesnt
use the syntax before thing)
```gdscript
func _decompose_impl(device: int) -> Array[StringName]:
	if not _last_inputs_by_device.has(device):
		return []
	
	return _last_inputs_by_device[device]
```

#### Example case
This occurs the same error
```gdscript
func test_array() -> Array[StringName]:
	var array = [&"a"]
	return array[0] if array.has(0) else []

func _ready():
	test_array()
```

Replacing the test_array with this also fixes it
```gdscript
func test_array() -> Array[StringName]:
	var array = [&"a"]
	
	if not array.has(0):
		return []
	
	return array[0]

func _ready():
	test_array()
```
