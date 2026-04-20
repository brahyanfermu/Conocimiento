# Conocimiento

# scrip de la base de datos

-- Crea la base de datos
CREATE DATABASE Autofactory;
-- usa la base de datos
USE Autofactory;

-- Crea la tabla modelo_vehiculo

CREATE TABLE modelo_vehiculo ( 
codigo_modelo int PRIMARY KEY auto_increment,
nombre varchar (100),
categoria varchar (100),
especificaciones_tecnicas varchar (250),
tiempo_ensamble int 
);

-- crea la tabla version_vehiculo

CREATE TABLE version_vehiculo( 
codigo_version int PRIMARY KEY auto_increment,
codigo_modelo int,
version_disponible varchar (250),
FOREIGN KEY (codigo_modelo) REFERENCES modelo_vehiculo (codigo_modelo)
); 

-- crea la tabla linea_produccion

CREATE TABLE linea_produccion ( 
numero_linea int PRIMARY KEY auto_increment,
tipo_vehiculo varchar (50),
capacidad_diaria int,
numero_estaciones int,
supervisor varchar (100),
turno_activo varchar (50),
estado_operativo varchar (50)
);

-- crea la tabla estacion_trabajo
 
CREATE TABLE estacion_trabajo (  
codigo_estacion int PRIMARY KEY auto_increment,
numero_linea int,
nombre varchar (100),
descripcion varchar (250),
herramientas_requeridas varchar (250),
tiempo_operacion int,
FOREIGN KEY (numero_linea) REFERENCES linea_produccion (numero_linea)
); 

-- crea la tabla empleado

CREATE TABLE empleado (  
numero_empleado int PRIMARY KEY auto_increment,
nombres varchar (50),
apellido varchar (100),
DNI varchar (20),
puesto varchar (50),
especializacion varchar (100),
numero_linea int,
turno varchar (50),
fecha_contratacion DATE, 
evaluacion_desempeno decimal (3,2),
FOREIGN KEY (numero_linea) REFERENCES linea_produccion (numero_linea)
);

-- crea la tabla empleado_habilidad

CREATE TABLE empleado_habilidad (  
numero_empleado int,
habilidad varchar (100),
PRIMARY KEY (numero_empleado,habilidad),
FOREIGN KEY (numero_empleado) REFERENCES empleado (numero_empleado)
);

-- crea la tabla proveedores

CREATE TABLE proveedores (
codigo_proveedor int PRIMARY KEY auto_increment,
razon_social varchar (255),
ruc varchar(50),
direccion varchar (255),
telefono varchar (50),
email varchar (50),
contacto varchar (100),
condiciones_pago varchar (100),
tiempo_entrega int,
evaluacion_calidad decimal (3,2)
); 

-- crea la tabla componente

CREATE TABLE componente (
codigo_componente int PRIMARY KEY auto_increment,
descripcion varchar (255),
categoria varchar (100),
especificaciones_tecnicas text,
codigo_proveedor int,
tiempo_entrega int,
costo_unitario decimal (10,2),
stock_minimo int,
FOREIGN KEY (codigo_proveedor) REFERENCES proveedores (codigo_proveedor)
); 

-- crea la tabla inventario

CREATE TABLE inventario (
codigo_inventario int PRIMARY KEY auto_increment,
codigo_componente int,
cantidad_disponible int,
ubicacion_almacen varchar(150),
fecha_entrada DATE,
calidad varchar (150),
FOREIGN KEY (codigo_componente) REFERENCES componente (codigo_componente)
);

-- crea la tabla orden_compra

CREATE TABLE orden_compra (
numero_orden_compra int PRIMARY KEY auto_increment,
codigo_proveedor int,
fecha DATE,
fecha_entrega DATE,
terminos_envio varchar (255),
condiciones_pago varchar (255),
estado varchar (50),
FOREIGN KEY (codigo_proveedor) REFERENCES proveedores (codigo_proveedor)
); 

-- crea la tabla especificacion_orden_compra

CREATE TABLE especificacion_orden_compra (
numero_orden_compra int,
codigo_componente int,
cantidad int,
precio decimal (10,2),
PRIMARY KEY (numero_orden_compra ,codigo_componente),
FOREIGN KEY (numero_orden_compra ) REFERENCES orden_compra (numero_orden_compra ),
FOREIGN KEY (codigo_componente) REFERENCES componente (codigo_componente)
); 

-- crea tabla orden_produccion

CREATE TABLE orden_produccion (
numero_produccion int PRIMARY KEY auto_increment,
fecha_emision DATE,
codigo_modelo int,
cantidad_producir int,
fecha_inicio DATE,
fecha_finalizacion DATE,
prioridad varchar (100),
estado_actual varchar (100),
FOREIGN KEY (codigo_modelo) REFERENCES modelo_vehiculo (codigo_modelo)
);

-- crea tabla vehiculo

CREATE TABLE vehiculo (
VIN varchar (50) PRIMARY KEY,
codigo_modelo int,
codigo_version int,
color varchar (50),
caracteristicas varchar (255),
fecha_inicio DATE,
fecha_finalizacion DATE,
numero_linea int,
resultado_final varchar (50),
FOREIGN KEY (codigo_modelo) REFERENCES modelo_vehiculo (codigo_modelo),
FOREIGN KEY (codigo_version) REFERENCES version_vehiculo (codigo_version),
FOREIGN KEY (numero_linea) REFERENCES linea_produccion (numero_linea)
); 

-- crea tabla control_calidad

CREATE TABLE control_calidad(
codigo_control int PRIMARY KEY auto_increment,
numero_produccion int,
VIN varchar (50),
codigo_estacion int,
fecha_hora datetime,
inspector varchar (100),
parametros_evaluados TEXT,
resultados varchar (255),
defectos_encontrados varchar (500),
decision_final varchar (255),
FOREIGN KEY (numero_produccion) REFERENCES orden_produccion (numero_produccion),
FOREIGN KEY (VIN) REFERENCES vehiculo (VIN),
FOREIGN KEY (codigo_estacion) REFERENCES estacion_trabajo (codigo_estacion)
); 

-- crea tabla pibot modelo_componente

CREATE TABLE modelo_componente (
codigo_modelo int,
codigo_componente int,
componentes_requeridos int,
PRIMARY KEY (codigo_modelo ,codigo_componente),
FOREIGN KEY (codigo_modelo) REFERENCES modelo_vehiculo (codigo_modelo),
FOREIGN KEY (codigo_componente) REFERENCES componente (codigo_componente)
); 

-- crea tabla pibot estacion_empleado

CREATE TABLE estacion_empleado (
codigo_estacion int,
numero_empleado int,
PRIMARY KEY (codigo_estacion ,numero_empleado),
FOREIGN KEY (codigo_estacion) REFERENCES estacion_trabajo (codigo_estacion),
FOREIGN KEY (numero_empleado) REFERENCES empleado (numero_empleado)
);

-- insertar en la tabla modelo_vehiculo los datos correspondientes.

INSERT INTO modelo_vehiculo (nombre, categoria, especificaciones_tecnicas, tiempo_ensamble) VALUES
('Modelo 1', 'Sedan', 'Gasolina', 10),
('Modelo 2', 'SUV', 'Híbrido', 12),
('Modelo 3', 'Pickup', 'Diesel', 15),
('Modelo 4', 'Sedan', 'Eléctrico', 11),
('Modelo 5', 'SUV', 'Gasolina', 13),
('Modelo 6', 'Pickup', 'Híbrido', 14),
('Modelo 7', 'Sedan', 'Eléctrico', 9),
('Modelo 8', 'SUV', 'Diesel', 16),
('Modelo 9', 'Pickup', 'Gasolina', 10),
('Modelo 10', 'Sedan', 'Híbrido', 12),
('Modelo 11', 'SUV', 'Eléctrico', 13),
('Modelo 12', 'Pickup', 'Diesel', 14),
('Modelo 13', 'Sedan', 'Gasolina', 10),
('Modelo 14', 'SUV', 'Híbrido', 12),
('Modelo 15', 'Pickup', 'Eléctrico', 15),
('Modelo 16','Sedan','Gasolina',11),
('Modelo 17','SUV','Híbrido',12),
('Modelo 18','Pickup','Eléctrico',13),
('Modelo 19','Sedan','Gasolina',10),
('Modelo 20','SUV','Diesel',14),
('Modelo 21','Pickup','Híbrido',15),
('Modelo 22','Sedan','Eléctrico',9),
('Modelo 23','SUV','Gasolina',13),
('Modelo 24','Pickup','Diesel',16),
('Modelo 25','Sedan','Híbrido',12),
('Modelo 26','SUV','Eléctrico',11),
('Modelo 27','Pickup','Gasolina',14),
('Modelo 28','Sedan','Diesel',10),
('Modelo 29','SUV','Híbrido',12),
('Modelo 30','Pickup','Eléctrico',15),
('Modelo 31','Sedan','Gasolina',11),
('Modelo 32','SUV','Diesel',13),
('Modelo 33','Pickup','Híbrido',14),
('Modelo 34','Sedan','Eléctrico',9),
('Modelo 35','SUV','Gasolina',12),
('Modelo 36','Pickup','Diesel',15),
('Modelo 37','Sedan','Híbrido',11),
('Modelo 38','SUV','Eléctrico',13),
('Modelo 39','Pickup','Gasolina',14),
('Modelo 40','Sedan','Diesel',10),
('Modelo 41','SUV','Híbrido',12),
('Modelo 42','Pickup','Eléctrico',15),
('Modelo 43','Sedan','Gasolina',11),
('Modelo 44','SUV','Diesel',13),
('Modelo 45','Pickup','Híbrido',14),
('Modelo 46','Sedan','Eléctrico',9),
('Modelo 47','SUV','Gasolina',12),
('Modelo 48','Pickup','Diesel',15),
('Modelo 49','Sedan','Híbrido',11),
('Modelo 50','SUV','Eléctrico',13);

