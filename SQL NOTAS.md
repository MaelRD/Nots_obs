

| **Operación** | **BD**                                                | **Tablas**                                        | **Columnas**                                            | **Registros**                                                 | **Relaciones**                                            | **Usuarios**                                           |
| ------------- | ----------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------- | --------------------------------------------------------- | ------------------------------------------------------ |
| **Altas**     | ```CREATE DATABASE mi_base;```                        | ```CREATE TABLE empleados (...);```               | ```ALTER TABLE empleados ADD nombre VARCHAR(100);```    | ```INSERT INTO empleados VALUES (...);```                     | ```ALTER TABLE ADD FOREIGN KEY...;```                     | ```CREATE USER 'user1';```                             |
| **Bajas**     | ```DROP DATABASE mi_base;```                          | ```DROP TABLE empleados;```                       | ```ALTER TABLE empleados DROP COLUMN nombre;```         | ```DELETE FROM empleados WHERE id_empleado = 1;```            | ```ALTER TABLE DROP FOREIGN KEY...;```                    | ```DROP USER 'user1';```                               |
| **Cambios**   | ```ALTER DATABASE mi_base ...;```                     | ```ALTER TABLE empleados ADD columna_edad INT;``` | ```ALTER TABLE empleados MODIFY nombre VARCHAR(150);``` | ```UPDATE empleados SET salario = salario * 1.1 WHERE ...;``` | ```UPDATE empleados SET id_dept = 2 WHERE ...;```         | ```ALTER USER 'user1' IDENTIFIED BY 'new_password';``` |
| **Consultas** | ```SELECT ... FROM mi_base;```                        | ```SELECT * FROM empleados;```                    | ```SELECT nombre FROM empleados;```                     | ```SELECT * FROM empleados WHERE salario > 3000;```           | ```SELECT * FROM empleados INNER JOIN deptos ...;```      | ```SHOW GRANTS FOR 'user1';```                         |
| **Funciones** | ```SELECT COUNT(*) FROM information_schema.tables;``` | ```SELECT COUNT(*) FROM empleados;```             | ```SELECT DISTINCT nombre FROM empleados;```            | ```SELECT AVG(salario) FROM empleados;```                     | ```SELECT * FROM empleados LEFT JOIN proyectos ON ...;``` | ```SELECT USER();```                                   |

---
## **1. Base de Datos (BD)**

### **Altas (Crear una base de datos)**
Para crear una nueva base de datos:
```sql
CREATE DATABASE nombre_base_datos;
```
- **Ejemplo**:
  ```sql
  CREATE DATABASE mi_base;
  ```

### **Bajas (Eliminar una base de datos)**
Para eliminar una base de datos:
```sql
DROP DATABASE nombre_base_datos;
```
- **Ejemplo**:
  ```sql
  DROP DATABASE mi_base;
  ```

### **Cambios (Modificar una base de datos)**
Si quieres cambiar aspectos de la configuración de la base de datos:
```sql
ALTER DATABASE nombre_base_datos ... ;
```
- **Ejemplo** (cambiar un parámetro de la base):
  ```sql
  ALTER DATABASE mi_base SET OWNER TO nuevo_propietario;
  ```

---

## **2. Tablas**

### **Altas (Crear una tabla)**
Para crear una nueva tabla dentro de una base de datos:
```sql
CREATE TABLE nombre_tabla (
    columna1 tipo_dato,
    columna2 tipo_dato,
    ...
);
```
- **Ejemplo**:
  ```sql
  CREATE TABLE empleados (
      id_empleado INT PRIMARY KEY,
      nombre VARCHAR(100),
      salario DECIMAL(10, 2)
  );
  ```

### **Bajas (Eliminar una tabla)**
Para eliminar una tabla de una base de datos:
```sql
DROP TABLE nombre_tabla;
```
- **Ejemplo**:
  ```sql
  DROP TABLE empleados;
  ```

### **Cambios (Modificar una tabla)**
Para agregar, modificar o eliminar columnas en una tabla:
- **Agregar columna**:
  ```sql
  ALTER TABLE nombre_tabla ADD nombre_columna tipo_dato;
  ```
- **Modificar columna**:
  ```sql
  ALTER TABLE nombre_tabla MODIFY nombre_columna tipo_dato;
  ```
- **Eliminar columna**:
  ```sql
  ALTER TABLE nombre_tabla DROP COLUMN nombre_columna;
  ```

