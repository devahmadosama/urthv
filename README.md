# تلفزيونك — نسخة شاشة Haier (مصر)

نسخة معدّلة من مشروع **[تلفزيونك (tshi-dev/UrTV)](https://github.com/tshi-dev/UrTV)**، مجهّزة ومجرّبة على شاشة
**Haier MatrixTV EE** (Google TV، أندرويد 14) في **الجيزة، مصر**.

كل الفضل في المشروع الأصلي لصاحبه: شاشة التوقف «شاشة تلفزيونك»، والواجهة الرئيسية العربية، وأدوات تسريع الشاشة.
هذه النسخة تضيف عليه ما تحتاجه شاشة في مصر. شرح المشروع الأصلي كاملاً موجود في
[docs/README-ORIGINAL.md](docs/README-ORIGINAL.md).

> ⚠️ **هذه النسخة مخصّصة لشاشة Haier وجُرّبت عليها فقط.** على شاشات أخرى (TCL، Sony…) استخدم المشروع الأصلي:
> https://github.com/tshi-dev/UrTV

---

## إيش اللي يسويه المشروع

- **شاشة توقف «شاشة تلفزيونك»:** الساعة، التاريخ الهجري والميلادي، الطقس، مواقيت الصلاة والصلاة القادمة، آية وحديث
  اليوم، مباريات فرقك، العدّ لرمضان، وصور خلفية.
- **واجهة رئيسية عربية «تلفزيونك»:** صف المفضلة، أسماء التطبيقات بالعربي، تلاوة القرآن والأذان، مؤقت النوم.
- **تسريع الشاشة بأمان:** إيقاف التتبّع والإعلانات والتطبيقات الزائدة، وتعديلات للذاكرة والسرعة. كل شيء قابل للتراجع.
- **تطبيقان مجانيان:** TV Bro (متصفح) و SmartTube (يوتيوب بدون إعلانات).

## الإضافات في هذه النسخة (مصر)

| الإضافة | الشرح | الأمر |
|---|---|---|
| **طريقة حساب الصلاة** | «الهيئة المصرية العامة للمساحة» (فجر ١٩٫٥°، عشاء ١٧٫٥°) بجانب «أم القرى». تطبّق على شاشة التوقف **وعلى الأذان** في الواجهة الرئيسية | `optimize/saver.sh pmethod egypt` (أو `umm`) |
| **الخلفية** | اختيار من أربعة: صور السعودية، صور مصر، صورة ثابتة، لون سادة بدون صور. تطبّق على شاشة التوقف وعلى خلفية الواجهة الرئيسية | `optimize/saver.sh bg egypt` (أو `saudi`، `fixed`، `plain`) |
| **صور مصر** | ١٠ صور مدمجة داخل التطبيق (ما تحتاج إنترنت): الأهرامات، أبو الهول، الأقصر، أسوان، أبو سمبل، الكرنك، مكتبة الإسكندرية، سانت كاترين، ابن طولون | ضمن `bg egypt` |
| **صورة ثابتة** | صورة واحدة من صور مصر ما تتغيّر | `optimize/saver.sh bgfix` يعرض القائمة، ثم `optimize/saver.sh bgfix 3` |
| **الدولار بالجنيه** | سعر الدولار مقابل الجنيه المصري أعلى الشاشة، مع نسبة التغيّر (من Yahoo) | `optimize/saver.sh markets egp` ثم `optimize/saver.sh mkstyle prices` |
| **التحديث التلقائي متوقف** | التطبيق الأصلي ينزّل صفحة الشاشة من مستودع صاحبه كل ٦ ساعات، وهذا كان سيمسح التعديلات. في هذه النسخة متوقف افتراضياً | — |

كل الخيارات موجودة أيضاً في إعدادات شاشة التوقف على التلفزيون نفسه: صفّي «الخلفية» و«طريقة حساب الصلاة».

## ملاحظات خاصة بشاشة Haier MatrixTV EE

اللي واجهناه على هذه الشاشة وكيف انحلّ:

1. **المنفذ 5555 مقفول:** الشاشة (أندرويد 14) ما تفتح ADB العادي، فالاتصال يكون عن طريق **«التصحيح اللاسلكي» (Wireless
   debugging)**. اقرن الشاشة مرة واحدة:
   `optimize/find-tv.sh pair <IP:port من نافذة الاقتران> <الرمز من ٦ أرقام>`. بعد كل إعادة تشغيل يتغيّر المنفذ، فإذا ما
   لقيه السكربت بنفسه خذ «IP address & Port» من شاشة Wireless debugging الرئيسية وشغّل
   `optimize/find-tv.sh connect <IP:port>`. وعنوان الشاشة نفسه ممكن يتغيّر بعد فترة.
2. **الواجهة الرئيسية ما كانت تتركّب** (`The setup wizard of this TV answers Home and owns its saved Wi-Fi network`): الشاشة
   كان فيها شبكات واي فاي قديمة محفوظة من أول تشغيل، وهي تابعة لمعالج الإعداد، فالسكربت يرفض إيقافه حتى لا تضيع الشبكة.
   **الحل:** مسح الشبكات القديمة غير المستخدمة (ما تظهر في الإعدادات لأنها بعيدة). اعرضها بـ
   `optimize/venv/bin/python -I optimize/tv.py "cmd wifi list-networks"`، وامسح **فقط** القديمة بـ
   `cmd wifi forget-network <رقمها>`. لا تمسح الشبكة المتصل عليها.
3. **تطبيق واجهة ثانية** (مثل تطبيق Aura) يخلي سكربت التركيب يطبع خطأ كاذب في آخره
   (`The safety switch put the stock home back`) لأن أندرويد يعرض «اختر تطبيقاً». الحل: إزالة الواجهة الإضافية، أو التأكد بـ
   `optimize/check-home.sh`.
4. **Play Protect** ممكن يطلع رسالة وقت تركيب الواجهة: اضغط «تثبيت على أي حال».
5. **الشاشة تنام** بالليل وتقطع الاتصال: صحّها بالريموت وأعد نفس الأمر.

## التركيب من الصفر

على كمبيوتر متصل بنفس شبكة الواي فاي حقت الشاشة (لينكس أو ماك، أو ويندوز عبر Git Bash).

**على الشاشة أولاً:** الإعدادات ← النظام ← حول ← اضغط على **Build** سبع مرات ← ارجع ← خيارات المطوّر ← شغّل
**USB debugging** و **Wireless debugging**.

### ١. التجهيز والاتصال

```bash
git clone https://github.com/devahmadosama/urthv UrTV
cd UrTV
optimize/setup.sh                         # يحتاج Python 3.10–3.13 (لا 3.14): uv python install 3.12
optimize/find-tv.sh pair <IP:port> <code> # مرة واحدة، من نافذة «Pair device with pairing code»
optimize/venv/bin/python -I optimize/tv.py --probe   # اضغط Allow على الشاشة
optimize/precheck.sh
```

### ٢. بناء التطبيقين (بهذه التعديلات)

يحتاج Android SDK و Java 17 و Gradle 8.8:

```bash
optimize/venv/bin/python src/build.py
ANDROID_HOME=~/Android/Sdk GITHUB_RUN_NUMBER=135 gradle -p android assembleRelease
cp android/app/build/outputs/apk/release/app-release.apk           optimize/urtv-screensaver.apk
cp android/launcher/build/outputs/apk/release/launcher-release.apk optimize/urtv-home.apk
```

التطبيقان يُوقَّعان بمفتاح الـ debug حق الكمبيوتر (`~/.android/debug.keystore`). **احتفظ بنفس المفتاح** لأي تحديث بعدين،
وإلا يلزم حذف التطبيقين وإعادة تركيبهما (وتضيع إعداداتهما).

### ٣. التسريع والتركيب

```bash
optimize/run.sh                           # يحفظ حالة الشاشة، يسرّعها، يركّب كل شيء، ويقيس قبل وبعد (٢٠–٣٠ دقيقة، يعيد التشغيل مرتين)
URTV_LOCAL_APKS=1 optimize/install.sh saver      # نسختنا من شاشة التوقف بدل نسخة المؤلف
URTV_LOCAL_APKS=1 optimize/install.sh launcher   # نسختنا من الواجهة الرئيسية (مع مفتاح أمان ٥ دقائق)
```

`URTV_LOCAL_APKS=1` يخلي سكربت التركيب يستخدم الملفات المبنية محلياً بدل تنزيل نسخة المؤلف من GitHub.

### ٤. الإعدادات (مثال الجيزة)

```bash
optimize/saver.sh city الجيزة 30.0131 31.2089 Africa/Cairo
optimize/saver.sh find-team الأهلي                     # يعطيك رقم الفريق
optimize/saver.sh teams "138995:Al Ahly" "138997:Zamalek"
optimize/saver.sh pmethod egypt
optimize/saver.sh bg egypt
optimize/saver.sh markets egp
optimize/saver.sh mkstyle prices
optimize/saver.sh card verse                           # جنب الساعة: آية اليوم
optimize/saver.sh show                                 # يعرض كل الإعدادات
optimize/dock.sh apps                                  # ثم: optimize/dock.sh set <pkg> <pkg> ...
optimize/check-home.sh                                 # يتأكد إن زر Home يفتح الواجهة الجديدة
optimize/keep.sh                                       # لازم ينتهي بـ «Protected features: all on.»
```

### ٥. التلاوة والأذان

«تلاوة» (القرّاء والإذاعات) والأذان التلقائي داخل الواجهة الرئيسية: زر «تلاوة» أعلى الشاشة. **الأذان التلقائي متوقف
افتراضياً**، وأي إعادة تركيب للواجهة (مثل خطوة ٣ بمفتاح مختلف) تمسح بياناتها، فيرجع متوقفاً وتضيع «متابعة التلاوة».
بعد كل تركيب:

```bash
optimize/music.sh adhan on          # الأذان التلقائي في أوقات الصلاة (والتلفزيون شغّال)
optimize/music.sh adhan             # يتأكد: adhan=on ووقت الأذان القادم
optimize/music.sh adhan voice 3     # اختياري: المؤذن (١ إلى ٢١، منها القاهرة)، و adhan volume 60 للصوت
```

باقي الإعدادات (شكل البطاقة، الأرقام، العدّ التنازلي، صورك الخاصة…) في [docs/SETTINGS.md](docs/SETTINGS.md)، وأوامر
التلاوة كلها في [docs/TILAWAH.md](docs/TILAWAH.md).

## الرجوع للوضع الافتراضي

### رجوع كامل: الشاشة مثل ما جات من المصنع

```bash
cd UrTV
optimize/reset.sh
```

يرجّع كل شيء: التطبيقات المطفأة ترجع تشتغل، واجهة Google TV وشاشة التوقف الأصلية ترجع، يحذف تطبيقي تلفزيونك و TV Bro و
SmartTube (إذا ركّبها التركيب)، ويرجّع إعدادات السرعة لافتراضي أندرويد. صورك وحساباتك وتطبيقاتك الثانية ما تتأثر.
لازم يتشغّل من نفس المجلد اللي تم منه التركيب (فيه ملفات النسخة الاحتياطية في `optimize/run/`).

ملاحظة: الشبكات القديمة اللي انمسحت (انظر ملاحظات Haier) ما ترجع بالأمر؛ إذا احتجتها اتصل عليها من جديد بكلمة السر.

### رجوع جزئي: خيار واحد فقط

| تبي ترجع | الأمر |
|---|---|
| صور السعودية الأصلية | `optimize/saver.sh bg saudi` |
| مواقيت أم القرى | `optimize/saver.sh pmethod umm` |
| الأسواق الأصلية (تاسي، S&P، بيتكوين) | `optimize/saver.sh markets tasi,spx,btc` ثم `optimize/saver.sh mkstyle arrows` |
| واجهة Google TV بدل تلفزيونك (مع بقاء شاشة التوقف) | `optimize/restore.sh` |
| شاشة التوقف الأصلية حق التلفزيون بدل شاشتنا | `optimize/saver.sh off` (وترجع بـ `optimize/saver.sh on`) |
| تطبيق أطفأه التسريع | `optimize/keep.sh off` يعرضها، ثم `optimize/keep.sh restore <package>` |

### الرجوع لنسخة المؤلف الأصلية (بدون تعديلات مصر)

نسخة المؤلف موقّعة بمفتاح ثاني، فالسكربت يحذف نسختنا ويركّب نسخته (تضيع الإعدادات، فأعد خطوة ٤):

```bash
optimize/install.sh saver
optimize/install.sh launcher
```

ونسخة المؤلف تحدّث نفسها تلقائياً من مستودعه.

## الملفات اللي تغيّرت عن الأصل

- `src/template.html`: طريقة الحساب المصرية، الخلفيات (مصر، ثابتة، سادة)، سوق الدولار بالجنيه.
- `src/build.py`: يضيف قائمة صور مصر للصفحة.
- `assets/egypt/`: الصور العشر و `egypt.json` (العنوان والمصدر والترخيص لكل صورة).
- `android/app/…/screensaver/`: الإعدادات الجديدة (`Prefs`، `SettingsActivity`، `SaverReceiver`)، وتقديم صور مصر من داخل
  التطبيق (`PhotoCache`)، والتحديث التلقائي متوقف افتراضياً.
- `android/launcher/…/home/`: الخلفية حسب الاختيار (`MainActivity`، `ConfigSnapshot`)، والأذان بالطريقة المصرية
  (`tilawah/Place`، `PrayerMath`، `PrayerTimes`).
- `optimize/saver.sh`: الأوامر `bg` و `bgfix` و `pmethod`، وسوق `egp`.
- `optimize/install.sh`: الخيار `URTV_LOCAL_APKS=1`.

## حقوق الصور

صور مصر من [Wikimedia Commons](https://commons.wikimedia.org)، كل صورة بترخيصها، ومعدّلة (قصّ إلى ١٩٢٠×١٠٨٠):

| الصورة | المصوّر | الترخيص |
|---|---|---|
| أهرامات الجيزة | Ricardo Liberato | CC BY-SA 2.0 |
| جمل عند الأهرامات | kallerna | CC BY-SA 3.0 |
| أبو الهول | Petar Milošević | CC BY-SA 4.0 |
| منطاد فوق الأقصر | Marcosleal | CC BY-SA 3.0 |
| النيل في أسوان | Marc Ryckaert (MJJR) | CC BY 3.0 |
| معبد أبو سمبل | Diego Delso | CC BY-SA 4.0 |
| معبد الكرنك | Hamerani | CC BY-SA 4.0 |
| مكتبة الإسكندرية | Mono Abo-Abda | CC BY-SA 4.0 |
| دير سانت كاترين | Berthold Werner | CC BY-SA 3.0 |
| مسجد ابن طولون | Berthold Werner | CC BY 3.0 |

روابط الصفحات الأصلية في `assets/egypt/egypt.json`. باقي الحقوق (الصور السعودية، الخطوط، نصوص القرآن، التلاوات) في
[docs/README-ORIGINAL.md](docs/README-ORIGINAL.md#credits).

## المصدر

مبني على [tshi-dev/UrTV](https://github.com/tshi-dev/UrTV). سجلّ تعديلات المؤلف الأصلي محفوظ في تاريخ هذا المستودع.
