# Ejercicio Funciones

## Caso 1: Cálculo de Bonificaciones de Empleados
```sql
CREATE TABLE empleados (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50),
    salario DECIMAL(10,2)
);

INSERT INTO empleados (nombre, salario) VALUES
('Juan Pérez', 1500.00),
('Ana Gómez', 3000.00),
('Carlos Ruiz', 6000.00);

DELIMITER //

CREATE FUNCTION calcular_bonificacion(salario DECIMAL(10,2)) 
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    DECLARE bonificacion DECIMAL(10,2);

    IF salario < 2000 THEN
        SET bonificacion = salario * 0.10;
    ELSEIF salario >= 2000 AND salario <= 5000 THEN
        SET bonificacion = salario * 0.07;
    ELSE
        SET bonificacion = salario * 0.05;
    END IF;

    RETURN bonificacion;
END //

DELIMITER ;

SELECT 
    nombre,
    salario,
    calcular_bonificacion(salario) AS bonificacion
FROM empleados;
```

## Caso 2: Cálculo de Edad de Clientes
```sql
CREATE TABLE clientes (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50),
    fecha_nacimiento DATE
);

INSERT INTO clientes (nombre, fecha_nacimiento) VALUES
('Luis Martínez', '1990-06-15'),
('María López', '1985-09-20'),
('Pedro Gómez', '2000-03-10');

DELIMITER //

CREATE FUNCTION calcular_edad(fecha_nacimiento DATE) 
RETURNS INT
DETERMINISTIC
BEGIN
    RETURN TIMESTAMPDIFF(YEAR, fecha_nacimiento, CURDATE());
END //

DELIMITER ;

SELECT 
    nombre,
    fecha_nacimiento,
    calcular_edad(fecha_nacimiento) AS Edad
FROM clientes;
```

## Caso 3: Formatear Números de Teléfono
```sql
CREATE TABLE contactos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50),
    telefono VARCHAR(10)
);

INSERT INTO contactos (nombre, telefono) VALUES
('Maria Cristina', '3153318966'),
('Fabio Hernandez', '3158325271'),
('Sebastian', '3154559242');

INSERT INTO clientess (nombre, fecha_nacimiento) VALUES
('Luis Martínez', '1990-06-15'),
('María López', '1985-09-20'),
('Pedro Gómez', '2000-03-10');

DELIMITER //

CREATE FUNCTION formatear_telefono(telefono VARCHAR(10))
RETURNS VARCHAR(14)
DETERMINISTIC
BEGIN
    RETURN CONCAT('(', SUBSTRING(telefono, 1, 3), ') ', SUBSTRING(telefono, 4, 3), '-', SUBSTRING(telefono, 7, 4));
END //

DELIMITER ;

SELECT 
    nombre,
    formatear_telefono(telefono) AS telefono_formateado
FROM contactos;
```

## Caso 4: Clasificación de Productos por Precio
```sql
CREATE TABLE productos (
    id_producto INT PRIMARY KEY AUTO_INCREMENT,
    nombre_producto VARCHAR(100),
    precio DECIMAL(10, 2)
);

INSERT INTO productos (nombre_producto, precio)
VALUES 
('Laptop', 799.99),
('Cámara Digital', 120.50),
('Auriculares', 45.00),
('Teclado', 25.75),
('Smartphone', 350.00),
('Reloj Inteligente', 199.99),
('Altavoces Bluetooth', 55.99),
('Monitor', 299.00),
('Mochila', 35.50),
('Tablet', 180.00);

DELIMITER //

CREATE FUNCTION clasificar_precio(precio DECIMAL(10,2)) RETURNS VARCHAR(10)
DETERMINISTIC
BEGIN
    DECLARE resultado VARCHAR(10);

    SET resultado = CASE
        WHEN precio < 50 THEN 'Bajo'
        WHEN precio BETWEEN 50 AND 200 THEN 'Medio'
        ELSE 'Alto'
    END;

    RETURN resultado;
END //

DELIMITER ;

SELECT 
    nombre_producto,
    precio,
    clasificar_precio(precio) AS Clasificación_Precio
FROM productos;
```