-- insertar en la tabla version_vehiculo los datos correspondientes.

INSERT INTO version_vehiculo (codigo_modelo, version_disponible) VALUES
(1,'Base'),(2,'Full'),(3,'Sport'),(4,'Luxury'),(5,'Base'),
(6,'Full'),(7,'Sport'),(8,'Luxury'),(9,'Base'),(10,'Full'),
(11,'Sport'),(12,'Luxury'),(13,'Base'),(14,'Full'),(15,'Sport'),
(16,'Base'),(17,'Full'),(18,'Sport'),(19,'Luxury'),(20,'Base'),
(21,'Full'),(22,'Sport'),(23,'Luxury'),(24,'Base'),(25,'Full'),
(26,'Sport'),(27,'Luxury'),(28,'Base'),(29,'Full'),(30,'Sport'),
(31,'Luxury'),(32,'Base'),(33,'Full'),(34,'Sport'),(35,'Luxury'),
(36,'Base'),(37,'Full'),(38,'Sport'),(39,'Luxury'),(40,'Base'),
(41,'Full'),(42,'Sport'),(43,'Luxury'),(44,'Base'),(45,'Full'),
(46,'Sport'),(47,'Luxury'),(48,'Base'),(49,'Full'),(50,'Sport');


-- insertar en la tabla linea_produccion los datos correspondientes.

INSERT INTO linea_produccion (tipo_vehiculo, capacidad_diaria, numero_estaciones, supervisor, turno_activo, estado_operativo) VALUES
('Sedan',100,10,'Supervisor1','Mañana','Activa'),
('SUV',120,12,'Supervisor2','Tarde','Activa'),
('Pickup',80,8,'Supervisor3','Noche','Activa'),
('Sedan',90,9,'Supervisor4','Mañana','Activa'),
('SUV',110,11,'Supervisor5','Tarde','Activa'),
('Pickup',70,7,'Supervisor6','Noche','Activa'),
('Sedan',95,9,'Supervisor7','Mañana','Activa'),
('SUV',130,13,'Supervisor8','Tarde','Activa'),
('Pickup',85,8,'Supervisor9','Noche','Activa'),
('Sedan',100,10,'Supervisor10','Mañana','Activa'),
('SUV',120,12,'Supervisor11','Tarde','Activa'),
('Pickup',75,7,'Supervisor12','Noche','Activa'),
('Sedan',90,9,'Supervisor13','Mañana','Activa'),
('SUV',115,11,'Supervisor14','Tarde','Activa'),
('Pickup',80,8,'Supervisor15','Noche','Activa'),
('Sedan',95,9,'Supervisor16','Mañana','Activa'),
('SUV',110,11,'Supervisor17','Tarde','Activa'),
('Pickup',85,8,'Supervisor18','Noche','Activa'),
('Sedan',100,10,'Supervisor19','Mañana','Activa'),
('SUV',120,12,'Supervisor20','Tarde','Activa'),
('Pickup',90,9,'Supervisor21','Noche','Activa'),
('Sedan',105,10,'Supervisor22','Mañana','Activa'),
('SUV',130,13,'Supervisor23','Tarde','Activa'),
('Pickup',80,8,'Supervisor24','Noche','Activa'),
('Sedan',95,9,'Supervisor25','Mañana','Activa'),
('SUV',115,11,'Supervisor26','Tarde','Activa'),
('Pickup',85,8,'Supervisor27','Noche','Activa'),
('Sedan',100,10,'Supervisor28','Mañana','Activa'),
('SUV',120,12,'Supervisor29','Tarde','Activa'),
('Pickup',90,9,'Supervisor30','Noche','Activa'),
('Sedan',105,10,'Supervisor31','Mañana','Activa'),
('SUV',125,12,'Supervisor32','Tarde','Activa'),
('Pickup',80,8,'Supervisor33','Noche','Activa'),
('Sedan',95,9,'Supervisor34','Mañana','Activa'),
('SUV',115,11,'Supervisor35','Tarde','Activa'),
('Pickup',85,8,'Supervisor36','Noche','Activa'),
('Sedan',100,10,'Supervisor37','Mañana','Activa'),
('SUV',120,12,'Supervisor38','Tarde','Activa'),
('Pickup',90,9,'Supervisor39','Noche','Activa'),
('Sedan',105,10,'Supervisor40','Mañana','Activa'),
('SUV',130,13,'Supervisor41','Tarde','Activa'),
('Pickup',80,8,'Supervisor42','Noche','Activa'),
('Sedan',95,9,'Supervisor43','Mañana','Activa'),
('SUV',115,11,'Supervisor44','Tarde','Activa'),
('Pickup',85,8,'Supervisor45','Noche','Activa'),
('Sedan',100,10,'Supervisor46','Mañana','Activa'),
('SUV',120,12,'Supervisor47','Tarde','Activa'),
('Pickup',90,9,'Supervisor48','Noche','Activa'),
('Sedan',105,10,'Supervisor49','Mañana','Activa'),
('SUV',125,12,'Supervisor50','Tarde','Activa');

-- insertar en la tabla estacion_trabajo los datos correspondientes.

INSERT INTO estacion_trabajo (numero_linea,nombre,descripcion,herramientas_requeridas,tiempo_operacion) VALUES
(1,'Est1','Montaje','Llaves',5),(1,'Est2','Soldadura','Soldador',6),
(2,'Est3','Pintura','Pistola',4),(2,'Est4','Motor','Herramientas',7),
(3,'Est5','Chasis','Equipo',6),(3,'Est6','Pruebas','Tester',5),
(4,'Est7','Montaje','Llaves',5),(5,'Est8','Pintura','Pistola',4),
(6,'Est9','Motor','Herramientas',7),(7,'Est10','Chasis','Equipo',6),
(8,'Est11','Pruebas','Tester',5),(9,'Est12','Montaje','Llaves',5),
(10,'Est13','Pintura','Pistola',4),(11,'Est14','Motor','Herramientas',7),
(12,'Est15','Pruebas','Tester',5),(1,'Est16','Montaje','Llaves',5),
(3,'Est18','Pintura','Pistola',4),(4,'Est19','Motor','Herramientas',7),
(5,'Est20','Chasis','Equipo',6),(6,'Est21','Pruebas','Tester',5),
(7,'Est22','Montaje','Llaves',5),(8,'Est23','Pintura','Pistola',4),
(9,'Est24','Motor','Herramientas',7),(10,'Est25','Chasis','Equipo',6),
(11,'Est26','Pruebas','Tester',5),(12,'Est27','Montaje','Llaves',5),
(13,'Est28','Pintura','Pistola',4),(14,'Est29','Motor','Herramientas',7),
(15,'Est30','Chasis','Equipo',6),(1,'Est31','Pruebas','Tester',5),
(2,'Est32','Montaje','Llaves',5),(3,'Est33','Pintura','Pistola',4),
(4,'Est34','Motor','Herramientas',7),(5,'Est35','Chasis','Equipo',6),
(6,'Est36','Pruebas','Tester',5),(7,'Est37','Montaje','Llaves',5),
(8,'Est38','Pintura','Pistola',4),(9,'Est39','Motor','Herramientas',7),
(10,'Est40','Chasis','Equipo',6),(11,'Est41','Pruebas','Tester',5),
(12,'Est42','Montaje','Llaves',5),(13,'Est43','Pintura','Pistola',4),
(14,'Est44','Motor','Herramientas',7),(15,'Est45','Chasis','Equipo',6),
(1,'Est46','Pruebas','Tester',5),(2,'Est47','Montaje','Llaves',5),
(3,'Est48','Pintura','Pistola',4),(4,'Est49','Motor','Herramientas',7),
(5,'Est50','Chasis','Equipo',6),(2,'Est17','Soldadura','Soldador',6);


-- insertar en la tabla empleado los datos correspondientes.

