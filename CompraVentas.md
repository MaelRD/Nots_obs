### 1.  Ajustar los tipos de dato, según las características del SMBD y ejecutar todos los comandos DDL adjuntos.

```sql
-- Creación de la tabla Personal
CREATE TABLE Personal (
  NumSS INT NOT NULL,  -- Clave primaria sin cambios
  Nombre VARCHAR(45) NULL,  -- Cambiado a VARCHAR para admitir texto
  ApePaterno VARCHAR(45) NULL,  -- Cambiado a VARCHAR
  ApeMaterno VARCHAR(45) NULL,  -- Cambiado a VARCHAR
  TelPersonal VARCHAR(15) NULL,  -- Cambiado de INT a VARCHAR(15) para números telefónicos
  TelCasa VARCHAR(15) NULL,  -- Lo mismo de arriba
  FechaContrato DATE NULL,  -- Tipo de dato DATE para almacenar fechas
  FrecuenciaPago ENUM('Semana', 'Quincena') NULL,  -- Uso de ENUM para limitar las opciones
  SueldoBase DECIMAL(7,2) NULL,  -- DECIMAL para valores monetarios con 2 decimales
  PRIMARY KEY (NumSS)  -- Llave primaria
);

-- Creación de la tabla Clientes
CREATE TABLE Clientes (
  RFC VARCHAR(13) NOT NULL,  -- RFC usualmente tiene longitud fija
  Nombre VARCHAR(45) NULL,  -- Cambiado a VARCHAR
  ApePaterno VARCHAR(45) NULL,  -- Cambiado a VARCHAR
  ApeMaterno VARCHAR(45) NULL,  -- Cambiado a VARCHAR
  Calle VARCHAR(50) NULL,  -- Ajuste a 50 caracteres para dirección
  Num VARCHAR(10) NULL,  -- Cambiado a VARCHAR para admitir números y letras
  Colonia VARCHAR(45) NULL,  -- Cambiado a VARCHAR
  CP VARCHAR(10) NULL,  -- Cambiado de INT a VARCHAR(10) para códigos postales con letras o guiones
  Alcaldia VARCHAR(45) NULL,  -- Cambiado a VARCHAR
  PRIMARY KEY (RFC)  -- Llave primaria
);

-- Creación de la tabla Proveedores
CREATE TABLE Proveedores (
  RFC VARCHAR(13) NOT NULL,  -- RFC usualmente tiene longitud fija
  NomEmpresa VARCHAR(45) NULL,  -- Cambiado a VARCHAR
  NombreRepresentante VARCHAR(45) NULL,  -- Cambiado a VARCHAR
  ApeP_Representante VARCHAR(45) NULL,  -- Cambiado a VARCHAR
  TelContacto VARCHAR(15) NULL,  -- Cambiado de INT a VARCHAR(15)
  DiaPedido ENUM('Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes', 'Sábado', 'Domingo') NULL,  -- Uso de ENUM para restringir días de pedido
  DiaEntrega ENUM('Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes', 'Sábado', 'Domingo') NULL,  -- Uso de ENUM para restringir días de entrega
  TipoPago ENUM('Efectivo', 'Transferencia') NULL,  -- Uso de ENUM para tipos de pago
  PRIMARY KEY (RFC)  -- Llave primaria
);

-- Creación de la tabla Productos
CREATE TABLE Productos (
  CodProd INT NOT NULL AUTO_INCREMENT,  -- AUTO_INCREMENT para generar el código del producto
  DescripL VARCHAR(45) NULL,  -- Descripción larga como VARCHAR
  DescripC VARCHAR(20) NULL,  -- Descripción corta como VARCHAR
  PrecioV DECIMAL(7,2) NULL,  -- DECIMAL para precio de venta con 2 decimales
  Existencias INT NULL,  -- Usamos INT para cantidades enteras
  StockMin INT NULL,  -- Usamos INT para el stock mínimo
  StockMax INT NULL,  -- Usamos INT para el stock máximo
  Oferta TINYINT(1) NULL,  -- TINYINT(1) para booleanos (1 = sí, 0 = no)
  PRIMARY KEY (CodProd)  -- Llave primaria
);

-- Creación de la tabla Ventas
CREATE TABLE Ventas (
  NumV INT NOT NULL,  -- Llave primaria para número de venta
  Fecha DATE NULL,  -- Tipo DATE para la fecha de la venta
  Hora TIME NULL,  -- Tipo TIME para la hora de la venta
  Subtotal DECIMAL(7,2) NULL,  -- DECIMAL para subtotal con 2 decimales
  IVA DECIMAL(7,2) NULL,  -- DECIMAL para IVA con 2 decimales
  Total DECIMAL(7,2) NULL,  -- DECIMAL para total con 2 decimales
  Clientes_RFC VARCHAR(13) NULL,  -- Se relaciona con el RFC de la tabla Clientes
  Personal_NumSS INT NOT NULL,  -- Se relaciona con la tabla Personal
  PRIMARY KEY (NumV),  -- Llave primaria
  FOREIGN KEY (Clientes_RFC) REFERENCES Clientes(RFC),  -- Llave foránea hacia Clientes
  FOREIGN KEY (Personal_NumSS) REFERENCES Personal(NumSS)  -- Llave foránea hacia Personal
);

-- Creación de la tabla ProductosVentas
CREATE TABLE ProductosVentas (
  Cantidad INT NULL,  -- Cantidad de productos en la venta
  PrecioV DECIMAL(7,2) NULL,  -- Precio de venta por producto
  Subtotal_Prod DECIMAL(7,2) NULL,  -- Subtotal por producto
  Ventas_NumV INT NOT NULL,  -- Llave foránea hacia la tabla Ventas
  Productos_CodProd INT NOT NULL,  -- Llave foránea hacia la tabla Productos
  PRIMARY KEY (Ventas_NumV, Productos_CodProd),  -- Llave primaria compuesta
  FOREIGN KEY (Ventas_NumV) REFERENCES Ventas(NumV),  -- Llave foránea hacia Ventas
  FOREIGN KEY (Productos_CodProd) REFERENCES Productos(CodProd)  -- Llave foránea hacia Productos
);

-- Creación de la tabla Compras
CREATE TABLE Compras (
  NumC INT NOT NULL AUTO_INCREMENT,  -- Llave primaria con AUTO_INCREMENT para número de compra
  FechaHoraP DATETIME NULL,  -- Tipo DATETIME para la fecha y hora del pedido
  FechaHoraE DATETIME NULL,  -- Tipo DATETIME para la fecha y hora de la entrega
  TipoPago ENUM('Efectivo', 'Transferencia') NULL,  -- Uso de ENUM para restringir los tipos de pago
  Subtotal DECIMAL(7,2) NULL,  -- DECIMAL para subtotal con 2 decimales
  IVA DECIMAL(7,2) NULL,  -- DECIMAL para IVA con 2 decimales
  Total DECIMAL(7,2) NULL,  -- DECIMAL para total con 2 decimales
  Proveedores_RFC VARCHAR(13) NULL,  -- Llave foránea hacia la tabla Proveedores
  PRIMARY KEY (NumC),  -- Llave primaria
  FOREIGN KEY (Proveedores_RFC) REFERENCES Proveedores(RFC)  -- Llave foránea hacia Proveedores
);

-- Creación de la tabla ProductosCompras
CREATE TABLE ProductosCompras (
  Cantidad INT NULL,  -- Cantidad de productos en la compra
  PrecioC DECIMAL(7,2) NULL,  -- Precio de compra por producto
  Subtotal_Prod DECIMAL(7,2) NULL,  -- Subtotal por producto
  Compras_NumC INT NOT NULL,  -- Llave foránea hacia la tabla Compras
  Productos_CodProd INT NOT NULL,  -- Llave foránea hacia la tabla Productos
  PRIMARY KEY (Compras_NumC, Productos_CodProd),  -- Llave primaria compuesta
  FOREIGN KEY (Compras_NumC) REFERENCES Compras(NumC),  -- Llave foránea hacia Compras
  FOREIGN KEY (Productos_CodProd) REFERENCES Productos(CodProd)  -- Llave foránea hacia Productos
);

```

