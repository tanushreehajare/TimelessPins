# 🌸 Our PinMap — Setup Guide

A romantic memory-mapping web app. 100% free to run on Vercel + Supabase.

---

## 📦 Files
```
our-pinmap/
├── index.html     ← entire app (edit SUPABASE_URL + KEY inside)
├── vercel.json    ← deployment config
└── README.md
```

---

## 🗄️ Step 1: Supabase Setup

### Create project
1. Go to [supabase.com](https://supabase.com) → free account → **New Project**

### Run this SQL (SQL Editor in Supabase dashboard)
```sql
-- Love Coordinates
create table lc_pins (
  id uuid default gen_random_uuid() primary key,
  created_at timestamptz default now(),
  name text not null,
  location text,
  date date,
  description text,
  images text[] default '{}',
  lat double precision not null,
  lng double precision not null
);

-- SweetSpots
create table sweet_pins (
  id uuid default gen_random_uuid() primary key,
  created_at timestamptz default now(),
  name text not null,
  lat double precision not null,
  lng double precision not null
);

-- Snuggle Spots
create table snuggle_pins (
  id uuid default gen_random_uuid() primary key,
  created_at timestamptz default now(),
  name text not null,
  lat double precision not null,
  lng double precision not null,
  song_url text,
  song_img text,
  song_title text
);

-- Love Trails
create table trail_pins (
  id uuid default gen_random_uuid() primary key,
  created_at timestamptz default now(),
  name text not null,
  points jsonb not null,
  lat double precision not null,
  lng double precision not null
);

-- Row-level security (allow public read+write for personal use)
alter table lc_pins      enable row level security;
alter table sweet_pins   enable row level security;
alter table snuggle_pins enable row level security;
alter table trail_pins   enable row level security;

create policy "Public" on lc_pins      for all using (true) with check (true);
create policy "Public" on sweet_pins   for all using (true) with check (true);
create policy "Public" on snuggle_pins for all using (true) with check (true);
create policy "Public" on trail_pins   for all using (true) with check (true);
```

### Create Storage Bucket
1. Go to **Storage** → **New bucket**
2. Name: `lc-images`
3. Toggle **Public** ON → Create
4. Go to bucket → **Policies** → add policy: allow all for INSERT and SELECT

### Get your keys
Go to **Project Settings → API**:
- Copy **Project URL**
- Copy **anon public** key

---

## ⚙️ Step 2: Configure index.html

Open `index.html` in VS Code and find these two lines:

```js
const SUPABASE_URL      = 'YOUR_SUPABASE_URL';
const SUPABASE_ANON_KEY = 'YOUR_SUPABASE_ANON_KEY';
```

Replace with your actual values.

---

## 🚀 Step 3: Deploy to Vercel

**Drag & drop method (easiest):**
1. Go to [vercel.com](https://vercel.com) → New Project
2. Drag the `our-pinmap` folder onto the deploy area
3. Click **Deploy** → done!

**GitHub method:**
1. Push folder to GitHub repo
2. Connect repo to Vercel → auto-deploys on every push

---

## 🌸 Features Guide

| Feature | How to use |
|---------|-----------|
| **Love Coordinates** | Click 📍 → tap map → fill form with name, date, photos |
| **SweetSpots** | Click 💗 → tap map → enter title |
| **Snuggle Spots** | Click 🧸 → tap map → add song info |
| **Love Trails** | Click ⭐ → tap multiple points → click Done |
| **Journey Map** | Click 🗺️ Journey button — connects pins by date |
| **Memory Garden** | Click 🌿 Garden button — shows pink clusters (needs 7+ pins) |
| **PinDom** | Click 📋 — sidebar list of all pins with filters |
| **PinBoard** | Click 🖼️ — Pinterest-style photo gallery |

### Love Trail tips
- Star icon on map toggles the trail line on/off when clicked
- Need at least 2 points to save a trail

### Snuggle Spot song image
- Right-click a Spotify album art → "Copy image address"
- Paste that URL in the "Spotify Cover Image URL" field

---

## 💰 Cost: $0
- Supabase free tier: 500MB DB, 1GB storage, 50k realtime messages/month
- Vercel free tier: unlimited static deploys
- OpenStreetMap/CARTO tiles: completely free
- No API keys needed for the map