INSERT INTO empleado (nombres,apellido,DNI,puesto,especializacion,numero_linea,turno,fecha_contratacion,evaluacion_desempeno) VALUES
('Juan','Perez','0101','Operario','Soldadura',1,'Mañana','2020-01-01',4.5),
('Ana','Gomez','0202','Operario','Pintura',2,'Tarde','2021-02-01',4.2),
('Luis','Diaz','0303','Supervisor','Motor',3,'Noche','2019-03-01',4.8),
('Maria','Lopez','0404','Operario','Montaje',4,'Mañana','2022-01-01',4.0),
('Pedro','Ruiz','0505','Operario','Chasis',5,'Tarde','2020-05-01',4.1),
('Jose','Torres','0606','Operario','Pruebas',6,'Noche','2021-06-01',4.3),
('Laura','Castro','0707','Operario','Soldadura',7,'Mañana','2020-07-01',4.6),
('Miguel','Vega','0808','Operario','Pintura',8,'Tarde','2021-08-01',4.4),
('Sofia','Rios','0909','Operario','Motor',9,'Noche','2022-09-01',4.2),
('Diego','Mora','1010','Operario','Montaje',10,'Mañana','2020-10-01',4.7),
('Paula','Silva','1111','Operario','Chasis',11,'Tarde','2021-11-01',4.3),
('Andres','Rey','1212','Operario','Pruebas',12,'Noche','2022-12-01',4.1),
('Elena','Navas','1313','Operario','Soldadura',13,'Mañana','2020-01-15',4.5),
('Raul','Mejia','1414','Operario','Pintura',14,'Tarde','2021-02-15',4.2),
('Camila','Ortega','1515','Operario','Motor',15,'Noche','2022-03-15',4.6),
('Andres','Cardona','1616','Operario','Soldadura',1,'Mañana','2025-01-01',4.3),
('Felipe','Giraldo','1717','Operario','Pintura',2,'Tarde','2025-01-02',4.1),
('Jorge','Arango','1818','Operario','Motor',3,'Noche','2025-01-03',4.6),
('Sebastian','Henao','1919','Operario','Montaje',4,'Mañana','2025-01-04',4.0),
('Daniel','Ospina','2020','Operario','Pruebas',5,'Tarde','2025-01-05',4.2),
('Cristian','Restrepo','2121','Operario','Chasis',6,'Noche','2025-01-06',4.4),
('Kevin','Salazar','2222','Operario','Soldadura',7,'Mañana','2025-01-07',4.5),
('Brayan','Castaño','2323','Operario','Pintura',8,'Tarde','2025-01-08',4.1),
('Camilo','Montoya','2424','Operario','Motor',9,'Noche','2025-01-09',4.3),
('Santiago','Valencia','2525','Operario','Montaje',10,'Mañana','2025-01-10',4.0),
('Julian','Correa','2626','Operario','Pruebas',11,'Tarde','2025-01-11',4.6),
('Mateo','Zapata','2727','Operario','Chasis',12,'Noche','2025-01-12',4.2),
('Esteban','Patiño','2828','Operario','Soldadura',13,'Mañana','2025-01-13',4.5),
('Nicolas','Uribe','2929','Operario','Pintura',14,'Tarde','2025-01-14',4.1),
('JuanDavid','Quintero','3030','Operario','Motor',15,'Noche','2025-01-15',4.4),
('David','Marin','3131','Operario','Montaje',1,'Mañana','2025-01-16',4.0),
('Alejandro','Bermudez','3232','Operario','Pruebas',2,'Tarde','2025-01-17',4.5),
('LuisFernando','Vargas','3333','Operario','Chasis',3,'Noche','2025-01-18',4.3),
('Oscar','Molina','3434','Operario','Soldadura',4,'Mañana','2025-01-19',4.6),
('Ricardo','Navarro','3535','Operario','Pintura',5,'Tarde','2025-01-20',4.1),
('Fernando','Gallego','3636','Operario','Motor',6,'Noche','2025-01-21',4.4),
('Hector','Londoño','3737','Operario','Montaje',7,'Mañana','2025-01-22',4.0),
('Alvaro','Castillo','3838','Operario','Pruebas',8,'Tarde','2025-01-23',4.5),
('Mauricio','Rojas','3939','Operario','Chasis',9,'Noche','2025-01-24',4.2),
('Gustavo','Mejia','4040','Operario','Soldadura',10,'Mañana','2025-01-25',4.6),
('DiegoFernando','Cano','4141','Operario','Pintura',11,'Tarde','2025-01-26',4.1),
('CarlosAndres','Franco','4242','Operario','Motor',12,'Noche','2025-01-27',4.4),
('Ivan','Agudelo','4343','Operario','Montaje',13,'Mañana','2025-01-28',4.0),
('Jhon','Bedoya','4444','Operario','Pruebas',14,'Tarde','2025-01-29',4.5),
('Wilson','Cifuentes','4545','Operario','Chasis',15,'Noche','2025-01-30',4.2),
('Edwin','Acevedo','4646','Operario','Soldadura',1,'Mañana','2025-02-01',4.6),
('Harold','Tobon','4747','Operario','Pintura',2,'Tarde','2025-02-02',4.1),
('Yeferson','Mosquera','4848','Operario','Motor',3,'Noche','2025-02-03',4.4),
('Cristobal','Peña','4949','Operario','Montaje',4,'Mañana','2025-02-04',4.0),
('Emerson','Sierra','5050','Operario','Pruebas',5,'Tarde','2025-02-05',4.5);


-- insertar en la tabla empleado_habilidad los datos correspondientes.


INSERT INTO empleado_habilidad (numero_empleado, habilidad) VALUES
(1,'Soldadura'),(2,'Pintura'),(3,'Supervisión'),
(4,'Montaje'),(5,'Chasis'),(6,'Pruebas'),
(7,'Soldadura'),(8,'Pintura'),(9,'Motor'),
(10,'Montaje'),(11,'Chasis'),(12,'Pruebas'),
(13,'Soldadura'),(14,'Pintura'),(15,'Motor'),
(16,'Soldadura'),(17,'Pintura'),(18,'Motor'),(19,'Montaje'),
(20,'Pruebas'),(21,'Chasis'),(22,'Soldadura'),(23,'Pintura'),
(24,'Motor'),(25,'Montaje'),(26,'Pruebas'),(27,'Chasis'),
(28,'Soldadura'),(29,'Pintura'),(30,'Motor'),(31,'Montaje'),
(32,'Pruebas'),(33,'Chasis'),(34,'Soldadura'),(35,'Pintura'),
(36,'Motor'),(37,'Montaje'),(38,'Pruebas'),(39,'Chasis'),
(40,'Soldadura'),(41,'Pintura'),(42,'Motor'),(43,'Montaje'),
(44,'Pruebas'),(45,'Chasis'),(46,'Soldadura'),(47,'Pintura'),
(48,'Motor'),(49,'Montaje'),(50,'Pruebas');

-- insertar en la tabla provedores los datos correspondientes.

INSERT INTO proveedores (razon_social,ruc,direccion,telefono,email,contacto,condiciones_pago,tiempo_entrega,evaluacion_calidad) VALUES
('AutoParts1','011','Dir1','111','p1@mail.com','Carlos arango','30 dias',5,4.5),
('AutoParts2','222','Dir2','222','p2@mail.com','Ana celi','30 dias',7,4.2),
('AutoParts3','333','Dir3','333','p3@mail.com','Luis rozo','30 dias',6,4.1),
('AutoParts4','444','Dir4','444','p4@mail.com','Maria magdalena','30 dias',8,4.0),
('AutoParts5','555','Dir5','555','p5@mail.com','Pedro zanches','30 dias',9,4.3),
('AutoParts6','666','Dir6','666','p6@mail.com','Jose munera','30 dias',4,4.6),
('AutoParts7','777','Dir7','777','p7@mail.com','Laura ospina','30 dias',3,4.7),
('AutoParts8','888','Dir8','888','p8@mail.com','Miguel zar','30 dias',5,4.2),
('AutoParts9','999','Dir9','999','p9@mail.com','Sofia vergara','30 dias',6,4.3),
('AutoParts10','110','Dir10','101','p10@mail.com','Diego gil','30 dias',7,4.1),
('AutoParts11','111','Dir11','102','p11@mail.com','Paula rodriguez','30 dias',8,4.0),
('AutoParts12','112','Dir12','103','p12@mail.com','Andres ocampo','30 dias',5,4.4),
('AutoParts13','113','Dir13','104','p13@mail.com','Elena mesa','30 dias',6,4.3),
('AutoParts14','114','Dir14','105','p14@mail.com','Raul cardona','30 dias',7,4.2),
('AutoParts15','115','Dir15','106','p15@mail.com','Camila rios','30 dias',9,4.1),
('AutoParts16','116','Dir16','116','p16@mail.com','Andres Cardona','30 dias',5,4.3),
('AutoParts17','117','Dir17','117','p17@mail.com','Felipe Giraldo','30 dias',6,4.2),
('AutoParts18','118','Dir18','118','p18@mail.com','Jorge Arango','30 dias',4,4.6),
('AutoParts19','119','Dir19','119','p19@mail.com','Sebastian Henao','30 dias',7,4.1),
('AutoParts20','120','Dir20','120','p20@mail.com','Daniel Ospina','30 dias',5,4.4),
('AutoParts21','121','Dir21','121','p21@mail.com','Cristian Restrepo','30 dias',6,4.3),
('AutoParts22','122','Dir22','122','p22@mail.com','Kevin Salazar','30 dias',4,4.5),
('AutoParts23','123','Dir23','123','p23@mail.com','Brayan Castaño','30 dias',7,4.2),
('AutoParts24','124','Dir24','124','p24@mail.com','Camilo Montoya','30 dias',5,4.3),
('AutoParts25','125','Dir25','125','p25@mail.com','Santiago Valencia','30 dias',6,4.4),
('AutoParts26','126','Dir26','126','p26@mail.com','Julian Correa','30 dias',4,4.6),
('AutoParts27','127','Dir27','127','p27@mail.com','Mateo Zapata','30 dias',7,4.1),
('AutoParts28','128','Dir28','128','p28@mail.com','Esteban Patiño','30 dias',5,4.3),
('AutoParts29','129','Dir29','129','p29@mail.com','Nicolas Uribe','30 dias',6,4.2),
('AutoParts30','130','Dir30','130','p30@mail.com','Juan David Quintero','30 dias',4,4.5),
('AutoParts31','131','Dir31','131','p31@mail.com','David Marin','30 dias',7,4.2),
('AutoParts32','132','Dir32','132','p32@mail.com','Alejandro Bermudez','30 dias',5,4.3),
('AutoParts33','133','Dir33','133','p33@mail.com','Luis Fernando Vargas','30 dias',6,4.4),
('AutoParts34','134','Dir34','134','p34@mail.com','Oscar Molina','30 dias',4,4.6),
('AutoParts35','135','Dir35','135','p35@mail.com','Ricardo Navarro','30 dias',7,4.1),
('AutoParts36','136','Dir36','136','p36@mail.com','Fernando Gallego','30 dias',5,4.3),
('AutoParts37','137','Dir37','137','p37@mail.com','Hector Londoño','30 dias',6,4.2),
('AutoParts38','138','Dir38','138','p38@mail.com','Alvaro Castillo','30 dias',4,4.5),
('AutoParts39','139','Dir39','139','p39@mail.com','Mauricio Rojas','30 dias',7,4.2),
('AutoParts40','140','Dir40','140','p40@mail.com','Gustavo Mejia','30 dias',5,4.3),
('AutoParts41','141','Dir41','141','p41@mail.com','Diego Cano','30 dias',6,4.4),
('AutoParts42','142','Dir42','142','p42@mail.com','Carlos Franco','30 dias',4,4.6),
('AutoParts43','143','Dir43','143','p43@mail.com','Ivan Agudelo','30 dias',7,4.1),
('AutoParts44','144','Dir44','144','p44@mail.com','Jhon Bedoya','30 dias',5,4.3),
('AutoParts45','145','Dir45','145','p45@mail.com','Wilson Cifuentes','30 dias',6,4.2),
('AutoParts46','146','Dir46','146','p46@mail.com','Edwin Acevedo','30 dias',4,4.5),
('AutoParts47','147','Dir47','147','p47@mail.com','Harold Tobon','30 dias',7,4.2),
('AutoParts48','148','Dir48','148','p48@mail.com','Yeferson Mosquera','30 dias',5,4.3),
('AutoParts49','149','Dir49','149','p49@mail.com','Cristobal Peña','30 dias',6,4.4),
('AutoParts50','150','Dir50','150','p50@mail.com','Emerson Sierra','30 dias',4,4.6);

