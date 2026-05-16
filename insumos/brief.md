# Brief: Water Habit Tracker

## 1. El problema

Muchas personas quieren tomar más agua durante el día, pero no tienen una forma clara de saber si están sosteniendo el hábito. El seguimiento suele quedar en la memoria, en notas sueltas o en recordatorios que se ignoran después de unos días.

Las soluciones existentes suelen ser demasiado genéricas, demasiado cargadas o centradas en métricas de salud que exceden lo que una persona necesita para empezar. Este proyecto busca una app simple para registrar hábitos de hidratación, hacer check-ins diarios y ver progreso mediante rachas de constancia.

## 2. Núcleo obligatorio

1. Autenticación y espacio privado
El usuario puede registrarse, iniciar sesión y cerrar sesión. La app debe mostrar solo los hábitos y registros de hidratación del usuario autenticado.
Decisiones abiertas: ¿el registro será con email/password únicamente o se permitirá otro flujo soportado por Supabase?

2. Gestión de hábitos de hidratación
El usuario puede crear, ver, editar y eliminar hábitos relacionados con hidratación. Cada hábito representa una conducta concreta, como tomar agua al despertar, durante el trabajo o antes de dormir.
Decisiones abiertas: ¿los hábitos tendrán frecuencia diaria fija o podrán tener variaciones por día de la semana?

3. Check-in diario
El usuario puede marcar en el día actual si cumplió cada hábito de hidratación. El registro debe permitir distinguir claramente qué hábitos ya se completaron y cuáles siguen pendientes.
Decisiones abiertas: ¿se podrá desmarcar o corregir un check-in del día actual?

4. Vista principal de seguimiento
El usuario ve en una pantalla principal sus hábitos de hidratación, el estado del día y una acción rápida para registrar cumplimiento. La prioridad es que la app sirva para uso diario sin fricción.
Decisiones abiertas: ¿la vista principal mostrará solo el día actual o también un resumen reciente?

5. Progreso básico
La app muestra el progreso mediante rachas de constancia y una lectura simple del historial reciente. El objetivo es que el usuario entienda si está sosteniendo sus hábitos sin entrar en análisis complejo.
Decisiones abiertas: ¿cómo se calcula una racha cuando un usuario omite un día o modifica un hábito?

## 3. Extensiones (elegir máximo 1)

| Extensión | Descripción |
|---|---|
| Recordatorios | Permitir avisos configurables para hábitos de hidratación en momentos del día. |
| Meta diaria | Definir una meta personal de hidratación y mostrar avance diario hacia ella. |
| Estadísticas históricas | Mostrar cumplimiento por semana o mes con métricas agregadas de constancia. |
| Plantillas de hábitos | Ofrecer hábitos sugeridos de hidratación que el usuario pueda activar. |
| Exportación de datos | Descargar hábitos e historial en un formato portable como CSV o JSON. |
| Notas por día | Permitir que el usuario agregue una nota breve sobre su hidratación diaria. |

## 4. Restricciones técnicas

- Proyecto web con Next.js 15 y App Router.
- Supabase para Postgres y autenticación.
- Deploy en Vercel.
- TypeScript estricto.
- Tailwind para estilos.
- Alcance pensado para 2-3 semanas de trabajo enfocado por un developer con experiencia básica dirigiendo agentes de IA.

## 5. Lo que NO se evalúa

- Diseño visual premium o sistema de marca completo.
- Performance avanzada más allá de una experiencia razonablemente fluida.
- Cobertura exhaustiva de tests automatizados.
- Responsive perfecto en todos los tamaños de pantalla.
- Funcionalidades comerciales como pagos, planes, onboarding complejo o soporte multi-equipo.
