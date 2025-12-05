## Qué está fallando
- El build usa `src/environments/environment.prod.ts` (Angular `fileReplacements` con configuración por defecto `production`).
- En `environment.prod.ts` solo existe `production: true` y falta `url_api`, que sí está en `environment.ts`.
- Los servicios (`administradores`, `alumnos`, etc.) consumen `environment.url_api`, por eso aparece TS2339.

## Cambios a realizar
1. Añadir `url_api` en `src/environments/environment.prod.ts` con la URL del backend en producción.
   - Ejemplo:
     ```ts
     export const environment = {
       production: true,
       url_api: "https://TU_API_PROD"
     };
     ```
   - Si aún no tienes backend público, puedes poner temporalmente la misma de dev para compilar, pero en Vercel no funcionará `127.0.0.1`; debes usar una URL accesible públicamente.
2. Revisar que todos los servicios importen desde `src/environments/environment` (ya lo hacen) y no usen otras rutas.
3. Opcional (robustez): Tipar el `environment` para asegurar que ambas variantes tengan las mismas propiedades.
   - Crear `src/environments/environment.model.ts` y usar la interfaz en ambos archivos para evitar futuros desajustes.

## Verificación
- Ejecutar `ng build --configuration production` localmente para confirmar que el error desaparece.
- Desplegar a Vercel. Si el backend no permite CORS desde el dominio de Vercel, ajustar la configuración del backend para permitir ese origen.

## Entregables
- `environment.prod.ts` actualizado con `url_api`.
- (Opcional) Interfaz de `environment` unificada.
- Build de producción sin errores listo para subir a Vercel.