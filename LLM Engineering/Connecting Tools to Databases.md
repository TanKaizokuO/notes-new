---
created: 2026-02-01
tags:
  - extension
---
---

Function calling becomes powerful when backed by persistent storage. We can replace hardcoded dictionaries with a SQL database.

## Setup: Persistent Price Database

```python
import sqlite3

DB = "prices.db"

def init_db():
    with sqlite3.connect(DB) as conn:
        cursor = conn.cursor()
        cursor.execute('CREATE TABLE IF NOT EXISTS prices (city TEXT PRIMARY KEY, price REAL)')
        conn.commit()

# Setter Tool (for admin or setup)
def set_ticket_price(city, price):
    with sqlite3.connect(DB) as conn:
        cursor = conn.cursor()
        # Upsert logic (Insert or Update if exists)
        cursor.execute('''INSERT INTO prices (city, price) VALUES (?, ?) 
                          ON CONFLICT(city) DO UPDATE SET price = ?''', 
                          (city.lower(), price, price))
        conn.commit()
````

## The Getter Tool (LLM Accessible)

This is the function the LLM will actually "call".

```python
def get_ticket_price(city):
    print(f"DATABASE TOOL CALLED: Getting price for {city}", flush=True)
    
    with sqlite3.connect(DB) as conn:
        cursor = conn.cursor()
        cursor.execute('SELECT price FROM prices WHERE city = ?', (city.lower(),))
        result = cursor.fetchone()
        
    if result:
        return f"Ticket price to {city} is ${result[0]}"
    else:
        return "No price data available for this city"
```

## Why this matters

- **Persistence**: Data survives restart.    
- **Scale**: Can handle thousands of cities.
- **Dynamic**: Prices can be updated by other systems in real-time, and the LLM will immediately "know" the new price.