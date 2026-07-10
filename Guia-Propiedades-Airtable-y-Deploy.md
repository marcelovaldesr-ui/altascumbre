# Guía — Administrar propiedades (Airtable) y publicar la web (Vercel)

Esta guía deja la sección de **Propiedades** funcionando para que el estudio la administre **solo**, sin tocar código, y explica cómo **publicar la web**.

> Las pantallas de Airtable y Vercel pueden cambiar con el tiempo. Si algo se ve distinto, guíate por los nombres de los botones; el flujo es el mismo.

---

## PARTE 1 — Airtable (para que el estudio suba las propiedades)

La idea: el estudio carga sus propiedades y fotos en Airtable, y la web las muestra sola.

### Paso 1 · Crear la base
1. Entra a **airtable.com** y crea una cuenta con el **correo del estudio** (gestionesjuridicas.ayp@gmail.com).
2. Crea una **base** nueva y ponle, por ejemplo, "Altas Cumbres Propiedades".
3. Dentro, deja una **tabla** llamada exactamente **`Propiedades`**.

### Paso 2 · Crear los campos (nombres EXACTOS)
En la tabla `Propiedades`, crea estas columnas con **estos nombres tal cual** (respetando mayúsculas y **sin tildes** donde se indica). Lo más seguro es copiar y pegar cada nombre.

| Nombre del campo | Tipo en Airtable | Ejemplo |
|---|---|---|
| `Titulo` | Texto corto (Single line text) | Casa familiar con patio |
| `Operacion` | Selección única (Single select) → opciones: **Venta**, **Arriendo** | Venta |
| `Precio` | Texto corto | UF 4.500  /  $450.000 al mes |
| `Tipo` | Texto corto | Casa, Departamento, Terreno, Oficina, Parcela |
| `Comuna` | Texto corto | Temuco |
| `Dormitorios` | Número (Number, entero) | 3 |
| `Banos` | Número (entero) — **sin tilde** | 2 |
| `Superficie` | Texto corto | 120 m² |
| `Descripcion` | Texto largo (Long text) — **sin tilde** | Descripción de la propiedad |
| `Fotos` | **Adjunto (Attachment)** | arrastrar las fotos aquí |
| `Publicada` | **Casilla (Checkbox)** | ✔ marcada = aparece en la web |

### Paso 3 · Obtener el ID de la base
- Abre tu base en el navegador. La dirección se ve así: `airtable.com/appXXXXXXXXXXXXXX/...`
- Ese **`appXXXXXXXXXXXXXX`** es el **baseId**. Cópialo.

### Paso 4 · Crear el token (llave de solo lectura)
1. Entra a **airtable.com/create/tokens** (Personal access tokens).
2. **Create token** → nombre: "Web propiedades".
3. En **Scopes** agrega: **`data.records:read`** (solo lectura).
4. En **Access** agrega la base "Altas Cumbres Propiedades".
5. **Create token** y **copia** el token (empieza con `pat...`). ⚠️ Se muestra **una sola vez**, guárdalo.

> Crea el token en la **cuenta del estudio**. Si algún día se borra, la web deja de mostrar propiedades hasta pegar uno nuevo.

### Paso 5 · Conectar la web (una sola vez)
1. Abre el archivo **`propiedades.html`**.
2. Cerca del inicio del `<script>` busca el bloque `airtable:` y complétalo:
   ```js
   airtable: {
     token: "pat_XXXXXXXX...",      // el token del Paso 4
     baseId: "appXXXXXXXXXXXXXX",   // el ID del Paso 3
     tabla: "Propiedades"
   }
   ```
3. Guarda. Listo: la web mostrará las propiedades reales en vez de los ejemplos.

### Día a día · Cómo agregar o editar una propiedad (esto lo hace el estudio)
- Entra a Airtable → base `Propiedades` → **agrega una fila**.
- Llena los campos y **arrastra las fotos** al campo `Fotos`.
- Marca la casilla **`Publicada`**.
- Recarga la web: la propiedad aparece sola.
- Para **ocultar** una propiedad: desmarca `Publicada`. Para **borrarla**: elimina la fila.

---

## PARTE 2 — Publicar la web en Vercel

**Recomendación:** hazlo con una cuenta a nombre del **estudio**, para que la web sea 100% de ellos.

### Opción A · Rápida (arrastrar la carpeta)
1. Entra a **vercel.com** y crea una cuenta (correo del estudio).
2. Ve a **vercel.com/new**.
3. Busca la opción para **subir/arrastrar** una carpeta (Deploy).
4. Arrastra la carpeta **`ayp-asociados`** completa (con `index.html`, `propiedades.html` y las imágenes).
5. En **Framework Preset** elige **Other** (es un sitio estático). Presiona **Deploy**.
6. Vercel te da una dirección tipo `https://ayp-asociados.vercel.app` → esa es la web en línea.

### Opción B · Recomendada a futuro (con GitHub, se actualiza sola)
1. Crea una cuenta en **github.com** (del estudio) y un **repositorio** nuevo; sube ahí los archivos.
2. En Vercel: **Add New… → Project → Import** ese repositorio.
3. Framework **Other → Deploy**.
4. Cada vez que cambien un archivo en GitHub, la web se actualiza sola.

### Conectar el dominio propio (cuando lo tengan)
- En Vercel: **Project → Settings → Domains → Add** y escribe el dominio (ej: `aypasociados.cl`).
- Sigue las instrucciones de **DNS** que entrega Vercel (se configuran donde compraron el dominio).

---

## Checklist de traspaso (para dejar todo en manos del estudio)

- [ ] Cuenta de **Airtable** a nombre del estudio.
- [ ] Cuenta de **Vercel** a nombre del estudio.
- [ ] **Dominio** a nombre del estudio.
- [ ] **Token** de Airtable creado en la cuenta del estudio (no borrarlo).
- [ ] **WhatsApp** y **teléfono** reales cargados en el código (`CONFIG`, en `index.html` y `propiedades.html`).
- [ ] Entregar todos los **accesos** (usuarios/contraseñas) al estudio.

## Notas de seguridad
- El token de Airtable es de **solo lectura**: aunque queda en el código de la web, únicamente permite **leer** las propiedades (que de todos modos son públicas). No permite modificar nada ni acceder a otras bases.
- Mientras no configures Airtable, la página muestra **3 propiedades de ejemplo**. Al pegar los datos del Paso 5, pasan a mostrarse las reales.
