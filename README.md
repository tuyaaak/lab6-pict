# Лаборатори №6 — Category-Partition ба PICT

**Оюутны нэр:** С.Туяацэцэг
**Оюутны код:** B210910804

## A1. Сонголт ба төлөөлөх утгууд

| Параметр | Сонголт | Жишээ |
|---|---|---|
| From | Valid / Frozen / Missing | A001 / A002 / A999 |
| To | Valid / Missing / SameAsFrom | B001 / B999 / A001 |
| Amount | Zero / Normal / OverBalance / OverLimit | 0 / 100 / 1500 / 900 |
| Currency | Same / Different | MNT / USD |

Орчны нөхцөл: A001 данс 1000 MNT үлдэгдэлтэй, өдрийн үлдсэн лимит 800 MNT. Валютын ханшийг 1 USD = 3500 MNT гэж туршилтын өгөгдөлд тогтоосон.

## A2. Хязгаарлалтууд

| Сонголт | Тэмдэглэгээ | Үндэслэл |
|---|---|---|
| From Missing | ERROR | Эх данс байхгүй |
| From Frozen | ERROR | Данс царцаагдсан |
| To Missing | ERROR | Хүлээн авах данс байхгүй |
| To SameAsFrom | ERROR, IF | Өөр рүүгээ шилжүүлж болохгүй |
| Amount Zero | ERROR, SINGLE | Тэг утгыг тусад нь шалгана |
| Amount OverBalance | ERROR | Үлдэгдэл хүрэлцэхгүй |
| Amount OverLimit | ERROR | Өдрийн лимит хэтэрнэ |
| Currency Different | IF | Ханш хөрвүүлсний дараах дүнг шалгана |

Бүрэн олонлог: 3 × 3 × 4 × 2 = **72**.

Сонгосон IF хязгаарлалтууд: From Missing үед Amount Normal; To Missing үед Amount Normal; To SameAsFrom үед From Valid.

Эдгээр хязгаарлалтын дараах зөвшөөрөгдсөн хослол: **72 → 32**.

Олон алдааны давуу эрэмбэ: **From → To → Amount → Currency**. Энэ нь кодоор батлагдсан дүрэм биш, тестийн хүлээгдэх үр дүнг нэг утгатай болгох зорилготой сонголт.

## A3. Тестийн спецификаци

| ID | From | To | Amount | Currency | Oracle |
|---|---|---|---|---|---|
| TC01 | Valid | Valid | Normal | Same | 201 OK |
| TC02 | Missing | Valid | Normal | Same | 200 ERROR_NO_FROM |
| TC03 | Frozen | Valid | Normal | Same | 200 ERROR_FROZEN |
| TC04 | Valid | Missing | Normal | Same | 200 ERROR_NO_TO |
| TC05 | Valid | SameAsFrom | Normal | Same | 200 ERROR_SAME_ACCOUNT |
| TC06 | Valid | Valid | Zero | Same | 200 ERROR_BAD_AMOUNT |
| TC07 | Valid | Valid | OverBalance | Same | 200 ERROR_INSUFFICIENT |
| TC08 | Valid | Valid | OverLimit | Same | 200 ERROR_LIMIT |
| TC09 | Valid | Valid | Normal | Different | 201 OK |
| TC10 | Missing | Missing | Normal | Same | 200 ERROR_NO_FROM |
| TC11 | Frozen | Missing | Normal | Different | 200 ERROR_FROZEN |

TC06 нь SINGLE тест. TC10, TC11 нь олон алдаа зэрэг илэрсэн нөхцөл. Эдгээрийн oracle-ийг өмнө сонгосон эрэмбээр тодорхойлсон.

## B. PICT pairwise туршилт

PICT-ийн эх кодыг repository-оос гадна бүтээсэн. Нотолгоо: `results/pict-build.txt`.

Моделиуд: `browser.model`, `transfer-nc.model`, `transfer.model`, `transfer-neg.model`.

PICT нь greedy арга ашигладаг тул заавал хамгийн бага мөртэй covering array үүсгэхгүй. Category-Partition нь алдааны утга болон oracle-ийг тодорхойлоход, PICT нь параметрүүдийн хослолын хамралтыг үүсгэхэд тохиромжтой.

## Дүгнэлт

Энэ лабораторид Category-Partition болон PICT гэсэн хоёр аргыг ашиглан тест тохиолдол боловсруулсан. Category-Partition аргаар банкны шилжүүлгийн параметрүүдийг сонголтуудад хуваасан. Сонголт бүрд төлөөлөх утга тодорхойлж ERROR, SINGLE, IF тэмдэглэгээ хэрэглэсэн. Олон алдаа зэрэг гарахад хүлээгдэх үр дүн тодорхойгүй тул давуу эрэмбэ сонгосон. PICT-ээр параметрүүдийн pairwise хослолыг автоматаар үүсгэсэн. Хязгаарлалтгүй модельд олон алдаатай хослол үүсэх боломжтой. Сөрөг утгыг ~ тэмдэглэгээгээр илэрхийлснээр error masking асуудлыг багасгаж болно. Гараар боловсруулсан тестүүд алдааны утга, хүлээгдэх үр дүнг илүү тодорхой харуулсан. Харин PICT нь олон хослолыг системтэйгээр хамрахад давуу талтай. Иймээс эдгээр аргыг хамтад нь ашиглах нь илүү тохиромжтой гэж үзсэн.