### 2. Cambiar el nombre de las tablas “ProductosVentas” y “ProductosCompras”, por “productosv” y “productosc” respectivamente.

Para cambiar el nombre de las tablas de `ProductosVentas` y `ProductosCompras` a `productosv` y `productosc`, respectivamente, puedes usar el comando `RENAME` en MySQL. A continuación te dejo el código que realiza este cambio, junto con anotaciones para que sepas qué se está haciendo:

```sql
-- Cambiar el nombre de la tabla ProductosVentas a productosv
RENAME TABLE ProductosVentas TO productosv;

-- Cambiar el nombre de la tabla ProductosCompras a productosc
RENAME TABLE ProductosCompras TO productosc;
```

1. **`RENAME TABLE ProductosVentas TO productosv;`**: Se utiliza el comando `RENAME TABLE` seguido del nombre actual de la tabla (`ProductosVentas`) y luego el nuevo nombre (`productosv`), de manera similar, este comando cambia el nombre de la tabla `ProductosCompras` a `productosc`.

### 3. Agregar a la tabla de personal las siguientes columnas:
El comando `ALTER` en SQL se utiliza para modificar la estructura de una tabla existente sin necesidad de eliminarla y volver a crearla. 
#### a. Puesto de tipo varchar (10)
```sql
ALTER TABLE personal
ADD COLUMN Puesto VARCHAR(15);
```
#### b. H_entrada de tipo time

