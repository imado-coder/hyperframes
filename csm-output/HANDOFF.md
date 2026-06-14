# 🎬 CSM Taksit Ad Reel — Handoff for Claude Desktop

> **هذا الملف لـ Claude Desktop يقرأه ويكمل العمل** — لا تحذفه

## 🎯 السياق الكامل

أنت Claude Desktop، شغّال على جهاز المستخدم في مجلد المشروع. هذا المشروع هو **إعلان فيديو للزبون "CSM Taksit"** — شركة جزائرية تبيع بلاكارات/دريسينغ/مطابخ حسب الطلب بنظام التقسيط.

### الزبون
- **العلامة:** Csm Taksit
- **الشعار:** CSM – Votre maison, vos rêves, en facilité
- **الهاتف:** 0770929291
- **Facebook:** facebook.com/csmtaksit (183K followers)
- **Instagram:** @csmelectromenager
- **المدينة:** الجزائر
- **المنتجات:** Placards / Dressings / Cuisines / Cabinets حسب الطلب

### الإطار التقني
- **Framework:** Hyperframes (npm package `hyperframes`)
- **المقاس:** 1080×1920 (Instagram/Facebook Reel عمودي)
- **المدة المستهدفة:** 45 ثانية @ 30fps
- **CLI:** `npx hyperframes preview` و `npx hyperframes render`

---

## ✅ ما تم في جلسة الويب السابقة

| الإنجاز | التفاصيل |
|---|---|
| ✓ تركيب HTML | 10-13 مشهد motion graphics في `index.html` |
| ✓ معالجة الصور | 7 صور منتجات (شِيلت phone status/nav bars) في `assets/product-*.jpg` |
| ✓ استخراج frames | 10 frames من فيديو CSM في `assets/frame-*.jpg` |
| ✓ تقطيع فيديو | 3 clips قصيرة في `assets/clip-*.mp4` |
| ✓ إعداد GSAP محلي | `assets/lib/gsap.min.js` (CDN كان محجوب) |
| ✓ Render نسختين | 15s + 45s — pushed إلى GitHub PR #1 |

## ⚠️ القيود اللي واجهتها (في Web Cloud)

| المشكلة | تأثيرها |
|---|---|
| Facebook/Instagram محجوبين | ما قدرت أزور صفحة CSM لجلب معلومات إضافية |
| Pixabay/Bensound/Mixkit محجوبين | ما قدرت أنزّل موسيقى تجارية حقيقية |
| Edge-TTS محجوب | ما قدرت أولد voice-over عربي طبيعي |
| Kokoro TTS متاح لكن **لا يدعم العربي** | فرنسي فقط للـ TTS |
| Headless Chrome يفشل مع `<video>` element | بدّلتهم بـ `<img>` ثابتة + Ken Burns |
| 5MB limit في tool calls | ما قدرت أنزّل ملفات كبيرة للمستخدم |

**أنت (Desktop) ما عندك هاي القيود — تقدر تحل كل شي.**

---

## 🎯 المطلوب منك (Claude Desktop)

المستخدم يريد:

### 1️⃣ موسيقى تجارية حقيقية (priority)
- **مش synthesized** — تجارية حقيقية
- ابحث في مكتبة المستخدم على جهازه (يقول إنه عنده ملف فيه موسيقى تجارية + SFX)
- إذا ما في، حمّل من Pixabay مباشرة:
  ```bash
  # مثلاً
  curl -L -o assets/music-commercial.mp3 \
    "https://cdn.pixabay.com/audio/2024/.../*.mp3"
  ```

### 2️⃣ Voice-over احترافي (priority)
- **فرنسي:** استعمل Kokoro (مدمج في Hyperframes):
  ```bash
  npx hyperframes tts "Vous avez un espace? CSM Taksit le transforme. Sur mesure. De qualité. Avec installation incluse. Et payable en plusieurs fois. Plus de 183 mille personnes nous font confiance. Suivez-nous." \
    --voice ff_siwis --speed 1.1 --output assets/sfx/voice-fr.wav
  ```
- **عربي جزائري** (إذا المستخدم يريده): استعمل Edge-TTS (Microsoft):
  ```bash
  pip install edge-tts
  edge-tts --voice ar-DZ-AminaNeural --rate "+10%" \
    --text "عندك مساحة فاضية ؟ CSM Taksit يحوّلها لك. حسب الطلب، جودة احترافية، التركيب علينا. والدفع بالتقسيط. أكثر من 183 ألف شخص يثقوا فينا. تابعنا." \
    --write-media assets/sfx/voice-ar.mp3
  # أو voice ذكوري: ar-DZ-IsmaelNeural
  ```

