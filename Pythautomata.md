# Pythautomata user guide <!-- TOC ignore:true -->

The aim of this document is to guide the user in the usage of the Pythautomata python library used for modeling finite state systems.

<!-- TOC -->

- [Pythautomata User guide](#pythautomata-user-guide)
    - [Install pythautomata](#install-pythautomata)
    - [Moore machines](#moore-machines)
        - [Creating a moore machine](#creating-a-moore-machine)
            - [Dependencies](#dependencies)
            - [Implementation](#implementation)
        - [Moore machine generator](#moore-machine-generator)
            - [Dependencies](#dependencies)
            - [Implementation](#implementation)
        - [DFA - moore machine comparator and converter](#dfa---moore-machine-comparator-and-converter)
            - [Dependencies](#dependencies)
                - [Implementation](#implementation)

<!-- /TOC -->

## Install pythautomata

```python
%pip install pythautomata 
```
## Moore machines

### Creating a moore machine

#### Dependencies 

```python
from pythautomata.base_types.alphabet import Alphabet
from pythautomata.base_types.moore_state import MooreState
from pythautomata.base_types.symbol import SymbolStr
from pythautomata.automata.moore_machine_automaton import MooreMachineAutomaton
from pythautomata.model_comparators.moore_machine_comparison_strategy import MooreMachineComparisonStrategy as ComparisonStrategy
from pythautomata.model_exporters.dot_exporting_mm_strategy import DotExportingMMStrategy
```

#### Implementation

```python
# Input alphabet with symbols a, b and c
input_alphabet = Alphabet(frozenset((SymbolStr('a'), SymbolStr('b'), SymbolStr('c'))))
# Output alphabet with symbols 0 and 1
output_alphabet = Alphabet(frozenset((SymbolStr('0'), SymbolStr('1'))))

a = input_alphabet['a']
b = input_alphabet['b']
c = input_alphabet['c']

# Define states
stateA = MooreState("State A", output_alphabet['0'])
stateB = MooreState("State B", output_alphabet['1'])
stateC = MooreState("State C", output_alphabet['1'])

# Add transitions
stateA.add_transition(a, stateA)
stateA.add_transition(b, stateB)
stateB.add_transition(a, stateC)
stateB.add_transition(c, stateB)

# Hole state
hole_state = MooreState(name="hole", value=SymbolStr('0'))

moore_machine = MooreMachineAutomaton(input_alphabet, output_alphabet, stateA, set([stateA, stateB, stateC]), ComparisonStrategy, "moore machine with 3 states", [DotExportingMMStrategy])

# Moore machine is generated in the directory ./output_models/moore_machines/
moore_machine.export('./output_models/moore_machines/')
```

### Moore machine generator

#### Dependencies 

```python
from pythautomata.utilities.nicaud_mm_generator import generate_moore_machine
```

#### Implementation

```python
# Amount of states
size = 20

generated_moore_machine = generate_moore_machine(input_alphabet, output_alphabet, size)

# Moore machine is generated in the directory ./output_models/moore_machines/
generated_moore_machine.export('./output_models/moore_machines/')
```

### DFA - moore machine comparator and converter

#### Dependencies 

```python
from pythautomata.model_comparators.moore_machine_comparison_strategy import MooreMachineComparisonStrategy
from pythautomata.utilities.automata_converter import AutomataConverter
from pythautomata.automata_definitions.tomitas_grammars import TomitasGrammars
from pythautomata.automata_definitions.sample_moore_machines import SampleMooreMachines
```

#### Implementation

```python
# Tomitas DFA example
tomitas = TomitasGrammars.get_automaton_1()

# Convert DFA to moore machine
converted_moore_machine = AutomataConverter.convert_dfa_to_moore_machine(tomitas)

# Tomitas moore machine
tomitas_moore_machine = SampleMooreMachines.get_tomitas_automaton_1()

# Comparate automatons
converted_moore_machine.__eq__(tomitas_moore_machine)
```
