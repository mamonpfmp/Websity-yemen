# خطة استنساخ موقع LUcreatuweb بالعربية

سننشئ نسخة عربية كاملة من https://crearwebbarata.lovable.app بنفس الهوية البصرية (داكن مع تدرّجات سماوي↔بنفسجي) ودعم RTL، مع تفاعل كامل مع قاعدة بيانات Supabase.

## الهوية البصرية
- خلفية سوداء عميقة (#0a0a0f تقريباً)
- تدرّجات نيون: سماوي `#22d3ee` ← بنفسجي `#a855f7`
- خط عربي حديث: **Cairo** للعناوين + **Tajawal** للنصوص
- بطاقات بإطارات خفيفة وتوهج (glow) خلف الأزرار
- اتجاه `dir="rtl"` على `<html>`

## بنية الصفحات (route واحد رئيسي + روابط تمرير)
صفحة هبوط واحدة `/` تضم الأقسام التالية (مع روابط `#hash` للتنقل الداخلي فقط لأنها صفحة واحدة طويلة، كما في الأصل):

1. **Header** ثابت: شعار LUcreatuweb + روابط (الخدمات، آلية العمل، لماذا نحن، الأسئلة) + زر "ابدأ الآن" + أيقونة سلة (واجهة فقط).
2. **Hero**: شارة "تسليم مضمون خلال 24 ساعة" + عنوان كبير "متجرك أو موقعك الاحترافي جاهز خلال 24 ساعة" مع كلمات "24 ساعة" بلون متدرج نيون.
3. **خدماتنا**: بطاقات أسعار (تُجلب من جدول `services` في Cloud).
4. **لمن نصمم**: 4 بطاقات (مستقلون، أعمال محلية، متاجر فعلية، شركات جاهزة للبيع أونلاين).
5. **كيف نعمل**: 4 خطوات مرقّمة.
6. **لماذا نختارنا**: 6 ميزات (كود نظيف، responsive، SEO، تصميم، دعم، تعديلات).
7. **قوالب وأمثلة**: accordion بـ 6 فئات (متجر، شركات، مطاعم، حجوزات، بورتفوليو، جيم).
8. **التقنيات**: شريط شعارات (Shopify، WordPress، HTML5، CSS3).
9. **آراء العملاء**: قائمة مراجعات من Cloud + نموذج إضافة مراجعة جديدة.
10. **الأسئلة الشائعة**: Accordion من 5 أسئلة.
11. **التواصل**: قسم بصورة جانبية + نموذج (الاسم، البريد، الهاتف، فكرة المشروع) يُرسل إلى جدول `leads`.
12. **Footer** + Cookie banner.

## Lovable Cloud — قواعد البيانات
ثلاث جداول:
- `services` — id, name, price, features (jsonb), highlighted, sort_order
- `reviews` — id, name, email, rating, comment, created_at, approved
- `leads` — id, name, email, phone, idea, created_at

يتم إدخال محتوى مبدئي placeholder (3 باقات وهمية + 4 مراجعات نموذجية) ليملأها المستخدم لاحقاً. القراءة عبر `createServerFn` مع caching.

## RTL
- `<html dir="rtl" lang="ar">` في `__root.tsx`
- استبدال جميع `ml-*/mr-*` بـ `ms-*/me-*` و `text-left/right` بـ `text-start/end`
- استيراد خطوط Google (Cairo, Tajawal) في styles.css
- معكوس الأيقونات السهمية حيث يلزم

## SEO
- عنوان: "LUcreatuweb — متجرك الإلكتروني جاهز خلال 24 ساعة"
- وصف عربي + og tags + canonical

## ما خارج النطاق
- لا توجد صفحة متجر فعلية (السلة أيقونة فقط)
- لا نظام دفع
- لا لوحة admin (إدارة المحتوى عبر Cloud Studio مباشرة)

## تفاصيل تقنية
- ملف routes: `src/routes/index.tsx` فقط
- مكونات في `src/components/sections/` (Hero, Services, Audiences, Process, Features, Templates, Tech, Reviews, FAQ, Contact, Header, Footer, CookieBanner)
- Server functions في `src/lib/site.functions.ts`
- تصميم tokens في `src/styles.css` (ألوان neon + خطوط + تدرّجات)
- Migration واحد لإنشاء الجداول + GRANTs + RLS + سياسات (قراءة عامة للـ services/reviews approved، إدخال عام للـ leads و reviews غير معتمدة)

بعد الموافقة سأنفذ المخطط مباشرة.
