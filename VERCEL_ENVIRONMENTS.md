# Vercel Environments: Production vs Preview

## Rozdiel medzi Production a Preview

### 🌐 Production (Produkcia)
- **Kedy**: Automaticky sa nasadí pri pushnutí do `main` branch
- **URL**: Používa vašu vlastnú doménu alebo `your-project.vercel.app`
- **Účel**: Live verzia pre koncových používateľov
- **Stabilita**: Mala by byť vždy stabilná a funkčná
- **DNS**: Pridelená doména smeruje na Production

### 🔍 Preview (Náhľad)
- **Kedy**: Automaticky sa vytvorí pri:
  - Pushnutí do inej branch ako `main`
  - Vytvorení Pull Request
  - Manuálnom nasadení
- **URL**: `your-project-git-branch-name.vercel.app`
- **Účel**: Testovanie zmien pred nasadením do Production
- **Stabilita**: Môže obsahovať experimentálne funkcie
- **DNS**: Vlastná doména NESMERUJE na Preview

## Ktorý zvoliť?

### ✅ Použite **Production** ak:
- Chcete nasadiť finálnu verziu
- Pushujete do `main` branch
- Chcete, aby vaša vlastná doména smerovala na túto verziu
- Je to finálna, otestovaná verzia

### 🔬 Použite **Preview** ak:
- Testujete nové funkcie
- Pushujete do feature branch
- Chcete otestovať zmeny pred nasadením
- Vytvárate Pull Request

## Ako to funguje na Vercel

### Automatické nasadenie:

1. **Push do `main` branch:**
   ```
   git push origin main
   ```
   → Vytvorí sa **Production** deployment

2. **Push do inej branch:**
   ```
   git checkout -b feature/new-feature
   git push origin feature/new-feature
   ```
   → Vytvorí sa **Preview** deployment

3. **Pull Request:**
   - Vytvorenie PR automaticky vytvorí Preview
   - URL sa zobrazí v PR komentári

## Manuálne nasadenie

### Production:
```bash
vercel --prod
```

### Preview:
```bash
vercel
# alebo
vercel --target production  # ak chcete explicitne production
```

## Vercel Dashboard

V dashboarde vidíte:
- **Production Deployments** - všetky production verzie
- **Preview Deployments** - všetky preview verzie

Môžete:
- Rollback na predchádzajúcu verziu
- Promotovať Preview na Production
- Zobraziť logy pre každé nasadenie

## Pre váš projekt

### Odporúčanie:

1. **Pre finálnu verziu:**
   - Pushnite do `main` branch
   - Vercel automaticky nasadí Production
   - Vaša doména bude smerovať na Production

2. **Pre testovanie:**
   - Vytvorte feature branch
   - Pushnite zmeny
   - Vercel vytvorí Preview URL
   - Otestujte na Preview URL
   - Ak je všetko OK, merge do `main`

## Príklad workflow:

```bash
# 1. Vytvorte feature branch
git checkout -b feature/improvements

# 2. Urobte zmeny a commitnite
git add .
git commit -m "Add new feature"

# 3. Pushnite (vytvorí Preview)
git push origin feature/improvements

# 4. Otestujte Preview URL z Vercel dashboard

# 5. Ak je OK, merge do main (vytvorí Production)
git checkout main
git merge feature/improvements
git push origin main
```

## Dôležité poznámky

- ✅ **Production** je vždy dostupný na vašej vlastnej doméne
- ✅ **Preview** má unikátnu URL pre každé nasadenie
- ✅ Môžete mať viacero Preview deployments súčasne
- ✅ Production je vždy len jeden (najnovší)
- ✅ Preview deployments sa automaticky zmazú po 14 dňoch (alebo podľa nastavenia)

## Nastavenie v Vercel Dashboard

V **Settings** → **Git** môžete nastaviť:
- Ktoré branchy vytvárajú Production
- Ktoré branchy vytvárajú Preview
- Ignorované branchy

---

**Záver:** Pre váš projekt použite **Production** pre finálnu verziu a **Preview** pre testovanie zmien.

