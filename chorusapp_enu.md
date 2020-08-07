# CHORUS APP

Esta aplicación está diseñada para dar solución a los procesos de manejo de inventario, venta y los diferentes procesos que la mayoría de empresas tradicionales tienen que sobrellevar dia con dia. El proyecto consta de múltiples fases comenzando con una versión básica y genérica hasta llegar a una aplicación hecha a la medida según el cliente así lo solicite. Algo muy muy importante es entender el hecho que la versión base de este sistema debe ser genérica pues no está dirigida a un cliente específico y debe de adaptarse a la mayoría de negocios sea cual sea su actividad.

**MUY IMPORTANTE**: Este sistema está pensado para trabajar con múltiples sucursales de manera que toda transacción, empleado e inventario dependen de su sucursal para poder operar.

La versión básica o core del sistema debe de tener los siguientes módulos: 


- Login
- Empleado
- Roles de Usuario
- Sucursal
- Producto
- Cliente
- Proveedor
- Facturación
- Transferencias (Producto de Sucursal a Sucursal)
- Ingreso de Productos
- Pedido a Proveedores
- Pedido de Clientes

A Continuación se detallarán estos módulos. Es importante entender que la metodología de trabajo será basada en prototipos y que los mismos serán reiterados una y otra vez hasta que cumplan con los requerimientos genéricos solicitados.


**Arquitectura**

- PWA con Angular
- Comunicación con API REST

- NodeJS
- DB Oracle Cloud


**Features del Front End** 
Para cualquier módulo es obligatorio que cuenten con las siguientes características

- Utilización Offline
- No será responsive, es pensado principalmente para móvil pero útil también en pc
Aunque el usuario se salga de la página, al volver a entrar los formularios deben recuperar la información (Se deberá manejar una copia de los datos en caché)
- Validaciones y feedback al momento de llenar campos
- Diseño simple pero altamente funcional y lo más minimalista posible, que no sature de información la vista del usuario
- El manejo de permisos (Que puede y que no puede hacer el usuario) depende de una lista de tareas que solo el Administrador puede definir
- Cifrar los JSON de comunicación con MD5

---
# Módulo de Login

Este es el módulo que permite a los diferentes usuarios entrar al sistema, debe de hacerse mediante autentificación por token. Este módulo debe de comunicarse con la API que le retornara el nombre de la empresa a la que pertenece. Luego se registrará mediante:

- DPI
- Password

Este módulo debe de recordar la sesión del usuario durante 24 horas para evitar inconvenientes de loguearse cada vez que se necesita usar la app. El DPI deve de estar automáticamente escrito en el campo de ID una vez que el usuario haya usado la app

Cualquier usuario entrara por medio de un password que asigna el Administrador de forma predeterminada y luego, este módulo incluye un submódulo para que el empleado pueda cambiar ese password por uno que solo el sepa











---

# Módulo de Empleado

La aplicación será utilizada por los siguientes tipos de usuarios: 

- Colaborador
- Jefe
- Administrador
- Súper Usuario (SU)

**Colaborador**
Este usuario puede realizar el ejercicio de venta, realizar nuevos pedidos a proveedores y consultar únicamente información de clientes y proveedores.

**Jefe**
Tiene todos los permisos de un vendedor pero puede editar la información de los clientes y proveedores, así como realizar devoluciones y anular facturas.

**Administrador**
Posee todos los permisos del jefe de la tienda pero este es el unico que podra administrar los permisos, agregar o quitar permisos a cada puesto. El es quien registra a los nuevos empleados y los asigna a cada sucursal.

**SU**
El único que posee todos los permisos y el único con capacidad para crear usuarios del tipo Administrador
Los únicos que pueden utilizar el Módulo de Empleado son SU y Administrador

Se considera que TODOS los actores del sistema son Empleados y se desea conocer:

- DPI
- Password (Alfanumérico, 8 Caracteres mínimo, al menos una mayúscula y dígitos)
- Nombre y Apellido
- Teléfono
- Dirección 
- EMail
- Puesto (Vendedor, Jefe, Administrador, SU)
- Sucursal a la que será asignado
- Estado(Suspendido,De vacaciones, Despedido, Activo)
- Horario de Trabajo 
- Fecha de contratación en la empresa
- Salario Neto del Puesto

NO se considera para esta versión un historial de estados o historial de fechas, solamente son campos que son actualizados en cualquier momento

---

# Módulo de Sucursal
Existen 2 tipos de sucursales:

- Tienda
- Bodega

La diferencia es que una bodega no puede vender ni facturar, únicamente recibir producto, realizar pedidos a proveedores, realizar traslados y también tiene colaboradores y un jefe asignados. Cuenta con una cantidad de productos que están a disponibilidad de las tiendas cuando se requiera

De una sucursal se desea conocer:

- Nombre
- Dirección
- EMail
- Teléfono
- Equipo de Trabajo
- Estado (Operativa, en mantenimiento,inventariado)
- Coordenadas según Googlemaps

---
# Módulo de Cliente

El cliente es el actor más importante para la lógica del negocio por lo que es importante recolectar la mayor cantidad de información posible, tanto información personal como información de su historial de compras. (El historial de compras es simplemente el listado de facturas que el cliente tiene)

Del cliente es necesario saber:

- Nombre y Apellido 
- Nit
- Teléfono
- Email
- Dirección

Este módulo servirá para darle seguimiento a un cliente y consultar su historial de facturas. Los clientes también podrán registrarse si su número de nit no está en el sistema y al momento de facturar solo desean dar un nombre y un apellido

Permisos: Jefe en adelante





---

# Módulo de Producto
El producto es también de los elementos más importantes en el negocio, de un producto se desea saber:

- SKU
- Descripción
- Proveedores de ese producto
- Imagen
- Costo de Adquisición
- Prorrateo (Listado de gastos adicionales como flete, impuestos, solo se registra una descripción y su valor)
- Precio Final
- Margen de Utilidad
- Clasificación (AA A AB B C)
- Cuidados de Almacenamiento
- Tiempo de Vida Útil (Para perecederos)

La creación de un nuevo producto es trabajo únicamente de los Administradores y los colaboradores y jefes solo puede leer la información

---
# Módulo de Ingreso de Productos

Este módulo corresponde al método por el cual se ingresa nueva mercadería al sistema, es importante que se pueda adjuntar una fotografía del documento original que nos lleva el proveedor para compararla con las cantidades físicas que se reciben. De un ingreso de productos se desea conocer:

- Proveedor
- Contacto del Proveedor
- Teléfono del Contacto del proveedor
- Fotografía del envío del proveedor (O foto de la Factura)
- Sucursal a la que ingresa
- fecha y hora de la recepción
- ID empleado encargado del conteo físico
- Observaciones
- Monto del Pago

El jefe de la tienda en adelante pueden recibir mercadería
