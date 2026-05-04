# NTUA SoftEng 2024-2025 Project

Repository για την εργασία του μαθήματος **Τεχνολογία Λογισμικού** της σχολής ΗΜΜΥ ΕΜΠ, χειμερινό εξάμηνο 2024-2025.

Το project αφορά την ανάπτυξη ενός συστήματος για τη διαχείριση της διαλειτουργικότητας διοδίων. Πιο συγκεκριμένα, το σύστημα διαχειρίζεται διελεύσεις οχημάτων από σταθμούς διοδίων διαφορετικών operators και υπολογίζει τις αντίστοιχες χρεώσεις μεταξύ τους.

## Περιγραφή

Στο πλαίσιο της εργασίας θεωρούμε ότι υπάρχουν πολλοί operators αυτοκινητοδρόμων, οι οποίοι διαθέτουν δικούς τους σταθμούς διοδίων και δικά τους ηλεκτρονικά tags. Παρόλα αυτά, ένα όχημα μπορεί να περάσει από σταθμό διοδίων άλλου operator χρησιμοποιώντας το tag του δικού του παρόχου.

Σε αυτή την περίπτωση δημιουργείται μια οφειλή μεταξύ των δύο operators. Για παράδειγμα, αν ένα όχημα με tag του operator Α περάσει από σταθμό του operator Β, τότε ο operator Α οφείλει στον operator Β το κόστος της διέλευσης.

Η εφαρμογή που υλοποιήθηκε αποθηκεύει τα δεδομένα των διελεύσεων και παρέχει τρόπους για την ανάκτησή τους, καθώς και για τον υπολογισμό των χρεώσεων.

## Τι περιλαμβάνει το project

Το project αποτελείται από τα εξής βασικά μέρη:

- Backend εφαρμογή με REST API
- MySQL βάση δεδομένων
- CLI client για κλήσεις από γραμμή εντολών
- Frontend web εφαρμογή
- OpenAPI documentation
- Τεκμηρίωση της εργασίας
- AI log με την καταγραφή χρήσης εργαλείων AI

## Δομή του repository

```text
.
├── Database_mysql/      # Αρχεία για τη βάση δεδομένων
├── back-end/            # Backend και REST API
├── cli-client/          # Command line client
├── documentation/       # SRS, UML και υπόλοιπη τεκμηρίωση
├── front-end/           # Frontend εφαρμογή
├── ai-log/              # Καταγραφή χρήσης AI εργαλείων
├── openapi.json         # OpenAPI specification
├── LICENSE
└── README.md
```

## Τεχνολογίες που χρησιμοποιήθηκαν

Για την υλοποίηση χρησιμοποιήθηκαν:

- **Node.js / Express** για το backend
- **MySQL** για τη βάση δεδομένων
- **Python** για το CLI client
- **HTML, CSS, JavaScript** για το frontend
- **OpenAPI 3.0** για την τεκμηρίωση του API
- **Git / GitHub** για version control

## Βάση δεδομένων

Τα αρχεία που σχετίζονται με τη βάση βρίσκονται στον φάκελο:

```text
Database_mysql/
```

Η βάση αποθηκεύει δεδομένα για:

- σταθμούς διοδίων
- operators
- tags
- διελεύσεις
- χρεώσεις
- χρήστες, εφόσον χρησιμοποιείται authentication

Για τη δημιουργία της βάσης μπορεί να χρησιμοποιηθεί η MySQL.

Παράδειγμα δημιουργίας βάσης:

```sql
CREATE DATABASE tolls_db;
USE tolls_db;
```

Στη συνέχεια μπορούν να φορτωθούν τα SQL αρχεία από τον φάκελο `Database_mysql`.

Ενδεικτικά:

```bash
mysql -u root -p tolls_db < Database_mysql/schema.sql
mysql -u root -p tolls_db < Database_mysql/dump.sql
```

Αν τα αρχεία έχουν διαφορετικά ονόματα, πρέπει να χρησιμοποιηθούν τα αντίστοιχα ονόματα που υπάρχουν στον φάκελο.

## Backend

Το backend βρίσκεται στον φάκελο:

```text
back-end/
```

Εκεί υλοποιείται το REST API της εφαρμογής. Το API χρησιμοποιείται τόσο από το frontend όσο και από το CLI.

Το backend τρέχει στο port:

```text
9115
```

Base URL:

```text
http://localhost:9115/api
```

Αν χρησιμοποιείται HTTPS:

```text
https://localhost:9115/api
```

