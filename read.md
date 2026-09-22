# دليل المستخدمين في Odoo 19.0

> **نسخة موحّدة مستقلة:** يجمع هذا الملف محتوى صفحات `content/applications/general/users` في دليل واحد، مع دمج موضوعات المصادقة، الصلاحيات، البوابة، واللغات. لا يعتمد ترتيب القراءة على الانتقال إلى ملفات أخرى.

## نطاق الدليل والمكوّنات والاعتمادات

يشمل هذا الدليل الصفحات التالية الموجودة في مجلد المستخدمين:

- إدارة الصلاحيات والمجموعات والمهل الزمنية ووضع المستخدم الخارق (`access_rights.rst`).
- المصادقة الثنائية (`2fa.rst`) وملفات صور خطواتها.
- تسجيل الدخول عبر Google (`google.rst`) وملفات إعداد Google Cloud.
- تسجيل الدخول عبر Microsoft Azure (`azure.rst`) وملفات إعداد Microsoft Entra.
- تسجيل الدخول عبر Facebook/Meta (`facebook.rst`) وملفات إعداد Meta.
- مصادقة LDAP (`ldap.rst`).
- تغيير اللغات (`language.rst`).
- بوابات المستخدمين (`user_portals.rst`) ومنح الوصول وتحديث معلومات البوابة.

### الاعتمادات المطلوبة

| المكوّن | الغرض | المتطلبات |
|---|---|---|
| Odoo Users & Companies | المستخدمون، الأدوار، المجموعات والصلاحيات | صلاحية مسؤول، ووضع المطوّر للخيارات التقنية |
| Contacts وPortal | إنشاء حسابات البوابة وإدارتها | جهة اتصال وبريد إلكتروني صالح |
| `auth_oauth` | تسجيل الدخول عبر Google أو Azure أو Facebook | تطبيق OAuth ومجال إعادة توجيه مطابق لعنوان Odoo |
| `auth_ldap` | تسجيل الدخول من دليل LDAP/Active Directory | عنوان الخادم والمنفذ وبيانات bind اختيارية |
| `auth_timeout` | مهلة الجلسة وعدم النشاط | تثبيت الوحدة؛ قد تأتي مع توطين محلي |
| تطبيق مصادقة | رموز 2FA الزمنية | هاتف أو مدير كلمات مرور يدعم TOTP |
| Google Cloud / Microsoft Entra / Meta | مزوّدو OAuth الخارجيون | حساب إداري لدى المزود، وبيانات Client ID |

> **تنبيه:** لا تستخدم OAuth لحساب مالك أو مدير قاعدة بيانات مستضافة على Odoo.com؛ فقد يؤدي ذلك إلى فصل القاعدة عن حساب Odoo.com ومنع نسخها أو إعادة تسميتها أو إدارتها من البوابة.

---

## 1. الصلاحيات والأدوار

الصلاحيات تحدد التطبيقات والمحتوى والسجلات التي يستطيع المستخدمون الوصول إليها أو تعديلها. يستطيع المسؤول فقط تغيير الصلاحيات. تغييرات الصلاحيات قد تؤدي إلى فقدان قدرة الإدارة (Impotent Admin)، لذلك اختبر التغييرات أولاً واستشر فريق الدعم عند تعديل قواعد حساسة.

لتعديل إعدادات مستخدم آخر يجب أن يمتلك المستخدم صلاحية **Administration: Access Rights** من:

**Settings → Manage Users → المستخدم → Access Rights → Administration → Access Rights → Save**.

### الأدوار الأربعة

- **Administrator:** مستخدم داخلي مع الميزات التقنية وإنشاء المنتجات والتصدير والصلاحيات المتقدمة.
- **User:** مستخدم داخلي يستطيع عادة إنشاء وتعديل السجلات، لكن بصلاحيات أقل من المدير.
- **Portal:** عميل أو مورّد يصل إلى بياناته من البوابة.
- **Public:** زائر الموقع أو مستخدم خارجي، وهو الأقل صلاحية.