```sql
ALTER TABLE personal
ADD COLUMN H_entrada TIME;
```
#### c. H_salida de tipo time
```sql
ALTER TABLE personal
ADD COLUMN H_salida TIME;
```

### 4. Ejecutar los comandos de inserción anexos y en caso de presentarse algún error, corregir indicando lo realizado.

```sql
/*Insert en personal*/
/*Corrección: Campo "FecuenciaPago" corregido a "FrecuenciaPago"*/
INSERT INTO Personal (NumSS,Nombre,ApePaterno,ApeMaterno,TelPersonal,TelCasa,FechaContrato,FrecuenciaPago,SueldoBase,Puesto,h_entrada,h_salida)
 VALUES (167,'Hilda','Guzmán','Reyes',55456789,21572456,'2015-05-16','Semana',1200.00,'Ayudante','07:35:30','12:36:19');
INSERT INTO Personal (NumSS,Nombre,ApePaterno,ApeMaterno,TelPersonal,TelCasa,FechaContrato,FrecuenciaPago,SueldoBase,Puesto,h_entrada,h_salida)
 VALUES (321,'Erendira','Del Valle','Merino',55123456,21573520,'1995-01-01','Quincena',3000.00,'Administrador','07:35:31','10:36:19');
INSERT INTO Personal (NumSS,Nombre,ApePaterno,ApeMaterno,TelPersonal,TelCasa,FechaContrato,FrecuenciaPago,SueldoBase,Puesto,h_entrada,h_salida)
 VALUES (456,'Luis','Garcia','Becerril',55234567,21571234,'1995-01-01','Quincena',3000.00,'Administrador','07:35:31','15:36:19');
INSERT INTO Personal (NumSS,Nombre,ApePaterno,ApeMaterno,TelPersonal,TelCasa,FechaContrato,FrecuenciaPago,SueldoBase,Puesto,h_entrada,h_salida)
 VALUES (687,'Veronica','Ordoñes','Flores',55001234,21579874,'2010-07-01','Semana',1000.00,'Ayudante','07:35:31','09:36:19');
INSERT INTO Personal (NumSS,Nombre,ApePaterno,ApeMaterno,TelPersonal,TelCasa,FechaContrato,FrecuenciaPago,SueldoBase,Puesto,h_entrada,h_salida)
 VALUES (897,'Cristina','Monroy','Jimenez',55345678,21571265,'2016-01-16','Semana',1200.00,'Ayudante','07:35:31','16:36:19');

/*Insert en clientes*/
/*Corrección: Nombres "Guzmá¡n" y "PeÃ±a" corregidos a "Guzmán" y "Peña"*/
INSERT INTO Clientes (RFC,Nombre,ApePaterno,ApeMaterno,Calle,Num,Colonia,CP,Alcaldia)
 VALUES ('CARH810911','Hilda','Guzmán','Reyes','Los Pinos',29,'La Peña',15230,'Milpa Alta');
INSERT INTO Clientes (RFC,Nombre,ApePaterno,ApeMaterno,Calle,Num,Colonia,CP,Alcaldia)
 VALUES ('DOME751202','Erick','Dominguez','Morales','El Mirador',12,'Tejomulco',15225,'Milpa Alta');
INSERT INTO Clientes (RFC,Nombre,ApePaterno,ApeMaterno,Calle,Num,Colonia,CP,Alcaldia)
 VALUES ('GUJA770724','Adriana','Guzmán','Jimenez','Gladiolas',30,'Jazmin',15220,'Milpa Alta');
INSERT INTO Clientes (RFC,Nombre,ApePaterno,ApeMaterno,Calle,Num,Colonia,CP,Alcaldia)
 VALUES ('REAM850330','Mauricio','Reyes','Aguilar','Las Flores',15,'La Peña',15230,'Milpa Alta');
INSERT INTO Clientes (RFC,Nombre,ApePaterno,ApeMaterno,Calle,Num,Colonia,CP,Alcaldia)
 VALUES ('VIPI800215','Israel','Vigueras','Perez','Aldama',48,'Jazmin',15220,'Milpa Alta');

/*Insert en proveedores*/
INSERT INTO Proveedores (RFC,NomEmpresa,NombreRepresentante,ApeP_Representante,TelContacto,DiaPedido,DiaEntrega,TipoPago)
 VALUES ('CCF911030','Coca-Cola','Esteban','Cruz',55999911,'Jueves','Jueves','Transferencia');
INSERT INTO Proveedores (RFC,NomEmpresa,NombreRepresentante,ApeP_Representante,TelContacto,DiaPedido,DiaEntrega,TipoPago)
 VALUES ('CPC650101','Sabritas,Gamesa,Pepsi','Enrique','Marin',55999922,'Jueves','Viernes','Efectivo');
INSERT INTO Proveedores (RFC,NomEmpresa,NombreRepresentante,ApeP_Representante,TelContacto,DiaPedido,DiaEntrega,TipoPago)
 VALUES ('GBI600203','Bimbo','Juan','Alcazar',55999999,'Lunes','Martes','Efectivo');

/*Insert en productos*/
INSERT INTO Productos(CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (1,'Pan tostado clásico','Pan T Clásico',35.00,8,1,10,1);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (2,'Pan integral grande','Pan Int Grnd',45.00,4,1,10,1);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (3,'Roles de canela glaseados','Roles Glas',20.00,5,1,10,0);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (4,'Panquecitos con chispas sabor chocolate','Panques Chispas Choc',25.00,5,1,10,0);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (5,'Donitas espolvoreadas','Donas Espolv',20.00,6,1,10,0);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (6,'Coca cola sabor original 235 ml botella de v','CocaCola vidrio 235',10.00,20,1,10,1);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (7,'Coca cola sabor original 600 ml no retornable','CocaCola 600',15.00,15,1,10,1);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (8,'Fanta sabor naranja 354 ml lata','Fanta lata',13.00,30,1,10,0);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (9,'Coca cola light 354 ml lata','CocaCola L lata',13.00,10,1,10,NULL);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (10,'Coca cola light 600 ml no retornable','CocaCola L 600',15.00,20,1,10,0);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (11,'Papas Sabritas adobadas 170 gr','PapasAdibadas ch',15.00,15,1,10,1);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (12,'Fritos limón 180 gr','Fritos ch',15.00,16,1,10,NULL);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (13,'Rufles de queso 200 gr','RuflesQ ch',15.00,17,1,10,NULL);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (14,'Churrumais 200 gr','Churrumais ch',15.00,18,1,10,NULL);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (15,'Gamesa Emperador de chocolate 200gr','Emperador choc ch',20.00,5,1,10,0);
INSERT INTO Productos (CodProd,DescripL,DescripC,PrecioV,Existencias,StockMin,StockMax,Oferta) VALUES (16,'Gamesa Emperador piruetas limón 200gr','Emperador limón ch',20.00,6,1,10,1);

/*Insert en ventas*/
/*Verificación: Clientes_RFC NULL, asegurarse que sea permitido en la tabla*/
INSERT INTO Ventas (NumV,Fecha,Hora,Subtotal,IVA,Total,Clientes_RFC,Personal_NumSS)
 VALUES (1,'2023-01-13','10:36:00',220.00,35.20,255.20,'CARH810911',167);
INSERT INTO Ventas (NumV,Fecha,Hora,Subtotal,IVA,Total,Clientes_RFC,Personal_NumSS)
 VALUES (2,'2023-01-14','11:36:00',120.00,19.20,139.20,'DOME751202',321);
INSERT INTO Ventas (NumV,Fecha,Hora,Subtotal,IVA,Total,Clientes_RFC,Personal_NumSS)
 VALUES (3,'2023-01-15','12:36:00',99.48,15.92,115.40,'GUJA770724',456);
INSERT INTO Ventas (NumV,Fecha,Hora,Subtotal,IVA,Total,Clientes_RFC,Personal_NumSS)
 VALUES (4,'2023-01-17','09:21:00',159.48,25.52,185.00,NULL,456);
INSERT INTO Ventas (NumV,Fecha,Hora,Subtotal,IVA,Total,Clientes_RFC,Personal_NumSS)
 VALUES (5,'2023-01-18','13:46:00',159.48,25.52,185.00,'REAM850330',321);

/*Insert en productosv*/
INSERT INTO productosv (Cantidad,PrecioV,Subtotal_Prod,Ventas_NumV,Productos_CodProd) VALUES (1,35.00,35.00,1,1);
INSERT INTO productosv (Cantidad,PrecioV,Subtotal_Prod,Ventas_NumV,Productos_CodProd) VALUES (2,20.00,40.00,1,3);
INSERT INTO productosv (Cantidad,PrecioV,Subtotal_Prod,Ventas_NumV,Productos_CodProd) VALUES (3,13.00,39.00,1,8);
INSERT INTO productosv (Cantidad,PrecioV,Subtotal_Prod,Ventas_NumV,Productos_CodProd) VALUES (1,45.00,45.00,2,2);
INSERT INTO productosv (Cantidad,PrecioV,Subtotal_Prod,Ventas_NumV,Productos_CodProd) VALUES (2,20.00,40.00,2,5);
INSERT INTO productosv (Cantidad,PrecioV,Subtotal_Prod,Ventas_NumV,Productos_CodProd) VALUES (2,25.00,50.00,3,4);
INSERT INTO productosv (Cantidad,PrecioV,Subtotal_Prod,Ventas_NumV,Productos_CodProd) VALUES (1,35.00,35.00,4,1);
INSERT INTO productosv (Cantidad,PrecioV,Subtotal_Prod,Ventas_NumV,Productos_CodProd) VALUES (3,20.00,60.00,4,3);
INSERT INTO productosv (Cantidad,PrecioV,Subtotal_Prod,Ventas_NumV,Productos_CodProd) VALUES (3,10.00,30.00,4,6);
INSERT INTO productosv (Cantidad,PrecioV,Subtotal_Prod,Ventas_NumV,Productos_CodProd) VALUES (4,15.00,60.00,4,7);

/*Insert en compras*/
INSERT INTO Compras (NumC,FechaHoraP,FechaHoraE,TipoPago,Subtotal,IVA,Total,Proveedores_RFC)
 VALUES (1050,'2022-01-20 00:00:00','2022-01-21 00:00:00','Efectivo',2198.28,351.72,2550.00,'CCF911030');
INSERT INTO Compras (NumC,FechaHoraP,FechaHoraE,TipoPago,Subtotal,IVA,Total,Proveedores_RFC)
 VALUES (1119,'2022-01-15 00:00:00','2022-01-15 00:00:00','Transferencia',1482.76,237.24,1720.00,'CPC650101');

/*Insert en productosc*/
INSERT INTO productosc (Cantidad,PrecioC,Subtotal_Prod,Compras_NumC,Productos_CodProd) VALUES (50,7.00,350.00,1050,6);
INSERT INTO productosc (Cantidad,PrecioC,Subtotal_Prod,Compras_NumC,Productos_CodProd) VALUES (50,12.00,600.00,1050,7);
INSERT INTO productosc (Cantidad,PrecioC,Subtotal_Prod,Compras_NumC,Productos_CodProd) VALUES (50,10.00,500.00,1050,8);
INSERT INTO productosc (Cantidad,PrecioC,Subtotal_Prod,Compras_NumC,Productos_CodProd) VALUES (50,10.00,500.00,1050,9);
INSERT INTO productosc (Cantidad,PrecioC,Subtotal_Prod,Compras_NumC,Productos_CodProd) VALUES (50,12.00,600.00,1050,10);
INSERT INTO productosc (Cantidad,PrecioC,Subtotal_Prod,Compras_NumC,Productos_CodProd) VALUES (20,13.00,260.00,1119,11);
INSERT INTO productosc (Cantidad,PrecioC,Subtotal_Prod,Compras_NumC,Productos_CodProd) VALUES (20,13.00,260.00,1119,12);
INSERT INTO productosc (Cantidad,PrecioC,Subtotal_Prod,Compras_NumC,Productos_CodProd) VALUES (20,13.00,260.00,1119,13);
INSERT INTO productosc (Cantidad,PrecioC,Subtotal_Prod,Compras_NumC,Productos_CodProd) VALUES (20,13.00,260.00,1119,14);
INSERT INTO productosc (Cantidad,PrecioC,Subtotal_Prod,Compras_NumC,Productos_CodProd) VALUES (20,17.00,340.00,1119,15);
INSERT INTO productosc (Cantidad,PrecioC,Subtotal_Prod,Compras_NumC,Productos_CodProd) VALUES (20,17.00,340.00,1119,16);

```

