

### **Day 36: Module Instantiation in SystemVerilog**

---

#### **1. Overview**

Module instantiation in SystemVerilog is the process of **creating instances of modules** inside other modules or testbenches. It allows a **hierarchical structure**, enabling modular and reusable hardware design.

By instantiating a module, you can integrate smaller design components into a larger system (e.g., instantiating an ALU inside a processor).

---

#### **2. Syntax for Module Instantiation**

```systemverilog
<module_name> <instance_name> (
    .<port_name>(signal_name),
    ...
);
```

- `module_name`: Name of the module you are instantiating.
- `instance_name`: Unique name for this specific instance.
- `.port_name(signal_name)`: Port-to-signal connection (named association).

---

#### **3. Example: Simple Instantiation**

Assume the following module:

```systemverilog
module and_gate (
    input  logic a,
    input  logic b,
    output logic y
);
    assign y = a & b;
endmodule
```

You can instantiate it like this in another module:

```systemverilog
module top;
    logic a, b, y;

    // Module instantiation
    and_gate u1 (
        .a(a),
        .b(b),
        .y(y)
    );
endmodule
```

---

#### **4. Positional vs Named Port Mapping**

##### a) **Named Port Mapping** (recommended)

```systemverilog
and_gate u1 (
    .a(a),
    .b(b),
    .y(y)
);
```

- Clear and readable.
- Less error-prone, especially if port order changes.

##### b) **Positional Port Mapping**

```systemverilog
and_gate u1 (a, b, y);
```

- Shorter but **error-prone**.
- Not recommended for large designs.

---

#### **5. Multiple Instantiations**

You can instantiate the same module multiple times with different instance names:

```systemverilog
and_gate and1 (.a(a1), .b(b1), .y(y1));
and_gate and2 (.a(a2), .b(b2), .y(y2));
```

---

#### **6. Instantiating with Parameters**

Modules can be parameterized using the `#()` syntax.

**Parameterized Module Example:**

```systemverilog
module adder #(parameter WIDTH = 8) (
    input  logic [WIDTH-1:0] a,
    input  logic [WIDTH-1:0] b,
    output logic [WIDTH-1:0] sum
);
    assign sum = a + b;
endmodule
```

**Instantiation with parameter override:**

```systemverilog
adder #(.WIDTH(16)) adder_16bit (
    .a(a),
    .b(b),
    .sum(sum)
);
```

---

#### **7. Arrays of Module Instances**

Useful when you want to instantiate the same module multiple times programmatically.

```systemverilog
module bit_and (
    input logic a,
    input logic b,
    output logic y
);
    assign y = a & b;
endmodule

module top;
    logic [3:0] a, b, y;

    // Array of instances
    bit_and u_and[3:0] (
        .a(a),
        .b(b),
        .y(y)
    );
endmodule
```

All array elements are connected bit-wise.

---

#### **8. Hierarchical Referencing**

Once a module is instantiated, its internal signals can be accessed using dot notation (though this is **not recommended** in good design practice except for debugging).

```systemverilog
$display("Internal signal: %0b", top.u1.internal_signal);
```

---

#### **9. Guidelines for Clean Instantiation**

- Use **named port mapping** for clarity.
- Give **descriptive instance names** (e.g., `alu_inst`, `regfile1`).
- Avoid accessing internal signals of submodules from the outside (breaks encapsulation).
- Use parameters for configurable modules.
- Group related signals into interfaces (later topics) to simplify port connections.

---

#### **10. Common Pitfalls**

- **Signal width mismatch** between port and connected wire
- **Wrong port order** in positional mapping
- **Forgetting to connect all ports**, leading to synthesis/simulation issues
- Using **non-unique instance names**
- Reusing **testbench code inside design hierarchy** by mistake

---

#### **11. Summary**

- Module instantiation allows you to **reuse and build hierarchical designs**.
- Use **named mapping** for safety and readability.
- **Parameterization** increases reusability and flexibility.
- Grouping multiple instantiations or using interfaces helps maintain scalable design.