### تعديل صلاحيات مستخدم

1. افتح **Settings → Users & Companies → Users** واختر المستخدم.
2. في تبويب **Access Rights** راجع كل تطبيق.
3. اختر المستوى المناسب من القائمة: **Blank/None**، أو **User: Own Documents**، أو **User: All Documents**، أو **Administrator**.
4. احفظ التغييرات.

تتحدد الخيارات الظاهرة حسب مجموعات الدور. اللون الأخضر يعني أن الصلاحية موروثة من صلاحية أخرى، والأحمر يعني تعارضاً، والمائل يعني صلاحية ضمنية.

### الصلاحيات التقنية والمجموعات

فعّل **Developer Mode**، ثم افتح **Settings → Users & Companies → Groups**. يمكن إنشاء مجموعة أو تعديل مجموعة موجودة:

1. اضغط **Create**، واختر التطبيق، وأدخل **Name**.
2. فعّل **Share Group** فقط إذا كانت المجموعة مخصصة لمشاركة البيانات.
3. في تبويب **Users** أضف المستخدمين.
4. في **Inherited** أضف المجموعات الموروثة؛ فالمستخدم الذي ينتمي إلى المجموعة سيرثها تلقائياً.
5. في **Menus** حدد القوائم، وفي **Views** حدد الواجهات.
6. في **Access Rights** أضف النموذج وحدد:
   - **Read:** عرض القيم الموجودة.
   - **Write:** تعديل القيم الموجودة.
   - **Create:** إنشاء قيم جديدة.
   - **Delete:** حذف القيم.
7. في **Record Rules** أضف قواعد السجلات وحدد تطبيقها على القراءة والكتابة والإنشاء والحذف.

قواعد السجلات تستخدم Domain لتصفية البيانات؛ مثال:

```python
[('mrp_production_ids', 'in', user.partner_id.commercial_partner_id.production_ids.ids)]
```

لا تعدل Domains ما لم تكن تفهم أثرها على السجلات. لإضافة صلاحية إلى مستخدم، استخدم **Technical Access Rights → Selected Groups → Add a line**. الصلاحيات في **Groups added automatically** موروثة ولا تحذف مباشرة.

### مهلة عدم النشاط ومهلة الجلسة

عند تثبيت `auth_timeout` يظهر تبويب **Timeouts** في نموذج المجموعة:

- **Inactivity:** فعّلها واختر **Screen lock** أو **Screen lock with two-factor authentication**، ثم أدخل المدة والوحدة (دقائق/ساعات/أيام).
- **Session:** فعّلها واختر **Logout** أو **Logout with two-factor authentication**، ثم أدخل مدة الجلسة والوحدة.

### وضع المستخدم الخارق

فعّل وضع المطوّر، ثم افتح قائمة التصحيح في الشريط العلوي واختر **Become Superuser**. يتجاوز هذا الوضع قواعد السجلات والصلاحيات، ولا يسمح به إلا لمن يملك **Administration: Settings**. استخدمه بحذر شديد؛ فقد تؤدي التغييرات إلى إقفال جميع المديرين. للخروج سجّل الخروج من حساب OdooBot، أو استخدم شاشة الدخول واختر **Log in as superuser**.

---

## 2. المصادقة الثنائية (2FA)

2FA تضيف رمزاً زمنياً من تطبيق مصادقة إلى كلمة المرور. أمثلة التطبيقات: Authy وFreeOTP وGoogle Authenticator وLastPass Authenticator وMicrosoft Authenticator، أو مدراء كلمات المرور مثل 1Password وBitwarden. هذه أمثلة وليست توصية حصرية.

### التفعيل

1. افتح صورة الحساب → **My Preferences → Security**.
2. اضغط **Enable 2FA**، وأدخل كلمة مرور Odoo ثم **Confirm Password**.
3. امسح رمز QR بتطبيق المصادقة. إذا تعذر المسح، اختر **Cannot scan it?** وأدخل السر يدوياً.
4. أدخل رمز التحقق ذي الستة أرقام واضغط **Enable Two-Factor Authentication**.
5. سجّل الخروج. عند الدخول أدخل اسم المستخدم وكلمة المرور ثم رمز المصادقة.