### 5.  Insertar un nuevo registro en las tablas de: Clientes, Proveedores y Personal.

```sql
/* Insertar nuevo registro en la tabla clientes */
INSERT INTO clientes (RFC, Nombre, ApePaterno, ApeMaterno, Calle, Num, Colonia, CP, Alcaldia)
VALUES ('NUEVO123456', 'Luis', 'Martínez', 'Sánchez', 'Reforma', 45, 'Centro', 16000, 'Cuauhtémoc');

/* Insertar nuevo registro en la tabla proveedores */
INSERT INTO proveedores (RFC, NomEmpresa, NombreRepresentante, ApeP_Representante, TelContacto, DiaPedido, DiaEntrega, TipoPago)
VALUES ('NUEVO987654', 'Comercializadora XYZ', 'Ana', 'García', 55998877, 'Lunes', 'Martes', 'Cheque');

/* Insertar nuevo registro en la tabla personal */
INSERT INTO personal (NumSS, Nombre, ApePaterno, ApeMaterno, TelPersonal, TelCasa, FechaContrato, FecuenciaPago, SueldoBase, Puesto, h_entrada, h_salida)
VALUES (123, 'Carlos', 'Lopez', 'Morales', 55667788, 21578901, '2024-09-01', 'Quincena', 2500.00, 'Contador', '08:00:00', '17:00:00');

```
### 6. Crear una consulta que muestre: número de seguro social, nombre completo del empleado (en una sola columna), número de ventas registradas por empleado y monto en ventas registradas por empleado. Se deben mostrar a todos los empleados registrados, hayan o no registrado ventas.
```sql
SELECT 
    p.NumSS,
    CONCAT(p.Nombre, ' ', p.ApePaterno, ' ', p.ApeMaterno) AS NombreCompleto,
    COUNT(v.NumV) AS NumeroVentas,
    COALESCE(SUM(v.Total), 0) AS MontoVentas
FROM 
    personal p
LEFT JOIN 
    ventas v ON p.NumSS = v.Personal_NumSS
GROUP BY 
    p.NumSS, 
    p.Nombre, 
    p.ApePaterno, 
    p.ApeMaterno;
```