Για εγκατάσταση των dependencies:

```bash
cd back-end
npm install
```

Για εκκίνηση:

```bash
npm start
```

ή, αν υπάρχει dev script:

```bash
npm run dev
```

Ενδεικτικό `.env` αρχείο:

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=tolls_db
DB_PORT=3306
PORT=9115
```

## REST API

Το API επιστρέφει δεδομένα σε δύο μορφές:

- JSON
- CSV

Αν δεν δοθεί format, χρησιμοποιείται default το JSON.

Παράδειγμα για JSON:

```bash
curl "http://localhost:9115/api/tollStationPasses/NAO01/20241101/20241130?format=json"
```

Παράδειγμα για CSV:

```bash
curl "http://localhost:9115/api/tollStationPasses/NAO01/20241101/20241130?format=csv"
```

Οι ημερομηνίες δίνονται στη μορφή:

```text
YYYYMMDD
```

Παράδειγμα:

```text
20241101
```

## Authentication

Το σύστημα περιλαμβάνει login και logout με χρήση token.

Το token στέλνεται στις προστατευμένες κλήσεις μέσω του header:

```text
X-OBSERVATORY-AUTH
```

### Login

```http
POST /api/login
```

Παράδειγμα:

```bash
curl -X POST "http://localhost:9115/api/login" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "username=admin&password=freepasses4all"
```

Ενδεικτική απάντηση:

```json
{
  "token": "example_token"
}
```

### Logout

```http
POST /api/logout
```

Παράδειγμα:

```bash
curl -X POST "http://localhost:9115/api/logout" \
  -H "X-OBSERVATORY-AUTH: example_token"
```

## Admin endpoints

### Healthcheck

```http
GET /api/admin/healthcheck
```

Ελέγχει αν το backend μπορεί να συνδεθεί σωστά με τη βάση δεδομένων.

Παράδειγμα:

```bash
curl "http://localhost:9115/api/admin/healthcheck"
```

Ενδεικτική απάντηση:

```json
{
  "status": "OK",
  "dbconnection": "mysql://localhost:3306/tolls_db",
  "n_stations": 100,
  "n_tags": 500,
  "n_passes": 10000
}
```

### Reset stations

```http
POST /api/admin/resetstations
```

Αρχικοποιεί τους σταθμούς διοδίων.

```bash
curl -X POST "http://localhost:9115/api/admin/resetstations"
```

### Reset passes

```http
POST /api/admin/resetpasses
```

Διαγράφει τις διελεύσεις και επαναφέρει τα σχετικά δεδομένα.

```bash
curl -X POST "http://localhost:9115/api/admin/resetpasses"
```

### Add passes

```http
POST /api/admin/addpasses
```

Προσθέτει νέες διελεύσεις από CSV αρχείο.

```bash
curl -X POST "http://localhost:9115/api/admin/addpasses" \
  -F "file=@passes.csv"
