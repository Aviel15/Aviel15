---
name: facebook-campaigner
description: ניהול והעלאת מודעות מטא של MAYRO (אביאל) - משגר המודעות, מבנה טסטים ABO, הגדרות קבועות למודעה, שמות, UTM, כיבוי אופטימיזציות, וכללי קניית מדיה מהקורס של דניאל. חובה לפני כל העלאת מודעות, "תעלה את התור", יצירת קריאייטיב/מודעה/אד-סט, החלטה על תקציב, סקייל, כיבוי מודעות או ניתוח ביצועי קמפיין.
---

# facebook-campaigner: מנהל הקמפיינים של MAYRO

אביאל הוא יזם איקומרס ישראלי (מותג MAYRO). מדברים איתו בעברית, ישר לעניין, בלי הקדמות.
הסקיל הזה מכיל את כל ההגדרות הקבועות. אל תשאל שוב דברים שכתובים כאן.

## 0. מקור האמת: קודם קוראים את המשגר

כל מה שמשתנה (מזהי קמפיין ואד-סטים, מוצרים, זוויות, קופי, קונספטים, UTM, גילוי AI) נשמר ב-db של המשגר. לכן הסקיל לא צריך עדכון בכל שינוי.

**בתחילת כל משימה**, עם `ArtifactData` ו-`url` = `https://claude.ai/artifact/5FGAHTubpdL7xADDKrL5kU`:
1. `get` את `config/main`.
2. `list` את `products` ואת `angles`.

אם יש סתירה בין ה-db לבין הסקיל, **ה-db גובר**. שינוי שאביאל מבקש (מזהה חדש, זווית חדשה, קופי חדש) נכנס ל-db, לא לסקיל.

## 1. נכסים קבועים

| מה | ערך |
|---|---|
| חשבון מודעות | `800413478935513` (Mayro, ILS) |
| עמוד ראשי | Mayro - מאיירו `410155838840115` |
| עמודים נוספים | גברים אמיתיים ממליצים `1346342818559497` · אמהות בודקות וממליצות `1343145742209647` |
| אינסטגרם | mayro.il, `instagram_user_id` = `17841462817997998`. מחובר לחשבון המודעות, ו-`ads_get_ig_accounts` מחזיר אותו. מקור האמת הוא `config.igUserId` |
| פיקסל | Mayro Pixel `780716898210630` |
| דומיין | mayro-il.com, Domain ID `2929779687223296` |
| חשבון מושבת | `619416970748434` (SMX) - לא לגעת |

בכל קריאה ל-Meta Ads MCP: `client_model` לפי המודל הנוכחי, ו-`client_conversation_id` שחוזר מהקריאה הקודמת.

## 2. מבנה החשבון (טסטים)

- **קמפיין טסטים ABO אחד לכל מוצר**: `TESTING | ABO | <שם המוצר>` (למשל `TESTING | ABO | Bamboo Boxer`, `TESTING | ABO | Bright Soap`). מטרה: מכירות, אופטימיזציה לרכישה (OFFSITE_CONVERSIONS).
- **אד-סט לכל קונספט**: `Voice over` / `AI Animation` / `Static Images` / `Ugly Ads` (בחשבון קיימים גם שמות כמו VO, AI, Image statics, Ugly ads Video). כמה זוויות יכולות לשבת באותו אד-סט.
- **מזהי קמפיין ואד-סטים**: ב-`products/<id>.campaignId` וב-`products/<id>.conceptAdsets` (למשל `{"VO": "<adSetId>"}`). אם חסר מזהה, מחפשים לפי שם הקונספט בתוך הקמפיין (גם עם `object_state=draft`) ושומרים אותו ב-db.
- **תקציבים**: אביאל מגדיר אותם בעצמו. לא לגעת בתקציב, לא ליצור קמפיין ולא ליצור אד-סט בלי בקשה מפורשת. אם האד-סט של הקונספט לא קיים, משאירים את הפריט בתור עם הערה ושואלים.
- **אף פעם לא תמונה וסרטון באותו אד-סט.** Static Images = רק תמונות.

