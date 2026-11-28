# McuShop Theme for Bagisto 2.5 – Installation & User Guide

**[Buy the McuShop Theme – $59](https://1164974116393.gumroad.com/l/bmfisj)**

One theme, five complete store designs: **fashion, electronics, cosmetics, home & living and grocery**. Switch between them from the admin panel in one click. Every design has a light and a dark mode.

- Theme version: 1.7.0
- Bagisto: 2.5.x
- PHP: whatever your Bagisto 2.5 install needs (the theme code itself runs on PHP 8.2+)
- Languages included: all 23 Bagisto languages, including right-to-left Arabic, Persian and Hebrew.

## Features

**Storefront**
- Five complete sector designs (fashion, electronics, cosmetics, home & living, grocery). Switch with one setting in the admin panel; your store data is not touched.
- Light and dark mode for every design, chosen by the visitor or following the device setting.
- 35 original starter images (hero banners, category tiles, sign-in page), placed automatically in empty home page sections.
- Smart search: product and category suggestions while typing, highlighted matches and popular searches.
- Home page sections: hero, category tiles, product carousels, flash deals with countdown, promo tiles, brands, testimonials, FAQ, image + text and custom HTML.
- Quick view and recently viewed products.
- Language and currency picker with flags and currency symbols; only one list opens at a time.
- Image search: a camera button in the search box when Bagisto's Image Search setting is on.

**Product cards and product page**
- Clean price display with a small "-15%" discount badge.
- Rule-based card labels (free shipping, installments, gift...) by category and price range; low-stock notice.
- Brand, colour swatches, rating or category line, price and labels stay aligned on every card in a row.
- Variant products open a picker from "Add to cart" (bottom sheet on mobile).
- Full-screen gallery with swipe, pinch zoom and swipe-down to close; sticky add-to-cart bar on mobile.
- Compare: an active "Remove from compare" state on cards and a dismissible compare bar at the bottom of the page (can be switched off per sector).

**Cart, checkout and account**
- Three-step checkout (Address › Shipping › Payment) on one page.
- Cart, checkout, My Account, order and return pages designed for each sector.
- Free-shipping progress bar in the mini cart (threshold and text set in the admin panel).
- Redesigned sign-in, sign-up and password pages per sector.
- Optional Distance Sales Agreement and Pre-information Form (required in Turkey, can be switched off elsewhere).

**Quality**
- Responsive WebP images sized for the screen; phones never download desktop images.
- Lighthouse accessibility score 100; respects "reduce motion".
- 23 languages included; texts Bagisto already translates use Bagisto's own translation. When you add a language later, the theme's home page sections are created in that language automatically.
- One "McuShop" admin menu for all settings.

## What is included, and what is sold separately

The theme package contains **only the theme** (`packages/McuShop/Theme`) and the shared McuShop admin menu (`packages/McuShop/Core`).

The live demo also runs McuShop **add-ons that are sold separately** and are **not** part of the theme. In the demo they are marked with an "Add-on" badge:

| Add-on (sold separately) | What it adds |
|---|---|
| Campaign Showcases | Campaign sections, deal of the day, campaign pages and ribbons |
| Verified Buyer Reviews | Reviews only from real buyers, with a "Verified buyer" badge |
| Questions & Answers | Product questions and answers on the product page |
| Google Sign-In | Google One Tap and "Sign in with Google" |
| Turkey Address | Province / district / neighbourhood address fields |
| Shipping (Turkey) | Yurtiçi, Aras, MNG and PTT rates, labels and tracking |
| PayTR, iyzico | Turkish payment gateways with installment tables |
| KVKK & Cookies | Turkish privacy consent and cookie banner |
| SMS & WhatsApp | Order notifications by SMS and WhatsApp |
| Phone Verification (OTP) | Sign-in and sign-up with a phone code |

## What is in the package

| Folder | Contents |
|---|---|
| `packages/McuShop/Theme` | Theme source code (no demo products; you add your own) |
| `packages/McuShop/Core` | The shared "McuShop" admin menu (loaded automatically by the theme, no registration needed) |
| `public/themes/shop/mcushop` | Pre-built CSS/JS, so you do **not** need npm on the server |

## 1. Upload the files

Take a database and file backup first. Unzip the package into the **root folder of your Bagisto install**. The folders merge with the existing ones; no Bagisto core file is overwritten.

## 2. Register the package

In `composer.json`, inside `autoload > psr-4`, add:

```json
"McuShop\\Theme\\": "packages/McuShop/Theme/src",
```

In `bootstrap/providers.php`, add this line at the end of the `return [ ... ];` array, exactly as written (fully qualified class name, no `use` import needed):

```php
McuShop\Theme\Providers\McuShopThemeServiceProvider::class,
```

> The provider is named `McuShopThemeServiceProvider` so it can never clash with Bagisto's own `Webkul\Theme\Providers\ThemeServiceProvider`, which `bootstrap/providers.php` already imports. If you prefer the import style used in that file, `use McuShop\Theme\Providers\McuShopThemeServiceProvider;` is also safe.
>
> Updating from 1.6.2 or older: replace the old `McuShop\Theme\Providers\ThemeServiceProvider::class` line with the one above.

## 3. Run the commands (in the Bagisto root folder)

```bash
composer dump-autoload
php artisan migrate
php artisan storage:link
php artisan mcushop:install moda
php artisan optimize:clear
php artisan config:cache
```

`php artisan storage:link` prints "ERROR The [public/storage] link already exists" when Bagisto's installer has already created the link. That message is harmless; just continue with the next command.

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
- **Catalog › Features:** sectors that show the compare bar.
- **Languages and currencies:** the picker lists the languages and currencies added to your channel (Admin › Settings › Channels). Upload a flag per language under Admin › Settings › Locales, otherwise a bundled flag is used. Add exchange rates for extra currencies.
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

## 7. Languages and currencies

- **23 languages ready:** the theme ships all Bagisto languages, including right-to-left Arabic, Persian and Hebrew. Texts Bagisto already translates (Add to cart, Search, Checkout...) use Bagisto's own translation; theme-only texts are translated in every language.
- **Which languages the visitor sees:** the picker lists the languages added to your channel at Admin › Settings › Channels. Add a language there and the theme text appears in it right away.
- **Languages added later:** the theme's home page sections are created in the new language automatically on its first visit. Untouched starter texts are translated; texts you wrote yourself are kept.
- **Flags:** upload a flag image per language at Admin › Settings › Locales. Without one, a bundled flag is shown.
- **Currencies:** add currencies to the channel and enter their exchange rates at Admin › Settings › Exchange Rates. The picker shows each currency's symbol and code; with a single currency it is shown for information only.
- **Changing a text:** edit the matching file in `packages/McuShop/Theme/src/Resources/lang/<locale>/`, then run `php artisan optimize:clear`.

## License

One license = one store on one domain (plus its test copies). See `LICENSE.txt` in the package.

## Support

Contact the developer through the support link on the Bagisto listing.

**[Buy the McuShop Theme](https://1164974116393.gumroad.com/l/bmfisj)**
