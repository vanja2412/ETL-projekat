# ETL-projekat
Projekat iz 'ETL i skaldista podataka'; 3. godina na FON-u, modul: informacioni inzenjering

# Olist ETL & Data Warehouse Project

Repozitorijum sadrži OLTP podatke (Olist e-commerce dataset), ETL proces koji puni Data Warehouse, i Power BI izveštaj nad DW modelom.

## Struktura repozitorijuma

```
├── olist_customers_dataset.csv       → OLTP podaci: kupci
├── olist_orders_dataset.csv          → OLTP podaci: porudžbine
├── olist_order_items_dataset.csv     → OLTP podaci: stavke porudžbina
├── olist_order_reviews_dataset.csv   → OLTP podaci: recenzije porudžbina
├── olist_products_dataset.csv        → OLTP podaci: proizvodi
├── olist_sellers_dataset.csv         → OLTP podaci: prodavci
├── DW.sql                            → Skripta za kreiranje strukture Data Warehouse-a (šema, bez podataka)
├── ETL_proces/                       → ETL proces koji puni DW iz OLTP podataka
└── IzvestajniProgram.pbix            → Power BI izveštaj nad DW modelom
```

## Kako rekonstruisati projekat od nule

### 1. OLTP podaci
CSV fajlovi u root folderu predstavljaju izvorne OLTP tabele. Da bi ih vratili u bazu:
1. Napravi novu praznu bazu u SSMS-u (npr. `OLTP`)
2. Uvezi svaki CSV preko **Import Flat File** (desni klik na bazu → Tasks → Import Flat File)

### 2. Data Warehouse
1. Napravi novu praznu bazu u SSMS-u (npr. `DW`)
2. Pokreni `DW.sql` — kreira strukturu (dimenzije i fact tabele), bez podataka
3. Pokreni ETL proces iz `ETL_proces/` da napuniš DW tabele iz OLTP baze

### 3. ETL proces
Sadržaj foldera `ETL_proces/` predstavlja transformacije koje povezuju OLTP i DW bazu.
>[SSIS paket]

### 4. Power BI izveštaj
`IzvestajniProgram.pbix` je Power BI izveštaj konstruisan nad DW modelom.
1. Otvori fajl u Power BI Desktop-u
2. Ažuriraj konekciju ka DW bazi (Home → Transform Data → Data Source Settings) da pokazuje na tvoju lokalnu instancu SQL Servera
3. Osveži podatke (Refresh)
