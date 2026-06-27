# Claude Code Configuration Project

## Reglas de git

- Nunca hagas commit, push ni abras PRs por iniciativa propia a menos que sea a través de un command de claude o por solicitud directa del usuario.
- Detente siempre después de implementar una nueva feature y espera confirmación explícita
- Nunca elimines ramas remotas
- Nunca commitees directamente en main, master o develop

### Convención de commits

```
feat(scope): description
fix(scope): description
docs(scope): description
refactor(scope): description
```

Un commit por cambio lógico.

---

## Human in the Loop — obligatorio

Detente y espera confirmación explícita antes de:

- Instalar o actualizar dependencias
- Modificar archivos de configuración del proyecto
- Crear o modificar migraciones de base de datos
- Ejecutar comandos que afecten infraestructura o servicios externos

---

## Reglas de implementación

- Implementa un grupo de tareas a la vez — nunca todo el plan de golpe
- Reporta qué hiciste después de cada grupo y espera confirmación
- No instales dependencias sin preguntar primero
- No propongas tecnologías fuera del stack del proyecto sin preguntar

---

## SDD — Spec-Driven Development

Este proyecto usa SDD. Si existe `specs/` en el proyecto,
lee la constitución antes de hacer cualquier cosa.

Los slash commands de commit solo se ejecutan cuando yo los invoco
explícitamente — nunca por iniciativa propia.

---

## Idioma

- Código y commits: inglés
- Comentarios en código: español
- Respuestas al usuario: español