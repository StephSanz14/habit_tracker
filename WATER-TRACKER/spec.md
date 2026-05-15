# spec.md — Water Habit Tracker

## Objetivo

Aplicación web para que usuarios registren hábitos relacionados con hidratación, realicen check-ins diarios y visualicen su progreso mediante rachas de constancia.

---

## Scope

### Qué SÍ entra en este proyecto

- Registro y autenticación de usuarios con email y contraseña.
- CRUD de hábitos de hidratación (nombre, descripción y frecuencia diaria o semanal).
- Check-in diario sobre hábitos de hidratación.
- Consulta de hábitos existentes del usuario autenticado.
- Vista de progreso con racha actual de cumplimiento.
- Registro del consumo objetivo asociado a cada hábito.
- Persistencia de usuarios, hábitos y check-ins en base de datos MongoDB.
- Restricción para evitar más de un check-in por hábito en la misma fecha.
- Visualización de hábitos completados y pendientes del día actual.

### Qué NO entra (no-goals)

- Ver ni interactuar con datos de otros usuarios.
- Notificaciones push, recordatorios o emails automatizados.
- Integración con apps externas o wearables.
- Registro detallado por mililitros u horas específicas de consumo.
- Estadísticas avanzadas o gráficas comparativas.
- Compartir progreso en redes sociales.
- Recuperación de contraseña.
- Login con Google u otros proveedores externos.
- Edición manual de check-ins pasados.
- Modo offline.
- Hábitos compartidos entre usuarios.

---

## Criterios de aceptación

### Autenticación

1. Dado que el usuario no tiene cuenta, cuando completa el formulario de registro con email y contraseña válidos y lo envía, entonces se crea su cuenta y queda autenticado en la aplicación.

2. Dado que el usuario tiene cuenta, cuando ingresa email y contraseña correctos en el formulario de login, entonces accede a su sesión autenticada.

3. Dado que el usuario está autenticado, cuando ejecuta la acción de logout, entonces su sesión termina y es redirigido a la pantalla de login.

4. Dado que alguien intenta acceder a una pantalla protegida sin sesión activa, cuando navega a esa URL, entonces es redirigido al login sin visualizar contenido protegido.

---

### CRUD de hábitos

5. Dado que el usuario está autenticado, cuando crea un hábito con nombre, descripción y frecuencia (diaria o semanal), entonces el hábito aparece en su lista de hábitos.

6. Dado que el usuario crea un hábito de hidratación, cuando define el objetivo de vasos de agua, entonces el sistema guarda ese valor asociado al hábito.

7. Dado que el usuario tiene hábitos creados, cuando edita el nombre, descripción, frecuencia u objetivo de vasos, entonces el hábito refleja los nuevos valores en la lista.

8. Dado que el usuario tiene hábitos creados, cuando elimina uno, entonces ese hábito desaparece de su lista y no es posible hacer check-in sobre él.

9. Dado que el usuario no tiene hábitos creados, cuando accede a la lista de hábitos, entonces visualiza un estado vacío sin error.

---

### Check-in diario

10. Dado que el usuario tiene al menos un hábito activo, cuando realiza check-in para el día actual, entonces el hábito queda marcado como completado para esa fecha.

11. Dado que un hábito ya tiene check-in registrado en la fecha actual, cuando el usuario intenta marcarlo nuevamente, entonces el sistema no permite un segundo check-in para ese mismo día.

12. Dado que el usuario tiene hábitos pendientes y completados, cuando accede a la pantalla principal, entonces puede distinguir visualmente cuáles hábitos ya fueron completados en el día actual.

13. Dado que el usuario consulta un hábito con check-in registrado en el día actual, cuando visualiza el detalle del hábito, entonces puede ver la fecha del último check-in realizado.

---

### Vista de progreso y racha

14. Dado que el usuario tiene check-ins consecutivos en un hábito, cuando consulta el progreso del hábito, entonces el sistema muestra la racha actual como número de días consecutivos cumplidos.

15. Dado que el usuario omite un día esperado según la frecuencia del hábito, cuando consulta la racha actual, entonces la racha se reinicia.

16. Dado que un hábito no tiene check-ins registrados, cuando el usuario consulta su progreso, entonces la racha actual mostrada es 0.

17. Dado que el usuario tiene varios hábitos registrados, cuando accede a la pantalla principal, entonces puede visualizar para cada hábito:
- nombre
- frecuencia
- objetivo de vasos
- estado del día actual
- racha actual

---

## Decisiones técnicas

- La app tendrá frontend y backend separados.
- El frontend será una aplicación web.
- El backend expondrá una API REST.
- La información se guardará en MongoDB.
- La autenticación utilizará email y password.
- Las contraseñas se almacenarán hasheadas.
- El backend utilizará JWT para proteger rutas autenticadas.
- Cada usuario únicamente podrá acceder a sus propios hábitos y check-ins.
- Cada hábito pertenecerá a un único usuario.
- Cada check-in pertenecerá a un único hábito.
- La frecuencia del hábito únicamente podrá ser diaria o semanal.
- La fecha de check-in se basará en la hora local del servidor.
- La racha se calculará utilizando únicamente los check-ins registrados del hábito.