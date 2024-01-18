# Evidencia-1-Demostraci-n-de-Data-Cleansing-con-SQL
Actividad final de clase de Integración de base de datos
Se realizó en la página de https://sqliteonline.com/ con la base de datos proporcionadas por el socio formador (se presentan capturas de pantalla de esta base de datos) ;cabe destacar que para ser utilizado en la página debió convertirse en csv.

NOTA: Para visualizar la tabla de ventas se estará utilizando: SELECT*FROM tablaventas,mientras que para visualizar la tabla de detalle se utilizará:SELECT*FROM tabladetalle .

Es importante correr los siguiente datos en el orden que se presenta;se divide en dos secciones,para la tabla de ventas y tabla de detalle (se dividió en dos archivos para poder manejarse dentro de la plataforma).
Ortografía tabla ventas:
UPDATE tablaventas
SET cliente = 
  CASE
    WHEN cliente = 'Abarrotes de la esquina' THEN 'Abarrotes de la Esquina'
    WHEN cliente = 'Bicicletas Guerreo' THEN 'Bicicletas Guerrero'
    WHEN cliente = 'Cienfuego Produccions' THEN 'Cienfuego Producciones'
    WHEN cliente = 'Cienfuegos Produccions' THEN 'Cienfuego Producciones'
    WHEN cliente = 'JoseMari Productos' THEN 'JoseMary Productos'
    WHEN cliente = 'Juli�n Carrizoza' THEN 'Juli�n Carrizosa'
    WHEN cliente = 'Alamcenes del Fruto' THEN 'Almacenes del Fruto'
    ELSE cliente
  END;


Ortografía tabla detalle:
UPDATE tabladetalle
SET cte = 
  CASE
    WHEN cte = 'Abarrotes de la esquina' THEN 'Abarrotes de la Esquina'
    WHEN cte = 'Bicicletas Guerreo' THEN 'Bicicletas Guerrero'
    WHEN cte = 'Cienfuego Produccions' THEN 'Cienfuego Producciones'
    WHEN cte = 'Cienfuegos Produccions' THEN 'Cienfuego Producciones'
    WHEN cte = 'JoseMari Productos' THEN 'JoseMary Productos'
    WHEN cte = 'Juli�n Carrizoza' THEN 'Juli�n Carrizosa'
    WHEN cte = 'Cienfuegos Producciones' THEN 'Almacenes del Fruto'
    WHEN cte = 'Cienfuego Producciones' THEN 'Almacenes del Fruto'

    ELSE cte
  END;




Notas correcciones tabla ventas:
Teléfonos dobles , mover a otra columna

ALTER TABLE tablaventas ADD COLUMN telefono_primario TEXT;
ALTER TABLE tablaventas ADD COLUMN telefono_secundario TEXT;

UPDATE tablaventas
SET
  telefono_primario = CASE
    WHEN instr("Tel�fonos", char(10)) > 0 THEN substr("Tel�fonos", 1, instr("Tel�fonos", char(10))-1)
    ELSE "Tel�fonos"
  END,
  telefono_secundario = CASE
    WHEN instr("Tel�fonos", char(10)) > 0 THEN substr("Tel�fonos", instr("Tel�fonos", char(10))+1)
    ELSE "no proporcionado" 
  END;

SELECT * FROM tablaventas;




Creación de ID único para cliente por medio de la creación de números aleatorios para tabla de ventas NOTA: Este paso debe hacer antes que todo,pues se renombra la columna de Numero de Factura a ID_Cliente
ALTER TABLE tablaventas RENAME COLUMN "N�mero_de_factura" TO ID_cliente;
alter table tablaventas add column NuevoIDCliente int;
UPDATE tablaventas
SET NuevoIDCliente = (
    SELECT COUNT(DISTINCT ID_Cliente)
    FROM tablaventas AS d
    WHERE d.ID_Cliente <= tablaventas.ID_Cliente
);
alter table tablaventas drop column ID_Cliente;
alter table tablaventas rename column NuevoIDCliente to ID_Cliente;

SELECT * FROM tablaventas;



Llenar los datos faltantes en la columna “Email” por “No encontrado”.
UPDATE tablaventas
SET Email = 'No encontrado'
WHERE Email IS NULL OR Email = '';
SELECT*FROM tablaventas









Notas correcciones tabla detalle:

Cambiar el nombre de la columna “Cte” en la tabla “Detalle” por “Cliente”
ALTER TABLE tabladetalle
RENAME COLUMN "Cte" TO "Cliente";
SELECT*FROM tabladetalle


Creación de ID único para cliente por medio de la creación de números aleatorios dentro de la tabla detalle
ALTER TABLE tabladetalle RENAME COLUMN "Num_Fact" TO ID_cliente;
alter table tabladetalle add column NuevoIDCliente int;
UPDATE tabladetalle
SET NuevoIDCliente = (
    SELECT COUNT(DISTINCT ID_Cliente)
    FROM tabladetalle AS d
    WHERE d.ID_Cliente <= tabladetalle.ID_Cliente
);
alter table tabladetalle drop column ID_Cliente;
alter table tabladetalle rename column NuevoIDCliente to ID_Cliente;


SELECT*FROM tabladetalle


Abreviar los datos de las unidades

