# Dorar.net Hadith API

السلام عليكم ورحمة الله وبركاته

منذ مدة حاولت استعمال الـ `API` الخاص [بالدرر السنية](https://dorar.net/article/389/%D8%AE%D8%AF%D9%85%D8%A9-%D9%88%D8%A7%D8%AC%D9%87%D8%A9-%D8%A7%D9%84%D9%85%D9%88%D8%B3%D9%88%D8%B9%D8%A9-%D8%A7%D9%84%D8%AD%D8%AF%D9%8A%D8%AB%D9%8A%D8%A9-API)  
لكنني واجهت بعض الصعوبات منها أن الـ `API` يتعامل مع [JSONP](https://en.wikipedia.org/wiki/JSONP) فقط وأيضًا لن تستطيع التعامل معه بالطرق العادية بسبب الـ [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)

وإن نجحت بتجنب الـ `CORS` باستعمال `JSONP` فتحصل على الناتج بهيئة `HTML`
وأيضًا لا يوجد `documentation` أو مستند ما يشرح تفاصيل هذا الـ `API` وكيفية التعامل معه

فأنا قررت عمل `API` وسيط يتجنب مشكلة `CORS` ويعطيك البيانات كهيئة `JSON`
بجانب شرح مفصل للـ `API`

- الرابط: https://dorar-hadith-api.cyclic.app/
- يتم عمل `cache` لكل عملية بحث لمدة `5` ثواني

## Documentation

### Endpoints

يحتوي الـ `API` على مجموعة من الـ `endpoint`

- عندما يبدأ الـ `endpoint` بـ `/api` فهو هكذا يبحث عن طريق الـ `API` الرسمي الخاص بالدرر السنية
- عندما يبدأ الـ `endpoint` بـ `/site` فهو هكذا يبحث عن طريق صفحة البحث الخاص بالدرر السنية

#### /v1/api/hadith/search?value={text}

للبحث عن الأحاديث يعطي `15` نتيجة

شكل الرد كـ `JSON`

```json
{
  "metadata": {
    "length": "عدد نتائج البحث",
    "page": "رقم الصفحة",
    "removeHTML": "هل عناصر الـ HTML ممسوحة أم لا",
    "isCached": "هل هذه النتائج من الـ cache أم لا"
  },
  "data": [
    {
      "hadith": "الحديث",
      "rawi": "الراوي",
      "mohdith": "المحدث",
      "book": "الكتاب",
      "numberOrPage": "رقم الحديث او الصفحة",
      "grade": "درجة الصحة"
    }
  ]
}
```

#### /v1/site/hadith/search?value={text}

للبحث عن الأحاديث يعطي `30` نتيجة

شكل الرد كـ `JSON`

```json
{
  "metadata": {
    "length": "عدد نتائج البحث",
    "page": "رقم الصفحة",
    "removeHTML": "هل عناصر الـ HTML ممسوحة أم لا",
    "specialist": "نوع الاحاديث هل هي للمتخصصين أم لا",
    "numberOfNonSpecialist": "عدد الأحاديث لغير المتخصصين",
    "numberOfSpecialist": "عدد الأحاديث للمتخصصين",
    "isCached": "هل هذه النتائج من الـ cache أم لا"
  },
  "data": [
    {
      "hadith": "الحديث",
      "rawi": "الراوي",
      "mohdith": "المحدث",
      "mohdithId": "رقم المحدث",
      "book": "الكتاب",
      "bookId": "رقم الكتاب",
      "numberOrPage": "رقم الحديث او الصفحة",
      "grade": "درجة الصحة",
      "explainGrade": "توضيح درجة الصحة",
      "takhrij": "تخريج الحديث في كتب أخرى",
      "hadithId": "رقم الحديث لاستخدامه في البحث عن الأحاديث البديلة أو الحديث البديل الصحيح",
      "hasSimilarHadith": "هل الحديث له أحاديث مشابهة أم لا",
      "hasAlternateHadithSahih": "هل الحديث له حديث صحيح بديل أم لا",
      "similarHadithDorar": "رابط الأحاديث المشابهة في موقع الدرر",
      "alternateHadithSahihDorar": "رابط الحديث الصحيح في موقع الدرر",
      "urlToGetSmilarHadith": "رابط لكي تبحث عن الأحاديث المشابهة",
      "urlToGetAlternateHadithSahih": "رابط لكي تبحث عن الحديث الصحيح",
      "hasSharhMetadata": "هل الحديث له شرح أم لا",
      "sharhMetadata": {
        "id": "رقم الشرح",
        "isContainSharh": "هل يحتوى هذا الرد على شرح الحديث أم لا؟",
        "urlToGetSharh": "رابط لكي تبحث عن شرح الحديث"
      }
    }
  ]
}
```

#### /v1/site/hadith/similar/:id

يحضر لك أحاديث مشابهة المقابلة للـ `id`

شكل الرد كـ `JSON`

```json
[
  {
    "hadith": "الحديث",
    "rawi": "الراوي",
    "mohdith": "المحدث",
    "mohdithId": "رقم المحدث",
    "book": "الكتاب",
    "bookId": "رقم الكتاب",
    "numberOrPage": "رقم الحديث او الصفحة",
    "grade": "درجة الصحة",
    "explainGrade": "توضيح درجة الصحة",
    "hadithId": "رقم الحديث لاستخدامه في البحث عن الأحاديث البديلة أو الحديث البديل الصحيح",
    "hasSimilarHadith": "هل الحديث له أحاديث مشابهة أم لا",
    "hasAlternateHadithSahih": "هل الحديث له حديث صحيح بديل أم لا",
    "similarHadithDorar": "رابط الأحاديث المشابهة في موقع الدرر",
    "alternateHadithSahihDorar": "رابط الحديث الصحيح في موقع الدرر",
    "urlToGetSmilarHadith": "رابط لكي تبحث عن الأحاديث المشابهة",
    "urlToGetAlternateHadithSahih": "رابط لكي تبحث عن الحديث الصحيح",
    "hasSharhMetadata": "هل الحديث له شرح أم لا",
    "sharhMetadata": {
      "id": "رقم الشرح",
      "isContainSharh": "هل يحتوى هذا الرد على شرح الحديث أم لا؟",
      "urlToGetSharh": "رابط لكي تبحث عن شرح الحديث"
    }
  }
]
```

#### /v1/site/hadith/alternate/:id

يحضر لك الحديث الصحيح المقابل للـ `id`

شكل الرد كـ `JSON`

```json
{
  "hadith": "الحديث",
  "rawi": "الراوي",
  "mohdith": "المحدث",
  "mohdithId": "رقم المحدث",
  "book": "الكتاب",
  "bookId": "رقم الكتاب",
  "numberOrPage": "رقم الحديث او الصفحة",
  "grade": "درجة الصحة",
  "hadithId": "رقم الحديث لاستخدامه في البحث عن الأحاديث البديلة أو الحديث البديل الصحيح",
  "hasSimilarHadith": "هل الحديث له أحاديث مشابهة أم لا",
  "hasAlternateHadithSahih": "هل الحديث له حديث صحيح بديل أم لا",
  "similarHadithDorar": "رابط الأحاديث المشابهة في موقع الدرر",
  "urlToGetSmilarHadith": "رابط لكي تبحث عن الأحاديث المشابهة",
  "hasSharhMetadata": "هل الحديث له شرح أم لا",
  "sharhMetadata": {
    "id": "رقم الشرح",
    "isContainSharh": "هل يحتوى هذا الرد على شرح الحديث أم لا؟",
    "urlToGetSharh": "رابط لكي تبحث عن شرح الحديث"
  }
}
```

#### /v1/site/sharh/:id

للبحث عن شرح لحديث واحد عن طريق الـ `id`

شكل الرد كـ `JSON`

```json
{
  "metadata": {
    "isCached": "هل هذه النتائج من الـ cache أم لا"
  },
  "data": {
    "hadith": "الحديث",
    "rawi": "الراوي",
    "mohdith": "المحدث",
    "book": "الكتاب",
    "numberOrPage": "رقم الحديث او الصفحة",
    "grade": "درجة الصحة",
    "takhrij": "تخريج الحديث في كتب أخرى",
    "hasSharhMetadata": "هل الحديث له شرح أم لا",
    "sharhMetadata": {
      "id": "رقم الشرح",
      "isContainSharh": "هل يحتوى هذا الرد على شرح الحديث أم لا؟",
      "urlToGetSharh": "رابط لكي تبحث عن شرح الحديث",
      "sharh": "شرح الحديث"
    }
  }
}
```

#### /v1/site/sharh/text/:text

للبحث عن شرح لحديث واحد عن طريقة النص المعطى

شكل الرد كـ `JSON`

```json
{
  "metadata": {
    "specialist": "نوع الاحاديث هل هي للمتخصصين أم لا",
    "isCached": "هل هذه النتائج من الـ cache أم لا"
  },
  "data": {
    "hadith": "الحديث",
    "rawi": "الراوي",
    "mohdith": "المحدث",
    "book": "الكتاب",
    "numberOrPage": "رقم الحديث او الصفحة",
    "grade": "درجة الصحة",
    "takhrij": "تخريج الحديث في كتب أخرى",
    "hasSharhMetadata": "هل الحديث له شرح أم لا",
    "sharhMetadata": {
      "id": "رقم الشرح",
      "isContainSharh": "هل يحتوى هذا الرد على شرح الحديث أم لا؟",
      "urlToGetSharh": "رابط لكي تبحث عن شرح الحديث",
      "sharh": "شرح الحديث"
    }
  }
}
```

#### /v1/site/sharh/search?value={text}

للبحث عن شرح للأحاديث يعطي `30` نتيجة

شكل الرد كـ `JSON`

```json
{
  "metadata": {
    "length": "عدد نتائج البحث",
    "page": "رقم الصفحة",
    "removeHTML": "هل عناصر الـ HTML ممسوحة أم لا",
    "specialist": "نوع الاحاديث هل هي للمتخصصين أم لا",
    "isCached": "هل هذه النتائج من الـ cache أم لا"
  },
  "data": [
    {
      "hadith": "الحديث",
      "rawi": "الراوي",
      "mohdith": "المحدث",
      "book": "الكتاب",
      "numberOrPage": "رقم الحديث او الصفحة",
      "grade": "درجة الصحة",
      "takhrij": "تخريج الحديث في كتب أخرى",
      "hasSharhMetadata": "هل الحديث له شرح أم لا",
      "sharhMetadata": {
        "id": "رقم الشرح",
        "isContainSharh": "هل يحتوى هذا الرد على شرح الحديث أم لا؟",
        "urlToGetSharh": "رابط لكي تبحث عن شرح الحديث",
        "sharh": "شرح الحديث"
      }
    }
  ]
}
```

#### /v1/site/mohdith/:id

للبحث عن معلومات عن المحدث عن طريق الـ `id`

شكل الرد كـ `JSON`

```json
{
  "metadata": {
    "isCached": "هل هذه النتائج من الـ cache أم لا"
  },
  "data": {
    "name": "المحدث",
    "mohdithId": "رقم المحدث",
    "info": "معلومات عن المحدث"
  }
}
```

#### /v1/site/book/:id

للبحث عن معلومات عن الكتاب عن طريق الـ `id`

شكل الرد كـ `JSON`

```json
{
  "metadata": {
    "isCached": "هل هذه النتائج من الـ cache أم لا"
  },
  "data": {
    "name": "الكتاب",
    "bookId": "رقم الكتاب",
    "author": "المؤلف",
    "reviewer": "المراجع",
    "publisher": "دار النشر",
    "edition": "رقم الطبعة",
    "editionYear": "سنة الطبعة"
  }
}
```

### Query

عناصر الـ `query` التي يمكنك استخدامها  
مع شرح تفصيلي أسفل الجدول

<div dir=rtl>

|              key               |                      الوصف                      |
| :----------------------------: | :---------------------------------------------: |
|            `value`             |      وهي محتوى نص الحديث المراد البحث عنه       |
|             `page`             |            لتحديد الصفحة التي تريدها            |
|          `removehtml`          |         حذف عناصر الـ `HTML` في الحديث          |
|          `specialist`          | تستخدم لتحدد نوع الاحاديث هل هي للمتخصصين أم لا |
|            `xclude`            |      كلمة أو جملة تريد استبعادها من البحث       |
|              `st`              |                تحدد طريقة البحث                 |
|              `t`               |                تحديد نطاق البحث                 |
|  <span dir=ltr >`d[]`</span>   |       تحديد درجة الحديث سواء صحيح ام ضعيف       |
|  <span dir=ltr >`m[]`</span>   |        تحديد اسماء المحدثين التي تريدهم         |
|  <span dir=ltr >`s[]`</span>   |        تحديد الكتب التي تريد البحث فيها         |
| <span dir=ltr >`rawi[]`</span> |         تحديد اسماء الرواة التي تريدهم          |

 </div>

- هذه الاقواس `[]` تفيد ان هذا العنصر يقبل اكثر من اختيار

#### value

- محتوى نص الحديث المراد البحث عنه
- مثال: `/v1/api/hadith/search?value=جملة البحث`  
   هكذا سيبحث بناءًا على جملة البحث

#### page

- لتحديد الصفحة التي تريدها
- مثال: `/v1/api/hadith/search?value=جملة البحث&page=1`  
  هكذا سيبحث في نتائج البحث للصفحة الثالثة أي المجموعة الثالثة لنتائج البحث
- القيمة الافتراضية ستكون الصفحة رقم واحد `page=1`

#### removehtml

- حذف عناصر الـ `HTML` في الحديث  
  مثل `<span class="search-keys">...</span>`
- مثال: `/v1/api/hadith/search?value=جملة البحث&removehtml=true`  
  هكذا سيمسح عناصر الـ `HTML` من نتائج البحث
- القيمة الافتراضي هي `true`

#### specialist

- تستخدم لتحدد نوع الاحاديث هل هي للمتخصصين أم لا
- قيمها هي `true` للمتخصصين و `false` لغير المتخصصين
- مثال: `/v1/api/hadith/search?value=جملة البحث&specialist=true`  
  سيعطيك أحاديث أكثر ومختلفة خاصة للمتخصصين  
  وتكون بها معلومة اضافية مثل تخريج الأحاديث في الكتب الأخرى
- القيمة الافتراضية هي `false`

#### xclude

- كلمة أو جملة تريد استبعادها من البحث مثال
- مثال: `/v1/api/hadith/search?value=جملة البحث&xclude=اليهود`  
  هكذا سيستبعد كلمة `اليهود` من البحث

#### st

- تحدد طريقة البحث بثلاثة خيارات فقط
- هذه الطرق هي: `جميع الكلمات`، `أي كلمة`، `بحث مطابق`
- قيمها هي: `w`, `a`, `p`  
  للبحث بـ `جميع الكلمات` فستكون `st=w`  
  للبحث بـ `أي كلمة` فستكون `st=a`  
  للبحث بـ `بحث مطابق` فستكون `st=p`
- مثال: `/v1/api/hadith/search?value=جملة البحث&st=p`  
   هكذا سيبحث بشكل مطابق لجملة البحث

[كل القيم وطرق البحث التي تمثلها](./data/method-search.txt)  
أو يمكنك الحصول عليها كملف `JSON` من [هنا](./data/book.json)

#### t

- تحديد نطاق البحث
- هذه النطاقات هي: `جميع الأحاديث`، `الأحاديث المرفوعة`، `الأحاديث القدسية`، `آثار الصحابة`، `شروح الأحاديث`
- قيمها هي: `*`, `0`, `1`, `2`, `3`  
  للبحث عن `جميع الأحاديث` فستكون `t=*`  
  للبحث عن `الأحاديث المرفوعة` فستكون `t=0`  
  للبحث عن `الأحاديث القدسية` فستكون `t=1`  
  للبحث عن `آثار الصحابة` فستكون `t=2`  
  للبحث عن `شروح الأحاديث` فستكون `t=3`
- مثال: `/v1/api/hadith/search?value=جملة البحث&t=1`  
  هكذا سيبحث فقط عن الأحاديث القدسية المطابق لجملة البحث

[كل القيم ونطاقات البحث التي تمثلها](./data/zone-search.txt)  
أو يمكنك الحصول عليها كملف `JSON` من [هنا](./data/degree.json)

#### d[]

- تحديد درجة الحديث سواء صحيح ام ضعيف
- قيمها هي: `0`، `1`، `2`، `3`، `4`  
  للبحث عن `جميع الدرجات` فستكون `d[]=0`  
  للبحث عن `أحاديث حكم المحدثون عليها بالصحة` فستكون `d[]=1`  
  للبحث عن `أحاديث حكم المحدثون على أسانيدها بالصحة` فستكون `d[]=2`  
  للبحث عن `أحاديث حكم المحدثون عليها بالضعف` فستكون `d[]=3`  
  للبحث عن `أحاديث حكم المحدثون على أسانيدها بالضعف` فستكون `d[]=4`
- مثال: `/v1/api/hadith/search?value=جملة البحث&d[]=3`  
  هكذا سيبحث فقط عن الأحاديث المحكوم عليها بالضعف
- يمكنك تحديد اكثر من اختيار  
  مثال: `/v1/api/hadith/search?value=جملة البحث&d[]=1&d[]=2`  
  هكذا سيبحث فقط عن الأحاديث المحكوم عليها بالصحة من ناحية المتن و الاسناد

[كل القيم والدرجات التي تمثلها](./data/degree.txt)  
أو يمكنك الحصول عليها كملف `JSON` من [هنا](./data/methodSearch.json)

#### m[]

- تحديد اسماء المحدثين التي تريدهم
- قيمها متعددة ومنها: `0`، `179`، `204`، `256`، `261` ... إلخ  
  حيث `0` تمثل `جميع المحدثين`  
  و`179` تمثل `الإمام المالك`  
  و`204` تمثل `الإمام الشافعي`  
  و`256` تمثل `البخاري`  
  و`261` تمثل `مسلم`
- مثال: `/v1/api/hadith/search?value=جملة البحث&m[]=179`  
  هكذا سيبحث فقط عن الأحاديث التي حدث بها الإمام مالك
- يمكنك تحديد اكثر من اختيار  
  مثال: `/v1/api/hadith/search?value=جملة البحث&m[]=256&m[]=261`  
  هكذا سيبحث فقط عن الأحاديث التي حدثا بها بخاري ومسلم

[كل القيم وأسماء المحدثين التي تمثلها](./data/mohdith.txt)  
أو يمكنك الحصول عليها كملف `JSON` من [هنا](./data/mohdith.json)

#### s[]

- تحديد الكتب التي تريد البحث فيها
- قيمها متعددة ومنها: `0`، `13457`، `6216`، `3088`، `96` ... إلخ  
  حيث `0` تمثل `جميع المحدثين`
  و`13457` تمثل `الأربعون النووية`  
  و`6216` تمثل `صحيح البخاري`  
  و`3088` تمثل `صحيح مسلم`  
  و`96` تمثل `الصحيح المسند`
- مثال: `/v1/api/hadith/search?value=جملة البحث&s[]=96`  
  هكذا سيبحث فقط عن الأحاديث الواردة في كتاب الصحيح المسند
- يمكنك تحديد اكثر من اختيار  
  مثال: `/v1/api/hadith/search?value=جملة البحث&s[]=6216&s[]=13457`  
  هكذا سيبحث فقط عن الأحاديث التي وردت في كتابي الأربعون النووية وكتاب صحيح البخاري

[كل القيم وأسماء الكتب التي تمثلها](./data/book.txt)  
أو يمكنك الحصول عليها كملف `JSON` من [هنا](./data/rawi.json)

#### rawi[]

- تحديد اسماء الرواة التي تريدهم
- قيمها متعددة ومنها: `0`،`1819`، `8918`، `2664` ... إلخ  
  حيث `0` تمثل `جميع الرواة`  
  حيث `1819` تمثل `أسماء بنت أبي بكر`  
  و`8918` تمثل `عمر بن الخطاب`  
  و`2664` تمثل `ابن عباس`
- مثال: `/v1/api/hadith/search?value=جملة البحث&rawi[]=1819`  
  هكذا سيبحث فقط عن الأحاديث التي رواتها أسماء بنت أبي بكر
- يمكنك تحديد اكثر من اختيار  
  مثال: `/v1/api/hadith/search?value=جملة البحث&rawi[]=8918&rawi[]=2664`  
  هكذا سيبحث فقط عن الأحاديث التي رواها عمر بن الخطاب وابن عباس
- هنا قيم ترمز لأكثر من شخص في آن واحد  
  مثل `2665` ترمز لـ `ابن عباس أو أبو هريرة`  
  و`8924` ترمز لـ `عمر بن الخطاب وأبو هريرة`  
  و`264` ترمز لـ `أبو الدرداء وأبو أمامة وعبدالله بن عمر وابن عباس وجابر بن عبدالله`

[كل القيم وأسماء الرواة التي تمثلها](./data/rawi.txt)  
أو يمكنك الحصول عليها كملف `JSON` من [هنا](./data/zoneSearch.json)

### Data

كل البيانات المتاحة والقيم التي تمثلها يمكنكم الحصول عليه من هذه الـ `endpoints`

```json
[
  {
    "endpoint": "/v1/data/book",
    "description": "احضار كل الكتب المتاحة",
    "abstractResponse": [
      {
        "key": "الكلمة  المفتاحية",
        "value": "القيمة"
      }
    ]
  },
  {
    "endpoint": "/v1/data/degree",
    "description": "احضار كل درجات الحديث المتاحة",
    "abstractResponse": [
      {
        "key": "الكلمة  المفتاحية",
        "value": "القيمة"
      }
    ]
  },
  {
    "endpoint": "/v1/data/methodSearch",
    "description": "احضار كل طرق البحث المتاحة",
    "abstractResponse": [
      {
        "key": "الكلمة  المفتاحية",
        "value": "القيمة"
      }
    ]
  },
  {
    "endpoint": "/v1/data/mohdith",
    "description": "احضار كل المحدثين المتاحين",
    "abstractResponse": [
      {
        "key": "الكلمة  المفتاحية",
        "value": "القيمة"
      }
    ]
  },
  {
    "endpoint": "/v1/data/rawi",
    "description": "احضار كل الرواة المتاحين",
    "abstractResponse": [
      {
        "key": "الكلمة  المفتاحية",
        "value": "القيمة"
      }
    ]
  },
  {
    "endpoint": "/v1/data/zoneSearch",
    "description": "احضار كل نطاقات البحث المتاحة",
    "abstractResponse": [
      {
        "key": "الكلمة  المفتاحية",
        "value": "القيمة"
      }
    ]
  }
]
```