-- insertar en la tabla componente los datos correspondientes.


INSERT INTO componente (descripcion,categoria,especificaciones_tecnicas,codigo_proveedor,tiempo_entrega,costo_unitario,stock_minimo) VALUES
('Comp1','Mecanico','Alta potencia',1,5,1000,10),
('Comp2','Rueda','Radial',3,6,200,20),
('Comp3','Electrico','12V',7,4,150,15),
('Comp4','Seguridad','ABS',1,5,300,10),
('Comp5','Motor','Aire',3,6,50,30),
('Comp6','Motor','Aluminio',7,4,400,10),
('Comp7','Lubricante','Sintetico',1,5,80,25),
('Comp8','Motor','Iridio',3,6,20,40),
('Comp9','Electrico','12V',7,4,350,10),
('Comp10','Transmision','Manual',1,5,900,5),
('Comp11','Electronico','Digital',3,6,60,20),
('Comp12','Motor','Alta presión',7,4,800,5),
('Comp13','Transmision','Kit',1,5,500,8),
('Comp14','Sistema','Acero',3,6,250,10),
('Comp15','Seguridad','Frontal',7,4,700,5),
('Comp16','Mecanico','Alta potencia',16,5,950,10),
('Comp17','Rueda','Radial',17,6,210,20),
('Comp18','Electrico','12V',18,4,160,15),
('Comp19','Seguridad','ABS',19,5,310,10),
('Comp20','Motor','Aire',20,6,55,30),
('Comp21','Motor','Aluminio',21,5,420,10),
('Comp22','Lubricante','Sintetico',22,6,85,25),
('Comp23','Motor','Iridio',23,4,25,40),
('Comp24','Electrico','12V',24,5,360,10),
('Comp25','Transmision','Manual',25,6,920,5),
('Comp26','Electronico','Digital',26,4,65,20),
('Comp27','Motor','Alta presión',27,5,820,5),
('Comp28','Transmision','Kit',28,6,520,8),
('Comp29','Sistema','Acero',29,4,260,10),
('Comp30','Seguridad','Frontal',30,5,720,5),
('Comp31','Mecanico','Alta potencia',31,6,970,10),
('Comp32','Rueda','Radial',32,4,220,20),
('Comp33','Electrico','12V',33,5,170,15),
('Comp34','Seguridad','ABS',34,6,330,10),
('Comp35','Motor','Aire',35,4,60,30),
('Comp36','Motor','Aluminio',36,5,430,10),
('Comp37','Lubricante','Sintetico',37,6,90,25),
('Comp38','Motor','Iridio',38,4,30,40),
('Comp39','Electrico','12V',39,5,370,10),
('Comp40','Transmision','Manual',40,6,940,5),
('Comp41','Electronico','Digital',41,4,70,20),
('Comp42','Motor','Alta presión',42,5,840,5),
('Comp43','Transmision','Kit',43,6,540,8),
('Comp44','Sistema','Acero',44,4,280,10),
('Comp45','Seguridad','Frontal',45,5,740,5),
('Comp46','Mecanico','Alta potencia',46,6,990,10),
('Comp47','Rueda','Radial',47,4,230,20),
('Comp48','Electrico','12V',48,5,180,15),
('Comp49','Seguridad','ABS',49,6,350,10),
('Comp50','Motor','Aire',50,4,65,30);

-- insertar en la tabla inventario los datos correspondientes.

INSERT INTO inventario (codigo_componente,cantidad_disponible,ubicacion_almacen,fecha_entrada,calidad) VALUES
(1,5,'A1','2024-03-01','Aprobado'),
(2,30,'A2','2024-03-02','Aprobado'),
(3,10,'A3','2024-03-03','Aprobado'),
(4,8,'A4','2024-03-04','Aprobado'),
(5,50,'A5','2024-03-05','Aprobado'),
(6,7,'A6','2024-03-06','Aprobado'),
(7,20,'A7','2024-03-07','Aprobado'),
(8,60,'A8','2024-03-08','Aprobado'),
(9,9,'A9','2024-03-09','Aprobado'),
(10,4,'A10','2024-03-10','Aprobado'),
(11,25,'A11','2024-03-11','Aprobado'),
(12,3,'A12','2024-03-12','Aprobado'),
(13,6,'A13','2024-03-13','Aprobado'),
(14,15,'A14','2024-03-14','Aprobado'),
(15,2,'A15','2024-03-15','Aprobado'),
(16,20,'B1','2025-12-01','Aprobado'),
(17,15,'B2','2025-12-02','Aprobado'),
(18,10,'B3','2025-12-03','Aprobado'),
(19,8,'B4','2025-12-04','Aprobado'),
(20,5,'B5','2025-12-05','Aprobado'),
(21,25,'B6','2025-12-06','Aprobado'),
(22,30,'B7','2025-12-07','Aprobado'),
(23,12,'B8','2025-12-08','Aprobado'),
(24,7,'B9','2025-12-09','Aprobado'),
(25,9,'B10','2025-12-10','Aprobado'),
(26,20,'B11','2025-12-11','Aprobado'),
(27,18,'B12','2025-12-12','Aprobado'),
(28,16,'B13','2025-12-13','Aprobado'),
(29,14,'B14','2025-12-14','Aprobado'),
(30,12,'B15','2025-12-15','Aprobado'),
(31,22,'B16','2025-12-16','Aprobado'),
(32,24,'B17','2025-12-17','Aprobado'),
(33,26,'B18','2025-12-18','Aprobado'),
(34,28,'B19','2025-12-19','Aprobado'),
(35,30,'B20','2025-12-20','Aprobado'),
(36,10,'B21','2025-12-21','Aprobado'),
(37,9,'B22','2025-12-22','Aprobado'),
(38,8,'B23','2025-12-23','Aprobado'),
(39,7,'B24','2025-12-24','Aprobado'),
(40,6,'B25','2025-12-25','Aprobado'),
(41,5,'B26','2025-12-26','Aprobado'),
(42,4,'B27','2025-12-27','Aprobado'),
(43,3,'B28','2025-12-28','Aprobado'),
(44,2,'B29','2025-12-29','Aprobado'),
(45,1,'B30','2025-12-30','Aprobado'),
(46,15,'B31','2025-12-31','Aprobado'),
(47,17,'B32','2026-01-01','Aprobado'),
(48,19,'B33','2026-01-02','Aprobado'),
(49,21,'B34','2026-01-03','Aprobado'),
(50,23,'B35','2026-01-04','Aprobado');


-- insertar en la tabla orden_compra los datos correspondientes.


