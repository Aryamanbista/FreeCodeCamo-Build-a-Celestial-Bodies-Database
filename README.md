# Celestial Bodies Database (PostgreSQL)

## Overview
This repository contains a PostgreSQL database dump (**`universe.sql`**) created for freeCodeCamp’s Relational Database curriculum project: **Build a Celestial Bodies Database**.

The database models a small universe of:
- **Galaxies → Stars → Planets → Moons** (via foreign keys)
- **Comets** (standalone entity)

## Files
- `universe.sql` — schema + sample data dump of the `universe` database

## Requirements
- PostgreSQL 12+ (`universe.sql` was generated from PostgreSQL **12.22**)
- `psql` (PostgreSQL CLI)

## Restore / Import
### Recommended (create DB explicitly)
```bash
createdb universe
psql universe < universe.sql
```

### Alternative (run dump directly)
> Note: `universe.sql` includes `DROP DATABASE universe;` and `CREATE DATABASE universe;`.
```bash
psql -f universe.sql
```

## Schema
### Tables
#### `galaxy`
- `galaxy_id` (PK)
- `name` (unique, required)
- `galaxy_type` (required)
- `age_in_millions_of_years` (required)
- `distance_from_earth` (numeric(12,2), optional)
- `has_life` (required)

#### `star`
- `star_id` (PK)
- `name` (unique, required)
- `galaxy_id` (FK → `galaxy.galaxy_id`, required)
- `temperature` (required)
- `mass` (numeric(10,2), optional)
- `is_spherical` (required)

#### `planet`
- `planet_id` (PK)
- `name` (unique, required)
- `star_id` (FK → `star.star_id`, required)
- `planet_type` (required)
- `age_in_millions_of_years` (optional)
- `has_life` (required)

#### `moon`
- `moon_id` (PK)
- `name` (unique, required)
- `planet_id` (FK → `planet.planet_id`, required)
- `diameter_km` (required)
- `is_spherical` (required)
- `description` (text, optional)

#### `comet`
- `comet_id` (PK)
- `name` (unique, required)
- `speed_km_s` (required)
- `is_periodic` (required)

### Relationships
- `galaxy (1) → (many) star`
- `star (1) → (many) planet`
- `planet (1) → (many) moon`
- `comet` has no foreign keys

## Example Queries
### Galaxies and star counts
```sql
SELECT g.name AS galaxy, COUNT(s.star_id) AS star_count
FROM galaxy g
LEFT JOIN star s ON s.galaxy_id = g.galaxy_id
GROUP BY g.name
ORDER BY star_count DESC, galaxy;
```

### Planets with their star and galaxy
```sql
SELECT
  p.name  AS planet,
  s.name  AS star,
  g.name  AS galaxy,
  p.planet_type,
  p.has_life
FROM planet p
JOIN star s   ON p.star_id = s.star_id
JOIN galaxy g ON s.galaxy_id = g.galaxy_id
ORDER BY galaxy, star, planet;
```

### Moons per planet
```sql
SELECT p.name AS planet, COUNT(m.moon_id) AS moon_count
FROM planet p
LEFT JOIN moon m ON m.planet_id = p.planet_id
GROUP BY p.name
ORDER BY moon_count DESC, planet;
```

### Periodic comets
```sql
SELECT name, speed_km_s
FROM comet
WHERE is_periodic = true
ORDER BY speed_km_s DESC;
```

## Notes
- Each table enforces a unique `name` via `UNIQUE (name)` constraints.
- Primary keys are integer IDs backed by sequences.

## License
Intended for educational use. Add a license if you plan to redistribute or use this project beyond learning purposes.