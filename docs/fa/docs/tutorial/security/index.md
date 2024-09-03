# امنیت

روش‌های مختلفی برای مدیریت امنیت، تأیید هویت و اعتبارسنجی وجود دارد.

عموماً این یک موضوع پیچیده و "سخت" است.

در بسیاری از فریم ورک ها و سیستم‌ها، فقط مدیریت امنیت و تأیید هویت نیاز به تلاش و کد نویسی زیادی دارد (در بسیاری از موارد می‌تواند 50% یا بیشتر کل کد نوشته شده باشد).


فریم ورک **FastAPI** ابزارهای متعددی را در اختیار شما قرار می دهد تا به راحتی، با سرعت، به صورت استاندارد و بدون نیاز به مطالعه و یادگیری همه جزئیات امنیت، در مدیریت **امنیت** به شما کمک کند.

اما قبل از آن، بیایید برخی از مفاهیم کوچک را بررسی کنیم.

## عجله دارید؟

اگر به هیچ یک از این اصطلاحات اهمیت نمی دهید و فقط نیاز به افزودن امنیت با تأیید هویت بر اساس نام کاربری و رمز عبور دارید، *همین الان* به فصل های بعدی بروید.

## پروتکل استاندارد OAuth2

پروتکل استاندارد OAuth2 یک مشخصه است که چندین روش برای مدیریت تأیید هویت و اعتبار سنجی تعریف می کند.

این مشخصه بسیار گسترده است و چندین حالت استفاده پیچیده را پوشش می دهد.

در آن روش هایی برای تأیید هویت با استفاده از "برنامه های شخص ثالث" وجود دارد.

این همان چیزی است که تمامی سیستم های با "ورود با فیسبوک، گوگل، توییتر، گیت هاب" در پایین آن را استفاده می کنند.

### پروتکل استاندارد OAuth 1

پروتکل استاندارد  OAuth1 نیز وجود داشت که با OAuth2 خیلی متفاوت است و پیچیدگی بیشتری داشت، زیرا شامل مشخصات مستقیم در مورد رمزگذاری ارتباط بود.

در حال حاضر OAuth1 بسیار محبوب یا استفاده شده نیست.

پروتکل استاندارد OAuth2 روش رمزگذاری ارتباط را مشخص نمی کند، بلکه انتظار دارد که برنامه شما با HTTPS سرویس دهی شود.

/// نکته

در بخش در مورد **استقرار** ، شما یاد خواهید گرفت که چگونه با استفاده از Traefik و Let's Encrypt رایگان HTTPS را راه اندازی کنید.

///

## استاندارد OpenID Connect

استاندارد OpenID Connect، مشخصه‌ای دیگر است که بر پایه **OAuth2** ساخته شده است.

این مشخصه، به گسترش OAuth2 می‌پردازد و برخی مواردی که در OAuth2 نسبتاً تردید برانگیز هستند را مشخص می‌کند تا سعی شود آن را با سایر سیستم‌ها قابل ارتباط کند.

به عنوان مثال، ورود به سیستم گوگل از OpenID Connect استفاده می‌کند (که در زیر از OAuth2 استفاده می‌کند).

اما ورود به سیستم فیسبوک، از OpenID Connect پشتیبانی نمی‌کند. به جای آن، نسخه خودش از OAuth2 را دارد.

### استاندارد OpenID (نه "OpenID Connect" )

همچنین مشخصه "OpenID" نیز وجود داشت که سعی در حل مسائل مشابه OpenID Connect داشت، اما بر پایه OAuth2 ساخته نشده بود.

بنابراین، یک سیستم جداگانه بود.

اکنون این مشخصه کمتر استفاده می‌شود و محبوبیت زیادی ندارد.

## استاندارد OpenAPI

استاندارد OpenAPI (قبلاً با نام Swagger شناخته می‌شد) یک open specification برای ساخت APIs (که در حال حاضر جزئی از بنیاد لینوکس میباشد) است.

فریم ورک **FastAPI** بر اساس **OpenAPI** است.

این خاصیت، امکان دارد تا چندین رابط مستندات تعاملی خودکار(automatic interactive documentation interfaces)، تولید کد و غیره وجود داشته باشد.

مشخصه OpenAPI روشی برای تعریف چندین "schemes" دارد.

با استفاده از آن‌ها، شما می‌توانید از همه این ابزارهای مبتنی بر استاندارد استفاده کنید، از جمله این سیستم‌های مستندات تعاملی(interactive documentation systems).

استاندارد OpenAPI شیوه‌های امنیتی زیر را تعریف می‌کند:

* شیوه `apiKey`: یک کلید اختصاصی برای برنامه که می‌تواند از موارد زیر استفاده شود:
    * پارامتر جستجو.
    * هدر.
    * کوکی.
* شیوه `http`: سیستم‌های استاندارد احراز هویت HTTP، از جمله:
    * مقدار `bearer`: یک هدر `Authorization` با مقدار `Bearer` به همراه یک توکن. این از OAuth2 به ارث برده شده است.
    * احراز هویت پایه HTTP.
    * ویژگی HTTP Digest و غیره.
* شیوه `oauth2`: تمام روش‌های OAuth2 برای مدیریت امنیت (به نام "flows").
    * چندین از این flows برای ساخت یک ارائه‌دهنده احراز هویت OAuth 2.0 مناسب هستند (مانند گوگل، فیسبوک، توییتر، گیت‌هاب و غیره):
        * ویژگی `implicit`
        * ویژگی `clientCredentials`
        * ویژگی `authorizationCode`
    * اما یک "flow" خاص وجود دارد که می‌تواند به طور کامل برای مدیریت احراز هویت در همان برنامه به کار رود:
        * بررسی `password`: چند فصل بعدی به مثال‌های این مورد خواهیم پرداخت.
* شیوه `openIdConnect`: یک روش برای تعریف نحوه کشف داده‌های احراز هویت OAuth2 به صورت خودکار.
    * کشف خودکار این موضوع را که در مشخصه OpenID Connect تعریف شده است، مشخص می‌کند.

/// نکته

ادغام سایر ارائه‌دهندگان احراز هویت/اجازه‌دهی مانند گوگل، فیسبوک، توییتر، گیت‌هاب و غیره نیز امکان‌پذیر و نسبتاً آسان است.

مشکل پیچیده‌ترین مسئله، ساخت یک ارائه‌دهنده احراز هویت/اجازه‌دهی مانند آن‌ها است، اما **FastAPI** ابزارهای لازم برای انجام این کار را با سهولت به شما می‌دهد و همه کارهای سنگین را برای شما انجام می‌دهد.

///

## ابزارهای **FastAPI**

فریم ورک FastAPI ابزارهایی برای هر یک از این شیوه‌های امنیتی در ماژول`fastapi.security` فراهم می‌کند که استفاده از این مکانیزم‌های امنیتی را ساده‌تر می‌کند.

در فصل‌های بعدی، شما یاد خواهید گرفت که چگونه با استفاده از این ابزارهای ارائه شده توسط **FastAPI**، امنیت را به API خود اضافه کنید.

همچنین، خواهید دید که چگونه به صورت خودکار در سیستم مستندات تعاملی ادغام می‌شود.