```

## Βασικά functional endpoints

### Διελεύσεις ανά σταθμό

```http
GET /api/tollStationPasses/:tollStationID/:date_from/:date_to
```

Επιστρέφει τις διελεύσεις που έγιναν από έναν συγκεκριμένο σταθμό σε ένα χρονικό διάστημα.

Παράδειγμα:

```bash
curl "http://localhost:9115/api/tollStationPasses/NAO01/20241101/20241130?format=json"
```

### Ανάλυση διελεύσεων μεταξύ operators

```http
GET /api/passAnalysis/:stationOpID/:tagOpID/:date_from/:date_to
```

Επιστρέφει τις διελεύσεις που έγιναν σε σταθμούς ενός operator από οχήματα με tag άλλου operator.

Παράδειγμα:

```bash
curl "http://localhost:9115/api/passAnalysis/NAO/AM/20241101/20241130?format=json"
```

### Κόστος διελεύσεων μεταξύ operators

```http
GET /api/passesCost/:tollOpID/:tagOpID/:date_from/:date_to
```

Υπολογίζει πόσα περάσματα έγιναν και ποιο είναι το συνολικό κόστος που οφείλει ένας tag operator σε έναν toll operator.

Παράδειγμα:

```bash
curl "http://localhost:9115/api/passesCost/NAO/AM/20241101/20241130?format=json"
```

### Χρεώσεις προς έναν operator

```http
GET /api/chargesBy/:tollOpID/:date_from/:date_to
```

Επιστρέφει συγκεντρωτικά τις χρεώσεις όλων των άλλων operators προς έναν συγκεκριμένο operator.

Παράδειγμα:

```bash
curl "http://localhost:9115/api/chargesBy/NAO/20241101/20241130?format=json"
```

## Status codes

Το API χρησιμοποιεί τα παρακάτω HTTP status codes:

| Status code | Περιγραφή |
|---|---|
| 200 | Επιτυχής κλήση |
| 204 | Επιτυχής κλήση χωρίς δεδομένα |
| 400 | Λάθος παράμετροι |
| 401 | Μη εξουσιοδοτημένη πρόσβαση |
| 500 | Εσωτερικό σφάλμα server |

## CLI client

Το CLI βρίσκεται στον φάκελο:

```text
cli-client/
```

Το CLI λειτουργεί ως client του REST API και δίνει τη δυνατότητα εκτέλεσης των βασικών λειτουργιών από terminal.

Η γενική μορφή μιας εντολής είναι:

```bash
se24XX scope --param1 value1 --param2 value2 --format json
```

Το `XX` πρέπει να αντικατασταθεί με τον αριθμό της ομάδας.

Αν δεν δοθεί `--format`, το default format είναι `csv`.

### Παραδείγματα CLI εντολών

Healthcheck:

```bash
se24XX healthcheck
```

Reset passes:

```bash
se24XX resetpasses
```

Reset stations:

```bash
se24XX resetstations
```

Login:

```bash
se24XX login --username admin --passw freepasses4all
```

Logout:

```bash
se24XX logout
```

Διελεύσεις ανά σταθμό:

```bash
se24XX tollstationpasses --station NAO01 --from 20241101 --to 20241130 --format json
```

Ανάλυση διελεύσεων:

```bash
se24XX passanalysis --stationop NAO --tagop AM --from 20241101 --to 20241130 --format json
```

Κόστος διελεύσεων:

```bash
se24XX passescost --stationop NAO --tagop AM --from 20241101 --to 20241130 --format csv
```

Χρεώσεις ανά operator:

```bash
se24XX chargesby --opid NAO --from 20241101 --to 20241130 --format csv
```

Εισαγωγή διελεύσεων από CSV:

```bash
se24XX admin --addpasses --source ./newpasses.csv
```

Δημιουργία ή ενημέρωση χρήστη:

```bash
se24XX admin --usermod --username user1 --passw newpassword
```

Προβολή χρηστών:

```bash
se24XX admin --users
```

## Frontend

Το frontend βρίσκεται στον φάκελο:

```text
front-end/
```

Η frontend εφαρμογή παρέχει ένα απλό γραφικό περιβάλλον για τη χρήση βασικών λειτουργιών του συστήματος.

Μέσα από το frontend μπορούν να εμφανίζονται ενδεικτικά:

- διελεύσεις ανά σταθμό
- ανάλυση μεταξύ operators
- κόστος διελεύσεων
- συγκεντρωτικές χρεώσεις
- πίνακες αποτελεσμάτων

Για εκτέλεση:

```bash
cd front-end
npm install
npm start
```

Αν η εφαρμογή είναι static:

```bash
cd front-end
npx serve .
```

ή ανοίγουμε το `index.html` στον browser.

## API documentation

Η τεκμηρίωση του API υπάρχει στο αρχείο:

```text
openapi.json
```

Το αρχείο μπορεί να ανοιχτεί με:

- Swagger Editor
- Swagger UI
- Postman

Με αυτόν τον τρόπο μπορούν να ελεγχθούν τα διαθέσιμα endpoints, τα parameters και τα response schemas.

## Documentation

Ο φάκελος:

```text
documentation/
```

περιέχει την τεκμηρίωση της εργασίας.

Ενδεικτικά περιλαμβάνει:

- SRS
- UML diagrams
- data design diagrams
- activity diagrams
- state diagrams
- class diagrams
- component diagrams
- deployment diagrams
- περιγραφή αρχιτεκτονικής

## Testing

Για τον έλεγχο του API μπορούν να χρησιμοποιηθούν `curl`, Postman ή automated tests.

Παραδείγματα:

```bash
curl "http://localhost:9115/api/admin/healthcheck"
```

```bash
curl "http://localhost:9115/api/tollStationPasses/NAO01/20241101/20241130?format=json"
```

```bash
curl "http://localhost:9115/api/passAnalysis/NAO/AM/20241101/20241130?format=json"
```

```bash
curl "http://localhost:9115/api/passesCost/NAO/AM/20241101/20241130?format=json"
```

```bash
curl "http://localhost:9115/api/chargesBy/NAO/20241101/20241130?format=json"
```

Αν υπάρχουν tests στο backend:

```bash
cd back-end
npm test
```

Αν υπάρχουν tests για το CLI:

```bash
cd cli-client
pytest
```

## AI log

Στον φάκελο:

```text
ai-log/
```

υπάρχει η καταγραφή της χρήσης εργαλείων AI κατά την ανάπτυξη της εργασίας.

Η καταγραφή αφορά κυρίως βοήθεια σε:

- κατανόηση απαιτήσεων
- συγγραφή τεκμηρίωσης
- σχεδιασμό API
- debugging
- βελτίωση κώδικα
- δημιουργία παραδειγμάτων χρήσης

## Εγκατάσταση και εκτέλεση συνολικά

Μια τυπική σειρά εκτέλεσης είναι η εξής:

### 1. Clone του repository

```bash
git clone https://github.com/V99Anastasiadis/V99Anastasiadis-ntua-softeng2024-project.git
cd V99Anastasiadis-ntua-softeng2024-project
```

### 2. Δημιουργία και γέμισμα βάσης

```bash
mysql -u root -p
```

```sql
CREATE DATABASE tolls_db;
USE tolls_db;
```

```bash
mysql -u root -p tolls_db < Database_mysql/schema.sql
mysql -u root -p tolls_db < Database_mysql/dump.sql
```

### 3. Εκκίνηση backend

```bash
cd back-end
npm install
npm start
```

### 4. Έλεγχος API

```bash
curl "http://localhost:9115/api/admin/healthcheck"
```

### 5. Εκκίνηση frontend

```bash
cd front-end
npm install
npm start
```

### 6. Χρήση CLI

```bash
cd cli-client
pip install -r requirements.txt
```

Παράδειγμα:

```bash
se24XX healthcheck
```

## Πιθανά προβλήματα

### Δεν συνδέεται το backend με τη βάση

Ελέγχουμε ότι:

- η MySQL τρέχει
- το όνομα της βάσης είναι σωστό
- τα credentials είναι σωστά
- έχει φορτωθεί το schema
- το `.env` έχει σωστές τιμές

### Το API επιστρέφει 400

Συνήθως σημαίνει ότι κάποια παράμετρος είναι λάθος. Ελέγχουμε:

- μορφή ημερομηνιών `YYYYMMDD`
- σωστά station IDs
- σωστά operator IDs
- σωστό `format`, δηλαδή `json` ή `csv`

### Το API επιστρέφει 401

Ελέγχουμε ότι:

- έχει γίνει login
- το token είναι σωστό
- το header `X-OBSERVATORY-AUTH` έχει σταλεί σωστά

### Δεν δουλεύει το CLI

Ελέγχουμε ότι:

- το backend είναι ανοιχτό
- το base URL είναι σωστό
- έχουν εγκατασταθεί τα dependencies
- η εντολή έχει τις σωστές παραμέτρους

### Δεν εμφανίζει δεδομένα το frontend

Ελέγχουμε ότι:

- το backend τρέχει
- υπάρχει σύνδεση με τη βάση
- υπάρχουν δεδομένα στη βάση
- το frontend καλεί το σωστό API URL

## Παραδοτέα

Το repository περιλαμβάνει τα βασικά παραδοτέα της εργασίας:

### Τεκμηρίωση

- SRS
- UML diagrams
- data design diagrams
- activity/state diagrams
- class/API diagrams
- component/deployment diagrams

### Υλοποίηση

- backend
- REST API
- database
- CLI client
- frontend
- OpenAPI documentation

### Testing

- API tests
- CLI tests
- manual frontend testing

### Εργαλεία

- GitHub repository
- GitHub project management
- AI log

## Ενδεικτική ροή χρήσης

```bash
se24XX healthcheck
```

```bash
se24XX admin --addpasses --source ./passes.csv
```

```bash
se24XX tollstationpasses --station NAO01 --from 20241101 --to 20241130 --format json
```

```bash
se24XX passanalysis --stationop NAO --tagop AM --from 20241101 --to 20241130 --format json
```

```bash
se24XX passescost --stationop NAO --tagop AM --from 20241101 --to 20241130 --format csv
```

```bash
se24XX chargesby --opid NAO --from 20241101 --to 20241130 --format csv
```

## Συνεισφέρων

- Βασίλης Αναστασιάδης

## Άδεια

Το project διατίθεται υπό την άδεια MIT. Περισσότερες πληροφορίες υπάρχουν στο αρχείο:

```text
LICENSE
```