![[Pasted image 20240911234448.png]]
### 7. Crear una vista con la consulta anterior que se llame: “V_ventasXemp”.

```sql
-- Crear una vista llamada 'V_ventasXemp'
CREATE VIEW V_ventasXemp AS
-- Seleccionar el número de seguro social del empleado
SELECT 
    p.NumSS,
    -- Concatenar nombre, apellido paterno y apellido materno para formar el nombre completo del empleado
    CONCAT(p.Nombre, ' ', p.ApePaterno, ' ', p.ApeMaterno) AS NombreCompleto,
    -- Contar el número de ventas registradas por cada empleado
    COUNT(v.NumV) AS NumeroVentas,
    -- Sumar el monto total de ventas registradas por cada empleado, usando COALESCE para manejar casos sin ventas
    COALESCE(SUM(v.Total), 0) AS MontoVentas
FROM 
    personal p
-- Realizar una unión externa izquierda para incluir todos los empleados, incluso si no tienen ventas registradas
LEFT JOIN 
    ventas v ON p.NumSS = v.Personal_NumSS
-- Agrupar los resultados por el número de seguro social y el nombre completo del empleado
GROUP BY 
    p.NumSS, 
    p.Nombre, 
    p.ApePaterno, 
    p.ApeMaterno;

```