INSERT INTO orden_compra (codigo_proveedor, fecha, fecha_entrega, terminos_envio, condiciones_pago, estado) VALUES
(1,'2026-01-01','2026-01-05','Normal','30 dias','Pendiente'),
(2,'2026-01-02','2026-01-06','Normal','30 dias','Recibido'),
(3,'2026-01-03','2026-01-07','Normal','30 dias','Pendiente'),
(4,'2026-01-04','2026-01-08','Normal','30 dias','Recibido'),
(5,'2026-01-05','2026-01-09','Normal','30 dias','Pendiente'),
(6,'2026-01-06','2026-01-10','Normal','30 dias','Recibido'),
(7,'2026-01-07','2026-01-11','Normal','30 dias','Pendiente'),
(8,'2026-01-08','2026-01-12','Normal','30 dias','Recibido'),
(9,'2026-01-09','2026-01-13','Normal','30 dias','Pendiente'),
(10,'2026-01-10','2026-01-14','Normal','30 dias','Recibido'),
(11,'2026-01-11','2026-01-15','Normal','30 dias','Pendiente'),
(12,'2026-01-12','2026-01-16','Normal','30 dias','Recibido'),
(13,'2026-01-13','2026-01-17','Normal','30 dias','Pendiente'),
(14,'2026-01-14','2026-01-18','Normal','30 dias','Recibido'),
(15,'2026-01-15','2026-01-19','Normal','30 dias','Pendiente'),
(16,'2026-01-16','2026-01-20','Normal','30 dias','Recibido'),
(17,'2026-01-17','2026-01-21','Normal','30 dias','Pendiente'),
(18,'2026-01-18','2026-01-22','Normal','30 dias','Recibido'),
(19,'2026-01-19','2026-01-23','Normal','30 dias','Pendiente'),
(20,'2026-01-20','2026-01-24','Normal','30 dias','Recibido'),
(21,'2026-01-21','2026-01-25','Normal','30 dias','Pendiente'),
(22,'2026-01-22','2026-01-26','Normal','30 dias','Recibido'),
(23,'2026-01-23','2026-01-27','Normal','30 dias','Pendiente'),
(24,'2026-01-24','2026-01-28','Normal','30 dias','Recibido'),
(25,'2026-01-25','2026-01-29','Normal','30 dias','Pendiente'),
(26,'2026-01-26','2026-01-30','Normal','30 dias','Recibido'),
(27,'2026-01-27','2026-01-31','Normal','30 dias','Pendiente'),
(28,'2026-01-28','2026-02-01','Normal','30 dias','Recibido'),
(29,'2026-01-29','2026-02-02','Normal','30 dias','Pendiente'),
(30,'2026-01-30','2026-02-03','Normal','30 dias','Recibido'),
(31,'2026-01-31','2026-02-04','Normal','30 dias','Pendiente'),
(32,'2026-02-01','2026-02-05','Normal','30 dias','Recibido'),
(33,'2026-02-02','2026-02-06','Normal','30 dias','Pendiente'),
(34,'2026-02-03','2026-02-07','Normal','30 dias','Recibido'),
(35,'2026-02-04','2026-02-08','Normal','30 dias','Pendiente'),
(36,'2026-03-01','2026-03-05','Normal','30 dias','Recibido'),
(37,'2026-03-02','2026-03-06','Normal','30 dias','Pendiente'),
(38,'2026-03-05','2026-03-09','Normal','30 dias','Recibido'),
(39,'2026-03-06','2026-03-10','Normal','30 dias','Pendiente'),
(40,'2026-03-05','2026-03-09','Normal','30 dias','Recibido'),
(41,'2026-03-06','2026-03-10','Normal','30 dias','Pendiente'),
(42,'2026-03-07','2026-03-11','Normal','30 dias','Recibido'),
(43,'2026-03-08','2026-03-12','Normal','30 dias','Pendiente'),
(44,'2026-03-09','2026-03-13','Normal','30 dias','Recibido'),
(45,'2026-03-10','2026-03-14','Normal','30 dias','Pendiente'),
(46,'2026-03-11','2026-03-15','Normal','30 dias','Recibido'),
(47,'2026-03-12','2026-03-16','Normal','30 dias','Pendiente'),
(48,'2026-03-13','2026-03-17','Normal','30 dias','Recibido'),
(49,'2026-03-14','2026-03-18','Normal','30 dias','Pendiente'),
(50,'2026-03-15','2026-03-19','Normal','30 dias','Recibido');

-- insertar en la tabla especificacion_orden_compra los datos correspondientes.

INSERT INTO especificacion_orden_compra (numero_orden_compra, codigo_componente, cantidad, precio) VALUES
(1,1,10,1000),(2,2,20,200),(3,3,15,150),
(4,4,10,300),(5,5,25,50),(6,6,8,400),
(7,7,20,80),(8,8,30,20),(9,9,10,350),
(10,10,5,900),(11,11,15,60),(12,12,5,800),
(13,13,8,500),(14,14,12,250),(15,15,6,700),
(16,1,10,1000),(17,2,20,200),(18,3,15,150),
(19,4,10,300),(20,5,25,50),(21,6,8,400),
(22,7,20,80),(23,8,30,20),(24,9,10,350),
(25,10,5,900),(26,11,15,60),(27,12,5,800),
(28,13,8,500),(29,14,12,250),(30,15,6,700),
(31,16,10,950),(32,17,20,210),(33,18,15,160),
(34,19,10,310),(35,20,25,55),(36,21,8,420),
(37,22,20,85),(38,23,30,25),(39,24,10,360),
(40,25,5,920),(41,26,15,65),(42,27,5,820),
(43,28,8,520),(44,29,12,260),(45,30,6,720),
(46,31,10,970),(47,32,20,220),(48,33,15,170),
(49,34,10,330),(50,35,25,60);

-- insertar en la tabla orden_produccion los datos correspondientes.

INSERT INTO orden_produccion (codigo_modelo, fecha_emision, cantidad_producir, fecha_inicio, fecha_finalizacion, prioridad, estado_actual) VALUES
(1,'2026-02-01',12,'2026-02-02','2026-02-05','Alta','En proceso'),
(2,'2026-02-02',14,'2026-02-03','2026-02-06','Media','Pendiente'),
(3,'2026-02-03',16,'2026-02-04','2026-02-07','Alta','En proceso'),
(4,'2026-02-04',11,'2026-02-05','2026-02-08','Baja','Finalizada'),
(5,'2026-02-05',13,'2026-02-06','2026-02-09','Media','En proceso'),
(6,'2026-02-06',15,'2026-02-07','2026-02-10','Alta','Pendiente'),
(7,'2026-02-07',17,'2026-02-08','2026-02-11','Media','En proceso'),
(8,'2026-02-08',19,'2026-02-09','2026-02-12','Alta','Finalizada'),
(9,'2026-02-09',10,'2026-02-10','2026-02-13','Baja','En proceso'),
(10,'2026-02-10',12,'2026-02-11','2026-02-14','Media','Pendiente'),
(11,'2026-02-11',14,'2026-02-12','2026-02-15','Alta','En proceso'),
(12,'2026-02-12',16,'2026-02-13','2026-02-16','Media','Finalizada'),
(13,'2026-02-13',18,'2026-02-14','2026-02-17','Alta','Pendiente'),
(14,'2026-02-14',20,'2026-02-15','2026-02-18','Media','En proceso'),
(15,'2026-02-15',22,'2026-02-16','2026-02-19','Alta','Finalizada'),
(16,'2026-02-16',12,'2026-02-17','2026-02-20','Alta','En proceso'),
(17,'2026-02-17',14,'2026-02-18','2026-02-21','Media','Pendiente'),
(18,'2026-02-18',16,'2026-02-19','2026-02-22','Alta','En proceso'),
(19,'2026-02-19',11,'2026-02-20','2026-02-23','Baja','Finalizada'),
(20,'2026-02-20',13,'2026-02-21','2026-02-24','Media','En proceso'),
(21,'2026-02-21',15,'2026-02-22','2026-02-25','Alta','Pendiente'),
(22,'2026-02-22',17,'2026-02-23','2026-02-26','Media','En proceso'),
(23,'2026-02-23',19,'2026-02-24','2026-02-27','Alta','Finalizada'),
(24,'2026-02-24',10,'2026-02-25','2026-02-28','Baja','En proceso'),
(25,'2026-02-25',12,'2026-02-26','2026-03-01','Media','Pendiente'),
(26,'2026-02-26',14,'2026-02-27','2026-03-02','Alta','En proceso'),
(27,'2026-02-27',16,'2026-02-28','2026-03-03','Media','Finalizada'),
(28,'2026-02-28',18,'2026-03-01','2026-03-04','Alta','Pendiente'),
(29,'2026-03-01',20,'2026-03-02','2026-03-05','Media','En proceso'),
(30,'2026-03-02',22,'2026-03-03','2026-03-06','Alta','Finalizada'),
(31,'2025-03-01',10,'2025-03-02','2025-03-05','Alta','En proceso'),
(32,'2025-03-02',20,'2025-03-03','2025-03-06','Media','Pendiente'),
(33,'2025-03-03',15,'2025-03-04','2025-03-07','Alta','En proceso'),
(34,'2025-03-04',12,'2025-03-05','2025-03-08','Baja','Pendiente'),
(35,'2025-03-05',18,'2025-03-06','2025-03-09','Media','Finalizada'),
(36,'2025-03-06',14,'2025-03-07','2025-03-10','Alta','En proceso'),
(37,'2025-03-07',16,'2025-03-08','2025-03-11','Media','Pendiente'),
(38,'2025-03-08',22,'2025-03-09','2025-03-12','Alta','En proceso'),
(39,'2024-03-09',11,'2024-03-10','2024-03-13','Baja','Finalizada'),
(40,'2024-03-10',19,'2024-03-11','2024-03-14','Media','En proceso'),
(41,'2024-03-11',13,'2024-03-12','2024-03-15','Alta','Pendiente'),
(42,'2024-03-12',17,'2024-03-13','2024-03-16','Media','En proceso'),
(43,'2024-03-13',21,'2024-03-14','2024-03-17','Alta','Pendiente'),
(44,'2024-03-14',23,'2024-03-15','2024-03-18','Media','Finalizada'),
(45,'2024-03-15',25,'2024-03-16','2024-03-19','Alta','En proceso'),
(46,'2023-03-11',13,'2023-03-12','2023-03-15','Alta','Pendiente'),
(47,'2023-03-12',17,'2023-03-13','2023-03-16','Media','En proceso'),
(48,'2023-03-13',21,'2023-03-14','2023-03-17','Alta','Pendiente'),
(49,'2023-03-14',23,'2023-03-15','2023-03-18','Media','Finalizada'),
(50,'2023-03-15',25,'2023-03-16','2023-03-19','Alta','En proceso');