- **Ejemplos**:
  - Agregar una columna:
    ```sql
    ALTER TABLE empleados ADD fecha_ingreso DATE;
    ```
  - Modificar una columna:
    ```sql
    ALTER TABLE empleados MODIFY nombre VARCHAR(150);
    ```
  - Eliminar una columna:
    ```sql
    ALTER TABLE empleados DROP COLUMN fecha_ingreso;
    ```

---

## **3. Registros**

### **Altas (Insertar un registro en una tabla)**
Para agregar datos o registros a una tabla:
```sql
INSERT INTO nombre_tabla (columna1, columna2, ...) VALUES (valor1, valor2, ...);
```
- **Ejemplo**:
  ```sql
  INSERT INTO empleados (id_empleado, nombre, salario) 
  VALUES (1, 'Juan Pérez', 4500);
  ```

### **Bajas (Eliminar un registro de una tabla)**
Para eliminar datos de una tabla según una condición:
```sql
DELETE FROM nombre_tabla WHERE condición;
```
- **Ejemplo**:
  ```sql
  DELETE FROM empleados WHERE id_empleado = 1;
  ```

### **Cambios (Actualizar un registro en una tabla)**
Para modificar los valores de un registro existente:
```sql
UPDATE nombre_tabla SET columna1 = nuevo_valor1, columna2 = nuevo_valor2 WHERE condición;
```
- **Ejemplo**:
  ```sql
  UPDATE empleados SET salario = salario * 1.1 WHERE id_empleado = 1;
  ```

---

## **4. Relaciones (Claves Foráneas)**

### **Crear una relación (Clave foránea)**
Para establecer una clave foránea que relacione dos tablas:
```sql
ALTER TABLE nombre_tabla_hija ADD CONSTRAINT nombre_clave_foranea FOREIGN KEY (columna) REFERENCES nombre_tabla_padre (columna_padre);
```
- **Ejemplo**:
  ```sql
  ALTER TABLE pedidos ADD CONSTRAINT fk_cliente FOREIGN KEY (id_cliente) REFERENCES clientes(id_cliente);
  ```

### **Eliminar una relación (Clave foránea)**
Para eliminar una clave foránea:
```sql
ALTER TABLE nombre_tabla DROP FOREIGN KEY nombre_clave_foranea;
```
- **Ejemplo**:
  ```sql
  ALTER TABLE pedidos DROP FOREIGN KEY fk_cliente;
  ```

---

## **5. Usuarios**

### **Altas (Crear un usuario)**
Para crear un nuevo usuario en la base de datos:
```sql
CREATE USER 'nombre_usuario' IDENTIFIED BY 'contraseña';
```
- **Ejemplo**:
  ```sql
  CREATE USER 'usuario1' IDENTIFIED BY 'password';
  ```

### **Bajas (Eliminar un usuario)**
Para eliminar un usuario de la base de datos:
```sql
DROP USER 'nombre_usuario';
```
- **Ejemplo**:
  ```sql
  DROP USER 'usuario1';
  ```

### **Cambios (Modificar un usuario)**
Para cambiar la contraseña de un usuario:
```sql
ALTER USER 'nombre_usuario' IDENTIFIED BY 'nueva_contraseña';
```
- **Ejemplo**:
  ```sql
  ALTER USER 'usuario1' IDENTIFIED BY 'new_password';
  ```

---

## **6. Consultas (SELECT)**

### **Consulta básica (Seleccionar registros)**
Para recuperar datos de una tabla:
```sql
SELECT columna1, columna2 FROM nombre_tabla WHERE condición;
```
- **Ejemplo**:
  ```sql
  SELECT nombre, salario FROM empleados WHERE salario > 3000;
  ```

### **Consultas con agregados (SUM, AVG, COUNT)**
Para realizar cálculos sobre los datos:
- **Suma**:
  ```sql
  SELECT SUM(salario) FROM empleados;
  ```
- **Promedio**:
  ```sql
  SELECT AVG(salario) FROM empleados;
  ```
- **Contar registros**:
  ```sql
  SELECT COUNT(*) FROM empleados;
  ```

---

## **7. Funciones**

### **Funciones agregadas**
Las funciones agregadas permiten realizar operaciones como sumar, contar o promediar.
- **Ejemplo de suma de salarios**:
  ```sql
  SELECT SUM(salario) FROM empleados;
  ```
- **Ejemplo de promedio de salarios**:
  ```sql
  SELECT AVG(salario) FROM empleados;
  ```

### **Funciones de texto**
Para manipular cadenas de texto:
- **Concatenar texto**:
  ```sql
  SELECT CONCAT(nombre, ' ', apellido) AS nombre_completo FROM empleados;
  ```
- **Longitud de una cadena**:
  ```sql
  SELECT LENGTH(nombre) FROM empleados;
  ```