Consulta:
```sql
SELECT * FROM V_ventasXemp;
```
![[Pasted image 20240911235500.png]]

### 4. Con la vista anterior crea una consulta que muestre todos los campos y una comisión de 10% sobre el monto de ventas por empleado. 

```sql
SELECT 
    NumSS,
    NombreCompleto,
    NumeroVentas,
    MontoVentas,
    -- Calcular la comisión del 10% sobre el monto de ventas
    MontoVentas * 0.10 AS Comision
FROM 
    V_ventasXemp;
```
![[Pasted image 20240911235421.png]]

### 5. Crear una consulta que muestre: el código de producto, descripción corta, precio unitario y cantidad total de productos vendidos a la fecha. Ordenar de manera descendente según sus ventas, es decir primero se mostrará el producto más vendido. 

```sql
SELECT 
    p.CodProd AS CodigoProducto,
    p.DescripC AS DescripcionCorta,
    p.PrecioV AS PrecioUnitario,
    COALESCE(SUM(pv.Cantidad), 0) AS CantidadTotalVendida
FROM 
    productos p
LEFT JOIN 
    productosv pv ON p.CodProd = pv.Productos_CodProd
GROUP BY 
    p.CodProd, p.DescripC, p.PrecioV
ORDER BY 
    CantidadTotalVendida DESC;
```
![[Pasted image 20240911235819.png]]
![[Pasted image 20240911235840.png]]
### 6. Modificar la consulta anterior para que solo se muestren los productos con mas de 2 productos vendidos.

