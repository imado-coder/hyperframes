# Ads Pro — Production Rules

> قواعد إنتاج فيديوهات الإعلانات لكل الزبائن (مش CSM فقط).
> تطبق على كل مشروع.

## 🎯 الفلسفة العامة

**كل إعلان لازم يحس كأنه من شركة عالمية كبرى (Apple, Nike, Samsung)**, مش من فريلانسر.

- ⏱️ المدة: 30-45 ثانية للـ Instagram/Facebook Reels (عمودي 1080×1920)
- 🎬 سرعة: cuts سريعة (0.3-0.8s لكل مشهد)
- 🔥 كثافة: لا توقف الكتابة، رسائل متعددة على مدار الفيديو
- 🎵 صوت: موسيقى تجارية حقيقية (Pixabay/مكتبة الزبون) + voice-over + SFX
- 🎨 خطوط: Premium (Playfair Display, DM Serif, Manrope, IBM Plex Arabic — مش Inter/Cairo فقط)

## 🚫 ممنوع (Anti-patterns)

| ❌ ممنوع | ✅ بدلها |
|---|---|
| خلط فرنسي + عربي في نفس المشهد | لغة واحدة فقط لكل نسخة |
| كروت بسيطة (white pill + gold pill) | كروت animated مع shadows + animations معقدة |
| موسيقى synthesized | موسيقى تجارية حقيقية فقط |
| صور ثابتة طويلة | quick cuts + Ken Burns + zoom |
| Inter / Cairo (basic) | Playfair Display + Manrope + IBM Plex Arabic |
| اللوغو لوحده في outro | Outro motion graphics معقد |
| `<video>` في headless Chrome (يفشل) | استعمل image sequences سريعة أو try video مع workaround |
| 1-2 رسائل فقط | 8-12 رسالة عبر الفيديو |

## ✅ Required Patterns

### 1. Kinetic Typography (الإلزامي)
- نص كبير في الوسط (120-200px)
- زووم سريع (scale: 3 → 1 في 0.5s)
- كلمة لكل 0.5-0.8s
- ease: "expo.out" أو "back.out(2)"
- استعمل قالب blue-sweater-intro-video كمرجع

### 2. تركيب موسيقى احترافي
```
Background music (45s, ducked -12dB under voice)
  +
Voice-over (clear, bold, French/Arabic)
  +
SFX layers (whoosh at transitions, impact at hits, ding at CTA)
```

### 3. Sidechain Ducking
الموسيقى **لازم** تخفت عند الـ voice-over (compressor sidechain)
```bash
[bgm][voice]sidechaincompress=threshold=0.04:ratio=8:attack=5:release=200
```

### 4. الـ Scene Structure (تقريبي)
```
[0-2s]   LOGO PUNCH (impact SFX)
[2-4s]   HOOK QUESTION (kinetic type, big zoom)
[4-7s]   PROBLEM VISUAL (video clip + text overlay)
[7-9s]   3 KEY VALUES (fast cuts, 3 words)
[9-15s]  PRODUCT GALLERY (4-5 quick cuts)
[15-20s] PROCESS (worker shots + step numbers)
[20-25s] RESULT (hero shot Ken Burns)
[25-30s] USP GRID (4-6 features with icons)
[30-35s] SOCIAL PROOF (stats + testimonials)
[35-40s] OFFER/CTA (price/installments + button pulse)
[40-45s] FINAL CTA (logo + handle + phone + follow button)
```

### 5. Voice-over Script Template (FR)
```
"Vous cherchez [PROBLEM]? [BRAND] vous offre [SOLUTION].
Sur mesure. De qualité. Avec installation incluse.
Plus de [SOCIAL PROOF] personnes nous font confiance.
Contactez-nous au [PHONE] et suivez-nous sur Facebook."
```

### 6. Marketing Phrases (Sources)
- "Sur mesure" / "حسب الطلب"
- "Qualité professionnelle" / "جودة احترافية"
- "Installation incluse" / "تركيب شامل"
- "Payez en plusieurs fois" / "ادفع بالتقسيط"
- "Plus de X clients satisfaits" / "أكثر من X عميل راضٍ"
- "Garantie X ans" / "ضمان X سنوات"
- "Livraison gratuite" / "توصيل مجاني"
- "Service 7j/7"
- "Devis gratuit"
- "Made by [BRAND]" / "صنع في [BRAND]"

### 7. Font Pairings (لكل صناعة)

| الصناعة | Display | Body | Arabic |
|---|---|---|---|
| فاخر (موبيليا، مجوهرات) | Playfair Display | Manrope | IBM Plex Sans Arabic |
| تكنولوجيا | Bebas Neue | Inter | Readex Pro |
| موضة | Anton | Sora | Tajawal |
| مأكولات | DM Serif Display | DM Sans | Cairo |
| رياضة | Bebas Neue | Manrope | Tajawal |

### 8. SFX Library (المعرفة بالاسم)
- **whoosh-cinematic** → انتقالات بين المشاهد الرئيسية
- **whoosh-simple** → انتقالات سريعة بين quick cuts
- **thud** → عند نزول اللوغو أو كلمة قوية
- **cinematic-impact** → عند الـ HOOK والـ HERO frames
- **huge-cinematic-reverb-impact** → عند الـ CTA النهائي
- **riser-effect** → قبل الانتقالات الكبيرة
- **riser-wildfire** → buildups طويلة
- **pop** / **bubble-pop** → عند ظهور الكتابة
- **ding** → notifications، 183K, ⭐ stars
- **cash-register-kaching** → عند ذكر السعر/التقسيط
- **camera-shutter** / **camera-flash** → product showcase
- **zoom-sound** → عند الزووم
- **vinyl-stop** → hard stops
- **logo-fashion-future-bass** → outro logo sting

### 9. Music Categories (Pixabay)
- **hitslab — Product Launch** → إطلاق منتجات
- **the_mountain — Beautiful Commercial** → عالم الفخامة (موبيليا، مجوهرات)
- **viacheslavstarostin — Advertising** → عام تجاري
- **mondamusic — Promo** → tropical fun
- **the_mountain — Tech Commercial** → tech/digital
- **the_mountain — Agency Promo** → corporate
- **fatbunny — Sport Commercial** → رياضة/طاقة

### 10. Color Palettes (per client)
- استخرج 3 ألوان من لوغو الزبون
- استعمل: primary (60%) + secondary (30%) + accent (10%)
- لا تخترع ألوان — التزم بهوية الزبون

## 🔧 الـ Render Pipeline

```bash
# 1. Lint
npx hyperframes lint               # لازم 0 errors

# 2. Inspect (layout issues)
npx hyperframes inspect --samples 15  # لازم 0 issues

# 3. Validate (contrast)
npx hyperframes validate           # تأكد contrast 4.5:1+

# 4. Render
npx hyperframes render --output final.mp4

# 5. Verify duration + size
ffprobe -v error -show_entries format=duration final.mp4
```

## 📋 Per-Client Checklist

قبل تسليم أي إعلان:
- [ ] لوغو الزبون استعمل (مش placeholder)
- [ ] رقم الهاتف صحيح
- [ ] رابط social صحيح
- [ ] عدد followers محدّث
- [ ] الألوان من لوغو الزبون
- [ ] الخط مناسب للصناعة
- [ ] لغة واحدة فقط (لا خلط)
- [ ] Voice-over موجود + ducked
- [ ] SFX عند كل scene transition
- [ ] CTA واضح
- [ ] رقم هاتف على الشاشة 2+ ثواني
- [ ] WCAG contrast OK
