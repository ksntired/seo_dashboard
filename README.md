# SEO Dashboard

Веб-панель для SEO-специалиста: слева список проектов, справа рабочая область выбранного проекта.

## Что есть сейчас

- создание проекта вручную или по ссылке;
- первичное определение тематики эвристикой;
- структура проекта: позиционирование, аудитория, разделы, AI-инструменты;
- семантическое ядро: ручное добавление, редактирование, удаление, импорт `.xlsx/.xls/.csv`;
- контент-план: задачи, тип, ссылка, отметка готовности с вычеркиванием;
- ссылочная закупка: донор, анкор, целевая страница, цена, статус, DR, трафик;
- документы: ссылки на ТЗ, инструкции, промпты, отчеты;
- данные сохраняются в `localStorage` браузера.

## Открыть

Открой `index.html` в браузере. Сборка и сервер не нужны.

## Supabase

В `index.html` уже добавлены `SUPABASE_URL` и publishable key. Сейчас приложение работает локально, чтобы не падать без таблиц. Для постоянного хранения можно сделать таблицы:

```sql
create table projects (
  id uuid primary key default gen_random_uuid(),
  name text not null,
  url text,
  theme text,
  region text,
  description text,
  structure jsonb default '{}'::jsonb,
  created_at timestamptz default now()
);

create table semantic_keywords (
  id uuid primary key default gen_random_uuid(),
  project_id uuid references projects(id) on delete cascade,
  keyword text not null,
  frequency integer default 0
);

create table content_plan (
  id uuid primary key default gen_random_uuid(),
  project_id uuid references projects(id) on delete cascade,
  title text not null,
  type text,
  link text,
  done boolean default false
);

create table link_purchases (
  id uuid primary key default gen_random_uuid(),
  project_id uuid references projects(id) on delete cascade,
  donor text,
  anchor text,
  target text,
  price numeric default 0,
  status text,
  dr integer default 0,
  traffic integer default 0
);

create table project_documents (
  id uuid primary key default gen_random_uuid(),
  project_id uuid references projects(id) on delete cascade,
  title text not null,
  type text,
  url text,
  note text
);
```

Не добавляй во фронт `service_role` ключ. Только publishable/anon key.