-- insertar en la tabla orden_produccion los datos correspondientes.

INSERT INTO vehiculo (vin, codigo_modelo, codigo_version, color, caracteristicas, fecha_inicio, fecha_finalizacion, numero_linea, resultado_final) VALUES
('VIN001',1,1,'Rojo','Base','2024-03-02','2024-03-05',1,'OK'),
('VIN002',2,2,'Azul','Full','2024-03-03','2024-03-06',2,'OK'),
('VIN003',3,3,'Negro','Sport','2024-03-04','2024-03-07',3,'OK'),
('VIN004',4,4,'Blanco','Luxury','2024-03-05','2024-03-08',4,'OK'),
('VIN005',5,5,'Rojo','Base','2024-03-06','2024-03-09',5,'OK'),
('VIN006',6,6,'Azul','Full','2024-03-07','2024-03-10',6,'OK'),
('VIN007',7,7,'Negro','Sport','2024-03-08','2024-03-11',7,'OK'),
('VIN008',8,8,'Blanco','Luxury','2024-03-09','2024-03-12',8,'OK'),
('VIN009',9,9,'Rojo','Base','2024-03-10','2024-03-13',9,'OK'),
('VIN010',10,10,'Azul','Full','2024-03-11','2024-03-14',10,'OK'),
('VIN011',11,11,'Negro','Sport','2024-03-12','2024-03-15',11,'OK'),
('VIN012',12,12,'Blanco','Luxury','2024-03-13','2024-03-16',12,'OK'),
('VIN013',13,13,'Rojo','Base','2024-03-14','2024-03-17',13,'OK'),
('VIN014',14,14,'Azul','Full','2024-03-15','2024-03-18',14,'OK'),
('VIN015',15,15,'Negro','Sport','2024-03-16','2024-03-19',15,'OK'),
('VIN016',16,16,'Rojo','Full','2026-01-01','2026-01-03',1,'OK'),
('VIN017',17,17,'Azul','Híbrido','2026-01-02','2026-01-04',2,'OK'),
('VIN018',18,18,'Negro','Diesel','2026-01-03','2026-01-05',3,'OK'),
('VIN019',19,19,'Blanco','Base','2026-01-04','2026-01-06',4,'OK'),
('VIN020',20,20,'Gris','Full','2026-01-05','2026-01-07',5,'OK'),
('VIN021',21,21,'Rojo','Full','2026-01-06','2026-01-08',6,'OK'),
('VIN022',22,22,'Azul','Híbrido','2026-01-07','2026-01-09',7,'OK'),
('VIN023',23,23,'Negro','Diesel','2026-01-08','2026-01-10',8,'OK'),
('VIN024',24,24,'Blanco','Base','2026-01-09','2026-01-11',9,'OK'),
('VIN025',25,25,'Gris','Full','2026-01-10','2026-01-12',10,'OK'),
('VIN026',26,26,'Rojo','Full','2026-01-11','2026-01-13',11,'OK'),
('VIN027',27,27,'Azul','Híbrido','2026-01-12','2026-01-14',12,'OK'),
('VIN028',28,28,'Negro','Diesel','2026-01-13','2026-01-15',13,'OK'),
('VIN029',29,29,'Blanco','Base','2026-01-14','2026-01-16',14,'OK'),
('VIN030',30,30,'Gris','Full','2026-01-15','2026-01-17',15,'OK'),
('VIN031',31,31,'Rojo','Full','2026-01-16','2026-01-18',1,'OK'),
('VIN032',32,32,'Azul','Híbrido','2026-01-17','2026-01-19',2,'OK'),
('VIN033',33,33,'Negro','Diesel','2026-01-18','2026-01-20',3,'OK'),
('VIN034',34,34,'Blanco','Base','2026-01-19','2026-01-21',4,'OK'),
('VIN035',35,35,'Gris','Full','2026-01-20','2026-01-22',5,'OK'),
('VIN036',36,36,'Rojo','Full','2026-01-21','2026-01-23',6,'OK'),
('VIN037',37,37,'Azul','Híbrido','2026-01-22','2026-01-24',7,'OK'),
('VIN038',38,38,'Negro','Diesel','2026-01-23','2026-01-25',8,'OK'),
('VIN039',39,39,'Blanco','Base','2026-01-24','2026-01-26',9,'OK'),
('VIN040',40,40,'Gris','Full','2026-01-25','2026-01-27',10,'OK'),
('VIN041',41,41,'Rojo','Full','2026-01-26','2026-01-28',11,'OK'),
('VIN042',42,42,'Azul','Híbrido','2026-01-27','2026-01-29',12,'OK'),
('VIN043',43,43,'Negro','Diesel','2026-01-28','2026-01-30',13,'OK'),
('VIN044',44,44,'Blanco','Base','2026-01-29','2026-01-31',14,'OK'),
('VIN045',45,45,'Gris','Full','2026-01-30','2026-02-01',15,'OK'),
('VIN046',46,46,'Rojo','Full','2026-02-01','2026-02-03',1,'OK'),
('VIN047',47,47,'Azul','Híbrido','2026-02-02','2026-02-04',2,'OK'),
('VIN048',48,48,'Negro','Diesel','2026-02-03','2026-02-05',3,'OK'),
('VIN049',49,49,'Blanco','Base','2026-02-04','2026-02-06',4,'OK'),
('VIN050',50,50,'Gris','Full','2026-02-05','2026-02-07',5,'OK');

-- insertar en la tabla control_calidad los datos correspondientes.

