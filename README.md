# sistemas-de-ventas
CREATE TABLE [usuarios] (
  [id] int PRIMARY KEY IDENTITY(1, 1),
  [nombre] nvarchar(255) NOT NULL,
  [correo] nvarchar(255) UNIQUE NOT NULL,
  [contrasena] nvarchar(255) NOT NULL,
  [telefono] nvarchar(255),
  [direccion] text,
  [tipo_usuario] enum(comprador,vendedor) DEFAULT 'comprador',
  [created_at] timestamp DEFAULT (CURRENT_TIMESTAMP)
)
GO

CREATE TABLE [productos] (
  [id] int PRIMARY KEY IDENTITY(1, 1),
  [titulo] nvarchar(255) NOT NULL,
  [descripcion] text,
  [precio] decimal NOT NULL,
  [stock] int NOT NULL DEFAULT (0),
  [categoria_id] int NOT NULL,
  [vendedor_id] int NOT NULL,
  [created_at] timestamp DEFAULT (CURRENT_TIMESTAMP)
)
GO

CREATE TABLE [categorias] (
  [id] int PRIMARY KEY IDENTITY(1, 1),
  [nombre] nvarchar(255) UNIQUE NOT NULL,
  [descripcion] text
)
GO

CREATE TABLE [carrito] (
  [id] int PRIMARY KEY IDENTITY(1, 1),
  [usuario_id] int NOT NULL,
  [producto_id] int NOT NULL,
  [cantidad] int NOT NULL DEFAULT (1),
  [added_at] timestamp DEFAULT (CURRENT_TIMESTAMP)
)
GO

CREATE TABLE [ordenes] (
  [id] int PRIMARY KEY IDENTITY(1, 1),
  [usuario_id] int NOT NULL,
  [total] decimal NOT NULL,
  [estado] enum(pendiente,pagado,cancelado) DEFAULT 'pendiente',
  [created_at] timestamp DEFAULT (CURRENT_TIMESTAMP)
)
GO

CREATE TABLE [detalle_orden] (
  [id] int PRIMARY KEY IDENTITY(1, 1),
  [orden_id] int NOT NULL,
  [producto_id] int NOT NULL,
  [cantidad] int NOT NULL,
  [precio_unitario] decimal NOT NULL
)
GO

CREATE TABLE [pagos] (
  [id] int PRIMARY KEY IDENTITY(1, 1),
  [orden_id] int NOT NULL,
  [metodo_pago] enum(tarjeta,paypal,transferencia) DEFAULT 'tarjeta',
  [monto] decimal NOT NULL,
  [estado] enum(exitoso,fallido) DEFAULT 'exitoso',
  [realizado_at] timestamp DEFAULT (CURRENT_TIMESTAMP)
)
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Nombre completo del usuario',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'usuarios',
@level2type = N'Column', @level2name = 'nombre';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Correo electrónico único',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'usuarios',
@level2type = N'Column', @level2name = 'correo';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Contraseña encriptada',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'usuarios',
@level2type = N'Column', @level2name = 'contrasena';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Teléfono de contacto',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'usuarios',
@level2type = N'Column', @level2name = 'telefono';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Dirección de envío',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'usuarios',
@level2type = N'Column', @level2name = 'direccion';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Determina si es comprador o vendedor',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'usuarios',
@level2type = N'Column', @level2name = 'tipo_usuario';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Fecha de registro del usuario',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'usuarios',
@level2type = N'Column', @level2name = 'created_at';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Título descriptivo del producto',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'productos',
@level2type = N'Column', @level2name = 'titulo';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Descripción completa del producto',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'productos',
@level2type = N'Column', @level2name = 'descripcion';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Precio unitario',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'productos',
@level2type = N'Column', @level2name = 'precio';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Cantidad disponible en inventario',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'productos',
@level2type = N'Column', @level2name = 'stock';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Referencia a la categoría',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'productos',
@level2type = N'Column', @level2name = 'categoria_id';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Referencia al vendedor',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'productos',
@level2type = N'Column', @level2name = 'vendedor_id';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Fecha de publicación',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'productos',
@level2type = N'Column', @level2name = 'created_at';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Nombre único de la categoría',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'categorias',
@level2type = N'Column', @level2name = 'nombre';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Descripción de la categoría',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'categorias',
@level2type = N'Column', @level2name = 'descripcion';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Referencia al usuario',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'carrito',
@level2type = N'Column', @level2name = 'usuario_id';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Referencia al producto',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'carrito',
@level2type = N'Column', @level2name = 'producto_id';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Cantidad seleccionada del producto',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'carrito',
@level2type = N'Column', @level2name = 'cantidad';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Fecha en que se agregó al carrito',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'carrito',
@level2type = N'Column', @level2name = 'added_at';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Usuario que realiza la orden',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'ordenes',
@level2type = N'Column', @level2name = 'usuario_id';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Monto total a pagar',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'ordenes',
@level2type = N'Column', @level2name = 'total';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Estado de la orden',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'ordenes',
@level2type = N'Column', @level2name = 'estado';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Fecha de creación de la orden',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'ordenes',
@level2type = N'Column', @level2name = 'created_at';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Referencia a la orden',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'detalle_orden',
@level2type = N'Column', @level2name = 'orden_id';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Referencia al producto',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'detalle_orden',
@level2type = N'Column', @level2name = 'producto_id';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Cantidad comprada',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'detalle_orden',
@level2type = N'Column', @level2name = 'cantidad';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Precio del producto al momento de la compra',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'detalle_orden',
@level2type = N'Column', @level2name = 'precio_unitario';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Referencia a la orden',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'pagos',
@level2type = N'Column', @level2name = 'orden_id';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Método de pago utilizado',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'pagos',
@level2type = N'Column', @level2name = 'metodo_pago';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Monto pagado',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'pagos',
@level2type = N'Column', @level2name = 'monto';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Estado del pago',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'pagos',
@level2type = N'Column', @level2name = 'estado';
GO

EXEC sp_addextendedproperty
@name = N'Column_Description',
@value = 'Fecha del pago',
@level0type = N'Schema', @level0name = 'dbo',
@level1type = N'Table',  @level1name = 'pagos',
@level2type = N'Column', @level2name = 'realizado_at';
GO

ALTER TABLE [productos] ADD FOREIGN KEY ([categoria_id]) REFERENCES [categorias] ([id])
GO

ALTER TABLE [productos] ADD FOREIGN KEY ([vendedor_id]) REFERENCES [usuarios] ([id])
GO

ALTER TABLE [carrito] ADD FOREIGN KEY ([usuario_id]) REFERENCES [usuarios] ([id])
GO

ALTER TABLE [carrito] ADD FOREIGN KEY ([producto_id]) REFERENCES [productos] ([id])
GO

ALTER TABLE [ordenes] ADD FOREIGN KEY ([usuario_id]) REFERENCES [usuarios] ([id])
GO

ALTER TABLE [detalle_orden] ADD FOREIGN KEY ([orden_id]) REFERENCES [ordenes] ([id])
GO

ALTER TABLE [detalle_orden] ADD FOREIGN KEY ([producto_id]) REFERENCES [productos] ([id])
GO

ALTER TABLE [pagos] ADD FOREIGN KEY ([orden_id]) REFERENCES [ordenes] ([id])
GO
