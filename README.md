# Build a Celestial Bodies Database

## Overview
This repository contains the PostgreSQL database dump for freeCodeCamp's 'Build a Celestial Bodies Database' project. The project aims to educate users on relational databases by creating a structured database that represents various celestial bodies.

## What's Included
- **universe.sql**: The SQL dump file containing the database schema and data.

## Requirements
- PostgreSQL psql

## How to Restore the Database
1. Create a new database with the following command:
   ```
   createdb celestial_bodies
   ```
2. Import the database dump using:
   ```
   psql celestial_bodies < universe.sql
   ```

## How to Explore
After restoring the database, you can utilize the following sample commands in psql:
- List all tables:
  ```
  \	  
  ```
- Run example queries:
  ```
  SELECT * FROM celestial_bodies;
  SELECT COUNT(*) FROM celestial_bodies;
  ```

## Project Context
This project was built following freeCodeCamp's Relational Database curriculum and was implemented using Codespaces.

## License
The database dump and README are provided for educational purposes. If you plan to use this project commercially or share it publicly, please add an appropriate license.