# Spec: Water Habit Tracker

## Objetivo

Crear una app web simple para que una persona registre habitos de hidratacion, haga check-ins diarios y vea si esta sosteniendo la constancia mediante rachas e historial reciente.

## Scope

### Si entra

- Registro, inicio de sesion y cierre de sesion con email y password.
- Espacio privado: cada usuario ve solo sus habitos y registros.
- Crear, ver, editar y eliminar habitos de hidratacion.
- Habitos con frecuencia diaria fija.
- Check-in del dia actual para marcar un habito como cumplido.
- Vista principal con habitos, estado del dia y accion rapida de check-in.
- Diferenciacion clara entre habitos completados y pendientes.
- Progreso basico con racha actual e historial reciente.
- Persistencia en Supabase Postgres.
- Deploy en Vercel.

### No entra

- Login con proveedores externos.
- Frecuencias semanales o configuraciones avanzadas por dia.
- Check-ins para dias pasados.
- Desmarcar o corregir check-ins de dias pasados.
- Recordatorios, metas diarias, plantillas, notas o exportacion de datos.
- Estadisticas historicas avanzadas.
- Registro por mililitros, horarios exactos o metricas de salud complejas.

## Criterios de aceptacion

1. Dado un usuario nuevo, cuando se registra con email y password validos, entonces puede entrar a su espacio privado.
2. Dado un usuario autenticado, cuando crea un habito de hidratacion, entonces el habito aparece en su lista.
3. Dado un usuario autenticado, cuando edita o elimina un habito propio, entonces el cambio se refleja solo en su cuenta.
4. Dado un habito pendiente del dia actual, cuando el usuario marca check-in, entonces queda registrado como cumplido para hoy.
5. Dado un habito ya cumplido hoy, cuando el usuario intenta registrarlo otra vez, entonces no se duplica el check-in.
6. Dado un usuario con habitos completados y pendientes, cuando abre la vista principal, entonces puede distinguir ambos estados.
7. Dado un usuario con check-ins consecutivos, cuando revisa su progreso, entonces ve su racha actual.
8. Dado un usuario que omite un dia, cuando revisa su progreso, entonces la racha de ese habito se reinicia.

## No-goals

- No construir una app de salud avanzada.
- No incluir mas de una extension opcional del brief.
- No priorizar diseno visual premium.
- No exigir responsive perfecto en todos los tamanos.
- No buscar cobertura exhaustiva de tests automatizados.
- No incluir pagos, planes, onboarding complejo ni soporte multi-equipo.