UPDATE tabladetalle
SET unidad =
    CASE 
        WHEN unidad = 'Pieza' THEN 'pz'
        WHEN unidad = 'Litro' THEN 'lt'
        WHEN unidad = 'Kilogramo' THEN 'kg'
        WHEN unidad = 'Metro cuadrado' THEN 'm2'
        WHEN unidad = 'Paquete de 3' THEN 'paq'

        ELSE unidad   
    END;
SELECT * FROM tabladetalle;


Cambiar los nombres de los productos en la columna “Producto” de la tabla “Detalle” por palabras clave de 2-3 letras:
Limpiador - “LR”.
Desinfectante - “DI”.
Plumero - “PR”.
Tela - “TL”.
Desengrasante - “DE”.
Aspiradora - “AS”.
Esponja - “ES”.
Toalla - “TO”.
Protector - “PO”.
Recogedor - “RD”.
Multiusos - “MU”.
Pulidor - “PD”.
Franela - “FR”.
Detergente- “DT”.


UPDATE tabladetalle
SET Producto =
    CASE 
        WHEN Producto = 'Limpiador' THEN 'LR'
        WHEN Producto = 'Desinfectante' THEN 'DI'
        WHEN Producto = 'Plumero' THEN 'PR'
        WHEN Producto = 'Tela' THEN 'TL'
        WHEN Producto = 'Desengrasante' THEN 'DE'
        WHEN Producto = 'Aspiradora' THEN 'AS'
        WHEN Producto = 'Esponja' THEN 'ES'
        WHEN Producto = 'Toalla' THEN 'TO'
        WHEN Producto = 'Protector' THEN 'PO'
        WHEN Producto = 'Recogedor' THEN 'RD'
        WHEN Producto = 'Multiusos' THEN 'MU'
        WHEN Producto = 'Pulidor' THEN 'PD'
        WHEN Producto = 'Franela' THEN 'FR'
        WHEN Producto = 'Detergente' THEN 'DT'

        ELSE Producto   
    END;
SELECT * FROM tabladetalle;











Nueva columna dentro de la tabla detalle:

Creación de  una columna donde se multiplique cantidad por precio unitario para obtener costo total
ALTER TABLE tabladetalle
ADD COLUMN coste_total DECIMAL(10, 2) DEFAULT 0.0;

UPDATE tabladetalle
SET coste_total = Cantidad * CAST(REPLACE(REPLACE(SUBSTR(Precio_Unitario, 2), ',', ''), '$', '') AS DECIMAL(10, 2));


UPDATE tabladetalle
SET coste_total = Cantidad * CAST(REPLACE(REPLACE(SUBSTR(Precio_Unitario, 2), ',', ''), '$', '') AS DECIMAL(10, 2));

SELECT ID_Cliente,Cliente, Producto, Cantidad, Unidad, Precio_Unitario, '$' || coste_total AS Coste_Total
FROM tabladetalle;
SELECT*FROM tabladetalle



Nuevas tablas con datos de la tabla detalle:
Cambiar nombre de Cte a Cliente (esto ya fue anteriormente corrido y realizado en pasos anteriores para este punto).
ALTER TABLE tabladetalle
RENAME COLUMN "Cte" TO "Cliente";
SELECT*FROM tabladetalle 
Tabla de ventas totales por producto 
SELECT Producto, SUM(coste_total) AS ventas_totales
FROM tabladetalle
GROUP BY Producto;


Tabla de ventas totales por cliente
SELECT cliente, SUM(coste_total) AS ventas_totales
FROM tabladetalle
GROUP BY cliente;


Tabla de productos mas vendidos
SELECT Producto, SUM(Cantidad) AS cantidad_vendida
FROM tabladetalle
GROUP BY Producto
ORDER BY cantidad_vendida DESC;


Tablas de ventas en cantidad y monto por unidad
SELECT Unidad, SUM(Cantidad) AS cantidad_total_vendida, SUM(coste_total) AS monto_total
FROM tabladetalle
GROUP BY Unidad;


Consultas 
Cantidad de inventario por producto y por cliente.
Cliente
SELECT cliente, SUM(Cantidad) AS cantidad_total_vendida_producto
FROM tabladetalle
GROUP BY cliente;






Producto
SELECT producto, SUM(Cantidad) AS cantidad_total_vendida_producto
FROM tabladetalle
GROUP BY producto;


Precio promedio total por cliente.
SELECT cliente, avg(Cantidad) AS promedio_total_cliente
FROM tabladetalle
GROUP BY cliente;

Ventas totales por cliente.
SELECT cliente, SUM(coste_total) AS ventas_totales
FROM tabladetalle
GROUP BY cliente;

Costo unitario por producto.
SELECT producto, precio_unitario
FROM tabladetalle
GROUP BY producto;







Cantidad de facturas por cliente.
SELECT cliente, COUNT(id_cliente) AS cantidad_facturas_cliente
FROM tabladetalle
GROUP BY cliente;
*Se cambio el número de facturas por la columna “ID Cliente”


Ventas totales por producto.
SELECT Producto, SUM(coste_total) AS ventas_totales
FROM tabladetalle
GROUP BY Producto;



Cantidad por unidad de medida
SELECT unidad, SUM(cantidad) AS cantidad_por_unidad
FROM tabladetalle
GROUP BY unidad;