INSERT INTO control_calidad (numero_produccion, VIN, codigo_estacion, fecha_hora, inspector, parametros_evaluados, resultados, defectos_encontrados, decision_final) VALUES
(1,'VIN001',1,'2024-03-05 10:00','Inspector1','OK','Aprobado','Ninguno','Liberado'),
(2,'VIN002',2,'2024-03-06 11:00','Inspector2','OK','Aprobado','Ninguno','Liberado'),
(3,'VIN003',3,'2024-03-07 12:00','Inspector3','OK','Aprobado','Ninguno','Liberado'),
(4,'VIN004',4,'2024-03-08 13:00','Inspector4','OK','Aprobado','Ninguno','Liberado'),
(5,'VIN005',5,'2024-03-09 14:00','Inspector5','OK','Aprobado','Ninguno','Liberado'),
(6,'VIN006',6,'2024-03-10 15:00','Inspector6','OK','Aprobado','Ninguno','Liberado'),
(7,'VIN007',7,'2024-03-11 16:00','Inspector7','OK','Aprobado','Ninguno','Liberado'),
(8,'VIN008',8,'2024-03-12 17:00','Inspector8','OK','Aprobado','Ninguno','Liberado'),
(9,'VIN009',9,'2024-03-13 18:00','Inspector9','OK','Aprobado','Ninguno','Liberado'),
(10,'VIN010',10,'2024-03-14 19:00','Inspector10','OK','Aprobado','Ninguno','Liberado'),
(11,'VIN011',11,'2024-03-15 20:00','Inspector11','OK','Aprobado','Ninguno','Liberado'),
(12,'VIN012',12,'2024-03-16 21:00','Inspector12','OK','Aprobado','Ninguno','Liberado'),
(13,'VIN013',13,'2024-03-17 22:00','Inspector13','OK','Aprobado','Ninguno','Liberado'),
(14,'VIN014',14,'2024-03-18 23:00','Inspector14','OK','Aprobado','Ninguno','Liberado'),
(15,'VIN015',15,'2024-03-19 08:00','Inspector15','OK','Aprobado','Ninguno','Liberado'),
(16,'VIN016',1,'2026-02-05 10:00','Inspector16','OK','Aprobado','Ninguno','Liberado'),
(17,'VIN017',2,'2026-02-06 11:00','Inspector17','OK','Aprobado','Ninguno','Liberado'),
(18,'VIN018',3,'2026-02-07 12:00','Inspector18','OK','Aprobado','Ninguno','Liberado'),
(19,'VIN019',4,'2026-02-08 13:00','Inspector19','OK','Aprobado','Ninguno','Liberado'),
(20,'VIN020',5,'2026-02-09 14:00','Inspector20','OK','Aprobado','Ninguno','Liberado'),
(21,'VIN021',6,'2026-02-10 15:00','Inspector21','OK','Aprobado','Ninguno','Liberado'),
(22,'VIN022',7,'2026-02-11 16:00','Inspector22','OK','Aprobado','Ninguno','Liberado'),
(23,'VIN023',8,'2026-02-12 17:00','Inspector23','OK','Aprobado','Ninguno','Liberado'),
(24,'VIN024',9,'2026-02-13 18:00','Inspector24','OK','Aprobado','Ninguno','Liberado'),
(25,'VIN025',10,'2026-02-14 19:00','Inspector25','OK','Aprobado','Ninguno','Liberado'),
(26,'VIN026',11,'2026-02-15 20:00','Inspector26','OK','Aprobado','Ninguno','Liberado'),
(27,'VIN027',12,'2026-02-16 21:00','Inspector27','OK','Aprobado','Ninguno','Liberado'),
(28,'VIN028',13,'2026-02-17 22:00','Inspector28','OK','Aprobado','Ninguno','Liberado'),
(29,'VIN029',14,'2026-02-18 23:00','Inspector29','OK','Aprobado','Ninguno','Liberado'),
(30,'VIN030',15,'2026-02-19 08:00','Inspector30','OK','Aprobado','Ninguno','Liberado'),
(31,'VIN031',1,'2026-02-20 09:00','Inspector31','OK','Aprobado','Ninguno','Liberado'),
(32,'VIN032',2,'2026-02-21 10:00','Inspector32','OK','Aprobado','Ninguno','Liberado'),
(33,'VIN033',3,'2026-02-22 11:00','Inspector33','OK','Aprobado','Ninguno','Liberado'),
(34,'VIN034',4,'2026-02-23 12:00','Inspector34','OK','Aprobado','Ninguno','Liberado'),
(35,'VIN035',5,'2026-02-24 13:00','Inspector35','OK','Aprobado','Ninguno','Liberado'),
(36,'VIN036',6,'2026-02-25 14:00','Inspector36','OK','Aprobado','Ninguno','Liberado'),
(37,'VIN037',7,'2026-02-26 15:00','Inspector37','OK','Aprobado','Ninguno','Liberado'),
(38,'VIN038',8,'2026-02-27 16:00','Inspector38','OK','Aprobado','Ninguno','Liberado'),
(39,'VIN039',9,'2026-02-28 17:00','Inspector39','OK','Aprobado','Ninguno','Liberado'),
(40,'VIN040',10,'2026-03-01 18:00','Inspector40','OK','Aprobado','Ninguno','Liberado'),
(41,'VIN041',11,'2026-03-02 19:00','Inspector41','OK','Aprobado','Ninguno','Liberado'),
(42,'VIN042',12,'2026-03-03 20:00','Inspector42','OK','Aprobado','Ninguno','Liberado'),
(43,'VIN043',13,'2026-03-04 21:00','Inspector43','OK','Aprobado','Ninguno','Liberado'),
(44,'VIN044',14,'2026-03-05 22:00','Inspector44','OK','Aprobado','Ninguno','Liberado'),
(45,'VIN045',15,'2026-03-06 23:00','Inspector45','OK','Aprobado','Ninguno','Liberado'),
(46,'VIN046',11,'2026-03-02 19:00','Inspector41','OK','Aprobado','Ninguno','Liberado'),
(47,'VIN047',12,'2026-03-03 20:00','Inspector42','OK','Aprobado','Ninguno','Liberado'),
(48,'VIN048',13,'2026-03-04 21:00','Inspector43','OK','Aprobado','Ninguno','Liberado'),
(49,'VIN049',14,'2026-03-05 22:00','Inspector44','OK','Aprobado','Ninguno','Liberado'),
(50,'VIN050',15,'2026-03-06 23:00','Inspector45','OK','Aprobado','Ninguno','Liberado');


-- insertar en la tabla modelo_componente los datos correspondientes.

INSERT INTO modelo_componente (codigo_modelo, codigo_componente, componentes_requeridos) VALUES
(1,1,1),(2,2,4),(3,3,1),(4,4,2),(5,5,2),
(6,6,1),(7,7,3),(8,8,4),(9,9,1),(10,10,1),
(11,11,2),(12,12,1),(13,13,1),(14,14,2),(15,15,2),
(16,1,2),(16,2,3),(17,3,1),(17,4,2),(18,5,2),
(18,6,1),(19,7,3),(19,8,2),(20,9,1),(20,10,2),
(21,11,2),(21,12,1),(22,13,1),(22,14,2),(23,15,2),
(24,1,1),(25,2,2),(26,3,3),(27,4,1),(28,5,2),
(29,6,1),(30,7,3),(31,8,2),(32,9,1),(33,10,2),
(34,11,2),(35,12,1),(36,13,1),(37,14,2),(38,15,2),
(39,1,1),(40,2,2),(41,3,3),(42,4,1),(43,5,2),
(44,6,1),(45,7,3),(46,8,2),(47,9,1),(48,10,2),
(49,11,2),(50,12,1);


-- insertar en la tabla estacion_empleado los datos correspondientes.

INSERT INTO estacion_empleado (codigo_estacion, numero_empleado) VALUES
(1,1),(2,2),(3,3),(4,4),(5,5),
(6,6),(7,7),(8,8),(9,9),(10,10),
(11,11),(12,12),(13,13),(14,14),(15,15),
(16,16),(17,17),(18,18),(19,19),(20,20),
(21,21),(22,22),(23,23),(24,24),(25,25),
(26,26),(27,27),(28,28),(29,29),(30,30),
(31,31),(32,32),(33,33),(34,34),(35,35),
(36,36),(37,37),(38,38),(39,39),(40,40),
(41,41),(42,42),(43,43),(44,44),(45,45),
(46,46),(47,47),(48,48),(49,49),(50,50);


-- stored procedures

-- 1. CrearOrdenProduccion: Crea una orden de producción para un modelo específico.

DELIMITER $$

CREATE PROCEDURE CrearOrdenProduccion(
    IN p_codigo_modelo INT,
    IN p_cantidad INT,
    IN p_fecha_inicio DATE,
    IN p_fecha_fin DATE,
    IN p_prioridad VARCHAR(50)
)
BEGIN
    INSERT INTO orden_produccion(
        fecha_emision,
        codigo_modelo,
        cantidad_producir,
        fecha_inicio,
        fecha_finalizacion,
        prioridad,
        estado_actual
    )
    VALUES(
        CURDATE(),
        p_codigo_modelo,
        p_cantidad,
        p_fecha_inicio,
        p_fecha_fin,
        p_prioridad,
        'Pendiente'
    );
END$$

DELIMITER ;

-- 2. AsignarComponentesProduccion: Asigna componentes del inventario a una orden de producción.

DELIMITER $$

CREATE PROCEDURE AsignarComponentesProduccion(
    IN p_codigo_componente INT,
    IN p_cantidad INT
)
BEGIN
    UPDATE inventario
    SET cantidad_disponible = cantidad_disponible - p_cantidad
    WHERE codigo_componente = p_codigo_componente;
END$$

DELIMITER ;

-- 3. RegistrarControlCalidad: Registra una inspección de calidad para un vehículo.

DELIMITER $$

CREATE PROCEDURE RegistrarControlCalidad(
    IN p_numero_produccion INT,
    IN p_vin VARCHAR(50),
    IN p_codigo_estacion INT,
    IN p_inspector VARCHAR(100),
    IN p_resultado VARCHAR(100),
    IN p_defectos VARCHAR(255)
)
BEGIN
    INSERT INTO control_calidad(
        numero_produccion,
        VIN,
        codigo_estacion,
        fecha_hora,
        inspector,
        parametros_evaluados,
        resultados,
        defectos_encontrados,
        decision_final
    )
    VALUES(
        p_numero_produccion,
        p_vin,
        p_codigo_estacion,
        NOW(),
        p_inspector,
        'General',
        p_resultado,
        p_defectos,
        'Pendiente'
    );
END$$

DELIMITER ;

-- 4. GestionarCompraComponentes: Genera órdenes de compra para componentes con bajo stock.

DELIMITER $$

CREATE PROCEDURE GestionarCompraComponentes()
BEGIN
    INSERT INTO orden_compra (fecha, codigo_proveedor, fecha_entrega, terminos_envio, condiciones_pago, estado)
    SELECT 
        CURDATE(),
        c.codigo_proveedor,
        DATE_ADD(CURDATE(), INTERVAL 7 DAY),
        'Normal',
        '30 dias',
        'Pendiente'
    FROM componente c
    INNER JOIN inventario i ON c.codigo_componente = i.codigo_componente
    WHERE i.cantidad_disponible < c.stock_minimo;
END$$

DELIMITER ;

-- 5. AsignarPersonalLinea: Asigna operarios a estaciones de trabajo verificando habilidades.

DELIMITER $$

CREATE PROCEDURE AsignarPersonalLinea(
    IN p_codigo_estacion INT,
    IN p_habilidad VARCHAR(100)
)
BEGIN
    INSERT INTO estacion_empleado (codigo_estacion, numero_empleado)
    SELECT p_codigo_estacion, e.numero_empleado
    FROM empleado e
    INNER JOIN empleado_habilidad eh 
    ON e.numero_empleado = eh.numero_empleado
    WHERE eh.habilidad LIKE CONCAT('%', p_habilidad, '%');
