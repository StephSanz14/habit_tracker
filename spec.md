# Spec: Water Habit Tracker

## Objetivo

Construir una app web simple para que cada usuario registre habitos de hidratacion diarios, marque cumplimiento del dia actual y entienda su constancia mediante rachas, historial reciente y porcentaje basico de cumplimiento.

La app prioriza uso diario sin friccion, privacidad por usuario autenticado y un alcance viable para 2-3 semanas de trabajo enfocado.

## Scope

### Si entra

- App web con Next.js 15, App Router, TypeScript estricto y Tailwind.
- Supabase Auth con registro, login y logout por email y password.
- Supabase Postgres para persistir usuarios, habitos y check-ins.
- Uso del cliente Supabase en navegador con SWR para data fetching y revalidacion.
- Rutas: `/`, `/login`, `/signup`, `/habitos/[id]`, `/archivados` y `/estadisticas`.
- Habitos de hidratacion diarios con nombre requerido y descripcion opcional.
- Validaciones: nombre de 1 a 60 caracteres, descripcion hasta 200 caracteres y maximo 20 habitos activos por usuario.
- Los nombres de habitos pueden repetirse para el mismo usuario.
- Cada habito pertenece a un usuario y tiene `archived_at` nullable para archivado.
- Archivar un habito lo quita del flujo diario, conserva su historial y bloquea nuevos check-ins desde la UI.
- Check-in como fila unica por `user_id`, `habit_id` y `local_date`.
- `local_date` se guarda como `DATE` local calculado desde la zona horaria del navegador al hacer check-in.
- El check-in solo existe para marcar cumplimiento; no hay fila de "pendiente".
- Racha actual, mejor racha y porcentaje de cumplimiento se calculan desde check-ins, no se persisten como columnas derivadas.
- Vista principal con habitos activos, estado de hoy y accion rapida para registrar cumplimiento.
- Estadisticas basicas con historial de ultimos 14 dias incluyendo hoy.
- Los dias previos a la creacion del habito se muestran como "no aplica".
- Habitos archivados cuentan en estadisticas solo durante su periodo activo.
- Cambios entre pestanas o dispositivos se consideran sincronizados al recargar datos; no se exige realtime.
- Errores y confirmaciones operativas se muestran con toasts; sesion expirada redirige a `/login`.
- Inicializacion esperada: `npx create-next-app@latest . --ts --tailwind --eslint --app --no-src-dir --import-alias "@/*"` y fijar `next@15` en `package.json`.
- Migraciones en `supabase/migrations` y variables de entorno en `.env.local`.
- Variables requeridas: `NEXT_PUBLIC_SUPABASE_URL` y `NEXT_PUBLIC_SUPABASE_ANON_KEY`.

### No entra

- Habitos semanales, dias fijos de la semana o metas de N veces por semana.
- Check-ins para dias pasados.
- Desmarcar o corregir check-ins de dias anteriores.
- Login con proveedores externos.
- Extension opcional del brief en v1.
- Backend REST propio como requisito de producto.

## Criterios de aceptacion

### Autenticacion

1. Dado un visitante sin cuenta, cuando abre `/signup`, ingresa email y password validos y envia el formulario, entonces queda autenticado y ve la vista principal.
2. Dado un visitante con cuenta, cuando abre `/login`, ingresa credenciales validas y envia el formulario, entonces ve la vista principal.
3. Dado un visitante, cuando intenta iniciar sesion con credenciales invalidas, entonces permanece en `/login` y ve un toast de error.
4. Dado un usuario autenticado, cuando ejecuta logout, entonces vuelve a `/login` y no puede ver rutas privadas sin volver a autenticarse.
5. Dado un visitante sin sesion, cuando intenta abrir `/`, `/habitos/[id]`, `/archivados` o `/estadisticas`, entonces es redirigido a `/login`.

### Habitos

6. Dado un usuario autenticado con menos de 20 habitos activos, cuando crea un habito con nombre valido y descripcion opcional, entonces el habito aparece en la vista principal como pendiente para hoy.
7. Dado un usuario autenticado, cuando intenta crear o editar un habito con nombre vacio o mayor a 60 caracteres, entonces la app muestra un toast de error y no guarda el cambio.
8. Dado un usuario autenticado, cuando intenta guardar una descripcion mayor a 200 caracteres, entonces la app muestra un toast de error y no guarda el cambio.
9. Dado un usuario con 20 habitos activos, cuando intenta crear otro habito activo, entonces la app muestra un toast de error y no crea el habito.
10. Dado un usuario con dos habitos de igual nombre, cuando abre la vista principal, entonces ambos se muestran como habitos separados.
11. Dado un usuario con un habito existente, cuando edita su nombre o descripcion desde `/habitos/[id]`, entonces el cambio se ve al volver a la vista principal.
12. Dado un usuario con un habito activo, cuando lo archiva, entonces deja de aparecer en la vista principal y aparece en `/archivados`.

### Check-in diario

13. Dado un habito activo pendiente para hoy, cuando el usuario marca check-in, entonces el habito cambia a cumplido para hoy sin navegar fuera de la vista actual.
14. Dado un habito ya cumplido hoy, cuando el usuario vuelve a cargar la vista principal, entonces el habito sigue apareciendo como cumplido.
15. Dado un habito ya cumplido hoy, cuando el usuario intenta repetir la accion de check-in, entonces la UI no crea otro registro visible y muestra estado cumplido.
16. Dado un habito archivado, cuando el usuario lo ve en `/archivados`, entonces no tiene accion disponible para crear nuevos check-ins.

