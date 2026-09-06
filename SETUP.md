# Local Setup Notes (Berlin Techs / QloApps)

Notes for this specific local install, running on XAMPP (Windows).

## How to start it

1. Start MariaDB:
   `C:\xampp\mysql\bin\mysqld.exe --defaults-file="C:\xampp\mysql\bin\my.ini" --standalone`
2. Start Apache:
   `C:\xampp\apache\bin\httpd.exe -d "C:\xampp\apache"`
   (or use XAMPP Control Panel's Start buttons for MySQL/Apache — both pick up the config below automatically)

## URLs

- Storefront: http://localhost:8000/
- Admin panel: http://localhost:8000/admin876xhsqhd/

The site is served on port **8000** (not 80) because the database's `qlo_shop_url` record has the shop domain hardcoded as `localhost:8000`. Apache config was extended (not replaced) to add this:
- `C:\xampp\apache\conf\httpd.conf` — added `Listen 8000`
- `C:\xampp\apache\conf\extra\httpd-vhosts.conf` — added a `<VirtualHost *:8000>` pointing at this project folder

## Database

- Host: `127.0.0.1`, Port: `3307`, User: `root`, Password: *(none)*
- Database: `qloapps`, table prefix: `qlo_`
- Config file: `config/settings.inc.php`

## Admin login

- Email: `admin@gmail.com`
- Password: `admin123`

## What's been changed from stock QloApps in this instance

- **Rebranding**: visible "QloApps" text/branding replaced with "Berlin Techs" across the storefront, admin panel, module display names, and docs. Left untouched on purpose: code identifiers/constants (e.g. `_QLOAPPS_VERSION_`, `qloapps_versions_compliancy`), the `qlo_` DB table prefix, `composer.json`, and `qloapps.com` URLs/emails.
- **Logo**: swapped everywhere (storefront header, favicon, admin login/header, email header, PDF invoices, store-locator icon) — generated from `img/berlin_logo.webp`. Canonical source assets kept at `img/berlin_logo_full.png` (full lockup) and `img/berlin_logo_icon.png` (icon mark only).
- **Admin dashboard ad banner removed**: PrestaShop's built-in "recommendation" system (fetches a promo banner from a remote QloApps API) was disabled by clearing `RECOMMENDATION_CONTENT_FILE_PATH` in `controllers/admin/AdminDashboardController.php` and `controllers/admin/AdminModulesCatalogController.php`.

## Known caveat

`composer.json` declares PHP `>=5.4 <8.0`, but this install runs on PHP 8.1.25 (XAMPP's bundled version). Pages tested so far load fine, but this is PrestaShop-1.6-era code — some less-traveled paths may hit PHP 8 incompatibilities. Report the exact page/action if something breaks.
