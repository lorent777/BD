# Sistema de Base de Datos - Cafetería "Buen Sabor"

## 📋 Índice
1. [Caso de Estudio](#1-caso-de-estudio-ce--cafetería-buen-sabor)
2. [Modelo Entidad-Relación (MER)](#2-modelo-entidadrelación-mer)
3. [Tablas SQL y Explicación](#3-tablas-sql-y-explicación)
4. [Explicación Clara de los Elementos SQL Utilizados](#4-explicación-clara-de-los-elementos-sql-utilizados)
5. [Relaciones entre Tablas](#5-relaciones-entre-tablas)
6. [Justificación General del Diseño](#6-justificación-general-del-diseño)
7. [Datos de Ejemplo](#7-datos-de-ejemplo)
8. [Consultas SQL Útiles](#8-consultas-sql-útiles)

---

# **1. Caso de Estudio (CE) — Cafetería "Buen Sabor"**

La cafetería necesita un sistema básico para almacenar información sobre:

* Los productos que vende.
* Los clientes que compran.
* Los empleados que atienden.
* Las ventas realizadas.
* Los detalles de cada venta (productos y cantidades).

Con esta información se pueden generar reportes como:

* Productos más vendidos.
* Empleado con más ventas.
* Ventas por fecha.

Este caso sirve para construir el **MER** y las **tablas SQL**.

---

# **2. Modelo Entidad–Relación (MER)**

## **Relaciones y cardinalidades**

1. **Cliente — Venta**

   * Cliente: 1
   * Venta: N

2. **Empleado — Venta**

   * Empleado: 1
   * Venta: N

3. **Venta — DetalleVenta**

   * Venta: 1
   * DetalleVenta: N

4. **Producto — DetalleVenta**

   * Producto: 1
   * DetalleVenta: N

## **Llaves foráneas obligatorias**

* En **Venta**:

  * `idCliente`
  * `idEmpleado`

* En **DetalleVenta**:

  * `idVenta`
  * `idProducto`

---

# **3. Tablas SQL y explicación**

---

## **Tabla 1: Cliente**

```sql
CREATE TABLE Cliente (
    id_Cliente INTEGER PRIMARY KEY AUTOINCREMENT,
    nombre TEXT NOT NULL,
    telefono TEXT
);
```

**Representa:**
Clientes de la cafetería.

**Decisiones importantes:**

* `id_Cliente`: identificador único, se genera solo.
* `nombre`: obligatorio.
* `telefono`: tipo TEXT (un teléfono puede tener guiones, espacios o empezar con cero).

---

## **Tabla 2: Empleado**

```sql
CREATE TABLE Empleado (
    id_Empleado INTEGER PRIMARY KEY AUTOINCREMENT,
    nombre TEXT NOT NULL,
    cargo TEXT NOT NULL
);
```

**Representa:**
Empleados de la cafetería.

**Decisiones importantes:**

* `id_Empleado`: PK.
* `nombre`, `cargo`: obligatorios.

---

## **Tabla 3: Producto**

```sql
CREATE TABLE Producto (
    id_Producto INTEGER PRIMARY KEY AUTOINCREMENT,
    nombre TEXT NOT NULL,
    precio REAL NOT NULL,
    categoria TEXT NOT NULL
);
```

**Representa:**
Productos que se venden (cafés, postres, etc.).

**Decisiones importantes:**

* `precio`: tipo REAL para valores decimales.
* `categoria`: necesaria para clasificar productos.

---

## **Tabla 4: Venta**

```sql
CREATE TABLE Venta (
    id_Venta INTEGER PRIMARY KEY AUTOINCREMENT,
    fecha TEXT NOT NULL,
    id_Cliente INTEGER NOT NULL,
    id_Empleado INTEGER NOT NULL,
    FOREIGN KEY (id_Cliente) REFERENCES Cliente(id_Cliente),
    FOREIGN KEY (id_Empleado) REFERENCES Empleado(id_Empleado)
);
```

**Representa:**
Una venta realizada en la cafetería.

**Decisiones importantes:**

* `id_Cliente` → FK hacia Cliente.
* `id_Empleado` → FK hacia Empleado.
* Permite reportes como ventas por cliente o ventas por empleado.

---

## **Tabla 5: DetalleVenta**

```sql
CREATE TABLE DetalleVenta (
    id_Detalle INTEGER PRIMARY KEY AUTOINCREMENT,
    id_Venta INTEGER NOT NULL,
    id_Producto INTEGER NOT NULL,
    cantidad INTEGER NOT NULL,
    subtotal REAL NOT NULL,
    FOREIGN KEY (id_Venta) REFERENCES Venta(id_Venta),
    FOREIGN KEY (id_Producto) REFERENCES Producto(id_Producto)
);
```

**Representa:**
Cada ítem de una venta (líneas del ticket).

**Decisiones importantes:**

* `id_Venta` → FK hacia Venta.
* `id_Producto` → FK hacia Producto.
* `cantidad` y `subtotal` permiten cálculos y reportes.

---

# **4. Explicación clara de los elementos SQL utilizados**

| Elemento SQL      | Significado                 | Para qué sirve                                        |
| ----------------- | --------------------------- | ----------------------------------------------------- |
| **PRIMARY KEY**   | Llave primaria              | Identificador único. No se repite ni puede ser NULL.  |
| **AUTOINCREMENT** | Generación automática       | Crea IDs consecutivos sin que el usuario los escriba. |
| **NOT NULL**      | Valor obligatorio           | Evita filas incompletas.                              |
| **TEXT**          | Cadena de texto             | Para nombres, teléfonos, categorías.                  |
| **REAL**          | Número decimal              | Para precios y subtotales.                            |
| **FOREIGN KEY**   | Llave foránea               | Conecta tablas entre sí.                              |
| **REFERENCES**    | Tabla a la que apunta la FK | Garantiza integridad referencial.                     |

---

# **5. Relaciones entre tablas**

1. **Venta → Cliente**
   Cada venta pertenece a un cliente válido.

2. **Venta → Empleado**
   Cada venta la realiza un empleado válido.

3. **DetalleVenta → Venta**
   Cada detalle pertenece a una venta existente.

4. **DetalleVenta → Producto**
   Cada detalle registra un producto existente.

---

# **6. Justificación general del diseño**

* IDs únicos permiten buscar registros sin errores.
* Llaves foráneas aseguran relaciones correctas.
* Tipos de datos correctos (TEXT, REAL) evitan inconsistencias.
* NOT NULL exige datos completos.
* El diseño cumple los requisitos del caso de estudio y soporta reportes, JOINs y consultas avanzadas.

---

# **7. Datos de Ejemplo**

## **Inserción de Clientes (20 registros)**

```sql
INSERT INTO Cliente (nombre, telefono) VALUES
('María González', '0991234567'),
('Juan Pérez', '0987654321'),
('Ana Rodríguez', '0995551234'),
('Carlos Mendoza', '0981234567'),
('Lucía Fernández', '0993456789'),
('Pedro Sánchez', '0989876543'),
('Sofía Castro', '0992345678'),
('Diego Torres', '0986543210'),
('Valentina Ruiz', '0994567890'),
('Andrés López', '0988765432'),
('Camila Ortiz', '0990123456'),
('Miguel Herrera', '0985432109'),
('Isabella Morales', '0997654321'),
('Gabriel Romero', '0983210987'),
('Martina Vargas', '0996789012'),
('Sebastián Cruz', '0982109876'),
('Emma Jiménez', '0998901234'),
('Mateo Flores', '0984321098'),
('Mía Guzmán', '0991111111'),
('Lucas Ramírez', '0992222222');
```

```sql
INSERT INTO Cliente (nombre) VALUES
('Lorena Lopez')
```

---

## **Inserción de Empleados (20 registros)**

```sql
INSERT INTO Empleado (nombre, cargo) VALUES
('Roberto Moreno', 'Barista'),
('Laura Vega', 'Cajera'),
('Javier Salazar', 'Gerente'),
('Patricia Nunez', 'Barista'),
('Fernando Silva', 'Cajero'),
('Claudia Rojas', 'Mesera'),
('Ricardo Medina', 'Barista'),
('Monica Paredes', 'Cajera'),
('Esteban Delgado', 'Cocinero'),
('Daniela Molina', 'Mesera'),
('Alejandro Rios', 'Barista');


INSERT INTO Empleado (nombre, cargo, telefono) VALUES
('Veronica Campos', 'Cajera', '555-3344'),
('Raul Guerrero', 'Supervisor', '555-5566'),
('Teresa Navarro', 'Barista', '555-7788'),
('Hugo Peña', 'Mesero', '555-9900'),
('Gloria Marin', 'Cajera', '555-2211'),
('Oscar Reyes', 'Barista', '555-4433'),
('Adriana Luna', 'Mesera', '555-6655'),
('Victor Diaz', 'Cocinero', '555-8877'),
('Sandra Ibarra', 'Cajera', '555-0099'),
('Pablo Torres', 'Barista', '555-3345'),
('Natalia Flores', 'Mesera', '555-5567');


```

---

## **Inserción de Productos (20 registros)**

```sql
INSERT INTO Producto (nombre, precio, categoria) VALUES
('Cafe Americano', 2.50, 'Bebidas Calientes'),
('Cappuccino', 3.50, 'Bebidas Calientes'),
('Latte', 3.80, 'Bebidas Calientes'),
('Espresso', 2.00, 'Bebidas Calientes'),
('Mocha', 4.00, 'Bebidas Calientes'),
('Te Verde', 2.20, 'Bebidas Calientes'),
('Chocolate Caliente', 3.00, 'Bebidas Calientes'),
('Jugo Natural', 3.50, 'Bebidas Frias'),
('Smoothie', 4.50, 'Bebidas Frias'),
('Limonada', 2.50, 'Bebidas Frias'),
('Croissant', 2.80, 'Panaderia'),
('Brownie', 3.20, 'Postres'),
('Cheesecake', 4.50, 'Postres'),
('Galletas', 1.80, 'Postres'),
('Sandwich de Pollo', 5.50, 'Comida'),
('Ensalada Cesar', 6.00, 'Comida'),
('Panini', 5.00, 'Comida'),
('Muffin', 2.50, 'Panaderia'),
('Donas', 2.00, 'Panaderia'),
('Waffle', 4.80, 'Postres');
```

---

## **Inserción de Ventas (20 registros)**

```sql
INSERT INTO Venta (fecha, id_Cliente, id_Empleado) VALUES
('2025-11-20 08:30', 1, 1),
('2025-11-20 09:15', 2, 2),
('2025-11-20 10:00', 3, 1),
('2025-11-20 10:45', 4, 3),
('2025-11-20 11:30', 5, 4),
('2025-11-20 12:15', 6, 2),
('2025-11-20 13:00', 7, 5),
('2025-11-20 14:30', 8, 1),
('2025-11-20 15:15', 9, 6),
('2025-11-20 16:00', 10, 4),
('2025-11-21 08:45', 11, 7),
('2025-11-21 09:30', 12, 2),
('2025-11-21 10:15', 13, 8),
('2025-11-21 11:00', 14, 3),
('2025-11-21 12:30', 15, 9),
('2025-11-21 13:45', 16, 5),
('2025-11-21 14:30', 17, 10),
('2025-11-21 15:15', 18, 1),
('2025-11-21 16:00', 19, 2),
('2025-11-21 17:00', 20, 4);
```

---

## **Inserción de Detalles de Venta (27 registros)**

```sql
--calcular manualmente los subtotales de cada detalle de venta
INSERT INTO DetalleVenta (id_Venta, id_Producto, cantidad, subtotal) VALUES
(1, 1, 2, 5.00),        -- Café Americano 2 x 2.50
(1, 11, 1, 2.80),       -- Croissant 1 x 2.80
(2, 2, 1, 3.50),        -- Cappuccino 1 x 3.50
(2, 12, 2, 6.40),       -- Brownie 2 x 3.20
(3, 3, 1, 3.80),        -- Latte 1 x 3.80
(3, 14, 3, 5.40),       -- Galletas 3 x 1.80
(4, 5, 1, 4.00),        -- Mocha 1 x 4.00
(4, 15, 1, 5.50),       -- Sándwich de Pollo 1 x 5.50
(5, 8, 2, 7.00),        -- Jugo Natural 2 x 3.50
(6, 1, 1, 2.50),        -- Café Americano 1 x 2.50
(6, 13, 1, 4.50),       -- Cheesecake 1 x 4.50
(7, 7, 2, 6.00),        -- Chocolate Caliente 2 x 3.00
(8, 4, 3, 6.00),        -- Espresso 3 x 2.00
(9, 9, 1, 4.50),        -- Smoothie 1 x 4.50
(9, 20, 1, 4.80),       -- Waffle 1 x 4.80
(10, 6, 1, 2.20),       -- Té Verde 1 x 2.20
(10, 16, 1, 6.00),      -- Ensalada César 1 x 6.00
(11, 2, 2, 7.00),       -- Cappuccino 2 x 3.50
(12, 10, 1, 2.50),      -- Limonada 1 x 2.50
(13, 17, 1, 5.00),      -- Panini 1 x 5.00
(14, 3, 1, 3.80),       -- Latte 1 x 3.80
(15, 19, 2, 5.00),      -- Donas 2 x 2.50
(16, 5, 1, 4.00),       -- Mocha 1 x 4.00
(17, 1, 1, 2.50),       -- Café Americano 1 x 2.50
(18, 18, 2, 5.00),      -- Muffin 2 x 2.50
(19, 15, 1, 5.50),      -- Sándwich de Pollo 1 x 5.50
(20, 12, 1, 3.20);      -- Brownie 1 x 3.20

```

---

# **8. Consultas SQL Útiles**

## **Productos más vendidos**

```sql
SELECT 
    p.nombre,
    SUM(dv.cantidad) AS total_vendido,
    SUM(dv.subtotal) AS ingresos_totales
FROM DetalleVenta dv
JOIN Producto p ON dv.id_Producto = p.id_Producto
GROUP BY p.id_Producto
ORDER BY total_vendido DESC
LIMIT 10;
```

---

## **Empleado con más ventas**

```sql
SELECT 
    e.nombre,
    e.cargo,
    COUNT(v.id_Venta) AS total_ventas,
    SUM(dv.subtotal) AS ingresos_generados
FROM Empleado e
JOIN Venta v ON e.id_Empleado = v.id_Empleado
JOIN DetalleVenta dv ON v.id_Venta = dv.id_Venta
GROUP BY e.id_Empleado
ORDER BY ingresos_generados DESC;
```

---

## **Ventas por fecha**

```sql
SELECT 
    DATE(fecha) AS dia,
    COUNT(id_Venta) AS num_ventas,
    SUM(subtotal) AS total_dia
FROM Venta v
JOIN DetalleVenta dv ON v.id_Venta = dv.id_Venta
GROUP BY DATE(fecha)
ORDER BY dia DESC;
```

---

## **Clientes más frecuentes**

```sql
SELECT 
    c.nombre,
    c.telefono,
    COUNT(v.id_Venta) AS visitas,
    SUM(dv.subtotal) AS total_gastado
FROM Cliente c
JOIN Venta v ON c.id_Cliente = v.id_Cliente
JOIN DetalleVenta dv ON v.id_Venta = dv.id_Venta
GROUP BY c.id_Cliente
ORDER BY visitas DESC
LIMIT 10;
```

---

## **Reporte completo de una venta**

```sql
SELECT 
    v.id_Venta,
    v.fecha,
    c.nombre AS cliente,
    e.nombre AS empleado,
    p.nombre AS producto,
    dv.cantidad,
    dv.subtotal
FROM Venta v
JOIN Cliente c ON v.id_Cliente = c.id_Cliente
JOIN Empleado e ON v.id_Empleado = e.id_Empleado
JOIN DetalleVenta dv ON v.id_Venta = dv.id_Venta
JOIN Producto p ON dv.id_Producto = p.id_Producto
WHERE v.id_Venta = 1;
```

---

**Creado para:** Cafetería "Buen Sabor"  
**Base de Datos:** SQLite  
**Versión:** 1.0  
**Fecha:** Diciembre 2024