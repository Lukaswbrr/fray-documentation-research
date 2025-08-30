### Inputs type

#### Distinct
A distanct input is a input pressed without any overlapping inputs.
Meaning that it occured before when there were overlapping inputs
(probably means if the same input appears on other sequences???)

#### Echo
A echo input means that if the input is already detected.

#### Conditional Inputs
Conditional inputs are composite inputs used to create inputs that trigger based on conditions.
This is useful for making inputs like directional inputs change based on

#### Bind inputs
An input bind is used to map physical device presses to inputs fray input names.

Bind inputs are normal inputs. For example, ui_right, ui_left, etc.

#### Composite inputs
Composite inputs are inputs that contain more than one input, conditional inputs (like forward
only being forward if character is facing towards a opponent in a 2D fighting game)

Children of Composite inputs being virtual has no effect.

#### Group inputs
Group inputs are inputs that is accepted as along as it reaches the minimujm amount.

##### Example
Roman cancel in Guilty Gear (if atleast three of A, B, C, D) are pressed together

#### Simple inputs
Wrapper around composite inputs for simple inputs.

### Input bind
A input bind is used to map physical device presses to inputs fray input names.

### Input frame

### Input node

#### Next nodes
Nodes that are used for which input it can go to next, representing a sequence.

Once the node reaches the last input of sequence, is_match() function is runned to check if all
inputs did not get their time limit passed by. If not, the sequence is triggered.

#### Example
{
	"Composite1": {
		priority: 10
		child: {
			"Composite2": {
				priority: 20
			}
		}
	}
}

The priority of Composite2 is ignored

### Device
A device is like a controller, in which is where the inputs are being emitted from.

It's possible to make a virtual device which can be controlled via code, meaning bots can be made
via a virtual device.
