# Práctica 9 · Diagrama de clases UML

## 1. Clases identificadas

Para el módulo de gestión de pedidos y catálogo de Open Hub, se identificaron las clases de dominio Cliente, Pedido, DetallePedido, Producto, Inventario y Empleado. Cada clase se liga directamente a requisitos del DRS: Cliente y Pedido apoyan RF-01 y RF-02, Producto y Inventario cubren RF-03 y RF-04, mientras que Empleado y Pedido respaldan RF-05 y RNF-02. La propuesta mantiene el foco en entidades del negocio y evita una clase genérica Sistema, porque el dominio exige objetos con responsabilidad clara y trazabilidad funcional.

## 2. Atributos y tipos

- Cliente: `clienteId: entero`, `nombre: texto`, `correo: texto`, `telefono: texto`
- Pedido: `pedidoId: entero`, `fecha: fecha`, `total: decimal`, `estado: texto`
- Producto: `productoId: entero`, `nombre: texto`, `precioUnitario: decimal`, `stockActual: entero`
- DetallePedido: `cantidad: entero`, `subtotal: decimal`, `descuento: decimal`
- Inventario: `kilos: decimal`, `ubicacion: texto`, `nivelInventario: entero`
- Empleado: `empleadoId: entero`, `rol: texto`, `turno: texto`, `activo: booleano`

## 3. Operaciones

Las operaciones del dominio se definen con parámetros y se ligan a los requisitos del negocio. El caso principal es `registrarPedido(cliente: Cliente, producto: Producto, cantidad: entero): booleano` (RF-01), `validarDisponibilidad(producto: Producto, cantidad: entero): booleano` (RF-02), `calcularTotal(detalle: DetallePedido): decimal` (RF-03), `actualizarStock(producto: Producto, cantidad: entero): void` (RF-04), `asignarTurno(empleado: Empleado, turno: texto): void` (RF-05) y `enviarNotificacionPedido(pedido: Pedido): void` (RNF-02). Estas operaciones quedan ancladas al flujo de compra y a la logística interna del almacén.

## 4. Encapsulamiento y visibilidad

Se aplicó encapsulamiento para ocultar el estado sensible de cada clase. Los atributos como `- saldo: decimal`, `- stockActual: entero` y `- total: decimal` quedan privados, porque no deben modificarse desde fuera sin validación. En cambio, las operaciones clave se exponen con `+`, por ejemplo `+ calcularTotal(): decimal`, `+ registrarPedido(...): booleano` y `+ actualizarStock(...): void`, con el fin de mantener control sobre el comportamiento y la integridad del dominio.

## 5. Asociaciones y multiplicidad

Las asociaciones principales del diagrama son: Cliente `1..1` participa en `1..*` Pedido, Pedido `1..1` contiene `1..*` DetallePedido, Producto `1..1` se relaciona con `0..*` DetallePedido, Inventario `1..1` administra `1..*` Producto y Empleado `1..1` atiende `0..*` Pedido. La multiplicidad refleja que un cliente puede registrar varios pedidos y que un pedido puede contener varios productos, evitando relaciones ambiguas o inconsistentes.

## 6. Agregación, composición y herencia

En el modelo se usa agregación para representar que un Pedido está compuesto por varios DetallePedido y que un Inventario agrupa varios Productos. También se contempla composición cuando un Pedido depende de sus líneas de detalle para existir como entidad completa, ya que un detalle no tiene sentido fuera del pedido. En cuanto a herencia, se podrían definir especializaciones como `EmpleadoVendedor` y `EmpleadoAlmacen`, aunque en este diseño se mantuvo un único tipo base con atributos comunes para no complejizar la primera versión del modelo.

## 7. Trazabilidad

| Clase | Requisito | Justificación |
|---|---|---|
| Cliente | RF-01 | Registra la información del comprador y su acceso al proceso. |
| Pedido | RF-02 | Controla la compra y el estado del pedido. |
| DetallePedido | RF-03 | Representa los productos solicitados y sus cantidades. |
| Producto | RF-04 | Define los artículos que se venden y el stock disponible. |
| Inventario | RNF-02 | Mantiene la disponibilidad operativa del almacén. |
| Empleado | RF-05 | Responde a la asignación y seguimiento del trabajo del personal. |

## 8. Diagrama y notación UML

El diagrama se elaboró en Architect y se mantiene como archivo fuente en `modelo/OpenHub-Equipo.qea`. La exportación del diagrama se guarda en `docs/analisis/diagrama-clases.png` para revisión visual y trazabilidad del trabajo.

![Diagrama de clases](../docs/analisis/diagrama-clases.png)

## 9. Trabajo por pull request

El trabajo se distribuyó en dos pull requests principales y la revisión cruzada se documentó en conjunto. El equipo de backend revisó #12, el equipo de análisis validaró #14 y la coordinación final se cerró con #18. Cada integrante participó en la apertura y la revisión del mismo flujo para asegurar continuidad en la entrega.

## 10. Revisión cruzada y mejora

Durante la revisión cruzada, el revisor observó que la relación entre Pedido y Producto necesitaba un detalle más claro para evitar ambigüedad en la cantidad y el subtotal. Se corrigió la clase DetallePedido, se ajustó la multiplicidad y se justificó la decisión de mantener atributos de tipo decimal para el cálculo financiero. También se revisó la visibilidad de los atributos y se explicó por qué los datos críticos se mantienen privados para evitar consistencias erróneas en la operación.

---

> Documento validado como entrega de la práctica 9 del módulo de fundamentos de ingeniería de software.
