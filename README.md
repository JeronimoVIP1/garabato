# 🎨 Garabato — el teléfono roto, versión dibujada

Recreación jugable del clásico **teléfono roto dibujado** (estilo Gartic Phone): todos escriben una frase, las frases se convierten en dibujos, los dibujos en interpretaciones… y al final se revela el desastre completo.

Está hecho con **un único `index.html`** (HTML + CSS + JavaScript) y **multijugador P2P real** mediante WebRTC/PeerJS. No necesita servidor propio, base de datos ni variables de entorno.

---

## ✨ Qué incluye

- Salas privadas con código de 5 caracteres y enlace de invitación.
- Multijugador online de **2 a 8 jugadores** en tiempo real.
- Lienzo de dibujo con pincel, borrador, relleno, 8 colores, 3 grosores, deshacer y borrar todo.
- Cronómetro por turno y avance automático si alguien no envía su parte.
- Cadenas de frases/dibujos que rotan entre todos los jugadores.
- Revelación de resultados álbum por álbum, paso a paso.
- **Modo práctica con bots**: ideal para probar el juego sin esperar a nadie.
- Reacciones flotantes, sonidos, confeti y avatares automáticos.
- Sin anuncios, sin registro y sin instalar nada.

---

## 📁 Archivos del proyecto

| Archivo | Para qué sirve |
|---|---|
| `index.html` | **Todo el juego.** Es el único archivo imprescindible para jugar. |
| `README.md` | Esta guía de uso y despliegue. |
| `.nojekyll` | Evita que GitHub Pages procese el sitio con Jekyll. No se ve en la web, pero conviene subirlo. |

No hace falta ningún `package.json`, servidor Node ni proceso de compilación.

---

## 🚀 Publicarlo gratis en GitHub Pages

### Opción A — Desde la web de GitHub (la más fácil)

