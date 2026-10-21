# Sistema de Gestión de Productos — Tienda de guitarras eléctricas

Aplicación de consola en Java con JDBC y base de datos emulada H2 (embebida en archivo).

## Requisitos

- JDK 17 o superior
- VS Code con "Extension Pack for Java"
- Driver H2: descargar `h2-2.2.224.jar` desde
  https://repo1.maven.org/maven2/com/h2database/h2/2.2.224/h2-2.2.224.jar
  y copiarlo en la carpeta `lib/`

## Estructura

```
src/
  modelo/        Producto.java               (entidad + validaciones)
  datos/         ConexionBD.java             (conexión y creación de tabla)
                 ProductoDAO.java            (interfaz CRUD)
                 ProductoDAOImpl.java        (JDBC con PreparedStatement)
                 AccesoDatosException.java   (excepción de la capa de datos)
  presentacion/  MenuConsola.java            (interfaz de consola)
                 Main.java                   (punto de entrada)
lib/             h2-2.2.224.jar
data/            tienda_guitarras.mv.db      (se crea al primer arranque)
```

## Ejecución

- VS Code: abrir la carpeta, Run and Debug > "Sistema de Gestión de Productos".
- Windows: `ejecutar.bat`
- Linux/macOS: `sh ejecutar.sh`

## Tabla

```sql
CREATE TABLE IF NOT EXISTS productos (
  codigo      VARCHAR(20)   PRIMARY KEY,
  nombre      VARCHAR(100)  NOT NULL,
  descripcion VARCHAR(255),
  precio      DECIMAL(12,2) NOT NULL CHECK (precio >= 0),
  cantidad    INT           NOT NULL CHECK (cantidad >= 0)
);
```

## Cumplimiento de requisitos

| Requisito | Implementación |
|---|---|
| RF1 Registrar | `MenuConsola.registrar()` → `ProductoDAOImpl.insertar()` |
| RF2 Consultar | `listarTodos()`, `buscarPorCodigo()`, `buscarPorNombre()` (LIKE, sin distinguir mayúsculas) |
| RF3 Actualizar | `actualizar()`: precio, descripción o cantidad; Enter conserva el valor |
| RF4 Eliminar | `eliminar()` con confirmación S/N |
| RF5 Persistencia | H2 en archivo `./data/tienda_guitarras.mv.db` |
| PreparedStatement | Todas las consultas, incluidas las de lectura |
| Validaciones | Precio ≥ 0 y cantidad entera ≥ 0 en consola, en `Producto` y en `CHECK` de la tabla |
| Errores | `SQLException` envuelta en `AccesoDatosException`; código duplicado detectado por SQLState 23505 |
| Cierre de recursos | try-with-resources en `Connection`, `PreparedStatement`, `ResultSet` y `Scanner` |

## Git

```
git init
git add .
git commit -m "Sistema de Gestión de Productos - CRUD con JDBC y H2"
git branch -M main
git remote add origin https://github.com/<usuario>/gestion-guitarras.git
git push -u origin main
```
