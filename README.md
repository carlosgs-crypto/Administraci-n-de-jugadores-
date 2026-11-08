# ARENA E-SPORTS — proyecto inicial

Panel web responsive para administrar jugadores y varios administradores. Hecho con React + Vite + Supabase.

## 1. Crear Supabase
1. Crea un proyecto en https://supabase.com.
2. Abre SQL Editor y ejecuta todo `supabase/schema.sql`.
3. En Authentication > URL Configuration, configura las URLs de tu web publicada.
4. En Project Settings > API, copia Project URL y la clave pública `anon` (publishable/anon). Nunca uses `service_role` en el navegador.

## 2. Configurar el proyecto
1. Renombra `.env.example` a `.env`.
2. Coloca tu URL y clave pública:
   `VITE_SUPABASE_URL=...`
   `VITE_SUPABASE_ANON_KEY=...`
3. En tu computadora instala Node.js y ejecuta:
   `npm install`
   `npm run dev`
   Para producción: `npm run build`.

## 3. Crear la cuenta propietaria
1. Abre la web y crea una cuenta con tu correo.
2. Confirma el correo si Supabase lo solicita.
3. En Supabase > SQL Editor ejecuta, sustituyendo el correo:
   `update public.profiles set role='owner' where email='TU_CORREO';`
4. Cierra sesión y vuelve a entrar. Tu cuenta ya verá Administradores.

## 4. Activar invitaciones de administradores
La función `supabase/functions/invite-admin/index.ts` usa la clave secreta solo en el servidor.
1. Instala Supabase CLI y vincula el proyecto (`supabase login`, `supabase link --project-ref TU_REF`).
2. Despliega: `supabase functions deploy invite-admin`.
3. La función usa automáticamente los secretos de Supabase: `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`.
4. En Authentication > SMTP Settings configura correo saliente para entregar invitaciones de forma fiable (sin SMTP personalizado hay límites de envío).
5. El propietario invita desde el panel. La persona recibe correo, acepta la invitación y crea su contraseña.

## 5. Publicar en Vercel
1. Sube esta carpeta a un repositorio de GitHub.
2. En Vercel importa el repositorio.
3. Framework: Vite. Build command: `npm run build`. Output directory: `dist`.
4. Añade `VITE_SUPABASE_URL` y `VITE_SUPABASE_ANON_KEY` en Environment Variables.
5. Publica y configura la URL de Vercel en Supabase Authentication > URL Configuration.

## Permisos
- `owner`: propietario. Es el único que puede invitar administradores.
- `admin`: puede registrar y eliminar jugadores.
- `user`: cuenta básica sin permisos administrativos.

## Alcance actual
Incluye inicio/registro de sesión, panel, alta/listado/búsqueda/eliminación de jugadores y vista de administradores. La sección de torneos está preparada como próxima función. Las políticas RLS de Supabase protegen las tablas. Antes de producción, prueba con cuentas separadas de propietario, administrador y usuario normal.
