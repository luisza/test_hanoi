# Reto: El Cajero Automático — Banco Tres Patitos

## Contexto

El Banco Tres Patitos necesita una aplicación de cajero automático. Se le solicita desarrollar el sistema completo según los requisitos descritos a continuación.

---

## Requisitos funcionales

### 1. Pantalla de solicitud de monto

El sistema debe mostrar una pantalla inicial donde el usuario ingrese el monto que desea retirar.

- El monto debe ser un número entero positivo expresado en colones.
- El sistema no verifica si el cliente tiene fondos disponibles; se asume que siempre los tiene.

---

### 2. Algoritmo de desglose de billetes

El cajero dispone únicamente de billetes de **₡2 000**, **₡5 000** y **₡10 000**.

El sistema debe entregar el monto exacto solicitado usando la **menor cantidad total de billetes posible**. Cuando existan varias combinaciones con el mismo número de billetes, se debe **priorizar el uso de billetes de mayor denominación**.

#### Regla de imposibilidad

Si no existe ninguna combinación de billetes de ₡2 000, ₡5 000 y ₡10 000 que sume exactamente el monto solicitado, la transacción falla y se muestra un mensaje de error (ver sección 4).



#### Ejemplos de montos válidos

| Monto solicitado | Billetes entregados | Total billetes |
|-----------------|---------------------|----------------|
| ₡10 000 | 1×₡10 000 | 1 |
| ₡24 000 | 2×₡10 000 + 2×₡2 000 | 4 |
| ₡11 000 | 1×₡5 000 + 3×₡2 000 | 4 |
| ₡16 000 | 1×₡10 000 + 3×₡2 000 | 4 |
| ₡21 000 | 1×₡10 000 + 1×₡5 000 + 3×₡2 000 | 5 |
| ₡27 000 | 2×₡10 000 + 1×₡5 000 + 1×₡2 000 | 4 |
| ₡29 000 | 2×₡10 000 + 1×₡5 000 + 2×₡2 000 | 5 |

#### Ejemplos de montos inválidos (error)

| Monto solicitado | Motivo |
|-----------------|--------|
| ₡3 000 | No existe combinación exacta con las denominaciones disponibles |
| ₡1 000 | Menor que el billete más pequeño (₡2 000) |
| ₡7 000 | No existe combinación exacta (6 000 + 1 000 no funciona; 5 000 + 2 000 = 7 000 ✓) |

> **Aclaración sobre ₡7 000:** 1×₡5 000 + 1×₡2 000 = ₡7 000 sí es válido. El monto ₡3 000 es el ejemplo canónico de error porque no tiene solución posible.

---

### 3. Mensaje de transacción exitosa

Tras una transacción exitosa, se debe mostrar un mensaje con el siguiente formato:

```
Su dinero es: X billetes de 2000, Y billetes de 5000 y Z billetes de 10000.
```

- Solo se mencionan las denominaciones cuya cantidad sea mayor que cero.
- Si solo hay una denominación, no se usa "y":
  - `Su dinero es: 5 billetes de 2000.`
- Si hay exactamente dos denominaciones, se unen con "y":
  - `Su dinero es: 1 billete de 5000 y 1 billete de 10000.`
- Si hay tres denominaciones, se usa coma entre las primeras dos y "y" antes de la última:
  - `Su dinero es: 2 billetes de 2000, 1 billete de 5000 y 1 billete de 10000.`
- Se acepta el uso del plural de forma uniforme (ej. "1 billetes de 10000" es válido).

---

### 4. Mensaje de transacción fallida

Si el monto no puede entregarse exactamente con las denominaciones disponibles, se debe mostrar:

```
Lo sentimos, no es posible entregar el monto de ₡X con los billetes disponibles (₡2000, ₡5000, ₡10000). Por favor ingrese un monto diferente.
```

---

### 5. Flujo de la aplicación

```
[Pantalla de ingreso de monto]
        |
        v
[Procesamiento del desglose]
        |
   +----+----+
   |         |
[Éxito]   [Error]
   |         |
   v         v
[Mostrar  [Mostrar
 billetes]  error]
   |         |
   +----+----+
        |
        v
[Volver a la pantalla de ingreso de monto]
```

Después de mostrar el resultado (sea éxito o error), el sistema regresa automáticamente a la pantalla de ingreso de monto.

---

### 6. Bitácora de transacciones

Cada transacción ejecutada debe quedar registrada en una bitácora con al menos los siguientes campos:

- Fecha y hora de la transacción
- Monto solicitado
- Resultado: exitosa o fallida
- Detalle del desglose (en caso de éxito): cantidad de cada denominación entregada

---

### 7. Visor de bitácoras (área administrativa)

Debe existir una pantalla separada que liste todas las bitácoras registradas.

- **Acceso protegido:** El visor de bitácoras requiere autenticación (usuario y contraseña). Las credenciales pueden ser hardcodeadas o configurables; lo importante es que el acceso no sea público.
- El listado debe mostrar al menos: fecha/hora, monto, resultado y detalle del desglose.

---

## Resumen de casos de prueba sugeridos

| Monto | Resultado esperado |
|-------|--------------------|
| ₡2 000 | 1×₡2 000 |
| ₡5 000 | 1×₡5 000 |
| ₡10 000 | 1×₡10 000 |
| ₡4 000 | 2×₡2 000 |
| ₡7 000 | 1×₡5 000 + 1×₡2 000 |
| ₡11 000 | 1×₡5 000 + 3×₡2 000 |
| ₡16 000 | 1×₡10 000 + 3×₡2 000 |
| ₡21 000 | 1×₡10 000 + 1×₡5 000 + 3×₡2 000 |
| ₡27 000 | 2×₡10 000 + 1×₡5 000 + 1×₡2 000 |
| ₡29 000 | 2×₡10 000 + 1×₡5 000 + 2×₡2 000 |
| ₡3 000 | Error: monto no entregable |
| ₡1 000 | Error: monto no entregable |
| ₡0 o negativo | Error: monto inválido |
