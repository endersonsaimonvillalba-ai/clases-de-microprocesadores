# Explicación general del código
La clase TransformadaFourier implementa la DFT de forma explícita. Su objetivo es recibir una señal (lista de números) y devolver una lista de coeficientes complejos 
𝑋
 que representan la energía y fase en cada frecuencia discreta. A continuación se explica cada parte del código, qué hace, por qué y qué debes vigilar al usarla.

## Constructor
__init__(self, señal)

Qué hace: guarda la señal en self.señal y su longitud en self.N.

Por qué importa: N define el número de frecuencias que se calcularán y controla los bucles en dft.

Consideraciones prácticas: validar que señal sea una lista o iterable numérico y que N > 0 para evitar divisiones por cero o bucles vacíos.

## Método dft
dft(self)

Propósito: calcular la DFT usando la fórmula discreta, devolviendo una lista X de longitud N con números complejos.

## Paso a paso operativo:

Inicializa X = [] para almacenar los coeficientes.

Bucle externo for k in range(self.N): itera cada índice de frecuencia 
𝑘
 (0..N-1).

Dentro, inicializa suma = 0 (o 0+0j para asegurar tipo complejo).

Bucle interno for n in range(self.N): recorre cada muestra n de la señal.

Calcula angulo = -2j * math.pi * k * n / self.N: factor complejo que depende de k y n.

Actualiza suma += self.señal[n] * cmath.exp(angulo): multiplica la muestra por la exponencial compleja y acumula.

Tras el bucle interno, hace X.append(suma).

Al terminar todos los k, asigna self.transformada = X y return X.

Tipos y consistencia: usar 0+0j para suma evita conversiones implícitas; cmath.exp devuelve complejo.

Complejidad: tiempo 𝑂(𝑁2)
 por los dos bucles anidados; para señales grandes es lento.

Función abs_complejo_formula
abs_complejo_formula(z)

Qué hace: devuelve la magnitud de z como número complejo con parte imaginaria 0.

## Comportamiento detallado:

Si z es complex: calcula 
Re(𝑧)2+Im(𝑧)2.

Si no es complejo: aplica abs(z) (útil si z ya es real).

Devuelve el resultado convertido a tipo complejo (ej. complex(magnitud, 0)).

Por qué devolver complejo: mantiene consistencia de tipos cuando se trabaja con listas de coeficientes complejos; sin embargo, normalmente la magnitud es real, así que convertir a complejo es una decisión de diseño (puede confundir si se espera un float).

```python
import math
import cmath

class TransformadaFourier:
    def init(self, señal: list):
        """
        Inicializa la clase con una señal (lista de números).
        """
        self.señal = señal
        self.N = len(señal)
        self.transformada = None

    def dft(self) -> list:
        """
        Calcula la Transformada Discreta de Fourier (DFT) de la señal.
        """
        X = []
        for k in range(self.N):
            suma = 0
            for n in range(self.N):
                angulo = -2j * math.pi * k * n / self.N
                suma += self.señal[n] * cmath.exp(angulo)
            X.append(suma)
        self.transformada = X
        return X

    def abs_complejo_formula(z: complex) -> complex:
        """
        Calcula el valor absoluto de un número complejo y lo devuelve como complejo.
        """
        if isinstance(z, complex):
            a = z.real
            b = z.imag
            magnitud = math.sqrt(a*a + b*b)
            return complex(magnitud, 0)
        try:
            return complex(abs(z), 0)
        except Exception as e:
            raise TypeError(f"No se pudo calcular |z| para tipo {type(z)}: {e}")
```