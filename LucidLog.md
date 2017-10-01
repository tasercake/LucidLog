# Learning Lucid

> ###### 28 Sep
>
> At first glance, the Mojo board looks a lot like an Arduino. Here's hoping it's going to be just as easy to learn!

## First Steps

##### Setting up the Mojo IDE

Straightforward. Download and install the Mojo IDE, then create a new project from the base template at any location.

##### Setting up ISE

ISE is required in order to build the project and program the FPGA. Follow these steps to get it set up on Windows:

1. Create an account at https://www.xilinx.com/registration/create-account.html and activate it.
2. From https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/design-tools.html, download the *Split Installer Base* and the 3 *Data* files.
3. Extract the *first* file and run the installer inside it.

## Lucid Modules

> Lucid modules appear to be defined fairly similarly to JSim subcircuits.

They consist of the following components:

1. `module` statement
2. *(optional)* comma-separated parameter list
3. port list
4. body

Modules have signals that go in and out of them. Signals are explained in more detail later.

Modules can be *instantiated* (like a *subcircuit* in JSim or a *class* in any OOP).

### Parameters

If present, the parameter list must be of the format `#(---parameters---)` after the `module` statement.

#### Parameter Default Values

Optionally, each parameter may be assigned a default value in the parameter list. If no default value is provided, a value must be provided when the module is used. The assignment of a default value is very python-like.

**Syntax:**

```verilog
module module_name #( PARAM = <value> )
```

#### Parameter Constraints

For additional safety, parameters can be given constraints, which, when violated, will throw errors.

**Syntax:**

```verilog
module module_name #( PARAM : <constraint> )
```

### Module Ports

Every signal in/out of the module must be declared when defining the module.

#### Signals

Signals can be of 3 types:

- *Inputs*: read-only inside module.
- *Outputs:* write-only inside module.
- *InOuts:* Rarely used.

> ###### 30 Sep
>
> Lucid seems like a higher level language than I'd initially expected. Syntactically, it feels a lot like Java.

### Module Body

Module bodies often contain the following:

- Variable declarations
- Module initialisations
- 'Always' blocks



## Common Data Types(?)

### Numbers

Lucid supports 3 different radix (number bases):

- **Decimal**
  - Base 10
  - Number written as-is like *9*, or with prefix ***d*** like `d1234`
- **Binary**
  - Base 2
  - Number written with prefix ***b*** like `b100110`
- **Hexadecimal**
  - Base 16
  - Number written with prefix ***h*** like `A3F`

###### Specifying bit width of a number

By default, a number only takes up the minimum number of binary bits required to represent it.

It is possible to specify the exact width of a number in bits using a number prefix *before* the radix prefix. For example, to represent `d32` using 8 bits, write it as `8d32`.

Specifying a bit width lower than the minimum number of bits will drop the *most significant* bits and raise a warning.

> ^ This isn't the same as truncating a number.

For example, `4b11001` will be converted to `b1001`

### Strings

A string is a series of characters. A character is a series of 8 bits.

Use double quotes (`""`) to represent a string.

#### Every string is an array

*If a string contains 1 character*, it is a 1d array. Indexing a 1-character string will give you the ***bit*** at that index of the bit-representation of the character.

*If a string contains >1 character*, it is a 2d array. Indexing a >1-character string will give you the ***character*** at that index of the string.

> ***Indexing works differently in Lucid***:
>
> In Java and Python, arrays start from the ***left***-most element.
>
> In Lucid, arrays start from the ***right***-most element.
>
> In `const STRING = "string";`**,** `STRING[0]` is `"g"`

### Signals

A `sig` is used to *temporarily* store the value of an operation.

> ***All variable names must start with a lowercase letter***

Signals can be arrays.

**Syntax:**

```verilog
sig signal1[10];
sig singal2;
```

### D-type Flip Flops

`dff` is the basic memory element in an FPGA, for storage of a constant value between clock cycles.

A `dff` requires a clock signal to be specified at instantiation.

#### Instantiation

A `dff` has 3 inputs:

- *clk*: Clock
- *rst*: Reset
- *d*: Input value to flip-flop

A `dff` has one output `q` that reflects the value of `d` from the *previous* rising clock edge.

A `dff` has one *(optional)* parameter `INIT` that sets the initial value of `q` after the first reset. Default is 0.

> Inputs and outputs are accessed the same way you would access a member of a class in Java or Python - using a dot. For example:
>
> ```verilog
> out = flipflop.q;
> ```

A `dff` can be an array.

***Syntax:***

```verilog
dff ctr[8](.clk(clk), .rst(rst));
```

### Finite State Machines

An `fsm` has the same 3 inputs, the same output *q*, and the same parameter *INIT* as a `dff`.

A list of possible states must be provided at instantiation. By default, *INIT* is set to the first state.

***Syntax:***

```verilog
fsm state(.clk(clk), #INIT(RUN)) = {IDLE, START, RUN, STOP};
state.d = state.START;
```

FSM state names must start with an uppercase letter.

### Variables

Currently, `var` only has use as the index for a loop. A `var` does not have a representation in the circuit.

A `var` can be an array.

***Syntax:***

```vhdl
var index;
```

### Constants

A `const` is used to replace a number with a name.

A `const` cannot be an array.

***Syntax:***

```verilog
const MAX_VALUE = 100;
```

### Arrays

Arrays can be used to create multi-bit signals.

> Arrays in Lucid are used very similarly to arrays in Java.

### Bit Selectors

Bit selectors are used to select a sub-array of an array, just like *slicing* in Python.

> The only difference between this and Python slices is that in Lucid, the slice indices are inclusive.

***Syntax:***

```verilog
// Equivalent selectors:
array[3:8]; // select bits 3 to 8 (inclusive)
array[3+:6]; // select the next 6 bits up from 3 (inclusive)
array[8-:6]; // select the 6 bits down from 8 (inclusive) 
```

