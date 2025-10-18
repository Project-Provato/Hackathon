# PROVATO Data 

To dataset περιλαμβάνει οντότητες (φάρμες, ζώα, συσκευές) και χρονοσειρές (μετρήσεις καιρού)

## Αρχεία
- **`farms.csv`** — *Φάρμες*  
  Κύριες στήλες: `id` (KEY), `name`, `coordinates` (συντεταγμένες`(lon,lat)`). Χρησιμοποιείται ως αναφορά για τοποθεσία ζώον και μετεωρολογικά δεδομένα.

- **`animals.csv`** — *Μητρώο ζώων*  
  Κύριες στήλες: `id` (KEY), `id_api` (unique), `name`, `birth`, `type`, `sex`, `breed`, `breed_short`, `farm_id`, `farm_id_api`. Συνδέει κάθε ζώο με μία φάρμα τόσο με εσωτερικό (`farm_id`) .

- **`devices.csv`** — *Συσκευές/περιλαίμια*  
  Κύριες στήλες: `id` (KEY), `type` (π.χ. `GSM`, `Sigfox`), `id_animal` (→ `animals.id_api`). Συνδέει κάθε συσκευή σε συγκεκριμένο ζώο μέσω του API ID του ζώου.

- **`device_data.csv`** — *Τηλεμετρίες συσκευών (χρονοσειρά)*  
  Κύριες στήλες: `id` (ΚΕΥ), `id_api` (→ `devices.id_api`), `created` (timestamp), `acc_x/y/z`, `std_x/y/z`, `max_x/y/z`, `temperature`, `coordinates (lon,lat)`. Πακέτο από ακατέργαστες μετρήσεις επιτάχυνσης, παράθυρο στατιστικών (std/max), θερμοκρασία αισθητήρα και θέση.

- **`meteo_data.csv`** — *Μετεωρολογικές μετρήσεις πεδίου*  
Κύριες στήλες: `farm_id_api` (→ `farms.id_api`), `station_timedata` (χρόνος μέτρησης), `crawled` (χρόνος συλλογής), `station_city/nomos/longitude/latitude`, `temperature`, `humidity`, `wind`, `direction`, `yetos` (mm), `barometer` (hPa), `dew_point`, `heat_index`, `wind_chill`, `solar_radiation` (W/m²).

## Σχέσεις (Foreign Keys)
- `animals.farm_id` → `farms.id`  
- `animals.farm_id_api` → `farms.id_api`  
- `devices.id_animal` → `animals.id_api`  
- `device_data.id_api` → `devices.id_api`  
- `meteo_data.farm_id_api` → `farms.id_api`

## Συμβάσεις & Μονάδες
- **Χρόνος:** `created`/`station_timedata` είναι τοπικά timestamps (π.χ. `YYYY-MM-DD hh:mm:ss`)  
- **Μονάδες:** θερμοκρασία °C, υγρασία %, πίεση hPa, υετός mm, άνεμος (ταχύτητα/διεύθυνση), επιταχύνσεις σε raw bits (αισθητήρας).
Για τη μετατροπή των τιμών της επιτάχυνσης σε μονάδες $g$ χρησιμοποιείστε την παρακάτω εξίσωση:
$$ -2 + \frac{acc + 32768}{65535)}\times 4$$
## Αρχικό Colab
[Google Colab](https://colab.research.google.com/drive/1PyR5J0RFNKlmaX3egSx1W-8k0VS315vC?usp=sharing)