## 3. מפרט מודעה קבוע

| שדה | ערך |
|---|---|
| קישור | דף המוצר (למכשיר האדים: `https://www.mayro-il.com/products/%D7%9E%D7%9B%D7%A9%D7%99%D7%A8-%D7%90%D7%93%D7%99%D7%9D`) |
| Primary text | 2 הווריאציות הקבועות של הזווית. מילה במילה מ-`steam-copy.md` (וגם במשגר, collection `angles`). לבחור לפי הזווית שנבחרה בדרופ-דאון, ולא לערבב בין זוויות |
| Headline | תמיד 2: `הקלה ביובש בעיניים תוך 5 דקות מהבית` · `30 ימי אחריות. לא אהבת? לא שילמת.` (מכשיר האדים) |
| Description | תמיד ריק |
| CTA | תמיד `ORDER_NOW` |
| URL parameters | `utm_source={{site_source_name}}&utm_medium={{adset.name}}&utm_campaign={{campaign.name}}&utm_content={{ad.name}}` (תמיד, בדיוק ככה) |
| Display link | www.mayro-il.com |
| סטטוס | לפי מה שנבחר בתור (ברירת מחדל PAUSED) |

**שם המודעה** = שם הקובץ בלי סיומת. אם השם לא נגמר ב-`_מספר`, מוסיפים `_1`, `_2`… לפי הסדר. בדרייב זה מסודר כך: תיקייה לכל זווית ובתוכה הוקים, למשל `זווית 1 טיפות_1`, `זווית 1 טיפות_2`, `זווית 1 טיפות_3` (כל קובץ = הוק אחר = מודעה אחרת). המשגר כבר מחשב את השם ושומר אותו ב-`adName`.

**זוויות מכשיר האדים**: 1 תלות בטיפות (`steam-a1`) · 2 עיניים דומעות ושורפות (`steam-a2`) · 3 צריבה ותחושת חול (`steam-a3`). הקופי הקובע נמצא ב-db (`angles/<id>.primaryTexts`). [steam-copy.md](steam-copy.md) הוא גיבוי בלבד.

## 4. הגדרות קבועות ואופטימיזציות: הכול כבוי

**Ad setup**:
- Create ad, Manual upload, Single image or video.
- Multi-advertiser ads: **כבוי**.

**Identity**:
- Facebook Page: Mayro - מאיירו `410155838840115`.
- Instagram profile: mayro.il `17841462817997998`.
- Threads: Use Instagram account.
- WhatsApp: Use Facebook Page.

**Personalized destinations**: Optimize website destination, Product browsing ו-Browser add-ons, שלושתם **Off**.

**שימו לב**: בקריאייטיבים קיימים מופיעים `translate_voiceover` ו-`ig_video_native_subtitle` דלוקים. במודעות חדשות שלנו הם חייבים להיות כבויים.


לפי צילומי המסך של אביאל, בכל מודעה:
- Creative setup **0/3**: Site links, Summaries, Website highlights כבויים.
- Show products: **כבוי** (בלי קטלוג).
- Advantage+ creative enhancements **0/5**: Sticker CTA, Video touch-ups, Text improvements ועוד 2, הכול כבוי.
- Essential enhancements **0/3**: Relevant comments, Enhance CTA, Add video effects כבויים.
- Related media **0/10**: כבוי לגמרי.

ביישום: ב-`ads_create_ad` להעביר `advantage_plus_creative_defaults=OFF`; ב-`ads_create_creative` להעביר `advantage_plus_creative=false` ו-`degrees_of_freedom_spec` עם OPT_OUT לכל feature רלוונטי. לפני השימוש לאמת את שמות השדות מול `ads_get_field_context`. אחרי יצירה, לבדוק ב-`ads_get_ad_preview` או בקריאת הקריאייטיב שאין שיפור שנדלק. גילוי AI: לפי `config.aiDisclosure` (כרגע `OPT_OUT`, כלומר לא לסמן כנוצר ב-AI, כולל ב-AI Animation). אם ריק, שואלים.

