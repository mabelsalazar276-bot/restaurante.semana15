# New Beans Restaurant

**Autora:** Mabela del Cisne Salazar Ren
**Entrega:** Semana 15

## Descripción

New Beans Restaurant es una aplicación de escritorio hecha en Python con Tkinter/ttk. Esta versión continúa el trabajo de semanas anteriores: conserva el inicio de sesión y las secciones de usuarios y productos, e integra el registro y la consulta de ventas.

El código mantiene responsabilidades separadas entre archivos JSON, modelos, servicios, interfaz y punto de entrada. La interfaz recibe las acciones; `RestauranteServicio` aplica las reglas y `ArchivoServicio` lee o guarda los datos.

## Estructura

```text
restaurante_app/
├── .gitignore
├── main.py
├── README.md
├── datos/
│   ├── productos.json
│   ├── usuarios.json
│   └── ventas.json
├── assets/
│   ├── icono_inicio.xbm
│   ├── icono_productos.xbm
│   ├── icono_usuarios.xbm
│   └── icono_ventas.xbm
├── modelos/
│   ├── __init__.py
│   ├── producto.py
│   ├── usuario.py
│   └── venta.py
├── servicios/
│   ├── __init__.py
│   ├── archivo_servicio.py
│   ├── restaurante.py
│   └── restaurante_servicio.py
└── ui/
    ├── __init__.py
    ├── estilos.py
    ├── login_view.py
    ├── main_view.py
```

## Funcionamiento por componentes

- **Modelos:** describen productos, usuarios y ventas con sus referencias y fecha.
- **Servicios:** comprueban las operaciones permitidas y guardan la información en los JSON correspondientes.
- **Interfaz:** reúne la navegación, formularios, selectores y tablas, con una paleta lila y blanca. Los iconos de navegación permanecen en `assets/`; el logotipo original se conserva como archivo, pero no se muestra.
- **Inicio:** resume cuántos usuarios, platos y ventas hay registrados.
- **Ejecución:** `main.py` crea la ventana, carga los datos y presenta el login.

## Archivos de datos

La información utilizada por el programa se encuentra en la carpeta `datos/`:

- `productos.json`: catálogo disponible.
- `usuarios.json`: usuarios habilitados para ingresar.
- `ventas.json`: ventas registradas con identificadores de usuario y producto, y fecha/hora ISO.

## Ventas y flujo de eventos

En **Ventas** se elige un usuario y un plato en los selectores y se pulsa **Registrar venta**. El botón usa `command=` para llamar al callback de la vista; este recoge las opciones y solicita la operación a `RestauranteServicio`. El servicio valida ambas referencias y usa `ArchivoServicio` para guardar en `ventas.json`. Si se registra correctamente, la tabla se actualiza y la interfaz confirma el resultado. Al abrir el programa, se recupera el historial guardado.

Secuencia del evento: acción del usuario → botón `command=` → callback → servicio → persistencia JSON → tabla y mensaje de respuesta.

## Operaciones de productos

La sección **Productos** contiene el menú de platos de muestra: encebollado, ceviche de camarón, llapingachos, seco de pollo, jugo de naranjilla y tres leches. El formulario permite registrar, consultar, actualizar o eliminar sus datos, incluidos precio y existencias.

- **Registrar:** crea un producto nuevo.
- **Cargar / Consultar:** busca un producto mediante su ID y muestra sus datos en el formulario.
- **Actualizar:** reemplaza la información del producto seleccionado por su ID.
- **Eliminar:** retira el producto del catálogo.

Las acciones del menú se procesan mediante `RestauranteServicio`; la vista no modifica directamente `productos.json`. Después de cada operación, vuelve a consultar los datos y refresca la tabla. Para eliminar un plato solicita confirmación.

## Registro de usuarios

La tabla de **Usuarios** permite consultar las cuentas. El archivo inicial contiene una cuenta administradora y una cuenta de mesero; el formulario mantiene la opción de registrar más usuarios con uno de esos dos roles. El servicio asigna el ID y evita repetir el nombre de usuario o el correo.

## Cómo iniciar la aplicación

Desde una terminal situada en la carpeta del proyecto, inicia la aplicación:

```bash
python main.py
```

## Credenciales de demostración

```text
Usuario: admin
Contraseña: admin123

Usuario: mesero
Contraseña: mesero2026
```

Las contraseñas se almacenan como texto simple en el JSON; esta es una aplicación docente, no un sistema de autenticación para producción.

## Verificación de sintaxis

Para revisar que los archivos Python sean válidos, utiliza:

```bash
python -m compileall -q main.py modelos servicios ui
```

## Comprobación de la Semana 15

1. Iniciar `main.py` e ingresar como administrador o mesero.
2. Revisar las secciones de usuarios y menú; probar las acciones CRUD de platos.
3. Abrir **Ventas**, escoger una cuenta y un plato y registrar la operación.
4. Confirmar que la venta aparece en pantalla y queda almacenada en `datos/ventas.json`.
5. Cerrar y volver a iniciar la aplicación para comprobar que se conserva el historial.