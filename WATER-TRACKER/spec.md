# Spec: Water Habit Tracker

## Objetivo

Aplicacion web simple para que usuarios registren habitos relacionados con hidratacion, hagan check-ins diarios y visualicen si estan sosteniendo el habito mediante rachas de constancia e historial reciente.

---

## Scope

### Que SI entra en este proyecto

- Registro, login y logout con Supabase Auth usando email y password.
- Espacio privado por usuario autenticado.
- CRUD de habitos de hidratacion.
- Cada habito representa una conducta concreta de hidratacion, por ejemplo tomar agua al despertar, durante el trabajo o antes de dormir.
- Cada habito tiene nombre, descripcion opcional y frecuencia diaria.
- Pantalla principal con los habitos del usuario, estado del dia actual y accion rapida de check-in.
- Check-in del dia actual para marcar si el usuario cumplio cada habito.
- Diferenciacion visual entre habitos completados y pendientes del dia actual.
- Restriccion para evitar mas de un check-in por habito en la misma fecha.
- Vista de progreso basico con racha actual e historial reciente.
- Persistencia de usuarios, habitos y check-ins en Supabase/Postgres.
- Deploy en Vercel.

### Que NO entra en este proyecto

- Ver ni interactuar con datos de otros usuarios.
- Notificaciones push, recordatorios o emails automatizados.
- Integracion con apps externas o wearables.
- Registro detallado por mililitros u horas especificas de consumo.
- Estadisticas historicas avanzadas o graficas comparativas por semana/mes.
- Compartir progreso en redes sociales.
- Login con Google u otros proveedores externos.
- Recuperacion de contrasena implementada fuera del flujo soportado por Supabase.
- Edicion manual de check-ins pasados.
- Desmarcar o corregir check-ins de dias anteriores.
- Modo offline.
- Habitos compartidos entre usuarios.
- Pagos, planes, onboarding complejo o soporte multi-equipo.
- Diseno visual premium o sistema de marca completo.

---

## Decisiones de producto

- El registro sera solo con email y password.
- Los habitos tendran frecuencia diaria fija en la primera version.
- El usuario no podra crear mas de un check-in para el mismo habito en el mismo dia.
- La vista principal mostrara el dia actual y un resumen reciente breve.
- La racha se calcula con dias consecutivos cumplidos para cada habito diario.
- Si el usuario omite un dia, la racha se reinicia.
- Si el usuario edita un habito, sus check-ins anteriores se conservan.
- No se incluye una extension opcional en la primera version; el alcance queda limitado al nucleo obligatorio del brief.

---

## Criterios de aceptacion

### Autenticacion y espacio privado

1. Dado que el usuario no tiene cuenta, cuando completa el registro con email y password validos, entonces Supabase crea su cuenta y el usuario queda autenticado.

2. Dado que el usuario tiene cuenta, cuando ingresa email y password correctos, entonces accede a su sesion.

3. Dado que el usuario esta autenticado, cuando ejecuta logout, entonces su sesion termina y vuelve a una pantalla no autenticada.

4. Dado que una persona sin sesion intenta entrar a una pantalla protegida, cuando abre esa ruta, entonces no puede ver habitos ni check-ins privados.

5. Dado que un usuario esta autenticado, cuando consulta la aplicacion, entonces solo ve habitos y check-ins asociados a su propio usuario.

### Gestion de habitos de hidratacion

6. Dado que el usuario esta autenticado, cuando crea un habito con nombre y descripcion opcional, entonces el habito aparece en su lista.

7. Dado que el usuario tiene habitos creados, cuando edita el nombre o descripcion de uno, entonces la lista muestra los nuevos valores.

8. Dado que el usuario tiene habitos creados, cuando elimina uno, entonces ese habito deja de aparecer en su lista y no puede recibir nuevos check-ins.

9. Dado que el usuario no tiene habitos creados, cuando entra a la pantalla principal, entonces ve un estado vacio claro y sin error.

### Check-in diario

10. Dado que el usuario tiene al menos un habito activo, cuando marca check-in para el dia actual, entonces el habito queda completado para esa fecha.

11. Dado que un habito ya tiene check-in del dia actual, cuando el usuario intenta marcarlo otra vez, entonces el sistema impide duplicar el registro.

12. Dado que el usuario tiene habitos pendientes y completados, cuando entra a la pantalla principal, entonces puede distinguir visualmente ambos estados.

13. Dado que el usuario realizo un check-in hoy, cuando consulta el habito, entonces puede ver la fecha del ultimo check-in.

### Vista principal

14. Dado que el usuario esta autenticado, cuando entra a la aplicacion, entonces ve sus habitos de hidratacion, el estado del dia actual y una accion rapida para registrar cumplimiento.

15. Dado que el usuario usa la aplicacion diariamente, cuando vuelve a la pantalla principal, entonces puede completar los habitos del dia sin navegar por flujos largos.

### Progreso basico

16. Dado que el usuario tiene check-ins consecutivos en un habito diario, cuando consulta su progreso, entonces ve la racha actual como numero de dias consecutivos cumplidos.

17. Dado que el usuario omite un dia, cuando consulta su progreso, entonces la racha actual de ese habito se reinicia.

18. Dado que un habito no tiene check-ins registrados, cuando el usuario consulta su progreso, entonces la racha actual es 0.

19. Dado que el usuario tiene historial reciente, cuando consulta su progreso, entonces ve una lectura simple de los ultimos dias sin estadisticas avanzadas.

---

## Decisiones tecnicas

- El proyecto sera una aplicacion web con Next.js 15 y App Router.
- El lenguaje sera TypeScript estricto.
- Los estilos se implementaran con Tailwind CSS.
- La autenticacion se implementara con Supabase Auth.
- La base de datos sera Supabase Postgres.
- Las reglas de privacidad se apoyaran en el usuario autenticado y politicas de acceso por usuario.
- El deploy objetivo sera Vercel.
- No habra backend separado ni API REST propia como requisito inicial.
- La aplicacion podra usar Server Components, Server Actions o Route Handlers de Next.js cuando convenga.
- La fecha de check-in se guardara como fecha de calendario para evitar duplicados por habito y dia.
- Cada habito pertenecera a un unico usuario.
- Cada check-in pertenecera a un unico habito y a un unico usuario.
- El alcance esta pensado para 2-3 semanas de trabajo enfocado.
