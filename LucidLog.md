# Learning Lucid

###### 28 Sep

> At first glance, the Mojo board looks a lot like an Arduino. Here's hoping it's going to be just as easy to learn!

## First Steps

##### Setting up the Mojo IDE

Straightforward. Download and install the Mojo IDE, then create a new project from the base template at any location.

##### Setting up ISE

ISE is required in order to build the project and program the FPGA. Follow these steps to get it set up on Windows:

1. Create an account at https://www.xilinx.com/registration/create-account.html and activate it.
2. From https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/design-tools.html, download the *Split Installer Base* and the 3 *Data* files.
3. Extract the *first* file and run the installer.

## Lucid Modules

Lucid modules appear to be defined fairly similarly to JSim subcircuits.

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

###### 30 Sep

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

> ***INDEXING WORKS DIFFERENTLY IN LUCID***
>
> In Java and Python, arrays start from the ***left***-most element.
>
> In Lucid, arrays start from the ***right***-most element.
>
> In `const STRING = "string";`**,** `STRING[0]` is `"g"`

### Signals

A `sig` is used to *temporarily* store the value of an operation.

Signal names must start with a lowercase letter.

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