إذا فقد المستخدم تطبيق المصادقة، يجب على مسؤول تعطيل 2FA للحساب قبل أن يتمكن من الدخول.

### فرض 2FA

من **Settings → Permissions** فعّل **Enforce two-factor authentication**، واختر **Employees only** أو **All users**، ثم **Save**. خيار **All users** يشمل مستخدمي البوابة. تكرار طلب الرمز قد ينتج من إعدادات مهلة الجلسة أو عدم النشاط للمجموعات.

---

## 3. تسجيل الدخول عبر Google

### إعداد Google Cloud

1. افتح Google API Dashboard وأنشئ مشروعاً أو اختر المشروع الصحيح.
2. افتح **OAuth consent screen**، واختر **Internal** لحسابات Workspace أو **External** للحسابات الشخصية، ثم أكمل بيانات التطبيق.
3. في الوضع External التجريبي أضف المستخدمين في **Test users**؛ الحد المذكور في الوضع التجريبي 100 مستخدم.
4. افتح **Credentials → Create Credentials → OAuth client ID**.
5. اختر **Web Application**.
6. في **Authorized redirect URIs** أدخل:

```text
https://<odoo-domain>/auth_oauth/signin
```

مثال: `https://mydomain.odoo.com/auth_oauth/signin`.

7. اضغط **Create** وانسخ **Client ID**. احتفظ بـ **Client Secret** في مكان آمن.

### تفعيل Odoo

1. افتح **General Settings → Integrations** وفعّل **OAuth Authentication** ثم احفظ.
2. فعّل **Google Authentication**، ألصق **Client ID** واحفظ. يمكن الوصول إلى السجل من **Integrations → OAuth Providers**.
3. عند أول دخول اضغط **Log in with Google**. المستخدم الحالي قد يحتاج إلى إعادة ضبط كلمة المرور للوصول إلى شاشة إعادة الضبط، بينما المستخدم الجديد يستخدم رابط الدعوة ثم يسجل عبر Google دون إنشاء كلمة مرور جديدة.

---

## 4. تسجيل الدخول عبر Microsoft Azure

### إعداد Odoo والمعلمة النظامية

فعّل وضع المطوّر، ثم افتح **Settings → Technical → System Parameters → New** وأنشئ:

```text
Key:   auth_oauth.authorization_header
Value: 1
```

### تسجيل التطبيق في Microsoft Entra

1. افتح Azure Portal بحساب يملك صلاحية إدارة إعدادات Azure.
2. افتح **Manage Microsoft Entra ID → Add (+) → App registration**.
3. سمِّ التطبيق مثلاً `Odoo Login OAuth`.
4. اختر نوع الحساب المناسب: Single tenant للمستخدمين الداخليين أو Personal Microsoft accounts لمستخدمي البوابة.
5. اختر منصة **Web** وأدخل:

```text
https://<odoo-base-url>/auth_oauth/signin
```

6. بعد التسجيل افتح **Authentication** وفعّل **Access tokens** و **ID tokens** ثم احفظ.
7. من **Overview** انسخ **Application (client) ID**، ومن **Endpoints** انسخ **OAuth 2.0 authorization endpoint (v2)**.
8. في **Manage → API permissions** أضف Microsoft Graph → **Delegated permissions → User.Read**.

### إعداد مزود Odoo

من **Settings → Integrations** فعّل OAuth واحفظ، ثم افتح **OAuth Providers → New** وأدخل:

```text
Provider name: Azure
Client ID: <Application (client) ID>
Authorization URL: <OAuth 2.0 authorization endpoint (v2)>
UserInfo URL: https://graph.microsoft.com/oidc/userinfo
Scope: openid profile email
CSS class: fa fa-fw fa-windows
Login button label: Microsoft Azure
Allowed: مفعّل
```

