# PyModelExtractor <!-- TOC ignore:true -->

The aim of this document is to guide the user in the usage of the Neural Checker tool.

<!-- TOC -->

- [PyModelExtractor](#pymodelextractor)
    - [Install pymodelextractor](#install-pymodelextractor)
    - [Observation Table Algorithms](#observation-table-algorithms)
        - [Moore machine L*](#moore-machine-l)
            - [Dependencies](#dependencies)
            - [Implementation](#implementation)

<!-- /TOC -->

The PyModelExtractor is a Python framework for the active learning of Automata in a black box context.

## Install pymodelextractor

```python
%pip install pymodelextractor
```

## Observation Table Algorithms

### Moore machine L*

#### Dependencies

```python
from pythautomata.automata.moore_machine_automaton import MooreMachineAutomaton
from pythautomata.model_comparators.moore_machine_comparison_strategy import MooreMachineComparisonStrategy
from pymodelextractor.learners.observation_table_learners.mm_lstar_learner import MMLStarLearner as MooreMachineLearner
from pymodelextractor.teachers.moore_machines_teacher import MooreMachineTeacher as MMTeacher
from pythautomata.utilities.nicaud_mm_generator import generate_moore_machine
from pythautomata.base_types.alphabet import Alphabet
```

#### Implementation
```python
# Obtain the Moore Machine Learner that implements the L* algorithm
def get_mm_learner():
    return MooreMachineLearner()

# Obtain the teacher for the Moore Machine Learner
def get_mm_teacher(automaton: MooreMachineAutomaton) -> MMTeacher:
    return MMTeacher(automaton, MooreMachineComparisonStrategy())

def test_500_states_moore_machine():
    # Generate a random Moore Machine with 500 states
    input_alphabet = Alphabet.from_strings(["a", "b", "c"])
    output_alphabet = Alphabet.from_strings(['0', '1'])
    seed = 100
    automata = generate_moore_machine(input_alphabet, output_alphabet, 500, seed)

    # Learn the generated Moore Machine
    teacher = get_mm_teacher(automata)
    result = get_mm_learner().learn(teacher, verbose=True) # verbose=True prints the learning process
    
    # Show that the learned Moore Machine is equivalent to the generated Moore Machine
    print("Are equal: ", MooreMachineComparisonStrategy().are_equivalent(
        result.model, automata))

    # Export the learned Moore Machine
    result.model.export('./output_models/mm_lstar/')


test_500_states_moore_machine()
```