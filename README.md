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
CSV fajlovi u root folderu predstavljaju izvorne OLTP tabele. Da bi ih vratila u bazu:
1. Napravi novu praznu bazu u SSMS-u (npr. `OLTP`)
2. Kreiraj tabele koje odgovaraju CSV fajlovima (kolone/tipovi prema sadržaju CSV-a)
3. Uvezi svaki CSV preko **Import Flat File** (desni klik na bazu → Tasks → Import Flat File) u odgovarajuću tabelu

Redosled uvoza (zbog FK zavisnosti):
1. `olist_customers_dataset.csv`
2. `olist_sellers_dataset.csv`
3. `olist_products_dataset.csv`
4. `olist_orders_dataset.csv`
5. `olist_order_items_dataset.csv`
6. `olist_order_reviews_dataset.csv`

### 2. Data Warehouse
1. Napravi novu praznu bazu u SSMS-u (npr. `DW`)
2. Pokreni `DW.sql` — kreira strukturu (dimenzije i fact tabele), bez podataka
3. Pokreni ETL proces iz `ETL_proces/` da napuniš DW tabele iz OLTP baze

### 3. ETL proces
Sadržaj foldera `ETL_proces/` predstavlja transformacije koje povezuju OLTP i DW bazu.
> [Ovde dopuni: da li je u pitanju SSIS paket, SQL skripte, Python skripta? Ako ima konekcione stringove ka bazama, navesti da ih treba izmeniti pri pokretanju na novom računaru.]

### 4. Power BI izveštaj
`IzvestajniProgram.pbix` je Power BI izveštaj konstruisan nad DW modelom.
1. Otvori fajl u Power BI Desktop-u
2. Ažuriraj konekciju ka DW bazi (Home → Transform Data → Data Source Settings) da pokazuje na tvoju lokalnu instancu SQL Servera
3. Osveži podatke (Refresh)

## Napomena
- OLTP podaci su čuvani kao CSV (a ne kao `.bak` fajl) zbog ograničenja veličine fajlova na GitHub-u.
- DW baza je čuvana samo kao šema (bez podataka) jer se podaci generišu preko ETL procesa iz OLTP baze.
