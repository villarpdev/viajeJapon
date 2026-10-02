JAPÓN 2026 — GITHUB PAGES Y NOTAS COMPARTIDAS

PUBLICAR
1. Crea un repositorio en GitHub.
2. Sube index.html, manifest.webmanifest, sw.js y config.js a la raíz.
3. En Settings > Pages, selecciona Deploy from a branch, rama main y carpeta /(root).
4. Comparte la URL que genere GitHub Pages.

NOTAS COMUNES (Supabase requerido; GitHub Pages por sí solo no puede guardar datos)
1. Crea un proyecto en https://supabase.com/
2. En SQL Editor ejecuta:

create table public.shared_notes (
  id integer primary key check (id = 1),
  content text not null default '',
  updated_at timestamptz not null default now()
);
insert into public.shared_notes (id, content) values (1, '') on conflict (id) do nothing;
alter table public.shared_notes enable row level security;
create policy "Public read trip notes" on public.shared_notes for select to anon using (true);
create policy "Public update trip notes" on public.shared_notes for update to anon using (true) with check (true);

Además, para las notas independientes de cada día, ejecuta también:

create table public.shared_day_notes (
  id integer primary key check (id = 1),
  content text not null default '{}',
  updated_at timestamptz not null default now()
);
insert into public.shared_day_notes (id, content) values (1, '{}') on conflict (id) do nothing;
alter table public.shared_day_notes enable row level security;
create policy "Public read shared day notes" on public.shared_day_notes for select to anon using (true);
create policy "Public update shared day notes" on public.shared_day_notes for update to anon using (true) with check (true);

3. En Project Settings > API copia Project URL y la clave anon/public.
4. Pégalas en config.js como supabaseUrl y supabaseAnonKey.
5. Sube config.js actualizado a GitHub y recarga la web.

PRIVACIDAD
Con estas políticas, cualquier persona que conozca la URL pública puede leer y editar las notas. No guardéis datos personales, contraseñas ni números de reserva. Para restringirlo al grupo hace falta autenticación y políticas adicionales. La clave anon/public está diseñada para el cliente; nunca publiques una service_role key.
Las notas generales se comparten como un único texto y las notas diarias como listas independientes por día. Si varias personas editan a la vez, puede prevalecer el último guardado. Los checks de visitas y checklist permanecen locales por dispositivo.
Las fotos se cargan desde Unsplash y requieren conexión.
