-- Crear la base de datos
CREATE DATABASE tienda_tecnologica;
USE tienda_tecnologica;

-- Tabla de Proveedores
CREATE TABLE proveedores (
    id_proveedor INT AUTO_INCREMENT PRIMARY KEY,
    nombre_empresa VARCHAR(100) NOT NULL,
    nombre_contacto VARCHAR(50) NOT NULL,
    telefono VARCHAR(20),
    email VARCHAR(100),
    direccion VARCHAR(255),
    ciudad VARCHAR(50),
    pais VARCHAR(50),
    fecha_registro DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de Vendedores
CREATE TABLE vendedores (
    id_vendedor INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    apellido VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    telefono VARCHAR(20),
    direccion VARCHAR(255),
    fecha_ingreso DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de Categorías
CREATE TABLE categorias (
    id_categoria INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    descripcion TEXT,
    fecha_creacion DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de Productos
CREATE TABLE productos (
    id_producto INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    descripcion TEXT,
    precio DECIMAL(10, 2) NOT NULL,
    stock INT DEFAULT 0,
    categoria VARCHAR(50),
    imagen VARCHAR(255),
    fecha_agregado DATETIME DEFAULT CURRENT_TIMESTAMP,
    id_vendedor INT,
    id_proveedor INT,
    id_categoria INT,  -- Nueva referencia a categorías
    FOREIGN KEY (id_vendedor) REFERENCES vendedores(id_vendedor),
    FOREIGN KEY (id_proveedor) REFERENCES proveedores(id_proveedor),
    FOREIGN KEY (id_categoria) REFERENCES categorias(id_categoria)  -- Nueva relación
);

-- Tabla de Detalles del Producto
CREATE TABLE detalles_producto (
    id_detalle INT AUTO_INCREMENT PRIMARY KEY,
    id_producto INT,
    caracteristica VARCHAR(100),
    valor VARCHAR(255),
    FOREIGN KEY (id_producto) REFERENCES productos(id_producto)
);

-- Tabla de Garantías
CREATE TABLE garantias (
    id_garantia INT AUTO_INCREMENT PRIMARY KEY,
    id_producto INT,
    id_proveedor INT,
    fecha_inicio DATE NOT NULL,
    fecha_fin DATE NOT NULL,
    duracion_meses INT NOT NULL,
    proveedor_garantia VARCHAR(100),
    condiciones TEXT,
    FOREIGN KEY (id_producto) REFERENCES productos(id_producto),
    FOREIGN KEY (id_proveedor) REFERENCES proveedores(id_proveedor)
);

-- Tabla de Clientes
CREATE TABLE clientes (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    apellido VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    telefono VARCHAR(20),
    direccion VARCHAR(255)
);

-- Tabla de Métodos de Entrega
CREATE TABLE metodos_entrega (
    id_metodo INT AUTO_INCREMENT PRIMARY KEY,
    descripcion VARCHAR(100) NOT NULL,
    costo DECIMAL(10, 2) NOT NULL,
    tiempo_estimado VARCHAR(50) NOT NULL
);

-- Tabla de Órdenes
CREATE TABLE ordenes (
    id_orden INT AUTO_INCREMENT PRIMARY KEY,
    id_cliente INT,
    id_metodo INT,
    fecha_orden DATETIME DEFAULT CURRENT_TIMESTAMP,
    total DECIMAL(10, 2) NOT NULL,
    estado VARCHAR(20) DEFAULT 'pendiente',
    FOREIGN KEY (id_cliente) REFERENCES clientes(id_cliente),
    FOREIGN KEY (id_metodo) REFERENCES metodos_entrega(id_metodo)
);

-- Tabla de Detalle de Órdenes
CREATE TABLE detalle_orden (
    id_detalle INT AUTO_INCREMENT PRIMARY KEY,
    id_orden INT,
    id_producto INT,
    cantidad INT NOT NULL,
    precio_unitario DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (id_orden) REFERENCES ordenes(id_orden),
    FOREIGN KEY (id_producto) REFERENCES productos(id_producto)
);

-- Tabla de Pagos
CREATE TABLE pagos (
    id_pago INT AUTO_INCREMENT PRIMARY KEY,
    id_orden INT,
    metodo_pago VARCHAR(50),
    fecha_pago DATETIME DEFAULT CURRENT_TIMESTAMP,
    monto DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (id_orden) REFERENCES ordenes(id_orden)
);

-- Tabla de Órdenes a Proveedores (opcional)
CREATE TABLE ordenes_proveedor (
    id_orden_proveedor INT AUTO_INCREMENT PRIMARY KEY,
    id_proveedor INT,
    fecha_orden DATETIME DEFAULT CURRENT_TIMESTAMP,
    total DECIMAL(10, 2) NOT NULL,
    estado VARCHAR(20) DEFAULT 'pendiente',
    FOREIGN KEY (id_proveedor) REFERENCES proveedores(id_proveedor)
);

-- Tabla de Ventas
CREATE TABLE ventas (
    id_venta INT AUTO_INCREMENT PRIMARY KEY,
    id_vendedor INT,
    id_producto INT,
    cantidad INT NOT NULL,
    fecha_venta DATETIME DEFAULT CURRENT_TIMESTAMP,
    total DECIMAL(10, 2) NOT NULL,
    origen_encargo ENUM('telefono', 'instagram', 'otro') NOT NULL DEFAULT 'otro', -- Nuevo campo
    FOREIGN KEY (id_vendedor) REFERENCES vendedores(id_vendedor),
    FOREIGN KEY (id_producto) REFERENCES productos(id_producto)
);

-- Tabla de Tiendas
CREATE TABLE tiendas (
    id_tienda INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    direccion VARCHAR(255),
    telefono VARCHAR(20),
    fecha_creacion DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Tabla de Tiendas Online
CREATE TABLE tienda_online (
    id_tienda_online INT AUTO_INCREMENT PRIMARY KEY,
    id_tienda INT,
    nombre VARCHAR(100) NOT NULL,
    url VARCHAR(255),
    FOREIGN KEY (id_tienda) REFERENCES tiendas(id_tienda)
);

-- Tabla de Redes Sociales
CREATE TABLE redes_sociales (
    id_red_social INT AUTO_INCREMENT PRIMARY KEY,
    id_tienda_online INT,
    plataforma ENUM('instagram', 'facebook'),
    url VARCHAR(255),
    FOREIGN KEY (id_tienda_online) REFERENCES tienda_online(id_tienda_online)
);

