# Elevator System NuSMV Specification

This repository contains a formal model of a 5-floor elevator system implemented in NuSMV (New Symbolic Model Verifier). The model includes comprehensive safety and liveness properties to ensure correct elevator behavior.

## System Overview

The elevator system models a 5-floor building with the following key features:
- Single elevator car
- Bidirectional movement (up/down)
- Floor request buttons
- Door control system
- Safety mechanisms

## State Variables

The model uses the following state variables:
- `floor`: Current floor position (1-5)
- `direction_up`: Boolean indicating upward movement
- `direction_down`: Boolean indicating downward movement
- `door_closed`: Boolean indicating door state
- `button`: Array of boolean values for floor request buttons

## Key Features

### Helper Definitions
- `has_req_above`: Checks for pending requests above current floor
- `has_req_below`: Checks for pending requests below current floor
- `should_move`: Determines if elevator movement is needed

### Safety Properties
1. No movement with open doors
2. No upward movement at highest floor (5)
3. No downward movement at lowest floor (1)
4. Automatic door closure
5. Service guarantee for all floor requests

### Movement Control
- Intelligent direction control based on pending requests
- Continuous movement in current direction while requests exist
- Efficient request handling algorithm

## Verified Requirements

The specification verifies the following properties:

1. **Safety Requirements**
   - Door safety (no movement with open doors)
   - Floor boundary safety
   - Door operation safety

2. **Liveness Requirements**
   - Door will eventually close when open
   - Door will eventually open at requested floors
   - All floor requests will eventually be served
   - System can reach any floor infinitely often

3. **Directional Movement Requirements**
   - Maintains upward direction while higher requests exist
   - Maintains downward direction while lower requests exist

## Fairness Conditions

The model implements fairness conditions to ensure:
- Each button request is eventually served
- No floor request can be indefinitely postponed
- System remains responsive to all requests

## Model Checking

To verify this specification using NuSMV:

1. Install NuSMV on your system
2. Run the model checker:
   ```bash
   nusmv elevator.smv
   ```

The model checker will verify all specified LTL (Linear Temporal Logic) properties and report any violations.

## LTL Properties

The specification includes various LTL properties to verify system behavior:

```
LTLSPEC G(!door_closed -> !(direction_up | direction_down))  # Safety: No movement with open door
LTLSPEC G(!door_closed -> F door_closed)                    # Liveness: Door eventually closes
LTLSPEC G(button[1] -> F(floor = 1 & !door_closed))        # Service: Floor requests are served
```

## Usage Notes

- The model assumes atomic state transitions
- Door operations are simplified to open/closed states
- Button presses are persistent until serviced
- The system prioritizes direction continuity for efficiency

## Limitations

- Does not model mechanical failures
- Simplified door operation (no partial states)
- Does not include emergency operations
- No passenger capacity modeling
- No timing constraints

## Future Enhancements

Possible improvements to the model could include:
1. Weight limits and capacity checking
2. Emergency operation modes
3. Maintenance states
4. Multiple elevator coordination
5. Timing constraints for operations