1. Entra en [github.com](https://github.com) y crea un repositorio nuevo, por ejemplo **`garabato`**.
   - Puede ser **público** (recomendado) o privado.
   - No marques ninguna opción para añadir README, licencia o `.gitignore` si vas a subir estos archivos.
2. Sube los archivos `index.html`, `README.md` y `.nojekyll` a la **raíz** del repositorio.
   - En GitHub: **Add file → Upload files**.
   - Arrastra los tres archivos y pulsa **Commit changes**.
3. Ve a **Settings** (Ajustes) → **Pages** en el menú lateral.
4. En **Build and deployment** → **Source**, elige **Deploy from a branch**.
5. En **Branch**, selecciona **`main`** y la carpeta **`/ (root)`**. Pulsa **Save**.
6. Espera entre 20 y 60 segundos (verás un aviso verde con la URL).
7. Tu juego estará en:

   ```text
   https://TU-USUARIO.github.io/garabato/
   ```

   Comparte esa URL. Cualquier persona con el enlace puede crear una sala o unirse con el código.

> Si el repositorio se llama `TU-USUARIO.github.io`, la URL será directamente `https://TU-USUARIO.github.io/`.

### Opción B — Desde la terminal con Git

```bash
# En la carpeta que contiene index.html, README.md y .nojekyll
git init
git add .
git commit -m "Garabato: juego multijugador listo"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/garabato.git
git push -u origin main
```

Después activa **Settings → Pages → Deploy from a branch → main → / (root)** como en la opción A.

### Actualizar el juego más adelante

Edita `index.html`, sube los cambios al repositorio y espera a que GitHub Pages se regenere. La URL no cambia.

---

## 🎮 Cómo se juega online

1. Abre la URL publicada.
2. Pulsa **Crear una sala**, escribe tu nombre y comparte el código o el enlace de invitación.
3. Los demás pulsan **Unirme con código** o abren el enlace `.../?sala=CODIGO`.
4. Cuando haya al menos 2 jugadores, el anfitrión pulsa **Empezar la partida**.
5. Cada ronda todos escriben, dibujan o interpretan lo que reciben. Los libros rotan de mano.
6. Al terminar se muestran todos los álbumes. El anfitrión puede pulsar **Jugar otra vez**.

### Probar sin amigos

- Desde la portada: **«¿Sin amigos a mano? Juega una práctica con bots»**.
- O dentro de una sala: **＋ Añadir bot** (máximo 8 participantes en total).

### Probar en local

Puedes abrir `index.html` directamente, pero para que funcionen portapapeles y conexiones conviene servir la carpeta:

```bash
python3 -m http.server 8080
# Abre http://localhost:8080
```

Para probar el online real, abre la misma URL en dos pestañas o en dos dispositivos. Si usas dos pestañas en `localhost`, una crea la sala y la otra se une con el código.

---

## ⚙️ Cómo funciona el multijugador

- **No hay backend.** El anfitrión actúa como servidor del juego: valida los turnos y reparte los libros.
- Los mensajes viajan **directamente entre navegadores** por WebRTC usando [PeerJS](https://peerjs.com/) para el emparejamiento inicial.
- El servicio público de PeerJS se usa solo para descubrir a los jugadores; las frases y dibujos no se guardan en ningún servidor.
- GitHub Pages sirve los archivos estáticos. Que la web sea **HTTPS** (como GitHub Pages) es importante para WebRTC y para el portapapeles.
- Los dibujos se envían como imágenes JPEG/WebP comprimidas para que quepan bien por el canal P2P.

### Requisitos y límites

- **2–8 jugadores** por sala (límite pensado para que el intercambio de dibujos siga siendo fluido).
- Cada participante debe poder conectarse al servicio público de PeerJS. En redes corporativas muy restrictivas, el P2P puede bloquearse.
- Si una red bloquea la conexión directa, se necesitaría un servidor **TURN** propio. Se puede añadir en `PEER_OPTIONS` / `iceServers` dentro de `index.html`.
- El anfitrión debe permanecer en la partida. Si cierra la pestaña, la sala se cierra para todos.
- Cambiar de pestaña o bloquear el dispositivo puede retrasar los temporizadores (los navegadores limitan los timers en segundo plano).

---

## 🔧 Personalización rápida

Todo está en `index.html`:

- **Nombre, colores y textos:** busca el bloque `:root` dentro de `<style>` y el texto de la portada.
- **Frases sugeridas:** constante `PROMPT_IDEAS`.
- **Respuestas de los bots:** constante `GUESS_FALLBACKS` y array `WORDS`.
- **Máximo de jugadores:** constante `MAX_PLAYERS` (por defecto `8`).
- **Prefijo de las salas:** constante `ROOM_PREFIX`. Si publicas dos copias del juego, usa prefijos distintos para no mezclar salas.
- **Servidores ICE/TURN:** constante `PEER_OPTIONS`.
- **Tiempo de turno:** opciones dentro de `[60, 90, 120]` en la sala de espera y en `e.setSettings`.

---

## 🆘 Problemas frecuentes

| Problema | Solución |
|---|---|
| «No encontramos ninguna sala con ese código» | Revisa que el anfitrión siga en la sala y que el código sea el correcto. Códigos como `0/O` o `1/I` no se usan. |
| No conecta entre dos redes distintas | Es un problema de NAT/WebRTC. Prueba en la misma red o añade un servidor TURN en `iceServers`. |
| El enlace no abre el juego | Comprueba que GitHub Pages esté en **Deploy from a branch → main → / (root)** y espera a que termine el despliegue. |
| La web se ve sin estilos | Verifica que `index.html` esté en la raíz del repositorio y que hayas esperado a que GitHub Pages regenere el sitio. |
| No funciona el botón Copiar | En algunos navegadores, el portapapeles requiere HTTPS. GitHub Pages ya lo proporciona; en local usa `localhost`. |
| Cambié el nombre del repo y no carga | La URL de Pages cambia al cambiar el nombre del repositorio; revisa **Settings → Pages**. |

---

## 🧩 Créditos y aviso

Proyecto independiente creado como recreación del juego de teléfono roto dibujado, inspirado en Gartic Phone. No está afiliado, patrocinado ni respaldado por Gartic Phone. Las marcas y nombres de terceros pertenecen a sus respectivos propietarios.

Hecho para reírse en buena compañía. 🎨📞