### 3️⃣ تحسين الخطوط (priority)
المستخدم قال الخطوط الحالية مش بمستوى محترف. غيّر في `index.html`:
- **بدّل** `Anton, Bebas Neue, Inter, Cairo, Tajawal` 
- **بـ:**
  - Latin display: `"Playfair Display"` (luxury serif) أو `"DM Serif Display"`
  - Latin body: `"Manrope"` أو `"Sora"`
  - Arabic premium: `"IBM Plex Sans Arabic"` أو `"Readex Pro"` (أكثر عصرية من Cairo)

### 4️⃣ لغة واحدة فقط (مهم!)
المستخدم قال: **"لا تخلط فرنسي وعربي في نفس المشهد"**.
- خيار A: **نسخة فرنسية بالكامل** (recommended — Kokoro support قوي)
- خيار B: **نسخة عربية بالكامل** (تحتاج Edge-TTS للـ voice)
- خيار C: **نسختين منفصلتين** (best للزبون)

### 5️⃣ الـ render النهائي
```bash
cd csm-output/source
npx hyperframes lint
npx hyperframes inspect
npx hyperframes render --output ../final-pro.mp4
```

---

## 📁 هيكل الملفات (لما يكلون git pull)

```
hyperframes/
└── csm-output/
    ├── csm-reel.mp4                    ← النسخة 15s
    ├── csm-reel-pro-45s.mp4            ← النسخة 45s
    ├── HANDOFF.md                      ← هذا الملف
    ├── audio-library/                  ← الموسيقى/SFX الحالية
    │   ├── music-A-commercial-beat-128bpm.mp3  (synthesized — مش جودة كافية)
    │   ├── music-B-melodic-tech-mix.mp3        (current — tech)
    │   └── sfx-*.mp3                            (3 SFX قديمة)
    └── source/                         ← الـ project الكامل
        ├── index.html                  ← composition (45s pro)
        ├── package.json
        └── assets/
            ├── lib/gsap.min.js         ← GSAP محلي
            ├── product-*.jpg           ← 7 صور منتجات نظيفة
            ├── frame-*.jpg             ← 10 frames من فيديو
            ├── clip-*.mp4              ← 3 video clips
            ├── video.mp4               ← فيديو المصدر (60s, re-keyframed)
            ├── video-original.mp4      ← فيديو الأصلي
            ├── logo.jpg                ← لوغو CSM (1080×1080)
            └── sfx/
                ├── full-audio-45s.mp3  ← current mix (music + SFX)
                ├── commercial-beat.mp3 ← synthesized track
                └── music-45s.mp3       ← tech mix
```

---

## 🛠️ سكريبتات جاهزة

### Build script (للدمج النهائي)

`scripts/build-final.sh`:
```bash
#!/bin/bash
set -e

cd "$(dirname "$0")/../source"
ASSETS=assets
SFX=$ASSETS/sfx

# 1. Generate French voice
npx hyperframes tts \
  "Vous avez un espace ? CSM Taksit le transforme. Sur mesure. De qualité. Avec installation incluse. Et payable en plusieurs fois. Plus de 183 mille personnes nous font confiance. Suivez-nous au 0770 92 92 91." \
  --voice ff_siwis --speed 1.05 \
  --output $SFX/voice-fr.wav

# 2. Mix master audio: music (ducked) + voice + SFX hits
ffmpeg -y \
  -i music-commercial.mp3 \
  -i $SFX/voice-fr.wav \
  -i $SFX/whoosh-cinematic.mp3 \
  -i $SFX/thud.mp3 \
  -i $SFX/notification.mp3 \
  -filter_complex "
    [0:a]volume=0.55,aformat=channel_layouts=stereo[bgm];
    [1:a]volume=1.0,adelay=2500|2500,asplit=2[v1][v1d];
    [v1d]asplit=2[duck1][duck2];
    [bgm][duck1]sidechaincompress=threshold=0.04:ratio=8:attack=5:release=200[ducked];
    [2:a]volume=1.3,adelay=2500|2500[sfx1];
    [2:a]volume=1.3,adelay=21500|21500[sfx2];
    [3:a]volume=1.2,adelay=200|200[hit1];
    [3:a]volume=1.2,adelay=20500|20500[hit2];
    [4:a]volume=0.9,adelay=41000|41000[ding];
    [ducked][v1][sfx1][sfx2][hit1][hit2][ding]
      amix=inputs=7:duration=longest:dropout_transition=0:normalize=0,
      alimiter=limit=0.95
  " \
  -ac 2 -ar 44100 -c:a libmp3lame -b:a 192k \
  $SFX/final-audio.mp3

# 3. Update HTML to use final-audio.mp3 instead of full-audio-45s.mp3
sed -i 's|full-audio-45s.mp3|final-audio.mp3|g' index.html

# 4. Render
npx hyperframes render --output ../csm-reel-final.mp4

echo "✓ DONE — output: csm-output/csm-reel-final.mp4"
```

