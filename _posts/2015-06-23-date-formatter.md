---
layout : post
title : การใช้งาน Data Formater เพื่อแปลงวันที่, เวลา
---

Date Formatter  จะทำให้การแปลงข้อมูลวันที่ ให้เป็นเรื่องง่ายๆ ไม่ว่าจะแปลงเป็น วันที หรือเวลา แสดงวันที่แบบย่อๆ หรือแบบยาวๆ  ได้หลายแบบ อีกทั้งยังแสดงเวลาตาม `language` ที่ได้ตั้งค่าไว้ได้ เช่น th ก็จะแสดงภาษาไทย ถ้าเป็น en-US ก็จะแสดงภาษาอังกฤษ ลองดูตัวอย่างกันครับ

เราสามารถเรียกผ่าน `Yii::$app->formatter->as` แล้วต่อด้วย format ที่ต้องการเช่น 

- `Yii::$app->formatter->asDate()`
- `Yii::$app->formatter->asDateTime()`
- `Yii::$app->formatter->asEmail()`
- `Yii::$app->formatter->asUrl()`
- `Yii::$app->formatter->asPercent()`
- `Yii::$app->formatter->asBoolean()`

จริงๆ มีเยอะกว่านี้ สามารถดูเพิ่มเติมที่ [Class Refferent](http://www.yiiframework.com/doc-2.0/yii-i18n-formatter.html#asBoolean()-detail) ได้

ในส่วนตัวอย่างจะเน้นเฉพาะที่เราได้ใช้บ่อยๆ คือ `asDate`,`asDateTime`

> เราสามารถเปลี่ยนการแสดงผลเป็นภาษาไทยได้ โดยไปที่ config/main.php เพิ่ม `language=>'th'` เข้าไป ซึ่งโดยปกติค่า default จะเท่ากับ `en-US`

## asDate()
เป็นการแปลงข้อมูลให้แสดงผลในรูปแบบเฉพาะวันที่ แม้ว่าค่าที่รับเข้ามาจะมี เวลาอยู่ก็จะแสดงเฉพาะวันที่

`asDate()` มีการรับ parameter 2 ตัว

- ตัวแรกจะรับค่าเป็นวันที่หรือ timestamp ก็ได้ เล่น `time()` ,`2015-06-23`, `2015-06-23 22:00:31`
- ตัวที่สองเป็นการระบุ format ที่จะให้มันแสดงผล ซึ่ง asDate ก็มีให้  4 แบบคือ `short`,`medium`,`long`,`full`

ตัวอย่างการเรียกใช้งาน

```php
<?php

$time = time();

Yii::$app->formatter->locale = 'en-US';
echo Yii::$app->formatter->asDate($time, 'short');
echo Yii::$app->formatter->asDate($time, 'medium');
echo Yii::$app->formatter->asDate($time, 'long');
echo Yii::$app->formatter->asDate($time, 'full');

Yii::$app->formatter->locale = 'th';
echo Yii::$app->formatter->asDate($time, 'short');
echo Yii::$app->formatter->asDate($time, 'medium');
echo Yii::$app->formatter->asDate($time, 'long');
echo Yii::$app->formatter->asDate($time, 'full');

// กำหนดเอง
// ICU format
echo Yii::$app->formatter->asDate($time, 'yyyy-MM-dd'); // 2014-10-06
// PHP date()-format
echo Yii::$app->formatter->asDate($time, 'php:Y-m-d'); // 2014-10-06

```
> [ICU format ดูที่นี่](http://userguide.icu-project.org/formatparse/datetime), PHP date() [ดูทีนี่](http://php.net/manual/en/function.date.php)

แสดงผล

```
6/24/15
Jun 24, 2015
June 24, 2015
Wednesday, June 24, 2015

24/6/2015
24 มิ.ย. 2015
24 มิถุนายน 2015
วันพุธที่ 24 มิถุนายน ค.ศ. 2015

2015-06-24
2015-06-24
```

## asDateTime()

จะคล้ายกับ adDate แต่เพิ่มเวลาเข้ามา

ตัวอย่างการเรียกใช้งาน

```php
<?php

$time = time();

Yii::$app->formatter->locale = 'en-US';
echo Yii::$app->formatter->asDateTime($time, 'short');
echo Yii::$app->formatter->asDateTime($time, 'medium');
echo Yii::$app->formatter->asDateTime($time, 'long');
echo Yii::$app->formatter->asDateTime($time, 'full');

Yii::$app->formatter->locale = 'th';
echo Yii::$app->formatter->asDateTime($time, 'short');
echo Yii::$app->formatter->asDateTime($time, 'medium');
echo Yii::$app->formatter->asDateTime($time, 'long');
echo Yii::$app->formatter->asDateTime($time, 'full');

// กำหนดเอง
// ICU format
echo Yii::$app->formatter->asDateTime($time, 'yyyy-MM-dd'); // 2014-10-06
// PHP date()-format
echo Yii::$app->formatter->asDateTime($time, 'php:Y-m-d'); // 2014-10-06

```
> [ICU format ดูที่นี่](http://userguide.icu-project.org/formatparse/datetime), PHP date() [ดูทีนี่](http://php.net/manual/en/function.date.php)

แสดงผล

```
6/24/15 5:32 AM
Jun 24, 2015 5:32:54 AM
June 24, 2015 5:32:54 AM GMT+07:00
Wednesday, June 24, 2015 5:32:54 AM Indochina Time


24/6/2015, 5:32
24 มิ.ย. 2015, 5:32:54
24 มิถุนายน 2015, 5 นาฬิกา 32 นาที 54 วินาที GMT+07:00
วันพุธที่ 24 มิถุนายน ค.ศ. 2015, 5 นาฬิกา 32 นาที 54 วินาที GMT+07:00

2015-06-24
2015-06-24
```
> ผมตั้งค่า   `Yii::$app->formatter->locale` เพราะอยากให้เป็นความแตกต่าง 



## ตั้งคา defaut ให้กับ formatter

ไปที่ `config/main.php`

```php
//...
'components' => [
    'formatter' => [
        'dateFormat' => 'dd.MM.yyyy',
        'decimalSeparator' => ',',
        'thousandSeparator' => ' ',
        'currencyCode' => 'EUR',
   ],
],
```

เพิ่มเติม format อื่นๆ

```
echo Yii::$app->formatter->asPercent(0.125, 2); // output: 12.50%
echo Yii::$app->formatter->asEmail('cebe@example.com'); 
```