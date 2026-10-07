# 💳 Kartalar

UzCard, Humo va Visa kartalarini bitta joyda saqlaydigan va tez ko'rsatadigan shaxsiy veb-ilova. Neon-cyberpunk uslubida, har bir karta animatsiyali dizayn bilan.

> 🔐 Ma'lumotlar brauzerda **AES-GCM** bilan shifrlanib saqlanadi (kalit paroldan PBKDF2 orqali olinadi). Parolni unutsangiz, kartalarni tiklab bo'lmaydi.

## Imkoniyatlar

- **3 bo'lim:** UzCard, Humo, Visa (alohida tablar)
- **Yashirin ko'rinish:** `8600 •••• •••• 1234`. Kartani bossangiz to'liq raqam va amal qilish muddati ochiladi, 10 soniyadan keyin o'zi yana yashirinadi
- **Nusxalash:** raqamni bir bosishda clipboard'ga olish
- **Animatsiyali dizayn:** har tur uchun default gradient va yaltirash effekti (CSS, loop)
- **Google login:** har bir foydalanuvchining kartalari faqat o'ziga ko'rinadi
- **PWA:** telefon ekraniga ilova sifatida o'rnatiladi
- **Avto-qulf:** 2 daqiqa harakatsiz qolsa yoki ilova 1 daqiqa yopiq tursa qulflanadi
- **Bulutda saqlash:** Firestore orqali istalgan qurilmada bir xil

## Texnologiyalar

- HTML, CSS, JavaScript (framework yo'q, bitta fayl)
- Firebase Authentication (Google)
- Cloud Firestore
- GitHub Pages

## Ishga tushirish

### 1. Firebase loyihasi

1. [Firebase Console](https://console.firebase.google.com)'da loyiha oching va **Web app** qo'shing
2. **Authentication → Sign-in method → Google** ni yoqing
3. **Authentication → Settings → Authorized domains** ga o'z domeningizni qo'shing (masalan `username.github.io`)
4. **Firestore Database** yarating (Standard edition)

### 2. Firestore rules

Har kim faqat o'z hujjatini o'qiy va yoza oladi:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{db}/documents {
    match /users/{uid}/data/{doc} {
      allow read, write: if request.auth != null && request.auth.uid == uid;
    }
  }
}
```

### 3. Config

`index.html` ichidagi `firebaseConfig` qiymatlarini o'z loyihangizniki bilan almashtiring (Project settings → Your apps).

### 4. Deploy

1. `kartalar.html` ni `index.html` deb nomlab, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png` bilan birga repoga yuklang
2. **Settings → Pages → Branch: main / root** ni tanlang
3. `https://username.github.io/repo-nomi/` manzilida oching

## Ma'lumotlar tuzilmasi

```
users/{uid}/data/cards
  └── list: [ { id, type, num, exp, name } ]
```

`type` qiymatlari: `uzcard`, `humo`, `visa`.

## Reja

- [x] Bo'limlar, yashirin raqam, animatsiyali default dizaynlar
- [x] Google login va foydalanuvchiga xos saqlash
- [x] Brauzerda shifrlash (parol asosida, AES-GCM)
- [x] CVV: alohida qulf bilan ochiladigan, shifrlangan holda
- [x] O'z rasmini karta dizayni qilib yuklash
- [x] Kartani tahrirlash va tartibini o'zgartirish
- [x] Telefon ekraniga o'rnatish (PWA) va avtomatik qulflash
- [ ] Animatsiyali o'z dizaynlari (GIF/WebM)

## Xavfsizlik

- Firebase `apiKey` yashirin kalit emas, u brauzer kodida ochiq turadi. Himoya **Firestore rules** va **Authorized domains** orqali ta'minlanadi
- Shifrlash tayyor bo'lguncha haqiqiy karta raqamlari va CVV'ni saqlamang
- CVV kodini hech qachon ochiq matnda saqlamang

## Muallif

[HiroKasimov](https://github.com/HiroKasimov)
