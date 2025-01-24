# lab01- sumador 
## CARLOS ANDRÉS ESCOBAR BEJARANO

## informe de laboratorio

Acá detallo el funcionamiento de dos implementaciones de un sumador binario de un bit: `sum1bcc.v` y `sum1bcc_primitive.v`. Los sumadores son bloques fundamentales en sistemas digitales, utilizados en operaciones aritméticas y circuitos de mayor complejidad, como unidades aritmético-lógicas (ALU).

La diferencia más notable es su enfoque, uno emplea registros para manejar el resultado, mientras que la otra utiliza puertas lógicas primitivas. A continuación, se explica la lógica de cada código y el rol de cada instrucción.

---

### Código 1: `sum1bcc.v`
```verilog
module sum1bcc (A, B, Ci, Cout, S);

  input  A;
  input  B;
  input  Ci;
  output Cout;
  output S;

  reg [1:0] st;   // REGISTRO QUE GUARDA LA SUMA COMPLETA (Cout y S)
  assign S = st[0];   // El bit menos significativo de "st" se asigna a la salida S
  assign Cout = st[1]; // El bit más significativo de "st" se asigna a la salida Cout

  always @ ( * ) begin
  	st = A + B + Ci; // Suma combinacional de las entradas A, B y Ci
  end

endmodule
```
![Sumador](C:\Users\caran\Documents\GitHub\lab01-2024-2-carlosunal\imagenes\Sum1normal.jpg)



#### Explicación del Funcionamiento
1. **Entradas**: `A`, `B` y `Ci` representan los bits de entrada y el acarreo de entrada, respectivamente.
2. **Salidas**: 
   - `S` es el bit de suma resultante (menos significativo).
   - `Cout` es el acarreo de salida (más significativo).
3. **Registro `st`**:
   - Es un registro de 2 bits que almacena el resultado completo de la suma binaria.
   - El bit 0 (`st[0]`) corresponde a la suma (`S`), y el bit 1 (`st[1]`) corresponde al acarreo (`Cout`).
4. **Bloque combinacional**:
   - La instrucción `st = A + B + Ci` realiza la suma de las entradas y guarda el resultado en `st`.

#### Lógica
El módulo utiliza un registro para manejar la operación aritmética y divide el resultado entre suma y acarreo. Es una implementación sencilla que abstrae la suma sin detallar el funcionamiento interno de las puertas lógicas.

---

### Código 2: `sum1bcc_primitive.v`
```verilog
module sum1bcc_primitive (A, B, Ci, Cout, S);

  input  A;
  input  B;
  input  Ci;
  output Cout;
  output S;

  wire a_ab;    // Resultado de la operación AND entre A y B
  wire x_ab;    // Resultado de la operación XOR entre A y B
  wire cout_t;  // Acarreo temporal de la operación XOR y Ci

  and(a_ab, A, B);       // Operación AND entre A y B
  xor(x_ab, A, B);       // Operación XOR entre A y B

  xor(S, x_ab, Ci);      // La suma S es el XOR entre x_ab y Ci
  and(cout_t, x_ab, Ci); // Acarreo temporal entre x_ab y Ci

  or (Cout, cout_t, a_ab); // La salida Cout es la combinación del acarreo temporal y a_ab
endmodule
```
![Sumador Primitivo](C:\Users\caran\Documents\GitHub\lab01-2024-2-carlosunal\imagenes\Sum1Primitivo.jpg)


#### Explicación del Funcionamiento
1. **Entradas**: `A`, `B` y `Ci` son los bits de entrada y el acarreo de entrada.
2. **Salidas**:
   - `S` es el bit de suma calculado usando operaciones XOR.
   - `Cout` es el acarreo de salida calculado combinando operaciones AND y OR.
3. **Cálculos intermedios**:
   - `a_ab`: Resultado de la operación AND entre `A` y `B`.
   - `x_ab`: Resultado de la operación XOR entre `A` y `B`.
   - `cout_t`: Acarreo temporal entre `x_ab` y `Ci`.
4. **Lógica**:
   - La suma (`S`) se obtiene con una doble operación XOR.
   - El acarreo (`Cout`) es el resultado de combinar el acarreo parcial (`cout_t`) y el AND de las entradas `A` y `B`.

---

Ambas implementaciones cumplen con la misma función de sumar tres bits y generar una salida de suma y un acarreo. Pero de este laboratorio se rescató que `sum1bcc_primitive.v` proporciona un modelo más detallado del funcionamiento interno, siendo más representativo del hardware real. Por otro lado, de los experimentos se pudo notar que `sum1bcc.v` es más sencillo y legible, ideal para aplicaciones donde el diseño no requiere granularidad en las operaciones lógicas.











### sumador 
