# Presupuestos Eléctricos

Aplicación web para armar presupuestos de instalaciones eléctricas: cargar precios, crear
proyectos por obra, calcular cuántas térmicas y diferenciales hacen falta, guardar el plano
de la obra, y generar los PDF para el cliente y para la compra de materiales.

No necesita instalación ni backend propio: es un único archivo HTML que corre en el
navegador. Los datos se guardan solos, y opcionalmente se pueden sincronizar entre dos
personas usando un repositorio de GitHub como almacenamiento compartido.

🔗 **URL de la app:** `https://myunis8.github.io/ElectricBudget/presupuestos-app.html`

---

## 1. Proyectos

Es la pantalla principal. Ahí ves todos los presupuestos que armaste, cada uno como una
tarjeta con cliente, fecha y total.

- **+ Nuevo proyecto**: crea un presupuesto vacío.
- **Abrir**: retoma un proyecto ya cargado para seguir editándolo.
- **Eliminar**: lo borra (no se puede deshacer).

Dentro de cada proyecto vas a encontrar, de arriba hacia abajo:

### Datos generales
Nombre del proyecto, cliente, dirección de la obra, fecha, y si la instalación es
monofásica o trifásica.

### Cantidades del presupuesto
Los ítems que se le cobran al cliente: puntos, tomas, iluminación, tableros, puesta a
tierra y trabajos adicionales (como la conexión al medidor, que se cotiza aparte). Cargás
la cantidad de cada uno y abajo se arma el total solo.

Si cambiaste un precio en la lista general después de haber creado el proyecto, tocá
**"Actualizar precios desde la lista"** para traer los precios nuevos a ese presupuesto.

### Plano de obra
Subís una imagen o un PDF (hasta 3 MB) del plano y lo ves ahí mismo, dentro de la app,
sin tener que abrir otro programa. Se guarda junto con el proyecto. Podés reemplazarlo,
descargarlo o quitarlo cuando quieras.

### Circuitos y tableros
Esto **no suma al total** del presupuesto — es solo para saber cuánto material comprar
(el costo de térmicas y diferenciales ya está incluido en el precio del tablero). Cargás:

- Cantidad de tableros del proyecto.
- Circuitos de iluminación y de tomas generales (cantidad + térmica).
- Tomas especiales, una por una (por ejemplo "Aire acondicionado", cantidad, térmica) —
  así podés poner una térmica más grande si hay más de un equipo en el mismo circuito.
- Diferenciales (nombre/ubicación, tipo, amperaje, cantidad).

Abajo se arma solo un resumen: cuántas térmicas de cada amperaje y cuántos diferenciales
de cada tipo necesitás en total.

### Generar los PDF
Hay dos botones separados:

- **Generar PDF presupuesto**: el documento para el cliente, con los ítems cotizados y el
  total (sin la parte de circuitos/materiales).
- **Generar PDF materiales**: la lista de térmicas y diferenciales a comprar, sin precios
  — para llevar a la casa de electricidad.

Si el PDF no se descarga solo, va a aparecer un botón "Descargar" al lado para bajarlo a mano.

---

## 2. Precios

La lista de precios unitarios que se usa en todos los proyectos nuevos.

- Tocá el precio de cualquier ítem para editarlo — se guarda solo al salir del campo.
- **+ Agregar** un ítem nuevo (elegí una categoría existente o escribí una nueva).
- Marcá **"Trabajo aparte"** si ese ítem se cotiza por fuera de la instalación (como la
  conexión al medidor).
- El botón **"Actualizar categorías"** solo hace falta si en algún momento la app agrega
  categorías nuevas de fábrica; se ofrece solo cuando corresponde.

---

## 3. Exportar / Importar (respaldo manual)

Arriba a la derecha:

- **⇩ Exportar todo**: descarga un `.json` con toda tu lista de precios y proyectos. Sirve
  como respaldo, o para pasarle tus datos a otra persona a mano (por mail, WhatsApp, Drive, etc.).
- **⇧ Importar**: carga un `.json` exportado. Agrega los proyectos que no tengas y actualiza
  los que ya tengas solo si la versión importada es más nueva — no te pisa cambios recientes
  por accidente.

---

## 4. Sincronización entre dos personas (⚙ Sincronización)

Para que dos personas usen la misma información sin perder tiempo mandándose archivos, la
app se puede conectar a un repositorio privado de GitHub que actúa como almacenamiento
compartido.

### Configuración (una sola vez)

1. **Crear el repositorio** (lo hace uno de los dos): en github.com → botón verde "New" →
   nombre, por ejemplo `presupuestos-datos` → marcar **Private** → "Create repository".
2. **Invitar a la otra persona**: dentro del repositorio → Settings → Collaborators →
   "Add people" → buscarla por su usuario de GitHub. Tiene que aceptar la invitación.
3. **Cada uno genera su propio token** (esto sí lo hace cada persona en su cuenta):
   github.com → foto de perfil → Settings → Developer settings → Personal access tokens →
   Fine-grained tokens → "Generate new token" → elegir el repositorio `presupuestos-datos` →
   en "Repository permissions" poner **Contents: Read and write** → Generate → copiar el token.
4. **En la app**, tocar **⚙ Sincronización** y completar: usuario/organización dueña del
   repositorio, nombre del repositorio, y el propio token → "Guardar y sincronizar".

Estos mismos pasos están también dentro de la app, tocando "¿Cómo consigo el usuario,
repositorio y token?" en esa misma ventana.

### Cómo funciona una vez configurada

- Cada vez que guardás un proyecto o cambiás un precio, se sube solo en segundo plano.
- Cada 45 segundos (y al abrir la app) revisa si hay cambios nuevos de la otra persona y los
  trae solo.
- Arriba a la izquierda hay un indicador de estado: 🟢 Sincronizado / 🟡 Sincronizando /
  🔴 Sin conexión / ⚪ No configurada.
- El botón **"Sincronizar ahora"** fuerza una sincronización inmediata.
- **"Desconectar"** saca la configuración de sincronización de ese navegador (no borra los
  datos ya guardados localmente).

### Limitaciones a tener en cuenta

- **No es tiempo real estricto**: si los dos guardan el *mismo* proyecto casi al mismo
  tiempo, gana el que se sube después. Trabajando cada uno en proyectos distintos, esto no
  suele ser un problema.
- **Los proyectos eliminados no se sincronizan como borrado**: si una persona elimina un
  proyecto que la otra todavía no bajó, puede reaparecer al sincronizar. No hay "papelera
  compartida" en esta versión.
- El token es personal e intransferible: no hay que compartirlo entre los dos, cada uno
  genera el propio.

---

## 5. Preguntas frecuentes

**¿Dónde se guardan mis datos si no configuro la sincronización?**
En el almacenamiento del navegador donde abrís la app. Si la abrís desde otra computadora
o con otro navegador, vas a ver una lista vacía a menos que hayas exportado/importado o
configurado la sincronización.

**¿Puedo usarla sin internet?**
Podés cargar datos sin conexión, pero no se va a sincronizar ni vas a poder generar el PDF
hasta que vuelva la conexión (la app necesita internet para las librerías de PDF y, si está
configurada, para sincronizar).

**¿Sirve para más de dos personas?**
Sí, el mismo repositorio puede tener más colaboradores, cada uno con su propio token.