### Progreso

17. Dado un habito recien creado sin check-ins, cuando el usuario abre `/estadisticas`, entonces su racha actual se muestra como 0.
18. Dado un habito con check-ins consecutivos hasta hoy, cuando el usuario abre `/estadisticas`, entonces ve la racha actual calculada en dias consecutivos.
19. Dado un habito que omitio un dia aplicable, cuando el usuario abre `/estadisticas`, entonces la racha actual se reinicia desde el siguiente check-in consecutivo.
20. Dado un habito activo, cuando el usuario abre `/estadisticas`, entonces ve los ultimos 14 dias incluyendo hoy.
21. Dado un habito creado hace menos de 14 dias, cuando se muestra su historial, entonces los dias anteriores a su creacion aparecen como "no aplica".
22. Dado un habito archivado, cuando se muestra su historial o porcentaje, entonces solo cuentan los dias entre su creacion y su archivado.
23. Dado un usuario que marca un check-in en una pestana, cuando abre o recarga otra pestana, entonces la segunda pestana muestra el estado actualizado.

### Errores y estados

24. Dado un fallo de red o de Supabase durante carga de datos, cuando la app no puede completar la operacion, entonces muestra un toast de error.
25. Dado un fallo al crear, editar, archivar o marcar check-in, cuando Supabase rechaza la operacion, entonces la UI muestra un toast de error y no muestra el cambio como guardado.
26. Dado que la sesion expira, cuando el usuario intenta usar una ruta privada, entonces la app redirige a `/login`.

## Pruebas tecnicas fuera de QA manual

- Verificar con pruebas o revision de politicas RLS que un usuario no puede leer ni escribir habitos o check-ins de otro usuario.
- Verificar que existe constraint unico para impedir mas de un check-in por `user_id`, `habit_id` y `local_date`.
- Verificar que un check-in no puede quedar asociado a un `habit_id` que pertenece a otro usuario.
- Verificar que archivar un habito escribe `archived_at` y no borra sus check-ins.
- Verificar que no existen columnas persistidas para `current_streak` ni `best_streak`; las rachas se derivan desde check-ins.
- Verificar que las migraciones viven en `supabase/migrations` y pueden aplicarse en un proyecto Supabase limpio.
- Verificar que `.env.local` define `NEXT_PUBLIC_SUPABASE_URL` y `NEXT_PUBLIC_SUPABASE_ANON_KEY`.

## No-goals

- No construir mobile nativo.
- No construir PWA ni soporte offline.
- No incluir pagos, planes, monetizacion, soporte multi-equipo ni onboarding extenso.
- No incluir gamificacion: badges, niveles, retos, confeti ni animaciones de celebracion.
- No incluir notificaciones ni recordatorios por email, push o in-app.
- No incluir compartir social.
- No crear identidad de marca premium, sistema de marca completo ni personalizacion visual avanzada.
- No buscar diseno visual premium.
- No incluir estadisticas historicas avanzadas por semana o mes.
- No incluir metricas de salud complejas.
- No registrar consumo por mililitros, horarios exactos o volumen de agua.
- No incluir plantillas de habitos, exportacion de datos, notas por dia ni meta diaria en v1.
- No exigir responsive perfecto en todos los tamanos de pantalla.
- No exigir cobertura exhaustiva de tests automatizados.

## Decisiones tomadas en entrevista

- La frecuencia de habitos en v1 es solo diaria.
- La frecuencia semanal queda fuera de v1; no hay N veces por semana ni dias fijos.
- El check-in se modela como fila unica por dia local.
- Los habitos se archivan con `archived_at`, no se borran en cascada.
- Los check-ins de habitos archivados se conservan.
- Los habitos archivados salen del flujo diario y no admiten nuevos check-ins desde la UI.
- Los nombres de habitos duplicados por usuario estan permitidos.
- La unicidad fuerte aplica a check-ins por usuario, habito y fecha local.
- Las rachas se calculan desde check-ins y no se persisten.
- La fecha de cumplimiento se guarda como `DATE` local.
- La sincronizacion entre dispositivos o pestanas se valida al recargar.
- Un habito sin check-ins muestra racha 0.
- El historial reciente cubre 14 dias incluyendo hoy.
- Los dias previos a la creacion del habito se muestran como "no aplica".
- Los porcentajes usan dias aplicables del periodo activo.
- Aislamiento de datos, RLS y constraints se prueban fuera del QA manual.
- La app usa cliente Supabase en navegador y SWR.
- Las rutas de v1 son `/`, `/login`, `/signup`, `/habitos/[id]`, `/archivados` y `/estadisticas`.
- Los errores se muestran con toasts.
- La sesion expirada redirige a `/login`.
- El limite por usuario es de 20 habitos activos.
- El setup se basa en `create-next-app` con Next.js 15, App Router, TypeScript estricto y Tailwind.
- Las migraciones viven en `supabase/migrations` y las variables en `.env.local`.
- Mobile nativo, PWA, monetizacion, gamificacion, notificaciones, social y marca premium quedan fuera como no-goals explicitos.
