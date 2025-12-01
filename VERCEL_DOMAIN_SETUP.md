# Nastavenie Vlastnej Domény na Vercel

## Krok 1: Vercel Dashboard

1. Prihláste sa na [vercel.com](https://vercel.com)
2. Vyberte váš projekt `vedici-reaction-game`
3. Prejdite na **Settings** → **Domains**

## Krok 2: Pridanie Domény

### Možnosť A: Vercel Subdoména (Zadarmo)

Vercel automaticky poskytuje subdoménu:
- Formát: `vedici-reaction-game.vercel.app`
- Táto doména je dostupná okamžite po nasadení
- SSL certifikát je automaticky nastavený

### Možnosť B: Vlastná Doména

1. V sekcii **Domains** kliknite na **Add Domain**
2. Zadajte vašu doménu (napr. `game.vedici.gg` alebo `reaction.vedici.gg`)
3. Kliknite na **Add**

## Krok 3: DNS Konfigurácia

Vercel vám zobrazí inštrukcie pre DNS nastavenie. Máte dve možnosti:

### Možnosť 1: A Record (IPv4)

Pridajte A record u vášho DNS poskytovateľa:
```
Type: A
Name: @ (alebo subdoména, napr. game)
Value: 76.76.21.21
```

### Možnosť 2: CNAME Record (Odporúčané)

Pridajte CNAME record:
```
Type: CNAME
Name: @ (alebo subdoména, napr. game)
Value: cname.vercel-dns.com
```

**Poznámka:** Pre root doménu (@) niektorí poskytovatelia vyžadujú A record namiesto CNAME.

## Krok 4: Čakanie na Propagáciu DNS

- DNS zmeny sa môžu šíriť 24-48 hodín
- Vercel automaticky skontroluje DNS nastavenie
- Keď je všetko správne, zobrazí sa zelená fajfka ✅

## Krok 5: SSL Certifikát

- Vercel automaticky vytvorí SSL certifikát (Let's Encrypt)
- Certifikát sa obnovuje automaticky
- HTTPS je dostupné okamžite po nastavení DNS

## Príklady DNS Nastavenia

### Pre subdoménu `game.vedici.gg`:

**Ak máte prístup k DNS (napr. Cloudflare, Namecheap, GoDaddy):**

```
Type: CNAME
Name: game
Value: cname.vercel-dns.com
TTL: Auto (alebo 3600)
```

### Pre root doménu `vedici.gg`:

**Možnosť 1 - A Record:**
```
Type: A
Name: @
Value: 76.76.21.21
TTL: Auto
```

**Možnosť 2 - CNAME (ak váš poskytovateľ podporuje):**
```
Type: CNAME
Name: @
Value: cname.vercel-dns.com
TTL: Auto
```

## Overenie DNS

Po pridaní DNS záznamov môžete overiť pomocou:

```bash
# Pre subdoménu
dig game.vedici.gg

# Alebo
nslookup game.vedici.gg
```

## Troubleshooting

### Doména sa nezobrazuje ako "Valid"

1. Skontrolujte DNS záznamy (môže trvať až 48 hodín)
2. Uistite sa, že hodnoty sú správne
3. Skúste vyčistiť DNS cache: `nslookup -flushdns` (Windows) alebo `sudo dscacheutil -flushcache` (Mac)

### SSL Certifikát sa nevytvoril

- Počkajte 5-10 minút po nastavení DNS
- Skontrolujte, či DNS záznamy sú správne propagované
- Vercel automaticky vytvorí certifikát po úspešnom DNS overení

### Redirect z www na non-www (alebo naopak)

V **Settings** → **Domains** môžete:
- Pridať `www.vedici.gg` ako ďalšiu doménu
- Nastaviť redirect v **Settings** → **Domains** → **Redirects**

## Tipy

1. **Cloudflare**: Ak používate Cloudflare, uistite sa, že SSL/TLS je nastavený na "Full" alebo "Full (strict)"
2. **DNS Propagation**: Použite [whatsmydns.net](https://www.whatsmydns.net) na kontrolu šírenia DNS
3. **Vercel CLI**: Môžete spravovať domény aj cez CLI:
   ```bash
   vercel domains add game.vedici.gg
   ```

## Bezpečnosť

- Vercel automaticky poskytuje HTTPS
- Certifikáty sa obnovujú automaticky
- HSTS je zapnuté automaticky pre všetky domény

---

**Poznámka:** Po nastavení domény môže trvať niekoľko minút až hodín, kým bude dostupná. Vercel vám pošle email, keď je doména pripravená.