احفظ. لربط حساب جديد استخدم رابط دعوة المستخدم أو صفحة إعادة ضبط كلمة المرور، واضغط **Microsoft Azure**، ثم سجّل الدخول بحساب Microsoft واقبل طلب الصلاحيات.

---

## 5. تسجيل الدخول عبر Facebook / Meta

1. افتح Meta for Developers → **My Apps → Create App**.
2. اختر **Authenticate and request data from users with Facebook Login**.
3. أدخل اسم التطبيق وبريد التواصل، راجع الشروط ثم أنشئ التطبيق.
4. من تخصيص Facebook Login افتح **Settings**، وأدخل في **Valid OAuth Redirect URIs**:

```text
https://<odoo-base-url>/auth_oauth/signin
```

5. من **App settings → Basic** أدخل سياسة الخصوصية `https://www.odoo.com/privacy`، ارفع أيقونة التطبيق، وأدخل رابط حذف بيانات المستخدم، واختر **Business and pages**، ثم احفظ.
6. انشر التطبيق من **Publish** وأكمل أي تحقق مطلوب.
7. انسخ **App ID**.
8. في Odoo فعّل وضع المطوّر، ثم **Settings → Integrations → OAuth Authentication → Save**.
9. افتح **Settings → Users & Companies → OAuth Providers → Facebook Graph**، ألصق **App ID** في **Client ID**، وفعّل **Allowed** ثم احفظ.

---

## 6. مصادقة LDAP وActive Directory

1. افتح **Settings → Integrations** وفعّل **LDAP Authentication** ثم احفظ.
2. افتح **LDAP Server → New** واختر الشركة.
3. في **Server information** أدخل عنوان الخادم والمنفذ.
4. فعّل **Use TLS** إذا كان الخادم يدعم StartTLS.
5. في **Login information** أدخل `LDAP binddn` و`LDAP password`؛ تركهما فارغين يجعل الاستعلام مجهولاً.
6. في **Process parameter** أدخل قاعدة LDAP بصيغة LDAP، مثل:

```text
dc=example,dc=com
```

واجعل المرشح:

```text
uid=%s
```

7. في **User information** فعّل **Create user** لإنشاء ملف Odoo عند أول دخول، واختر **User template**. إذا لم تختر قالباً يستخدم النظام ملف المدير.

عند استخدام Microsoft Active Directory وحدوث مشكلة رغم صحة البيانات، فعّل وضع المطوّر، ثم أنشئ System Parameter:

```text
Key:   auth_ldap.disable_chase_ref
Value: True
```

---

## 7. تغيير اللغات

### إضافة لغة

من صورة الحساب اختر **My Profile** واضغط أيقونة الكرة الأرضية بجانب **Language**، أو افتح **Settings → Languages → Add Languages**. اختر اللغات واضغط **Add**.

### اختيار لغة المستخدم

يفتح المستخدم صورة الحساب → **My Profile** ويختار اللغة. لتغيير لغة مستخدم آخر:

1. **Settings → Manage Users**.
2. اختر المستخدم.
3. افتح **Preferences**.
4. اختر لغة مثبتة من **Language**.

ترسل رسائل البريد والمستندات للمستخدم باللغة المحددة له.

---

## 8. بوابات المستخدمين

بوابة المستخدم متاحة افتراضياً، وتسمح للعملاء والمورّدين بعرض بياناتهم. يمكنهم متابعة الطلبات ودفعها، عرض الفواتير وتنزيلها ودفعها، إدارة وسائل الدفع والاشتراكات، تعديل العناوين، وضبط معاملات الاتصال بخدمات خارجية. مستخدم البوابة يملك صلاحية قراءة/عرض فقط ولا يعدّل مستندات قاعدة البيانات.

### منح وصول البوابة

1. افتح تطبيق **Contacts**.
2. أنشئ جهة الاتصال أو افتح جهة موجودة.
3. من **Actions → Grant portal access**.
4. راجع **Contact** و**Email** ووقت **Latest Authentication**.
5. أدخل بريد الدخول واضغط **Grant Access**.

