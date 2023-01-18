# Translate Text Between Languages in the Cloud

## Introduction

As a developer, language translation is a challenge you will face when developing multi-lingual websites and applications, translating user-generate content, or supporting real-time communication in an application. Amazon Translate enables you to meet this challenge by delivering accurate natural sounding translation using a cloud based deep learning API.

## Prerequisite

AWS free tier account.

## Use Case

Use Amazon Translate to enable multilingual sentiment analysis of social media content, provide the on-demand translation of user-generated content, and add the real-time translation for communications applications.

## Services Covered

Amazon Translate

## Try yourself

### Step 1 — 
- Go to Amazon Translate console.
- Click on Launch real-time translation.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/017/day17.JPG)

### Step 2 — 
- For Source language, select Arabic. 
- For Target language, select English. 
- You can see the full list of languages that Amazon Translate supports from the drop-down list.
- In scenarios where you won't know the source language, Translate can auto detect it for you.
- In the Source Code box, copy then paste the following text:
    
 
```	
    لكن لا بد أن أوضح لك أن كل هذه الأفكار المغلوطة حول استنكار  النشوة وتمجيد الألم نشأت بالفعل، وسأعرض لك التفاصيل لتكتشف حقيقة وأساس تلك السعادة البشرية، فلا أحد يرفض أو يكره أو يتجنب الشعور بالسعادة، ولكن بفضل هؤلاء الأشخاص الذين لا يدركون بأن السعادة لا بد أن نستشعرها بصورة أكثر عقلانية ومنطقية فيعرضهم هذا لمواجهة الظروف الأليمة، وأكرر بأنه لا يوجد من يرغب في الحب ونيل المنال ويتلذذ بالآلام، الألم هو الألم ولكن نتيجة لظروف ما قد تكمن السعاده فيما نتحمله من كد وأسي.

    و سأعرض مثال حي لهذا، من منا لم يتحمل جهد بدني شاق إلا من أجل الحصول على ميزة أو فائدة؟ ولكن من لديه الحق أن ينتقد شخص ما أراد أن يشعر بالسعادة التي لا تشوبها عواقب أليمة أو آخر أراد أن يتجنب الألم الذي ربما تنجم عنه بعض المتعة ؟ 
    علي الجانب الآخر نشجب ونستنكر هؤلاء الرجال المفتونون بنشوة اللحظة الهائمون في رغباتهم فلا يدركون ما يعقبها من الألم والأسي المحتم، واللوم كذلك يشمل هؤلاء الذين أخفقوا في واجباتهم نتيجة لضعف إرادتهم فيتساوي مع هؤلاء الذين يتجنبون وينأون عن تحمل الكدح والألم .
 
```       

- The results of the translation process will automatically appear in the Target language section.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/017/day17.1.JPG)

### Step 3 — 
 - In the JSON samples panel, you can see the JSON input and output. This is useful for debugging your code when using the AWS CLI or AWS SDK.
 -  For workloads that need to scale, you would want to use the Translate API via the AWS CLI or the AWS SDK instead of using the console.

![Screenshot](https://github.com/aaditunni/100DaysOfCloud/blob/main/Journey/017/day17.2.JPG)

## ☁️ Cloud Outcome

Understood how Amazon Translate can enable you to translate text using the AWS Web Console.

## Social Proof

[Blog](https://dev.to/aaditunni/translate-text-between-languages-in-the-cloud-4c3o)

[LinkedIn](https://www.linkedin.com/posts/aaditunni_100daysofcloud-aws-cloud-activity-7021028184049500162-afn-?utm_source=share&utm_medium=member_desktop)
