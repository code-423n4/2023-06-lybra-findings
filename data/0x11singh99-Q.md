# Non Critical 
1. Function params name in all over the protocol doesn't follow one convention, sometimes it is prepended with _ sometimes not. Which is confusing.
2. named return is confusing
3. In many places natspec comments not written, which is really confusing to understand the intention of function or contract.
4. Prepend internal/private functions and variables with _ to make clear. Sometimes it is done sometimes not.
5. Use proper solidity style guide layout it is not followed all over the protocol
## Order of Solidity Layout :
### Layout contract elements in the following order:
Import statements
Interfaces
Libraries
Contracts

## Inside each contract, library or interface, use the following order:

Type declarations
State variables
Events
Errors
Modifiers
Functions

### Functions should be grouped according to their visibility and ordered:

constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private

#### Within a grouping, place the view and pure functions last.
