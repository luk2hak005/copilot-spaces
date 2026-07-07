# Copilot Instructions: Lift Traffic Light Application

You are implementing traffic light control logic for a lift system.

## System Scope
- There are 2 floor shafts.
- Each floor is front side only.
- Each floor has 3 lights:
  - RED
  - AMBER
  - GREEN

## Input Conditions
Model lift state using explicit boolean or enum inputs:
- isIdleParkingMode
- isRunningForCarCall
- isRunningForLandingCall
- isLoadedState
- isDoorOpeningForCarCall
- isDoorOpeningForLandingCall
- isDoorOpenedForCarCall
- isDoorOpenedForLandingCall

Additionally model floor position and call target using explicit inputs:
- currentFloor (1 or 2)
- targetFloor (1 or 2)
- hasCarCall
- hasLandingCall

## Movement and Door Operation Rules
- When a car call or landing call is given, the lift shall serve the respective floor.
- If currentFloor != targetFloor, the lift shall start moving toward targetFloor.
- If currentFloor == targetFloor, the lift shall not move.
- At the requested floor, door sequence shall be:
  - door opening
  - door opened
  - remain opened for 5 seconds
  - door closing
  - door closed

### Example Scenarios
- If lift is at floor 1 and a car/landing call is given for floor 2, the lift moves to floor 2, then the door is opening, then opened, remains opened for 5 seconds, then door is closing, then closed.
- If lift is at floor 2 and a car/landing call is given for floor 2, the lift does not move; the door is opening, then opened, remains opened for 5 seconds, then door is closing, then closed.

## Light Rules
Implement exactly these rules:

### RED light ON when any of the following is true
- Lift is idle in normal/parking mode.
- Lift is running for car call.
- Lift is in loaded state.
- Lift door is opening for car call.

### AMBER light ON when any of the following is true
- Lift is running for landing call.
- Lift doors are opening for landing call.

### GREEN light ON when doors are opened for landing call or car call
- Door opened for landing call.
- Door opened for car call.

## Priority Rule (Conflict Handling)
If multiple color conditions are true at the same time, apply this priority order:
1. GREEN (highest)
2. AMBER
3. RED (lowest)

This ensures only one light is ON at a time per floor.

## Output Contract
For each floor, return a single active light state:
- RED
- AMBER
- GREEN

Never return multiple active colors for the same floor.

## Implementation Guidance
- Keep logic deterministic and easy to test.
- Prefer a pure function for light calculation per floor.
- Add unit tests for each input condition and mixed-condition priority cases.

## Minimum Test Cases
Include tests for:
- RED from idle/parking mode.
- RED from running for car call.
- RED from loaded state.
- RED from door opening for car call.
- AMBER from running for landing call.
- AMBER from door opening for landing call.
- GREEN from door opened for landing call.
- GREEN from door opened for car call.
- Priority check: GREEN overrides AMBER and RED.
- Priority check: AMBER overrides RED when GREEN is false.
