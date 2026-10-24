# Charm lab orders

Demo checkout only. Nothing is charged, and orders stay in this browser (`localStorage` key `charmlab.orders`). There is no Google API key in the site. Tiago keeps the Google Sheet himself and imports this CSV.

## Demo

1. On the **order** tab, tap **checkout** (an empty bag gets the charm on screen). Or add a few charms, then open the bag.
2. Enter a name plus email or mobile. Tap **text me a link** or **email me a link** to open Messages or Mail with a prefilled note. Then tap **customer tapped the link**, or tap **I'll type the address**.
3. Fill street, city, state, and ZIP (country defaults to US).
4. Pick **Venmo**, **Cash App**, **Shop Pay**, or **Apple Pay**, then **payment received**.
5. The order is **ordered**. Download the Vograce ZIP, download the CSV, or open **orders** and move the status along `ordered → exported → received_by_3rd_party → shipped` (or `cancelled`). Downloading the ZIP marks it exported.

The text/email link only opens the address form in a browser that has the same saved checkout (this phone or computer). Use **customer tapped the link** when you are demoing on one device.

## CSV for Google Sheets

The download is the full order book, UTF-8 with a BOM, commas, and quoted fields. In Sheets: **File → Import → Upload → Replace current sheet** so rows are not duplicated. **Copy CSV for Sheets** copies the same table without a BOM for a paste.

Header row, in this order:

```
order_id,created_at,status,customer_name,customer_email,customer_phone,ship_line1,ship_line2,ship_city,ship_state,ship_zip,ship_country,payment_method,payment_demo,amount_usd,quantity,shape,size_in,print_color,outline_color,holo,attachment,attachment_vograce,hardware_color,text_content,text_font,text_color,text_size,stickers_json,vograce_product,vograce_sides,vograce_finish,vograce_size,vograce_accessory,vograce_accessory_color,vograce_qty,vograce_unit_cost_est,notes,export_zip_filename,exported_at,received_by_3rd_party_at,shipped_at,tracking_number
```

`created_at` is ISO time in Pacific/Honolulu (`-10:00`). `payment_method` is `venmo`, `cashapp`, `shop_pay`, or `apple_pay`. `payment_demo` is always `true`. `amount_usd` is the example retail line total (size + holo + attachment, times quantity). `holo` is `yes` or `no`. `size_in` is `1.5`, `2`, or `2.5`. `attachment` is `keyring` or `bag_clip`. Several text objects are joined with ` | `. `stickers_json` is a JSON list of `{id,x,y,s,r}`.

`vograce_unit_cost_est` is Vograce's public qty-1 unit price for that size, with or without front Holographic (Star). It does not include the 6+ bulk discount, shipping, or tax. Accessory choice does not change that unit price.

## Vograce ZIP

One ZIP per order row, named `OrderID-Shape_Qty_#AccessoryNo.zip` (Vograce's `DesignName_Qty_#AccessoryNo` pattern, with the order id in front).

| File | What it is |
|---|---|
| `front.png` | Transparent PNG, printable art only (fill, outline, stickers, text). No shadow, gloss, or holo wash. |
| `back.png` | Always included, for **Double sided different design**. Layout is mirrored left to right. Stickers and words are drawn the right way around. |
| `cutline.svg` | Cut outline plus the hole, in real millimetres. Red stroke. Longest side matches the size picked (the hole tab is included). |
| `mockup.png` | Flat preview of what the customer saw: acrylic, art, gloss, holo wash. Not a 3D screenshot, and the chrome chain is not in it. |
| `order.json` | The spreadsheet row, the design, and export pixel size / DPI. |

Accessory numbers in the file name are charm lab codes. Vograce does not publish numeric accessory ids.

| Code | Designer | Vograce accessory | Color |
|---|---|---|---|
| `#1` | keyring | Lobster Hook Clasp + Circle Split Ring + Chain | silver |
| `#2` | bag clip | D Hook Clasp | silver |
| `#3` | not offered | Phone Strap (Vograce only at 1.5" and under) | silver |

On Vograce, pick **Custom Clear Acrylic Keychains**, **Double sided different design**, finish **none** or **Holographic(Star) front**, the size, the accessory, silver, and the quantity. Upload the ZIP. Ask for a proof on small orders. Group same size and finish yourself if you want the 6+ price. The site does not merge ZIPs.

## Limits

- Payments, texts, and emails are UI only. `sms:` and `mailto:` open the device composer and do not send by themselves.
- Orders live in this browser only. Clearing site data clears them.
- Print files are RGB PNG, not CMYK Japan Color 2001. Vograce accepts RGB PNG. Neon swatches are already softened in the designer, and they can still print a bit duller.
- Export is 600 DPI, except 1.5" which is raised so the long side is at least 1000 px (about 667 DPI). 2" is 1200 px on the long side, 2.5" is 1500 px.
- The cutline is a smooth outline of about 420 points, not a few beziers. The hole is a 3 mm circle (Vograce's usual hole is about 2.8 mm).
- Shadows, gloss, and the holo star film are preview-only and are left out of `front.png` and `back.png`.
- Very small type and stickers are limited in the designer. They can still sit close to Vograce's minimum line widths.
- Hardware color is always silver. There is no phone-strap option in the designer.
- `mockup.png` is a flat 2D preview, not the WebGL viewer.
- A ZIP is one design. Combining designs for a bulk Vograce order is manual.
- On older iOS, Safari may open a download instead of saving it. Use the share sheet if the ZIP does not land in Files.