**degrees_of_freedom_spec שעבר אימות** (המאפיין `music` נדחה, לא לשלוח אותו):
`{"creative_features_spec":{"text_optimizations","enhance_cta","inline_comment","image_brightness_and_contrast","video_auto_crop","adapt_to_placement","add_text_overlay","description_automation","site_extensions","product_extensions","text_translation","reveal_details_over_time","show_summary","image_touchups","image_animation","translate_voiceover","ig_video_native_subtitle"}}`. כל אחד מהם עם `{"enroll_status":"OPT_OUT"}`.

**אינסטגרם**: אם מטא מחזירה "Ad account has no access to this Instagram account", צריך לחבר את mayro.il לחשבון המודעות ב-Business Settings. מסמנים error עם הסבר ולא יוצרים מודעה בלי אינסטגרם.

## 5. משגר המודעות (הממשק)

- דף: https://claude.ai/artifact/5FGAHTubpdL7xADDKrL5kU (קוד המקור: `ad-launcher.html` בריפו `aviel15/aviel15`).
- המידע נשמר ב-db של הארטיפקט. קוראים וכותבים עם `ArtifactData` ו-`url` של הדף:
  - `config/main`: נכסים, CTA, UTM, naming, concepts `[{key,name}]`, `advantageOn` (ריק = הכול כבוי), `aiDisclosure`.
  - `products/<id>`: name, url, pageId, campaignId, conceptAdsets `{VO: adSetId,...}`.
  - `angles/<id>`: productId, name, primaryTexts[], headlines[], descriptions[], cta.
  - `queue/<id>`: status (`pending`/`processing`/`done`/`error`), productId, angleId, conceptKey/conceptName, adSetId או `findAdSet {name, campaignId}`, scheduledAt (ms או null), launchStatus, adName, mediaType, mimeType, chunks `[{id,size,sha256}]`, posterAssetId, source (`asset`/`drive`), driveUrl, needsCompression, note.

### "תעלה את התור": תהליך

1. `ArtifactData query` על `queue` עם `status == pending`. לדלג על פריטים עם `scheduledAt` בעתיד.
2. לכל פריט: `update` ל-`processing` (עם `if_version`).
3. **קובץ**:
   - source=asset: להוריד כל חלק עם `Artifact read` + `path=<chunk id>`, ולחבר לפי הסדר: `cat part1 part2 … > file`. לאמת גודל ו-sha256 מול `chunks`.
   - source=drive: להוריד עם Google Drive MCP. לתיקייה, לפרוש לקבצים, וכל קובץ = מודעה.
   - וידאו מעל 100MB (`needsCompression`): לדחוס עם ffmpeg, H.264 + AAC, אותה רזולוציה, CRF/bitrate עד שיורד מתחת ל-95MB.
4. **אם בשורה כבר יש `metaVideoId`/`metaThumbHash`** (הרצה קודמת שנכשלה אחרי ההעלאה), מדלגים על שלבים 3 ו-4 ומשתמשים בהם.
   **העלאה למטא**: `ads_create_upload_url`, ואז curl POST עם כל ה-headers ו-`--data-binary`, ואז `ads_finalize_upload`.
   - וידאו: להמתין ב-`ads_get_ad_videos` עד `ready`.
   - Thumbnail: מה-poster (להוריד את `posterAssetId` ולהעלות כ-image/jpeg).
   - `rupload.facebook.com` פתוח בסביבה "מעקב מוצרים" (נבדק: HTTP 200).