عند منح الوصول لشركة، يُمنح الوصول لجهات الاتصال المرتبطة بها؛ ويمكن إزالة الوصول عن أفراد محددين. لمنح عدة مستخدمين، افتح الشركة ثم **Action → Grant portal access** واضغط **Grant Access** لكل جهة. يرسل Odoo رسالة دعوة.

### سحب الوصول

افتح جهة الاتصال، ثم **Action → Grant portal access → Revoke Access**.

### تحديث معلومات البوابة بواسطة المستخدم

من لوحة البوابة:

- **Edit information:** حدّث الاسم والبريد والهاتف واسم الشركة والعنوان وتفضيل تسليم الفواتير، ثم **Save Address** أو **Discard**. بعد إنشاء مستندات للحساب لا يمكن تغيير البلد إلا بواسطة مسؤول.
- **Payment methods:** أضف وسيلة دفع واحفظها. لا يمكن تعديل وسيلة موجودة؛ احذفها ثم أضف البيانات الجديدة.
- **Connection & Security → Change Password:** أدخل كلمة المرور الحالية والجديدة.
- **Two-factor authentication:** فعّل أو عطّل 2FA من الرابط الظاهر، مع إمكانية فتح شرح 2FA.
- **Passkeys:** اضغط **Add Passkey**، أكد كلمة المرور، سمِّ المفتاح، ثم أكمل مطالبة المتصفح. يمكن إعادة تسميته أو حذفه.
- **Log out from all devices:** أكد كلمة المرور لتسجيل الخروج من كل الجلسات عدا الحالية.
- **Delete Account:** أدخل كلمة المرور واسم الدخول للحذف النهائي غير القابل للتراجع، ويمكن إضافة البريد والهاتف إلى قائمة حظر الاتصالات.

كلمات مرور مستخدمي البوابة وOdoo.com منفصلة حتى إن استُخدم البريد نفسه.

### تحديث المستخدم بواسطة المسؤول

1. افتح **Settings → Users → Manage Users**.
2. أزل مرشح **Internal Users**، وأضف مرشح **Portal Users**.
3. افتح المستخدم المطلوب.
4. يمكن للمسؤول تعديل بريد الدخول من حقل **Email**. لا يستطيع المستخدم تغيير اسم الدخول بنفسه؛ تغيير **Login** يغير اسم المستخدم فقط.
5. من تبويب **Security** يمكن استخدام **Change password** لتغيير كلمة المرور أو **Invite to use 2FA** لإرسال دعوة المصادقة الثنائية.

---

## 9. قائمة تحقق للتنفيذ الآمن

- أنشئ نسخة احتياطية قبل تعديل المجموعات أو قواعد السجلات.
- اختبر التغيير بحساب تجريبي لا بحساب المدير الرئيسي.
- لا تمنح **Superuser** أو **Administration: Settings** إلا عند الحاجة.
- استخدم HTTPS، وقيّد عناوين إعادة التوجيه إلى نطاق قاعدة Odoo الصحيح.
- لا تشارك Client Secret أو كلمات مرور LDAP أو أسرار 2FA.
- تحقق من صلاحية `User.Read` في Microsoft Graph ومن حالة تطبيق OAuth في Google وMeta.
- راجع مهلات الجلسة عند ظهور تسجيل خروج متكرر.
- عند فقدان الوصول أو حدوث Impotent Admin، أوقف التغييرات واتصل بدعم Odoo.

## الأصول المرئية الأصلية

توجد لقطات الشاشة الأصلية في مجلد المصدر، وتشمل شاشات 2FA، مجموعات وصلاحيات المستخدمين، إعدادات Google وAzure وFacebook، وإدارة البوابة. هذا الملف يشرح الإجراء كاملاً نصياً، لذلك لا يلزم فتح صفحة أخرى لفهم خطوات الإعداد.
