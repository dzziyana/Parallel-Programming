#### Atomic Action = change in state of hardware
- **cannot interrupt**: happens either completely or not at all
- **cannot interleave** with any other single action
- **effects cannot be seen until the action is complete**, *e.g. no half-changed variables!*
- -> abstraction: it happens in a single moment 
- but several actions can still happen in different orders & form bad interleavings

#### Atomic Register = hardware component
- register = individual memory cell, stores 0/1 (or X/Z)
- atomic registers have hardware support for atomic actions
#### Atomic Operation
**= code construct that, if executed, triggers an atomic action in an atomic register**