---

## 💡 ملاحظات مهمة لـ Desktop Claude

1. **اقرأ هذا الملف كاملاً قبل أي عمل**
2. **اسأل المستخدم: شو يفضّل** — French/Arabic/Both versions
3. **إذا فيه ملفات موسيقى/SFX في مجلد على جهازه** — اقرأها أول (ls المجلد)
4. **الـ index.html طويل (~400 سطر)** — استعمل Read tool مش grep
5. **بعد كل تعديل:**
   - `npx hyperframes lint` (لازم 0 errors)
   - `npx hyperframes inspect` (لازم 0 layout issues)
   - `npx hyperframes render` (الناتج النهائي)
6. **حافظ على PR #1** — push للفرع `claude/install-code-skills-KKeJF`

---

## 🎬 الـ Reel السيناريو الحالي (للمرجعية)

| الوقت | المشهد | المحتوى |
|---|---|---|
| 0.0-2.5s | S1 LOGO PUNCH | لوغو CSM يدور + Flash white |
| 2.5-5.5s | S2 HOOK | "عندك مساحة فاضية؟" + Kinetic Type |
| 5.5-9.0s | S3 AVANT | فيديو غرفة فاضية + سهم متحرك |
| 9.0-12.0s | S4 IDEA | 💡 "نصمم لك" |
| 12.0-17.0s | S5 PROCESS 01 | عمال يركبون + "Mesure" |
| 17.0-20.5s | S6 PROCESS 02 | Close-up + "Installation Pro" |
| 20.5-22.0s | S7 ✓ DONE | White flash + checkmark |
| 22.0-25.0s | S8 PRODUCTS BURST | 4 صور سريعة |
| 25.0-30.0s | S9 APRÈS | غرفة كاملة + Ken Burns |
| 30.0-34.0s | S10 USP GRID | 4 ميزات بأيقونات |
| 34.0-37.5s | S11 TAKSIT | بطاقة دفع متحركة |
| 37.5-41.0s | S12 STATS | 183K Followers + ⭐⭐⭐⭐⭐ |
| 41.0-45.0s | S13 CTA | Facebook follow + رقم |

**يجب تعديل اللغة (لا خلط) + تحسين الموسيقى + إضافة Voice + تحسين الخطوط.**

---

## 🚀 Prompt جاهز للمستخدم — يلصقه في Desktop Claude

```
اقرأ csm-output/HANDOFF.md في هذا المشروع.
هذا فيه السياق الكامل لإعلان CSM Taksit.

عندي مجلد على جهازي اسمه "efect son and musc backgrond"
فيه موسيقى تجارية اعلانية و SFX جاهزة.

اقرأ ذلك المجلد كاملاً (استعمل ls/Read)، اعرض لي الملفات،
واختار:
- 1 موسيقى تجارية مناسبة للـ background (45 ثانية)
- 5-10 SFX (whoosh, impact, pop, notification, sting)

بعدين دمج كل شي حسب التعليمات في HANDOFF.

اسألني عن:
1. اللغة: فرنسي بالكامل ولا عربي بالكامل ولا نسختين؟
2. صوت ذكر ولا أنثى للـ voice-over؟
3. أي تعديل ثاني؟

وابدأ.
```

---

## 📂 موقع مكتبة المستخدم

```
المسار على جهاز المستخدم:
"efect son and musc backgrond"  ← اسم المجلد (Sound effects + Music background)

اللي محتمل يكون فيه:
- ملفات MP3 لموسيقى تجارية اعلانية
- ملفات MP3/WAV لـ Sound Effects (whoosh, impact, pop, ding, etc)
```

**خطوات Desktop Claude:**
1. اسأل المستخدم عن المسار الكامل للمجلد (مثلاً: `~/Desktop/efect son and musc backgrond` أو `C:\Users\xxx\Documents\efect son and musc backgrond`)
2. `ls` للمجلد لرؤية كل الملفات
3. اعرضهم في جدول مع الأحجام والمدة (`ffprobe`)
4. اقترح أحسن music + SFX combo