5. **אד-סט**: `adSetId`; אחרת למצוא ב-`ads_get_ad_entities` אד-סט בשם הקונספט בתוך קמפיין `TESTING | ABO | <מוצר>`. אם לא נמצא: `error` עם הסבר, ולא ליצור.
6. `ads_create_creative` עם השדות הבאים:
   - page_id=`410155838840115`, instagram_user_id=`17841462817997998`.
   - video_id או image_hash.
   - primary_texts: 2 הטקסטים של הזווית. headlines: 2 הכותרות. בלי descriptions.
   - link_url: דף המוצר. CTA: ORDER_NOW. url_tags: ה-UTM. display_link: www.mayro-il.com.
   - כל האופטימיזציות כבויות, וגם multi-advertiser ו-personalized destinations.
7. `ads_create_ad` עם `ad_name=adName`, `advantage_plus_creative_defaults=OFF`, ו-`creative={"creative_id":"..."}`. אם יש `scheduledAt`, להעביר `ad_schedule_start_time` בפורמט ISO עם אזור זמן ישראל.
   - **מצב טיוטה (ברירת המחדל של אביאל)**: הקמפיין נשאר טיוטה בכוונה. אם האד-סט קיים רק בטיוטה (`ads_get_ad_entities` עם `object_state=draft`), `ads_create_ad` מכניס את המודעה לטיוטה ומחזיר `status=DRAFT`. לבדוק `active_errors` ולתקן עם `ads_update_entity`.
   - **לא לפרסם** (`ads_activate_entity` על הקמפיין) בלי בקשה מפורשת. אביאל מפרסם בעצמו. בפרסום כל המודעות עולות ACTIVE.
   - אחרי יצירה בטיוטה: `ads_update_entity` עם `{"status":"PAUSED"}` (נבדק, עובד בטיוטה), כך שגם אחרי פרסום המודעה תישאר כבויה.
   - אם האד-סט כבר פורסם, המודעה נוצרת חיה ו-PAUSED. מפעילים עם `ads_activate_entity` רק אם `launchStatus=ACTIVE`.
   - לבדוק אם יש בטיוטה מודעות זבל שמטא יצרה אוטומטית ב-quick create (למשל "New Sales Ad": קרוסלת קטלוג, עמוד שגוי, SHOP_NOW). לא למחוק לבד. להתריע לאביאל.
8. `update` ל-`done` עם `adId`, `creativeId`, `processedAt`. בשגיאה: `error` + `error` טקסט קצר בעברית.
9. אחרי done: למחוק את חלקי הוידאו מהאחסון של הארטיפקט (`Artifact delete` + `path`) כדי לפנות מקום. את ה-poster להשאיר.
10. לסכם לאביאל בשורה לכל מודעה: שם → אד-סט → Ad ID.

**תזמון**: פריטים עם `scheduledAt` מעלים בזמן שנקבע, דרך משימה מתוזמנת (create_trigger/send_later) שבודקת את התור.

## 6. כללי קניית מדיה (מהקורס של דניאל, פרק "הקניית מדיה")

**מדדים**:
- המדד היחיד להחלטות הוא **ROAS ביחד עם ספנד**.
- CPM לא מעניין: גבוה זה לא רע, ו-CPM של 70 עד 100₪ זה כנראה קהל טוב.
- CPC = CPM / (CTR×10).
- CAC = CPC×100 / CR%.
- ROAS 1 = איזון.
- AOV לחישובים: מחיר יחידה.

**סוגי קמפיינים**:
- ABO לטסטים: כל קריאייטיב מקבל צ'אנס.
- CBO (Advantage Campaign Budget) לסקייל.
- ASC רק לסקייל. זה המקום היחיד שבו מותר לערבב תמונה עם וידאו.
- DPA לא רלוונטי.

**מבנה טסט**:
- קמפיין ABO לכל מוצר, אד-סט לכל מקבץ, 3 עד 6 מודעות באד-סט.
- וריאציות קרובות של אותו בסיס: הוקים, כותרות או רקעים שונים.
- תקציב נמוך (1 עד 2 פעמים ה-CPA): עד 3 מודעות באד-סט.
- אף פעם לא תמונה וסרטון באותו אד-סט.
- ארגון לפי אווטאר × 3 זוויות. לכל זווית: כמה סרטונים × 3 הוקים, ועוד 2 סטטיים.
- הקופי (2 טקסטים ראשיים ו-2 כותרות) זהה בכל המודעות של אותה זווית.

