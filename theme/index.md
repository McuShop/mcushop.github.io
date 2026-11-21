# McuShop Theme for Bagisto 2.5 – Installation & User Guide

**[Buy the McuShop Theme – $59](https://1164974116393.gumroad.com/l/bmfisj)**

One theme, five complete store designs: **fashion, electronics, cosmetics, home & living and grocery**. Switch between them from the admin panel in one click. Every design has a light and a dark mode.

- Theme version: 1.6.1
- Bagisto: 2.5.x
- PHP: whatever your Bagisto 2.5 install needs (the theme code itself runs on PHP 8.2+)
- Languages included: English and Turkish. Other locales fall back to English and can be translated by copying `src/Resources/lang/en/app.php`.

## What is in the package

| Folder | Contents |
|---|---|
| `packages/McuShop/Theme` | Theme source code (no demo products; you add your own) |
| `packages/McuShop/Core` | The shared "McuShop" admin menu used by the theme |
| `public/themes/shop/mcushop` | Pre-built CSS/JS, so you do **not** need npm on the server |

## 1. Upload the files

Take a database and file backup first. Unzip the package into the **root folder of your Bagisto install**. The folders merge with the existing ones; no Bagisto core file is overwritten.

## 2. Register the package

In `composer.json`, inside `autoload > psr-4`, add:

```json
"McuShop\\Core\\": "packages/McuShop/Core/src",
"McuShop\\Theme\\": "packages/McuShop/Theme/src",
```

In `bootstrap/providers.php`, add to the list:

```php
McuShop\Core\Providers\CoreServiceProvider::class,
McuShop\Theme\Providers\ThemeServiceProvider::class,
```

## 3. Run the commands (in the Bagisto root folder)

```bash
composer dump-autoload
php artisan migrate
php artisan storage:link
php artisan mcushop:install moda
php artisan optimize:clear
php artisan config:cache
```

The last argument of `mcushop:install` picks the starting design:

| Argument | Design |
|---|---|
| `moda` | Fashion |
| `elektronik` | Electronics |
| `kozmetik` | Cosmetics |
| `ev` | Home & living |
| `market` | Grocery |

The installer places 35 original starter images (banners, category tiles) in the home page sections. You can change the design later at **Admin › Configure › McuShop › Appearance**. When you switch, only empty sections are filled; images you uploaded yourself are kept.

## 4. Settings you may want to change

All theme settings live under **Admin › McuShop** and **Admin › Configure › McuShop**:

- **Appearance:** sector design, default light/dark mode, dark-mode logo.
- **Product cards:** rotating card labels (e.g. "Free shipping", "Installments"), sold-count line, discount badge. Label texts are editable.
- **Cart:** free-shipping progress bar threshold in the mini cart.
- **Header & general:** announcement bar, sticky header, support contacts, trust badges, back-to-top button.
- **Turkish legal forms:** the Distance Sales Agreement and Pre-information Form are required by Turkish law. Stores outside Turkey can **switch them off** under Admin › Configure › McuShop › Checkout. The templates are examples; have them checked by a lawyer.
- **Home page sections:** Admin › Appearance › Themes, as in standard Bagisto.

## 5. Going live

- In `.env`, set `APP_URL` to your real domain and keep `APP_DEBUG=false`.
- Rebuild the image cache after a big product import: `php artisan mcushop:images --fresh` (new products are processed automatically).

### Speed (Lighthouse)

The theme serves phone-sized WebP images, preloads the first product images and prevents layout shift. The rest depends on the server: enable compression and browser caching. For Apache, add to the end of `public/.htaccess`:

```apache
<IfModule mod_deflate.c>
    AddOutputFilterByType DEFLATE text/html text/plain text/css text/xml application/javascript application/json image/svg+xml
</IfModule>
<IfModule mod_expires.c>
    ExpiresActive On
    ExpiresByType text/css "access plus 1 year"
    ExpiresByType application/javascript "access plus 1 year"
    ExpiresByType image/webp "access plus 1 year"
    ExpiresByType image/jpeg "access plus 1 year"
    ExpiresByType image/png "access plus 1 year"
    ExpiresByType font/woff2 "access plus 1 year"
</IfModule>
```

For Nginx: `gzip on;` and `expires 1y;` for `/themes/` and `/storage/`.

## 6. Updating

Unzip the new version over the old one, then run `php artisan migrate`, `php artisan optimize:clear` and `php artisan config:cache`. Your settings and uploaded images are kept.

## 7. Translating to another language

Copy `packages/McuShop/Theme/src/Resources/lang/en` to a folder named after your locale code (for example `de`) and translate the values. Bagisto picks it up for that channel locale.

## License

One license = one store on one domain (plus its test copies). See `LICENSE.txt` in the package.

## Support

Contact the developer through the support link on the Bagisto listing.

**[Buy the McuShop Theme](https://1164974116393.gumroad.com/l/bmfisj)**
