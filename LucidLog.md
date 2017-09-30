# Learning Lucid

### 28 Sep

> At first glance, the Mojo looks a lot like the Arduino. Here's hoping it's going to be just as easy to learn!

## First Steps

##### Setting up the Mojo IDE

Straightforward. Download and install the Mojo IDE, then create a new project from the base template at any location.

##### Setting up ISE

ISE is required in order to build the project and program the FPGA. Follow these steps to get it set up on Windows:

1. Create an account at https://www.xilinx.com/registration/create-account.html and activate it.
2. From https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/design-tools.html, download the *Split Installer Base* and the 3 *Data* files.
3. ​

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

Optionally, each parameter may be assigned a default value in the parameter list. If no default value is provided, a value must be provided when the module is used. The assignment of a default value is very python-like.

**Syntax:**	`module module_name #( PARAM = <value> )`

#### Parameter Constraints

For additional safety, parameters can be given constraints, which, when violated, will throw errors.

**Syntax: **`module module_name #( PARAM : <constraint> )`

### Module Ports

Every signal in/out of the module must be declared when defining the module.

#### Signals

Signals can be of 3 types:

       	1. *Inputs*: read-only inside module.
        	2. *Outputs:* write-only inside module.
         	3. *InOuts:* Rarely used.

### Module Body

