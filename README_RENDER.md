# Despliegue en Render + comentarios

## Render

1. Sube este proyecto a GitHub.
2. En Render crea un **Static Site**.
3. Configura:
   - Root Directory: deja vacío si el repo contiene directamente este proyecto. Si subes una carpeta contenedora, usa `CV-Benja`.
   - Build Command: `npm install && npm run build`
   - Publish Directory: `dist`
   - Node Version: `22.12.0`

## Comentarios compartidos con Supabase

El sitio funciona igual sin Supabase, pero los comentarios se guardan solo en el navegador de cada visitante. Para que todos vean los mismos comentarios, crea una tabla en Supabase y agrega las variables en Render.

### SQL para Supabase

Ejecuta esto en Supabase > SQL Editor:

```sql
create table if not exists public.comments (
  id uuid primary key default gen_random_uuid(),
  name text not null,
  text text not null,
  rating numeric not null check (rating >= 0.5 and rating <= 5),
  created_at timestamptz not null default now()
);

alter table public.comments enable row level security;

create policy "comments_select_public"
on public.comments for select
to anon
using (true);

create policy "comments_insert_public"
on public.comments for insert
to anon
with check (true);

create policy "comments_delete_public"
on public.comments for delete
to anon
using (true);
```

### Variables en Render

En Render > Environment agrega:

```txt
PUBLIC_SUPABASE_URL=tu_url_de_supabase
PUBLIC_SUPABASE_ANON_KEY=tu_anon_key_de_supabase
```

Luego vuelve a desplegar.