```sql
SELECT
    p.CodProd AS CodigoProducto,
    p.DescripC AS DescripcionCorta,
    p.PrecioV AS PrecioUnitario,
    COALESCE(SUM(pv.Cantidad), 0) AS CantidadTotalVendida
FROM 
    productos p
LEFT JOIN 
    productosv pv ON p.CodProd = pv.Productos_CodProd
GROUP BY 
    p.CodProd, p.DescripC, p.PrecioV
HAVING 
    COALESCE(SUM(pv.Cantidad), 0) > 2
ORDER BY 
    CantidadTotalVendida DESC;
```
![[Pasted image 20240912000043.png]]
### 7. Con la consulta anterior crear la vista “ProdsMasVendidos”.

 ```SQL
 CREATE VIEW ProdsMasVendidos AS
SELECT 
    p.CodProd AS CodigoProducto,
    p.DescripC AS DescripcionCorta,
    p.PrecioV AS PrecioUnitario,
    COALESCE(SUM(pv.Cantidad), 0) AS CantidadTotalVendida
FROM 
    productos p
LEFT JOIN 
    productosv pv ON p.CodProd = pv.Productos_CodProd
GROUP BY 
    p.CodProd, p.DescripC, p.PrecioV
HAVING 
    COALESCE(SUM(pv.Cantidad), 0) > 2
ORDER BY 
    CantidadTotalVendida DESC;
```

![[Pasted image 20240912000410.png]]
### 8. Agregar a la tabla productos la columna “Vigente” de tipo booleano (según su SMBDR garantice que su funcionalidad es de un booleano). 
```SQL
ALTER TABLE productos
ADD COLUMN Vigente BOOLEAN DEFAULT TRUE;
```
### 9. En una sola actualización, a todos los productos que no tengan ventas registradas, insertar el valor de 0, False o No (según su SMNDR).

```SQL
UPDATE productos
SET Vigente = FALSE
WHERE CodProd NOT IN (
    SELECT DISTINCT Productos_CodProd
    FROM productosv
);
```
### 10.En una segunda actualización deberá insertarse el valor de 1, True o Si (según su SMNDR), para todos los demás registros que no cumplan la condición del punto anterior.
```SQL
UPDATE productos
SET Vigente = TRUE
WHERE CodProd IN (
    SELECT DISTINCT Productos_CodProd
    FROM productosv
);
```

```SQL
SELECT CodProd, DescripC, PrecioV, Vigente
FROM productos
WHERE Vigente = TRUE;
```
![[Pasted image 20240912001022.png]]