**תקציב טסט** (מידע בלבד, את התקציבים אביאל קובע):
- 0.5 עד 1 פעמים ה-AOV לאד-סט ליום, או 1 עד 2 פעמים ה-CPA הרצוי.
- סכום כל האד-סטים יחד: לפחות 50% ממחיר המוצר. האידיאל הוא 100% עד 200%.
- תאריך התחלה מומלץ: מחר ב-05:00.

**שלב הלמידה**:
- כדי לצאת ממנו צריך כ-10 מכירות ב-3 עד 4 ימים.
- **לא נוגעים בכלום 3 ימים מלאים** (עד אמצע או סוף היום השלישי).
- דברים שפוגעים בלמידה:
  - שינוי תקציב מעל 30% ב-24 שעות.
  - עריכת קופי, קריאייטיב, כותרת, כפתור או קישור.
  - **הוספת מודעה לאד-סט רץ**.
  - שינוי טרגוט.
  - כיבוי מעל שבוע.
- אם מעלים מודעות לאד-סט שכבר רץ, צריך להזכיר לאביאל שזה מאפס למידה.

**מתי לנתח**:
- רק אחרי שכל אד-סט הוציא 1.5 פעמים ה-AOV (או 1.5 פעמים ה-CPA היעד).
- לא מחליטים לפי יום 1 עד 2.
- אם לא עבד ב-4 הימים הראשונים, כנראה לא ימריא.
- מודעה שמטא לא מוציאה עליה כסף היא לא מודעה טובה, גם אם הביאה רכישה בודדת.
- לא מכבים ומדליקים מודעות ידנית בתוך אד-סט. האלגוריתם מחלק.

**החלטות**:
- אד-סט הפסדי מאוד בלי הוספות לעגלה: סוגרים.
- היו הוספות לעגלה: לא סוגרים מהר.
- סביב איזון (BE): להשאיר עוד יומיים.
- אחרי 3 ימים ו-500 עד 1000₪ בלי תוצאה: לסגור.
- רווחי ב-20% ומעלה: להעלות תקציב ב-20% עד 30%.
- 3 ימים טובים: להעלות עד 30%.
- 3 ימים רעים: להוריד ב-20% עד 30%.

**סקייל**:
- מודעה עם 5 רכישות ומעלה ב-ROAS 2.5 ומעלה: משכפלים לקמפיין סקייל נפרד לכל מוצר או הצעה.
- הקמפיין: CBO עם אד-סט אחד, או ASC.
- תקציב התחלה לפחות 2 פעמים ה-AOV.
- את המקור בטסט לא מכבים כל עוד הוא עובד.
- מעלים תקציב ב-20% כל 24 עד 48 שעות.

**כללי**:
- תנודות יומיות של 20% עד 30% זה נורמלי.
- 95% מההצלחה מחוץ לחשבון: הצעה, אתר, קריאייטיב.
- לא מקשיבים לנציגי מטא.
- מדיניות: בלי הבטחות לא ריאליות, בלי פנייה אישית שיודעת עליך יותר מדי, ובלי פגיעה בדימוי עצמי.

## 7. מה תמיד לבדוק לפני העלאה

1. לכל זווית יש 2 טקסטים ראשיים. אם חסר: לעצור ולבקש.
2. קונספט Static Images מקבל רק תמונות, ושאר הקונספטים רק וידאו.
3. האד-סט קיים. האם הוא כבר רץ? אם כן, להזכיר איפוס למידה.
4. כמות המודעות באד-סט אחרי ההוספה: מעל 6 → להזהיר.
5. UTM, CTA ו-Headlines בדיוק כמו בסעיף 3. Description ריק.
