# Periodic Table Database Project

## Overview
This project consists of a PostgreSQL database for storing chemical element information and a Bash script for querying the database.

## Project Structure

### 1. **Database Schema (`periodic_table.sql`)**
A PostgreSQL database dump containing three main tables with element data.

#### Tables:
- **`elements`**: Stores basic element information
  - `atomic_number` (integer, PRIMARY KEY)
  - `symbol` (varchar(2), UNIQUE)
  - `name` (varchar(40), UNIQUE)

- **`properties`**: Stores physical properties of elements
  - `atomic_number` (integer, PRIMARY KEY, FOREIGN KEY to elements)
  - `atomic_mass` (numeric)
  - `melting_point_celsius` (numeric)
  - `boiling_point_celsius` (numeric)
  - `type_id` (integer, FOREIGN KEY to types)

- **`types`**: Categorizes elements by type
  - `type_id` (integer, PRIMARY KEY)
  - `type` (varchar(20))

#### Sample Data Included:
- 10 chemical elements with their properties
- 3 element types: `nonmetal`, `metal`, `metalloid`

### 2. **Bash Script (`element.sh`)**
A command-line interface for querying element information from the database.

#### Features:
- Query elements by atomic number or name
- Case-insensitive partial name matching
- Formatted output with all element properties
- Error handling for invalid queries

#### Usage:
```bash
# Query by atomic number
./element.sh 1
# Output: The element with atomic number 1 is Hydrogen (H). It's a nonmetal, with a mass of 1.008 amu. Hydrogen has a melting point of -259.1 celsius and a boiling point of -252.9 celsius.

# Query by element name (full or partial)
./element.sh Hydrogen
./element.sh hydro  # partial match
./element.sh H      # symbol match

# Query by symbol
./element.sh He
```

#### Dependencies:
- PostgreSQL client (`psql`)
- Database connection credentials:
  - Username: `freecodecamp`
  - Database: `periodic_table`

## Installation & Setup

### 1. Restore the Database:
```bash
# Import the database dump
psql -U postgres < periodic_table.sql
```

### 2. Make the Script Executable:
```bash
chmod +x element.sh
```

### 3. Test the Script:
```bash
./element.sh 1
./element.sh Helium
./element.sh Lithium
```

## Database Schema Diagram
```
elements (1) ----- (1) properties (N) ----- (1) types
    |                   |                       |
atomic_number      atomic_number           type_id
    |                   |                       |
  PK                 PK/FK                   PK
```

## Notes
- The script connects to a local PostgreSQL database
- Ensure PostgreSQL service is running before using the script
- The database user `freecodecamp` must have appropriate permissions
- For production use, consider adding connection pooling and security enhancements

## Error Handling
The script handles:
- Missing arguments (prompts user)
- Invalid atomic numbers (non-numeric input)
- Non-existent elements (shows error message)
- Database connection errors (if psql fails)

## Example Output
```
$ ./element.sh 6
The element with atomic number 6 is Carbon (C). It's a nonmetal, with a mass of 12 amu. Carbon has a melting point of 3550 celsius and a boiling point of 4027 celsius.

$ ./element.sh Silicon
I could not find that element in the database.
```

## Potential Improvements
1. Add more elements to the database
2. Implement update/insert functionality
3. Add unit tests
4. Create a web interface
5. Add chemical property calculations