END$$

DELIMITER ;


-- VIEWS

-- 1. V_EstadoLineasProduccion: Muestra el estado actual de las líneas de producción.

CREATE VIEW V_EstadoLineasProduccion AS
SELECT numero_linea, tipo_vehiculo, estado_operativo, turno_activo
FROM linea_produccion;

-- SELECT * FROM V_EstadoLineasProduccion;

-- 2. V_InventarioComponentes: Detalla el inventario de componentes con stock y proveedores.

CREATE VIEW V_InventarioComponentes AS
SELECT c.descripcion, i.cantidad_disponible, c.stock_minimo, p.razon_social
FROM componente c
INNER JOIN inventario i 
ON c.codigo_componente = i.codigo_componente
INNER JOIN proveedores p 
ON c.codigo_proveedor = p.codigo_proveedor;

-- SELECT * FROM V_InventarioComponentes;

-- 3. V_OrdenesProduccionActivas: Lista de órdenes de producción en proceso.

CREATE VIEW V_OrdenesProduccionActivas AS
SELECT *
FROM orden_produccion
WHERE estado_actual = 'En proceso';

-- SELECT * FROM V_OrdenesProduccionActivas;

-- 4. V_EstadisticasCalidad: Estadísticas de defectos por modelo, estación y tipo.

CREATE VIEW V_EstadisticasCalidad AS
SELECT v.codigo_modelo,
       c.codigo_estacion,
       COUNT(*) AS total_inspecciones,
       SUM(CASE WHEN c.defectos_encontrados != 'Ninguno' THEN 1 ELSE 0 END) AS defectos
FROM control_calidad c
INNER JOIN vehiculo v 
ON c.VIN = v.VIN
GROUP BY v.codigo_modelo, c.codigo_estacion;

-- SELECT * FROM V_EstadisticasCalidad;


-- 5. V_RendimientoProveedores: Análisis de rendimiento de proveedores por tiempo, calidad y precio.

CREATE VIEW V_RendimientoProveedores AS
SELECT p.razon_social,
       AVG(c.tiempo_entrega) AS tiempo_promedio,
       AVG(p.evaluacion_calidad) AS calidad_promedio,
       AVG(c.costo_unitario) AS costo_promedio
FROM proveedores p
INNER JOIN componente c 
ON p.codigo_proveedor = c.codigo_proveedor
GROUP BY p.razon_social;

-- SELECT * FROM V_RendimientoProveedores;



# Descripción general del proyecto

El sistema Autofactory es una solución integral diseñada para la empresa Motores Eficientes S.A., cuyo propósito es gestionar, controlar y optimizar todos los procesos productivos dentro de una fábrica automotriz.

Este sistema permite administrar de manera centralizada la información relacionada con la fabricación de vehículos, desde la planificación de la producción hasta el control de calidad y la gestión de inventarios, proveedores y recursos humanos.

# Instrucciones de uso

1. Configuración inicial
Antes de utilizar el sistema, se debe:
Crear la base de datos:
CREATE DATABASE Autofactory;
USE Autofactory;
2. Ejecutar el script de creación de tablas.
3. Ejecutar los scripts de inserción de datos.
# Es importante ejecutar los scripts en orden para evitar errores de claves foráneas.


# Alcance del sistema
Autofactory cubre las principales áreas operativas de la empresa:

# Gestión de modelos de vehículos
El sistema registra cada modelo con su información esencial, incluyendo:
    • Código único 
    • Nombre y categoría (sedán, SUV, pickup) 
    • Versiones disponibles 
    • Especificaciones técnicas 
    • Componentes necesarios 
    • Tiempo estándar de ensamblaje 
Esto permite una planificación precisa de la producción.

 # Control de líneas y estaciones de producción
Se gestionan las líneas de ensamblaje y sus características:
    • Capacidad diaria 
    • Número de estaciones 
    • Supervisores 
    • Turnos y estado operativo 
Además, cada estación de trabajo define:
    • Operaciones realizadas 
    • Herramientas necesarias 
    • Tiempo estándar 
    • Personal asignado 
Esto facilita la organización eficiente del proceso productivo.

# Gestión de componentes e inventario
El sistema controla los insumos necesarios para la producción:
    • Componentes con sus características técnicas 
    • Proveedores asociados 
    • Costos y tiempos de entrega 
El inventario permite:
    • Controlar stock disponible 
    • Ubicación en almacén 
    • Calidad de los productos 
Esto asegura la disponibilidad oportuna de materiales.

 Planificación de la producción
Las órdenes de producción incluyen:
    • Modelo a fabricar 
    • Cantidad requerida 
    • Fechas programadas 
    • Prioridad y estado 
Esto permite llevar un seguimiento detallado del proceso de fabricación.

 # Control de calidad
El sistema registra inspecciones realizadas durante el proceso:
    • Vehículos evaluados (VIN) 
    • Estación de inspección 
    • Inspector responsable 
    • Resultados y defectos 
Garantiza que los vehículos cumplan con los estándares de calidad establecidos.

 # Gestión de empleados
Se administra la información del personal:
    • Datos personales 
    • Puesto y especialización 
    • Habilidades certificadas 
    • Línea y turno asignado 
    • Evaluación de desempeño 
Esto facilita la asignación eficiente del talento humano.

 # Gestión de proveedores y compras
El sistema permite:
    • Registrar proveedores y su información 
    • Evaluar su desempeño 
    • Gestionar órdenes de compra 
    • Controlar componentes adquiridos 
Esto mejora la relación con proveedores y el abastecimiento.

 # Registro de vehículos ensamblados
Cada vehículo producido queda registrado con:
    • VIN (identificación única) 
    • Modelo y versión 
    • Características específicas 
    • Fechas de ensamblaje 
    • Resultados finales 
Esto permite la trazabilidad completa del producto final.

# El sistema permite realizar consultas como:

  • Estado de órdenes de producción
  • Inventario disponible
  • Historial de vehículos producidos
  • Evaluación de proveedores
  • Desempeño de empleados

 # Estructura de los módulos implementados


# Sistema “Autofactory”
El sistema se encuentra dividido en módulos funcionales que permiten gestionar de manera organizada cada área del proceso productivo automotriz.

 1. Módulo de Gestión de Vehículos
    
Encargado de administrar la información relacionada con los vehículos.
Funciones principales:
    • Registro de modelos de vehículos 
    • Gestión de versiones disponibles 
    • Definición de especificaciones técnicas 
    • Asociación de componentes requeridos 
    • Control del tiempo de ensamblaje 
Tablas relacionadas:
    • modelo_vehiculo 
    • version_vehiculo 
    • modelo_componente 

 2. Módulo de Producción
Gestiona todo el proceso de fabricación de los vehículos.
Funciones principales:
    • Registro de líneas de producción 
    • Administración de estaciones de trabajo 
    • Asignación de empleados a estaciones 
    • Control de órdenes de producción 
    • Seguimiento del estado de producción 
Tablas relacionadas:
    • linea_produccion 
    • estacion_trabajo 
    • orden_produccion 
    • estacion_empleado 

 3. Módulo de Inventario y Componentes
Controla los insumos necesarios para la fabricación.
Funciones principales:
    • Registro de componentes 
    • Control de stock disponible 
    • Gestión de ubicación en almacén 
    • Seguimiento de calidad de los componentes 
    • Definición de stock mínimo 
Tablas relacionadas:
    • componente 
    • inventario 

 4. Módulo de Compras y Proveedores
Administra la relación con proveedores y adquisición de insumos.
Funciones principales:
    • Registro de proveedores 
    • Evaluación de proveedores 
    • Gestión de órdenes de compra 
    • Control de componentes solicitados 
    • Seguimiento de entregas 
Tablas relacionadas:
    • proveedores 
    • orden_compra 
    • especificacion_orden_compra 

 5. Módulo de Recursos Humanos
Gestiona la información del personal involucrado en la producción.
Funciones principales:
    • Registro de empleados 
    • Control de habilidades certificadas 
    • Asignación a líneas de producción 
    • Gestión de turnos 
    • Evaluación de desempeño 
Tablas relacionadas:
    • empleado 
    • empleado_habilidad 

 6. Módulo de Control de Calidad
Permite verificar la calidad del proceso y del producto final.
Funciones principales:
    • Registro de inspecciones 
    • Evaluación de vehículos por VIN 
    • Control de defectos encontrados 
    • Registro de resultados 
    • Toma de decisiones (aprobado/rechazado) 
Tablas relacionadas:
    • control_calidad 

 7. Módulo de Ensamblaje y Producto Final
Registra los vehículos producidos y su trazabilidad.
Funciones principales:
    • Registro de vehículos ensamblados 
    • Asociación con modelo y versión 
    • Control de fechas de ensamblaje 
    • Registro de características específicas 
    • Resultado final del proceso 
Tablas relacionadas:
    • vehiculo 

 # Integración entre módulos
Todos los módulos están interconectados mediante claves foráneas, lo que permite:
    • Integridad de los datos 
    • Trazabilidad completa del proceso 
    • Relación entre producción, inventario, empleados y calidad 

#  Conclusión
La estructura modular del sistema Autofactory permite:
    • Separar responsabilidades por áreas 
    • Facilitar el mantenimiento del sistema 
    • Escalar funcionalidades en el futuro 
    • Mejorar la organización de la información 












