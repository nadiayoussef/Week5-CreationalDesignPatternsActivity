# Week 5 Class Activity: Creational Design Patterns

In this activity, we will start with a version of the calculator that we developed during the previous class activity on Test Driven Development.  Your task is to modify the code to introduce Creational Design Patterns.

## Key Ideas

In this activity, we will follow these steps:
1. Refactor the code to introduce a design pattern.
2. Update the tests accordingly and run them to make sure that they pass.
3. Refactor code to remove duplication/inefficiencies.

## Starter Code Overview

To enforce some consistency across implementations (and allow us to build on the same code in the next few activities), we will start with a simple solution that we developed during the previous class activity on Test-Driven Development. 

The bulk of the implementation in this activity is in 2 files, namely `src/models/calculator.model.ts` and `src/models/calculator.model.spec.ts`.
We have enforced high level constraints on the implementation through the `ICalculatorModel` interface at `src/interfaces/calculator-model.interface.ts`.
The interface requires the existence of 4 methods on the CalculatorModel as shown below:

```typescript
import { ActionKeys } from '../enums/action-keys.enum';
import { NumericKeys } from '../enums/numeric-keys.enum';
import { OperatorKeys } from '../enums/operator-keys.enum';

export interface ICalculatorModel {
  // numeric key pressed <0, 1, 2, 3, 4, 5, 6, 7, 8, 9>
  pressNumericKey(key: NumericKeys): void;

  // operator key pressed <+, -, *, />
  pressOperatorKey(key: OperatorKeys): void;

  // action key pressed <C, =, .>
  pressActionKey(key: ActionKeys): void;

  // returns the contents of the calculator's display
  display(): string;

}

```

The interface assumes that there are handlers for 3 types of key presses: numeric (0 through 9), operators (+, -, *, /), actions (C, =, .).
Additionally, there is a display method which returns the current value on the calculator screen.
The values accepted by the different keys can be modified in `src/enums/*.enum.ts`

To get you started, we have provided an implementation this interface called CalculatorModel at `src/models/calculator.model.ts`. Please begin by examining the code. Then run the tests and confirm that they pass.

## Getting Started

To begin the activity, run `npm install` in the root of the starter code (location where package.json is present).
Once the installation is complete, run the test commands `npm run test`. You should see the output as below:

```bash
$ npm run test

> week-5-creational-DP@1.0.0 test
> node scripts/jest.js

 PASS  src/models/calculator.model.spec.ts
  CalculatorModel
    ✓ should contain a CalculatorModel class that implements ICalculatorModel (2 ms)
    ✓ should have an empty display on init
    ✓ should display `1` when the `1` key is pressed
    ✓ should display `2` when the `2` key is pressed
    ✓ should display `98` when the `9` key is pressed followed by the `8` key
    ✓ should display `13` when equals is clicked on `7 + 6`
    ✓ should display `5` when equals is clicked on `15 - 10`
    ✓ should display `21` when equals is clicked on `3 * 7` (1 ms)
    ✓ should display `12` when equals is clicked on `144 / 12` (1 ms)

 PASS  src/index.spec.ts
  week5-creational-DP
    CalculatorModel
      ✓ CalculatorModel exists (1 ms)

Test Suites: 2 passed, 2 total
Tests:       10 passed, 10 total
Snapshots:   0 total
Time:        0.503 s, estimated 1 s
Ran all test suites.
```

## Abstract Factory

Let's suppose that we want to support two types of calculators:
- a `StandardCalculator` that provides the functionality that exists now, and
- a `RoundingCalculator` that rounds up values (e.g., 4/3 would evaluate to 1.33 if two decimals of precision are used)

### Step 1
  - rename class `CalculatorModel` to `StandardCalculatorModel`
  - update the tests and ensure that they still pass by running them

### Step 2  
  - define a class `AbstractCalculatorModelFactory` that declares/defines methods that are common to both calculators,
  - define a class `StandardCalculatorModelFactory`, which extends `AbstractCalculatorModelFactory`, for creating a `StandardCalculatorModel`
  - update the tests to use the factory to create a `StandardCalculatorModel` object instead of calling its constructor directly
  - confirm that the tests pass

### Step 3
  - create a class `RoundingCalculator` that implements the rounding calculator, giving its constructor a parameter `nrDecimals` that represents the number of decimals to use
  - implement a `RoundingCalculatorModelFactory`, which extends `AbstractCalculatorModelFactory`, for creating a `RoundingCalculatorModel`
  - write a few tests that use `RoundingCalculatorModel` and `RoundingCalculatorModelFactory`

### Step 4
  - apply the `Singleton` design pattern to `StandardCalculatorModelFactory` 
  - ensure that your tests still pass
    (Hint: you will likely find it necessary to reset the calculator's state after each test by pressing the `C` action key)

For these steps, you are encouraged to work in TDD-style as much as possible, e.g., by writing tests before you fully implement your `RoundingCalculator`

## Submission

In the root directory, run the command `npm run zip`. This command will generate a zip file called `submission.zip`. Upload the `submission.zip` file to Gradescope and tag your partner on Gradescope on the submission.
