

# 🗄️ **`AWS Storage`** (التخزين على `AWS`)

بص، فيه شوية **نقط أساسية** لازم تبقى حاططها في دماغك كويس أوي لو بتستعد لامتحان `AWS Certified Solutions Architect - Associate` بخصوص الـ `Storage` (التخزين):

---

**المجال الأول: `Design Secure Architectures`** (إزاي تصمم بنى تحتية آمنة)

*   **🛡️ تأمين الوصول للـ `Resources`:**
    لازم تبقى بتفهم كويس إزاي تظبط مين اللي يقدر يخش على الـ `data` بتاعتك ومين لأ، وده طبعاً بنعمله باستخدام الـ `IAM` (Identity and Access Management، وهي خدمة `AWS` اللي بتدير صلاحيات المستخدمين والخدمات) والـ `Bucket Policies` 

*   **🔐 تأمين الـ `Applications` والـ `Workloads`:**
    هنا لازم تضمن إن الـ `programs` بتاعتك اللي شغالة بتكلم خدمات الـ `Storage` بشكل آمن، وده بيكون مثلاً عن طريق إنها تستخدم الـ `VPC Endpoints` (ودي نقاط اتصال خاصة وآمنة بتربط شبكتك بـ `AWS services` من غير ما تعدي على الإنترنت خالص) والـ `IAM Roles` (ودي هوية بصلاحيات مؤقتة، بتستخدمها خدمات `AWS` أو التطبيقات عشان توصل لموارد تانية).

*   **🔑 اختيار أدوات حماية الـ `Data` الصح:**
    لازم تكون بتطبق أحسن طرق الحماية زي الـ `encryption` للداتا بتاعتك، سواء وهي متخزنة (`data at-rest`، يعني الداتا وهي قاعدة مكانها مستقرة على الهارد ديسكات) أو وهي بتتبعت (`data in-transit`، يعني الداتا وهي بتتنقل عبر الشبكة)، وده بيكون باستخدام خدمات زي
     الـ `KMS` (Key Management Service، وهي خدمة `AWS` لإنشاء وإدارة مفاتيح التشفير بتاعتك).

---

**المجال التالت: `Design High-Performing Architectures`** 

*   **🚀 اختيار حلول `Storage` سريعة و`Scalable`:**
    هنا لازم تكون بتعرف تختار صح بين أنواع الـ `Storage` المختلفة اللي `AWS` بتوفرها (زي `S3`, `EBS`, `EFS`)، وده بيكون على حسب الـ `requirements` بتاعك، ولازم تبص على حاجات مهمة زي الـ `IOPS` (Input/Output Operations Per Second، اللي هي عدد عمليات القراءة والكتابة في الثانية الواحدة)، والـ `throughput` (وده حجم البيانات اللي بتتنقل في الثانية الواحدة)، وكمان الـ `latency` (يعني الوقت اللي بتاخده العملية عشان تستجيب).

---

**المجال الرابع: `Design Cost-Optimized Architectures`** (إزاي تبني بنى تحتية على قد فلوسك)

*   **💰 اختيار حلول `Storage` توفر فلوس:**
    لازم تبقى فاهم إزاي تستخدم الـ `S3 Storage Classes` (ودي أنواع التخزين المختلفة في `S3`، كل نوع ليه تكلفة وأداء مختلف) والـ `Lifecycle Policies` الفكرة إنك تنقل الداتا اللي مش بتستخدمها كتير (اللي بنسميها `infrequently accessed data`) لمخازن أرخص زي الـ `Glacier` والـ `Deep Archive` وده طبعاً عشان متدفعش فلوس على الفاضي.

## **مقدمة**





خدمة `Amazon Simple Storage Service (S3)` هي المكان اللي الأفراد، والتطبيقات، ولستة طويلة من

 خدمات `AWS` بيحفظوا فيه الداتا بتاعتهم. هي منصة ممتازة للحاجات دي:

*   الاحتفاظ بأArchives للـ `backup`، وملفات الـ `log`، ونسخ من  الـ `disaster recovery`.
*   عمل `analytics` على `big data` وهي `at rest`.
*   استضافة `static websites`.

![image-20250627010719063](assets/image-20250627010719063.png)

`S3` بيوفر `storage` رخيص وتقدر تعتمد عليه، وممكن، لو لزم الأمر، يتكامل بشكل كويس قوي مع عمليات شغالة جوه `AWS` أو براها.

ده **مش هو هو** نفس الـ `operating system volumes` اللي اتعلمتها في الفصل اللي فات؛ دي بتتحفظ على `block storage volumes` اللي بتشغل الـ `EC2 instances` بتاعتك. `S3`، على النقيض، بيوفر مساحة لـ `object storage` غير محدودة فعليًا.



![Understanding key storage systems.. (Block, File and Object storage) | by  Abhinav Vinci | Medium](assets/1Dwzy7_VfoCXPBXF_P60vGg.png)

> [!WARNING]
>
> ملحوظة مفيش حاجة اسمه update فى s3 لو ركزت شوية هتعرف ان مفيش مفهوم البلوك فعشان كده لما تعوز تعدل حاجة لازم تنزله وترفعه تاني مفيش حاجة فى الدنيا احسن من حاجة فى كل شئ فى الاخر كلها عملية trade off
> عشان كده مستحيل تتعامل فى Transactional Database (زي MySQL, PostgreSQL, SQL Server)  لان database بتتعامل مع blocks 

وبناءً على ده، في الـ `chapter` ده، هنتعلم الحاجات دي:

*   إزاي الـ `S3 objects` بتتحفظ، وبتتدار، وإزاي بنعملها `access`.
*   إزاي تختار ما بين الـ `storage classes` المختلفة عشان توصل للتوازن الصح بين الـ `durability` والـ `availability` والـ `cost`.
*   إزاي تدير الـ `life cycles` بتاعة تخزين الداتا على المدى الطويل، عن طريق إنك تدخل `Amazon S3 Glacier` في الديزاين بتاعك.
*   إيه هي الـ `AWS services` التانية اللي ممكن تساعدك في عمليات تخزين الداتا والـ `access` عليها.

## S3 Service Architecture

انت بتنظم الفايلات بتاعتك في `S3` جوه حاجة اسمها `buckets`. بشكل افتراضي، مسموحلك تعمل لحد 100 `bucket` لكل `AWS account` عندك. وزي أي `AWS service` تانية، ممكن تطلب من `AWS` إنهم يزودولك الـ `limit` ده.

مع إن الـ `S3 bucket` والمحتويات بتاعته بتبقى موجودة في `AWS region` واحدة بس، الاسم اللي بتختاره للـ `bucket` لازم يكون `globally unique` على مستوى الـ `S3 system` كله. والموضوع ده منطقي؛ لإنك غالبًا هتبقى عايز الداتا بتاعتك تكون في منطقة جغرافية معينة عشان أسباب ليها علاقة بالـ `operational needs` أو الـ `regulatory needs`. بس في نفس الوقت، فكرة إنك تقدر تعمل `reference` للـ `bucket` من غير ما تحدد الـ `region` بتاعته، ده بيبسط العملية.

ده الـ `URL` اللي هتستخدمه عشان تعمل `access` لفايل اسمه `filename` موجود جوه `bucket` اسمه `bucketname` عن طريق الـ `HTTP`:

`s3.amazonaws.com/bucketname/filename`

طبعًا، ده معناه إنك لازم تكون قادر تحقق الـ `permissions requirements` بتاعة الـ `object` ده.

ودي الطريقة اللي نفس الفايل ده بيتجاب بيها لو بتستخدم الـ `AWS CLI`:

`s3://bucketname/filename`

### Prefixes and Delimiters

زي ما اتفقنا قبل كده، `S3` في حقيقته عبارة عن `flat surface` ضخمة. يعني لو رميت فيه مليون `object`، هما فعليًا مرصوصين جنب بعض في مساحة واحدة كبيرة من غير أي فولدرات أو `subfolders` حقيقية. وده عكس تمامًا أي `file system` اتعاملنا معاه قبل كده زي اللي على جهازك.

**طيب، لو الدنيا بالبساطة دي، إزاي لما بفتح `AWS Console` بشوف فولدرات منظمة وشكلها حلو؟**

هنا بتيجي عبقرية وبساطة `S3`. النظام بيستخدم حيلة بسيطة جدًا لكن فعالة عشان يديك إحساس التنظيم ده. الحيلة دي قايمة على حاجتين: **`Prefixes`** و **`Delimiters`**.

![Take it Easy and Work on AWS S3 Files on Command Line | by Varpu Rantala |  Medium](assets/1hDwI7kuXlm7rZBPTaFIz-Q.png)

1.  **اسم الـ `Object` هو كل حاجة:**
    في `S3`، مفيش حاجة اسمها File Path. فيه حاجة واحدة بس اسمها **" الـ `Object`" (`Object Key`)**. الـ `key` ده هو الاسم unique للـ `object` جوه الـ `bucket`.

    يعني لو عندك ملف اسمه `report.pdf` وحطيته جوه فولدر اسمه `invoices`، في نظام الملفات العادي بيبقى عندك حاجتين: الفولدر والملف. في `S3`، أنت معندكش غير حاجة واحدة: `object` الـ `key` بتاعه هو `invoices/report.pdf`. **السطر ده كله على بعضه هو مجرد اسم.**

2.  **الـ `Prefix`:**
    الـ `prefix` هو أي جزء من بداية الـ `object key`.
    
    *   في المثال اللي فات (`invoices/report.pdf`)، كلمة `invoices/` تعتبر `prefix`.
    *   كلمة `invoices` لوحدها تعتبر `prefix`.
    *   كلمة `invoi` تعتبر `prefix`.
    الـ `prefix` ده بيسمحلك تعمل `query` على كل الـ `objects` اللي بتبدأ بنفس الحروف. يعني تقدر تقول لـ `S3`: "يا `S3`، هاتلي كل الـ `objects` اللي الـ `key` بتاعها بيبدأ بـ `invoices/`"، فهيجيبلك كل فواتيرك.
    
3.  **الـ `Delimiter`:**
    ده بقى هو سر الخدعة كلها. الـ `delimiter` هو مجرد حرف انت بتتفق عليه مع `S3 API` عشان يفصل مستويات التنظيم. الحرف الأشهر عالميًا هو الشرطة المايلة `/`.

    لما بتيجي تعرض محتويات الـ `bucket` بتاعك من خلال الـ `Console` أو بتعمل `API call`، انت بتقول للـ `API`: "اعرضلي محتويات الـ `bucket` ده، واعتبر إن الـ `delimiter` بتاعي هو `/`".

    **إيه اللي بيحصل جوه الـ `API`؟**
    الـ `API` بيبص على كل الـ `object keys` اللي عندك. أي `key` فيه `/`، بيعتبر كل الكلام اللي قبل الـ `/` ده "فولدر وهمي". ولو فيه كذا `object` ليهم نفس الـ `prefix` (زي `invoices/report1.pdf` و `invoices/report2.pdf`)، الـ `API` بيجمعهم كلهم تحت `prefix` واحد مشترك ويعرضهولك على شكل أيقونة فولدر اسمها `invoices`.

    **عشان كده، الـ `folder` اللي بتشوفه في الـ `console` ده مش `object` حقيقي، ده مجرد  `visual representation` لمجموعة `objects` بتشترك في نفس الـ `prefix`.** الدليل على كده، إنك لو مسحت كل الـ `objects` اللي جوه فولدر وهمي، الفولدر نفسه هيختفي، لأنه أصلًا مكنش موجود ككيان مستقل.

تخيل عندك الـ 3 `objects` دول في الـ `bucket` بتاعك:

1.  `images/cats/grumpy.jpg`
2.  `images/cats/happy.jpg`
3.  `images/dogs/sad.jpg`

في الحقيقة، دول 3 `objects` مرصوصين جنب بعض على الـ `flat surface`.

لكن لما تيجي تفتح الـ `Console` أو تكلم الـ `API` وتستخدم `/` كـ `delimiter`:
*   الـ `API` هيبص على الـ `keys` ويلاقي إن فيه `prefix` مشترك اسمه `images/`. هيعرضلك فولدر اسمه `images`.
*   لما تدوس على فولدر `images`، انت كأنك بتقول للـ `API`: "هاتلي كل اللي بيبدأ بـ `images/` واستخدم `/` كـ `delimiter` تاني".
*   الـ `API` هيبص على `images/cats/` و `images/dogs/`. هيلاقي اتنين `prefixes` جداد مشتركين، فهيعرضلك جوا فولدر `images` فولدرين تانيين اسمهم `cats` و `dogs`.

وهكذا. هي كلها حيلة برمجية ذكية بتحاكي شكل الـ `file system` اللي احنا متعودين عليه، عشان تخلي حياتنا أسهل، لكن من غير تعقيدات وتكاليف إدارة `file system` حقيقي.

مثال

```terraform
resource "aws_s3_bucket" "my_s3_demo_bucket" {
  bucket = "my-unique-s3-flat-demo-bucket-123456789" 
}
resource "aws_s3_object" "just_a_file" {
  bucket = aws_s3_bucket.my_s3_demo_bucket.id
  key    = "just_a_file.txt"
  source = "${path.root}/just_a_file.txt"
  content_type = "text/plain"
}
resource "aws_s3_bucket_object" "just_a_content" {
  bucket = aws_s3_bucket.my_s3_demo_bucket.id
  key    = "justcontent/just_a_file.txt"
  content = "Just some content in a file"
}
```

`aws_s3_bucket` ده resource اللي بتنشاء بيه s3 و bucket ده اللي بتحدد فيه اسمه ولازم يكون unique على aws بالكامل وregion بتتحط بشكل default من provider اللي انت حططه ممكن تعمل overwrite وتغير وaws_s3_object عشان تنقل ملف تحطه فى s3 و key انت فاهم ده ايه source هو مكان الملف اللي عايز انت ترفعه وبرده content_type مش الزامية هو ممكن يعرفه من امتداد الملف بشكل تلقائي والثانية انت بتنقل content تحطه فى ملف

![image-20250714195021075](assets/image-20250714195021075.png)



### **التعامل مع الـ `Large Objects`**

مع إنه مفيش حد أقصى نظريًا لكمية الداتا اللي ممكن تخزنها جوه أي `bucket`، لكن الـ `object` الواحد مينفعش حجمه يزيد عن 5 تيرابايت. وعمليات الـ `upload` المنفردة مينفعش تزيد عن 5 جيجابايت. عشان تقلل خطر إن الداتا تضيع أو عملية الـ `upload` تفشل في النص، `AWS` بتنصح إنك تستخدم `feature` اسمها **`Multipart Upload`** لأي `object` حجمه أكبر من 100 ميجابايت.

زي ما الاسم بيوحي، الـ `Multipart Upload` بيكسّر الـ `object` الكبير لأجزاء صغيرة متعددة، ويبعتهم واحد ورا التاني للـ `S3`. ولو أي جزء منهم فشل وهو بيتبعت، ممكن تعيد إرساله هو لوحده من غير ما تأثر على باقي الأجزاء اللي وصلت خلاص.

الـ `Multipart Upload` بيشتغل لوحده أوتوماتيك لو بتعمل الـ `upload` من خلال الـ **`AWS CLI`** أو من خلال **`high-level API`**. لكن لو شغال بـ **`low-level API`**، هتحتاج تكسّر الـ `object` بتاعك بنفسك بشكل يدوي.

ولو محتاج تنقل ملفات كبيرة لـ `S3 bucket`، فيه `configuration` اسمه 
**`Amazon S3 Transfer Acceleration`** ممكن يسرّعلك الدنيا. لما بتظبط الـ `bucket` إنه يستخدم `Transfer Acceleration`، عمليات الـ `upload` بتتوجه لأقرب `AWS edge location` ليك جغرافيًا، ومن هناك بتكمل طريقها باستخدام الشبكة الداخلية بتاعة أمازون نفسها (ودي أسرع بكتير من الإنترنت العادي).

عشان تعرف إذا كان الـ `Transfer Acceleration` ده هيحسن سرعة النقل فعلًا من مكانك لـ `AWS region` معينة، ممكن تستخدم أداة **`Amazon S3 Transfer Acceleration Speed Comparison tool`** . ولو اكتشفت إن عمليات النقل بتاعتك فعلًا هتستفيد من الـ `Transfer Acceleration`، المفروض تعمل `enable` للـ `setting` ده في الـ `bucket` بتاعك. وبعدها تقدر تستخدم `endpoint domain names` مخصوصة
 (زي `bucketname.s3-accelerate.amazonaws.com`) في عمليات النقل بتاعتك.

> [!IMPORTANT]
>
> براحه كده عشان نفهم هنمشي ازاي لو عايز انقل data من عندي على s3 فى aws
> وبتروح على الموقع ده
> https://s3-accelerate-speedtest.s3-accelerate.amazonaws.com/en/accelerate-speed-comparsion.html 
> ![image-20250714154215984](assets/image-20250714154215984.png)
>
> ![image-20250714154239423](assets/image-20250714154239423.png)
>
> بتشوف حجم الاستفادة اللي فى سرعة النقل ....بص حق الله انا مش عارف جاب الارقام دي ازاي بس كا طه انا بضرب كف على كف لان انا فى الغربية ازاي tokyo هتكون اسرع بالنسبة ليه من frankfurt بس هنقول ايه aws هي اللي ليك بتختار اسرع AWS edge location بالنسبة لجهازك او الجهاز اللي عليه data اللي انت عايزه 
>
> انت بس بتشوف اذا كنا فى استفادة او لاء![S3 Transfer Acceleration – Awesome Cloud – Medium](assets/1lHrgv1gFEXqNhtnpCVXH9A.png)
>
> بس خلي باللك فيه فلوس زيادة 
>
>
> ```terraform
> resource "aws_s3_bucket" "my_bucket" {
> bucket = "my-accelerated-bucket-123456"  
> }
> 
> 
> resource "aws_s3_bucket_accelerate_configuration" "acceleration" {
> bucket = aws_s3_bucket.my_bucket.id
> status = "Enabled"
> } 
> output "s3_accelerated_endpoint" {
> value = "https://${aws_s3_bucket.my_bucket.bucket}.s3-accelerate.amazonaws.com"
> }
> 
> ```
>
> وتاخد output وترفع عن طريق aws cli 
> ```bash
> $ aws s3 cp myfile.zip s3://my-accelerated-bucket-123456/ --endpoint-url https://my-accelerated-bucket-123456.s3-accelerate.amazonaws.com
> 
> ```
>
> على فكرة ممكن ترفع بـterraform بتستخدام  reference اسمه provisioner و null_resource  دا لو انت شاطر شوية 
> خلي باللك هي اللي بتختار edge location مش انت ,aws بتشوف الاسرع وبتنقل
> الموقع اللي فوق عشان تشوف لو في فايدة اساسا لو انت استخدمته او لاء

جرب بنفسك في **`Exercise 3.1`** واعمل الـ `bucket` بتاعك.

![image-20250626220607975](assets/image-20250626220607975.png)



### **الـ `Encryption`**

أي داتا بتتحط على `S3` المفروض دايمًا تكون `encrypted` (متشفرة)، إلا طبعًا لو كانت معمولة عشان تبقى متاحة للكل — زي مثلاً لو هي جزء من `website`. أنت ممكن تستخدم `encryption keys` عشان تحمي الداتا بتاعتك وهي متخزنة جوه `S3` (ودي بنسميها **`data at rest`**)، وكمان عشان تحميها وهي بتتنقل من وإلى `S3` (ودي بنسميها **`data in transit`**)، وده عن طريق إنك تستخدم الـ `encrypted API endpoints` بتاعة أمازون بس.

الـ `data at rest` ممكن نحميها بطريقتين: يا إما `server-side encryption` أو `client-side encryption`.

> [!IMPORTANT]
>
> > **Data at rest (الداتا وهي في مكانها):** دي الداتا وهي متخزنة فعليًا ومستقرة، يعني الفايلات وهي قاعدة على الهارد ديسكات في الداتا سنترز بتاعة أمازون. حمايتها معناها إن لو حد قدر يوصل للديسك نفسه، ميقدرش يقرأ الداتا اللي عليه.
> >
> > **Data in transit (الداتا وهي بتتحرك):** دي الداتا وهي في رحلتها بتتنقل من مكان للتاني عبر شبكة (زي الإنترنت). زي مثلاً وقت ما بتعمل upload لملف من جهازك لـ S3، أو بتعمل download لملف منه. حمايتها معناها إن لو حد بيتجسس على الشبكة، هيشوف كلام متلغبط ومشفّر مش هيفهم منه حاجة.



#### **الـ `Server-Side Encryption`**

الـ "server-side" هنا هو منصة `S3` نفسها. الفكرة إن `AWS` هي اللي بتعمل `encrypt` للـ `data objects` بتاعتك أول ما بتتخزن على الديسكات بتاعتها، وهي اللي بتعملها `decrypt` لما بتبعتلها `requests` سليمة وموثقة عشان تسترجع الداتا دي.

عندك تلات اختيارات ممكن تستخدمهم في النوع ده:

*   **`Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3)`**:
    في الاختيار ده، `AWS` بتستخدم الـ `keys` بتاعتها الخاصة بيها عشان تدير كل خطوة في عملية الـ `encryption` والـ `decryption`. ده أبسط نوع، انت مش بتعمل أي حاجة غير إنك بتفعّله.


```terraform
resource "aws_s3_bucket" "sse_s3_demo_bucket" {
  bucket = "my-sse-s3-demo-bucket-unique-123456789" 


  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "AES256" # This enables SSE-S3 as the default
      }
    }
  }
}
```

 **Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3)** ده يعتبر أبسط وأسهل نوع encryption ممكن تفعله على S3. كل اللي عليك إنك تحدده في الـ Terraform code أو في الـ console، وAWS بتتكفل بكل حاجة، من توليد الـ encryption keys وتخزينها لغاية rotation وحمايتها، وده طبعاً بيقلل الـ overhead بتاع إدارة الـ security عليك بشكل كبير لدرجة إن الـ Key Management بيبقى صفر. وفوق كل ده، مفيش أي تكاليف إضافية لاستخدام SSE-S3، هو free تماماً مع الـ storage العادي اللي بتدفعه لـ S3.

لكن لازم تكون واخد بالك إن ليه شوية عيوب. أولاً، مفيش Audit Trail لاستخدام الـ Keys، يعني مش هتقدر تعرف مين عمل access على الـ encryption keys اللي S3 بيستخدمها ولا إمتى، ده لأن AWS بتديرها بشكل داخلي ومش بيظهر في الـ logs بتاعتك زي CloudTrail (وده طبعاً عكس SSE-KMS). ثانياً، تحكمك في الـ Keys دي بيبقى محدود جداً، بما إن AWS هي اللي بتدير كل حاجة، فممكن ده ميكونش كافي لو عندك متطلبات compliance معينة بتفرض عليك تحكم أكبر في الـ keys. وأخيراً، الـ trust boundary بتاعتك كلها بتبقى عند AWS، يعني بما إن AWS بتدير الـ data بتاعتك وكمان بتدير الـ encryption keys بتاعتها، لو عندك شك أو قلق إن AWS نفسها ممكن توصل لداتك، فـ SSE-S3 مش هيمنع ده نظرياً لأن AWS عندها الـ keys.

![image-20250714222416958](assets/image-20250714222416958.png)

------

----

*   **`Server-Side Encryption with AWS KMS-Managed Keys (SSE-KMS)`**:
    هنا الموضوع أعمق شوية من الـ `SSE-S3`. `AWS` بتستخدم خدمة الـ `Key Management Service (KMS)`، وده بيضيف طبقة حماية زيادة اسمها `envelope key`، والأهم إنه بيديلك `audit trail` كامل تقدر تتبع منه مين استخدم الـ `key` ده وإمتى. وكمان ممكن اختيارياً تعمل `import` للـ `keys` بتاعتك الخاصة جوه خدمة `KMS`.


> [!TIP]
>
> الـ `envelope key` في سياق `SSE-KMS` هو ببساطة **الـ `Data Key` المشفر**.
>
> يعني: `KMS` بيولد `Data Key` (unique key لكل ملف) عشان يشفر الداتا بتاعتك. وبعدين بيشفر الـ `Data Key` ده نفسه باستخدام الـ `CMK` بتاعك (المفتاح الرئيسي في `KMS`).
>
> الـ `Data Key` المشفر ده هو اللي بيتسمى `envelope key` أو عشان يحمي الـ `Data Key` الأصلي اللي هو موجود فى kms ، وده بيضيف طبقة حماية زيادة.
> بس دا معناها كل عملية تشفير وفك تشفير لازم تروح تجيب master key عشان يفك تشفير data key وبعده تفك تشفير data الفعلية

بالنسبة لـ **`Server-Side Encryption with AWS KMS-Managed Keys (SSE-KMS)`**، هو بيوفرلك `Audit Trail` مفصل جداً، فـ كل عملية استخدام للـ `CMK` بتاعك (سواء تشفير أو فك تشفير) بتتسجل بالمللي في `AWS CloudTrail`، وده بيديلك سجل أمني كامل تقدر تعرف منه مين عمل `access` على الداتا وإمتى، وبيبقى مهم أوي لـ `compliance` والـ `auditing`. كمان بتلاقي تحكم دقيق في الـ `Keys` دي، بتقدر تتحكم في الـ `policies` بتاع الـ `CMK` وتحدد بالظبط مين مسموحله يستخدمه وإزاي. ده غير إن `KMS` نفسه بيوفر خدمة مدارة بتسهل عليك إدارة الـ `keys`، بما في ذلك `key rotation` تلقائي، وده طبعاً بيقلل التعقيد عليك. وفوق ده كله، `KMS` بيستخدم طريقة اسمها `envelope encryption`، اللي بتشفر الـ `data key` نفسه (اللي بيشفر الداتا) باستخدام الـ `CMK` بتاعك، وده بيضيف طبقة حماية إضافية. عندك كمان إمكانية إنك تعمل `Import` لـ `Keys` خاصة بيك لو جاي بيها من بره `AWS` وتستخدمها. ومؤخراً، في `feature` اسمها `Bucket Keys` اللي بتستخدم `bucket_key_enabled = true` ودي بتقلل تكلفة `KMS requests` بشكل كبير لو عندك ملايين الـ `objects`، لأن `S3` بيطلب `bucket key` واحد بدل `data key` لكل `object`.

> [!NOTE]
>
> لما بتفعّل `bucket_key_enabled = true` في S3 مع التشفير بـ KMS، بتقلل عدد المرات اللي S3 بيطلب فيها مفتاح التشفير من خدمة KMS. في الوضع العادي، كل ملف بيرفع أو بيتحمّل بيعمل طلب لـ KMS، وده بيتحاسب عليه. لكن لما الخاصية دي بتبقى مفعّلة، S3 بيستخدم مفتاح مؤقت مرتبط بالـ bucket نفسه (اسمه bucket key) وبيعيد استخدامه لفترة، بدل ما يطلب من KMS كل مرة. وده بيقلل عدد الطلبات على KMS وبالتالي بيقلل التكلفة، خصوصًا لو بتتعامل مع ملفات كتير. 
> الميزة دي كمان موجدة بس فى
>  DSSE-KMS (Dual-Layer Server-Side Encryption with AWS KMS Keys (DSSE-KMS))لسه هنشوف 

لكن على الناحية التانية، `SSE-KMS` ليه عيوب. أولها وأهمها هي التكلفة الإضافية، استخدام `KMS` مش مجاني، وبيتحاسب عليك على عدد الـ `CMKs` اللي انت عاملها، وكمان على كل `API request` لـ `KMS` (يعني كل عملية تشفير أو فك تشفير). وثانياً، بيعتبر تعقيد نسبي في الإعداد والإدارة عن `SSE-S3`، لأنك بتضيف خدمة `KMS` كاملة ولازم تكون فاهم الـ `permissions` بتاعتها كويس.



```terraform
resource "aws_kms_key" "my_s3_kms_key" {
  description             = "KMS key for S3 default SSE-KMS encryption"
  # deletion_window_in_days = 7 # Minimum is 7 days before delete
  # enable_key_rotation     = true # genertae new key every year
}

resource "aws_s3_bucket" "my_s3_demo_bucket" {
  bucket = "my-unique-s3-flat-demo-bucket-123456789" 
}
resource "aws_s3_bucket_server_side_encryption_configuration" "encrypt_s3" {
  
  bucket = aws_s3_bucket.my_s3_demo_bucket.id
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm     = "aws:kms"
      kms_master_key_id = aws_kms_key.my_s3_kms_key.id
    }
  }
}
resource "aws_s3_object" "send_file" {
  
  bucket = aws_s3_bucket.my_s3_demo_bucket.id
  key    = "file.txt"
  source = "${path.root}/just_a_file.txt"
  content_type = "text/plain"
}
```

![image-20250714222120297](assets/image-20250714222120297.png)

---

----



*   **`Server-Side Encryption with Customer-Provided Keys (SSE-C)`**:
    الاختيار ده بيخليك انت اللي تدي لـ `S3` الـ `key` اللي هيستخدمه في عملية الـ `encryption`. يعني مع كل `request` لرفع ملف أو قراءته، لازم تبعت الـ `key` بتاعك معاه.

**SSE-C** هو نوع من أنواع التشفير في S3 بيديك **تحكم كامل في مفتاح التشفير**، يعني إنت اللي بتولّد المفتاح وتحفظه، وAWS ما بتشيلش عندها نسخة منه خالص. ده بيخلي الطريقة دي مناسبة جدًا لو عندك سياسات أمان أو قوانين مش بتسمح إن المفاتيح تتخزن عند حد تاني، زي في بعض الشركات أو المؤسسات الحكومية. كمان لو عندك نظام خارجي هو اللي بيدير المفاتيح، أو مش عايز تعتمد على KMS من AWS، يبقى SSE-C حل كويس وسهل في بعض الحالات.

بس برضو ليه شوية عيوب مهمة. أول حاجة، **لو المفتاح ضاع، الملف كأنه عمره ما كان موجود**، AWS مش هتعرف تساعدك في استرجاعه. كمان **ماينفعش تتعامل مع الملفات دي من خلال الـ AWS Console** (اللي هو الموقع)، يعني مش هتعرف تفتح أو تنزّل الملفات من المتصفح، وهتشوف رسائل زي "Unknown Error" وخلاص.

كمان **SSE-C مش شغال مع كل خدمات AWS**، زي:

- Amazon Athena
- S3 Select
- CloudFront
- وبعض ميزات النسخ التلقائي أو التحليلات في S3

وفوق ده كله، **كل مرة ترفع أو تنزّل فيها ملف، لازم تبعت المفتاح يدوي**، وده ممكن يبقى خطر لو الاتصال مش متأمن كويس.

مثال

```terraform
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "sse_c_demo_bucket" {
  bucket = "my-sse-c-demo-bucket-unique-123456789"
  acl    = "private"
}


output "sse_c_bucket_name" {
  value       = aws_s3_bucket.sse_c_demo_bucket.bucket
  description = "Name of the S3 bucket used for SSE-C demo."
}
```



أولاً: اعمل generate للـ `Encryption Key` بتاعك :**

لازم يكون المفتاح بالضبط 32 بايت (256 بت) علشان يناسب خوارزمية AES256 ولازم يكون base64

ولازم اعمل md5 معاها 

```bash
KEY_B64=$(openssl rand -base64 32)
KEY_MD5=$(echo -n "$KEY_B64" | base64 -d | openssl dgst -md5 -binary | base64)

```



**ثانياً: ارفع الملف مشفراً بـ `SSE-C` باستخدام الـ `AWS CLI`:**

```
aws s3api put-object \
  --bucket my-unique-s3-flat-demo-bucket-123456789 \
  --key myfile.txt \
  --body ./just_a_file.txt \
  --sse-customer-algorithm AES256 \
  --sse-customer-key "$KEY_B64" \
  --sse-customer-key-md5 "$KEY_MD5"
```

لما تفتح الـ `AWS S3 Console` بعد ما تنفذ أمر الـ `CLI`، هتلاقي الملف `myfile.txt.txt` في الـ `properties`

وعشان تعمل download

```
aws s3api get-object \
  --bucket my-unique-s3-flat-demo-bucket-123456789 \
  --key myfile.txt \
  --sse-customer-algorithm AES256 \
  --sse-customer-key "$KEY_B64" \
  --sse-customer-key-md5 "$KEY_MD5" \
  ./downloaded_myfile.txt

```

![image-20250714225942485](assets/image-20250714225942485.png)
كان المفروض يظهر معايا  بس طلع مينفعش
![image-20250714230046805](assets/image-20250714230046805.png)

---

---



الـ **`Dual-Layer Server-Side Encryption with AWS KMS Keys (DSSE-KMS)`** يعتبر أحدث وأعلى مستوى من الـ `server-side encryption` اللي بتوفره `S3` بالتعاون مع `KMS`. هو بيزود طبقة `encryption` إضافية فوق `SSE-KMS` العادي، ده غير إنه بيضيف `cryptographic signature` (بيضمن `data integrity` و`non-repudiation`، يعني مش بس الداتا بتتشفر بطبقتين عشان تضمن أعلى مستوى `Security` زي kms، لأ كمان بيتم digital signature  عليها  عشان تتأكد إنها متغيرتش من ساعة ما اتخزنت في `S3`، وإنها جاية فعلاً من `S3`، وده طبعاً بيخليه مثالي لـ `workloads` اللي ليها `compliance requirements` صارمة جداً زي اللي في قطاعات `financial`, `healthcare`, و `government` اللي بتحتاج أقصى درجات التأكيد على `security` و`integrity` الداتا.

> [!TIP]
>
> هنا object data بتتشفر **مرتين** (طبقتين `encryption`) بشكل متتالي:
>
> 1.  **الطبقة الأولى:** الـ `object data` بتتشفر بواسطة `Data Key #1`. الـ `Data Key #1` ده بيتشفر بالـ `CMK` بتاعك في `KMS`.
> 2.  **الطبقة الثانية:** الـ `object` اللي بقى مشفر بالطبقة الأولى ده (يعني `encrypted data #1`) بيتم **إعادة تشفيره** (تشفر تاني) بواسطة `Data Key #2`. والـ `Data Key #2` ده برضه بيتشفر بالـ `CMK` بتاعك في `KMS`.
>
> *    الـ `object` نفسه بيعدي على عمليتين تشفير متتاليتين، عشان كده اسمها `Dual-Layer`. وده غير الـ `cryptographic signature` اللي بيضيفه عشان يضمن data integrity.
>
> 

لكن من الناحية التانية، `DSSE-KMS` ليه عيوب، أهمها التكلفة الأعلى مقارنة بـ `SSE-KMS` العادي، وده بسبب عمليات التشفير وdigital signature الإضافية في `KMS`. كمان فيه أداء أقل نسبياً، لأن طبقات الـ `encryption` الإضافية وعملية digital signature دي ممكن تأثر على الـ `latency`و`throughput`  لعمليات الكتابة والقراءة على `S3`، خصوصاً لو بتتعامل مع عدد ضخم جداً من الـ `objects` أو `high-throughput workloads`. ورغم إن `Terraform` بيسهل عليك إعداده، إلا إنه بيظل أكثر تعقيداً في الفهم والتطبيق من الأنواع التانية.



```terraform

resource "aws_kms_key" "my_dsse_kms_key" {
  description             = "KMS key for S3 DSSE-KMS encryption"
  deletion_window_in_days = 7  # Minimum 7 days before the key can be permanently deleted after scheduling
  enable_key_rotation     = true # Best practice: enables automatic key material rotation annually
}

resource "aws_s3_bucket" "dsse_demo_bucket" {
  bucket = "my-dsse-kms-demo-bucket-unique-123456789" # IMPORTANT: Change this to a globally unique name!
}

resource "aws_s3_bucket_server_side_encryption_configuration" "dsse_bucket_encryption_config" {
  bucket = aws_s3_bucket.dsse_demo_bucket.id

  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm     = "aws:kms:dsse" 
      kms_master_key_id = aws_kms_key.my_dsse_kms_key.arn 
      # bucket_key_enabled = true
    }
  }
}

resource "aws_s3_object" "dsse_encrypted_object" {
  bucket       = aws_s3_bucket.dsse_demo_bucket.id
  key          = "just_a_file.txt" 
  source       = "${path.root}/just_a_file.txt" 
  content_type = "text/plain" 
}


```



![image-20250714231701238](assets/image-20250714231701238.png)

#### **الـ `Client-Side Encryption`**

الـ `Client-Side Encryption` ده قصة تانية خالص في عالم تشفير الداتا على `S3`. الفكرة هنا إنك بتشفر الداتا بتاعتك وهي لسه عندك على جهازك أو في الـ `application` بتاعتك، قبل ما تطلع حتى من شبكتك وتروح لـ `S3`. يعني على عكس الـ `Server-Side Encryption` بأنواعه اللي فاتت، اللي `S3` هو اللي بيتولى عملية التشفير بعد ما الداتا توصل عنده، هنا أنت اللي بتعمل الشغلانة دي كلها. وده طبعاً معناه إن `encryption keys` بتاعتك أنت اللي بتديرها بالكامل.

في طريقتين رئيسيتين عشان تعمل كده:

1.  **الطريقة الأولى: باستخدام `AWS KMS–Managed Customer Master Key (CMK)`**
    هنا الفكرة إنك بتعتمد على خدمة `AWS KMS` بس مش عشان `S3` يشفر. لأ، ده عشان الـ `client` بتاعك (يعني الـ `application` أو السكريبت اللي بيعمل `upload` للداتا) هو اللي يروح لـ `KMS`. الـ `client` بيطلب من `KMS` إنه يولدله `data key` فريد لكل `object` انت عايز ترفعه. الـ `KMS` بتولد الـ `data key` ده وبتشفرلك الـ `data key` ده بالـ `CMK` بتاعك (اللي هو المفتاح الرئيسي اللي انت بتديره في `KMS`). بعد كده، الـ `client` بيستخدم الـ `data key` الـ `plaintext` ده عشان يشفر الداتا الأصلية بتاعتك على جهازك local، وأول ما الداتا تتشفر، الـ `data key` الـ `plaintext` بيتم مسحه، وبيتم رفع الداتا المشفرة ومعاها نسخة من الـ `data key` اللي كان مشفر بالـ `CMK` بتاعك. النقطة المهمة هنا إن `AWS` أو `S3` عمرهم ما شافوا الداتا بتاعتك `unencrypted`، ولا حتى الـ `data key` الـ `plaintext`.

2.  **الطريقة التانية: باستخدام `Client-Side Master Key`**
    دي معناها إنك بتجيب `encryption key` رئيسي خاص بيك أنت (مفتاح أنت اللي عملته وبتديره برا `AWS` خالص)، وده اللي بيسموه `Client-Side Master Key`. الـ `encryption client` بتاع `S3` اللي عندك على جهازك بيستخدم المفتاح الرئيسي ده عشان يولد `data keys` تكون unique لكل `object`، ويشفر بيها الداتا بتاعتك localy قبل ما ترفعها لـ `S3`. هنا بقى أنت المسؤول عن كل حاجة، من أول توليد الـ `master key` ده، تخزينه بأمان خارجي، و`rotation`، وحمايته من أي اختراق.

طيب، ليه أصلاً ممكن نلجأ للتعقيد ده كله؟ معظم الوقت، الـ `Server-Side Encryption` بأنواعه بيكفي وبيكون أسهل بكتير. لكن في حالات معينة، ممكن تكون شركتك أو الجهة الرقابية اللي بتتعامل معاها عندها `requirements` صارمة جداً بتطلب منك تحكم كامل 100% في الـ `encryption keys` بتاعتك. المتطلبات دي ممكن تكون إن `AWS` نفسها متشوفش الـ `data` بتاعتك `plaintext` أبداً، أو متقدرش توصل لـ `encryption keys` بتاعتك تحت أي ظروف (وده بيحقق مفهوم `zero-knowledge` بالنسبة لـ `AWS`). في الحالات دي، الـ `client-side encryption` بيبقى هو الخيار الوحيد اللي بيوفر مستوى الأمان ده.

بس زي ما في مميزات، فيه تعقيد. الـ `Client-Side Encryption` ده بيزود مسؤولية كبيرة عليك في إدارة الـ `keys` والـ `encryption process` كلها، وده بيخليه أكتر تعقيداً في الإعداد والإدارة عن الـ `server-side encryption` اللي `AWS` بتتكفل فيه بالجزء الأكبر.



---

### **الـ `Logging` في `S3`: **

 الـ `Logging` في `S3` – واللي هو ببساطة تتبع كل `event` أو نشاط بيحصل على الـ `S3 buckets` بتاعتك وتسجيله في `log files` – بيكون مقفول **بشكل افتراضي (`off by default`)**.

**طب ليه مقفول من الأول؟**
السبب الرئيسي بسيط وواضح: الـ `S3 buckets` دي ممكن يحصل عليها `activity` كتير قوي، يعني كميات `requests` مهولة في الثانية الواحدة. تخيل معايا إن `S3` لو كان بيسجل كل `request` بشكل افتراضي، كنا هنلاقي نفسنا غرقانين في كميات `log data` مهولة بشكل مش طبيعي. ومش كل `use case`  يستاهل الكميات دي كلها من الـ `logs` ولا التكلفة بتاعت تخزينها ومعالجتها. ساعات بتكون مجرد داتا عامة، أو داتا مش محتاجة `auditing` مفصل.

**إزاي بتشغّله؟ **
لما بتقرر إنك تفعل الـ `logging` ده عشان تراقب النشاط على الـ `buckets` بتاعتك، هتحتاج تحدد حاجتين رئيسيتين:

1.  **الـ `source bucket`**: ده الـ `bucket` الأصلي بتاعك اللي فيه الداتا اللي عايز تراقب مين بيعملها `access` أو بيعدل فيها. يعني ده الـ `bucket` اللي بيطلع منه الـ `events`.
2.  **الـ `target bucket`**: ده الـ `bucket` التاني اللي `S3` هيحفظ فيه الـ `log files` اللي هيطلعها من الـ `source bucket`. **مهم جداً:** الـ `target bucket` ده لازم يكون في **نفس الـ `AWS Region`** بتاع الـ `source bucket`. وليه بنستخدم `bucket` تاني؟ عشان `S3` ميخشش في `loop` لا نهائي وهو بيسجل `events` الـ `log files` اللي بيحطها في نفس الـ `bucket` اللي بيراقبه، وكمان عشان الـ `security` وفصل الـ `logs` عن الداتا الأساسية.

```terraform
resource "aws_s3_bucket" "s3_log_storage_bucket" {
  bucket = "my-s3-logs-destination-bucket-unique-123456789"
}

resource "aws_s3_bucket" "s3_source_data_bucket" {
  bucket = "my-s3-source-data-bucket-unique-123456789"
}

resource "aws_s3_bucket_logging" "source_bucket_access_logging" {
  bucket        = aws_s3_bucket.s3_source_data_bucket.id
  target_bucket = aws_s3_bucket.s3_log_storage_bucket.id
  target_prefix = "s3-access-logs/"
}
resource "aws_s3_bucket_object" "upload_file_as_example" {
  bucket = aws_s3_bucket.s3_source_data_bucket.id
  key    = "file.txt"
  content = "This is an example file for S3 access logging."
  
}
resource "aws_s3_bucket_policy" "allow_s3_logging" {
  bucket = aws_s3_bucket.s3_log_storage_bucket.id

  policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Sid = "S3ServerAccessLogsPolicy",
        Effect = "Allow",
        Principal = {
          Service = "logging.s3.amazonaws.com"
        },
        Action = "s3:PutObject",
        Resource = "${aws_s3_bucket.s3_log_storage_bucket.arn}/*"
      }
    ]
  })
}

```

، تسجيل الـ logs هنا مش بيتم real-time (يعني مش لحظي). فيه تأخير طبيعي في ظهور الـ log files في الـ target bucket، والتأخير ده ممكن يوصل لحد 30 دقيقة أو أكثر بين وقت حدوث الـ event الفعلي ووقت حفظ الـ log file بتاعه على S3. وده لأن S3 بيجمع الـ events على دفعات قبل ما يكتبها، وده بيساعد على تحسين الأداء وتقليل التكلفة. على عكس خدمات تانية في AWS (هنتكلم عنها قدام) ممكن توفرلك logs بشكل شبه لحظي، خدمة logging.s3.amazonaws.com المخصصة لتسجيل S3 access logs دي مبتشتغلش بالطريقة دي. أما الجزء الخاص بالـ aws_s3_bucket_policy، فده حيوي جداً! وظيفته الأساسية هي إنه يدي تصريح صريح للخدمة الداخلية logging.s3.amazonaws.com (وهي اللي بتمثل S3 Log Delivery Service) إنها تقدر تكتب log files على الـ s3_log_storage_bucket بتاعك. بدون الـ policy دي، S3 مش هيقدر يوصل للـ bucket ويكتب فيه الـ logs، وبالتالي مش هتظهر عندك.



> [!NOTE]
> فى اكثر من خدمة داخلية جو s3 لو لقيت الدنيا فى مساحة هعمل عليهم امثلة وشوف فى مصادر تانية 
>
> | الوظيفة أو الميزة                             | Service Principal                  | وظيفتها                                 |
> | --------------------------------------------- | ---------------------------------- | --------------------------------------- |
> | **S3 Server Access Logging**                  | `logging.s3.amazonaws.com`         | بتكتب logs عن الـ access داخل S3        |
> | **S3 Replication (Cross-Region Replication)** | `replication.s3.amazonaws.com`     | بتنسخ الملفات بين buckets  replication  |
> | **S3 Object Lambda**                          | `s3-object-lambda.amazonaws.com`   | بتعدل في الداتا أثناء عملية `GET` من S3 |
> | **S3 Multi-Region Access Points**             | `s3.amazonaws.com` (بشكل عام)      | بتدير الوصول الموحد عبر اكثر من region  |
> | **S3 Batch Operations**                       | `batchoperations.s3.amazonaws.com` | بتنفذ عمليات على كميات كبيرة من الملفات |

انا قعدة معايا فوق 30 دقيقة تقريبا 

![image-20250715142817121](assets/image-20250715142817121.png)

**تنظيم الـ `Logs` (اختياري بس مهم):**
عشان تسهل على نفسك بعدين لما تيجي تدور على حاجة في الـ `logs` دي، خصوصاً لو عندك كذا `source bucket` بيحطوا الـ `logs` بتاعتهم في نفس الـ `target bucket`، ممكن تحدد:

*   **`Prefixes`**: دي زي اسم فولدر وهمي بتحطه في بداية اسم كل `log file`. مثلاً: `my-app-logs/`, `web-server-logs/`, أو حتى `date-based prefixes` زي `2024/04/23/` عشان تخلي الـ `logs` متنظمة بتاريخها. ده بيساعدك تفلتر الـ `logs` بسهولة.
*   **`Delimiters`**: لو الـ `prefix` بتاعك فيه `/` (زي `2024/04/23/`)، الـ `S3 Console` هيعرضهولك كفولدرات وهمية (زي ما شرحنا قبل كده). الـ `delimiter` بيساعدك في التنظيم البصري للـ `logs`.

*   

> [!IMPORTANT]
>
> 
>
>
> لسه هنكلم عن s3 life cycle و هنكلم عن s3 storage classes لو عندك فكرة اقراء معندكش سيبه
>
> **إدارة    `Logs` ( `Log Retention Policies` ): **
>
> بص يا معلم، الـ `logs` اللي `S3` بيطلعها دي – زي ما عرفنا – ممكن تكون ضخمة جداً، وكمياتها بتزيد باستمرار. وطبعاً، كل `log file` بيترفع على `S3` ليه تكلفة `storage` بتاعته. فلو سيبت الـ `logs` دي تتراكم من غير إدارة، التكلفة بتاعت الـ `S3 storage` بتاعتك ممكن تزيد بشكل ملحوظ مع الوقت.
>
> هنا بيجي دور **`Log Retention Policies`**، واللي بنطبقها باستخدام الـ **`life cycle rules`** على الـ `target bucket` (البوكيت اللي بنحفظ فيه الـ `logs`). الـ `life cycle rules` دي زي ما يكون نظام أوتوماتيكي بتقوله: "يا `S3`، الداتا دي لما تكبر في السن شوية، اعمل بيها كذا وكذا". الـ `rules` دي بتسمحلك تحدد للداتا دي تعيش قد إيه، وإزاي يتم التعامل معاها على مدار عمرها.
>
> **الهدف الرئيسي هنا هو:** إنك تقلل التكلفة على المدى الطويل، وفي نفس الوقت تحافظ على الـ `logs` للمدة اللي أنت محتاجها لـ `auditing` أو `compliance` (المتطلبات القانونية أو التنظيمية).
>
> **إزاي `Life Cycle Rules` بتشتغل مع الـ `Logs`؟**
>
> الـ `life cycle rules` فيها نوعين أساسيين من الـ `actions`  ممكن تطبقها على الـ `log files` بتاعتك:
>
> 1.  **`Transition` (نقل الداتا لـ `Storage Class` أرخص):**
>     ده اللي بيسمحلك تنقل الـ `log files` القديمة لـ `storage classes` (أنواع تخزين) أرخص تلقائياً بعد عدد معين من الأيام.
>
>     **نقطة مهمة جداً عشان متتلخبطش:** الـ `S3 bucket` نفسه **مش بيكون كله على `storage class` واحدة**. يعني أنت مش بتعمل `bucket` نوعه `Glacier` كله على بعضه. لأ، `S3` بيشتغل على مستوى الـ **`object`** (الملف الواحد). كل `object` جوه الـ `bucket` هو اللي بيكون ليه `storage class` خاص بيه. الـ `lifecycle rules` دي بتسمحلك تغير الـ `storage class` بتاع الـ `object` ده تلقائياً.
>
>     *   **مثلاً:** ممكن تقول لـ `S3`: "أي `log file` جديد يترفع على الـ `target bucket`، خليه الأول في `S3 Standard` (عشان لو احتاجته بسرعة أول ما يترفع).
>     *   **بعد 30 يوم** (مثلاً)، انقل الـ `log file` ده تلقائياً لـ **`S3 Standard-IA`**. ده بيوفرلك في تكلفة الـ `storage` لأن الـ `log` ده مبقاش يتشاف كتير، لكن لسه تقدر توصله في `millisecond access` لو احتجته.
>     *   **بعد 90 يوم** (مثلاً، من تاريخ إنشاء الـ `log` الأصلي)، انقل الـ `log file` ده لـ **`S3 Glacier`**. كده أنت بتوفر أكتر بكتير، بس بتكون عارف إنك هتحتاج تستنى من دقايق لساعات عشان تسترجعه.
>     *   **بعد 365 يوم** (أو حسب احتياجك)، ممكن تنقله لـ **`S3 Glacier Deep Archive`**، وده أرخص حاجة خالص، بس هتستنى ساعات عشان تسترجعه.
>
> 2.  **`Expiration` (مسح الـ `Logs` خالص):**
>     بعد فترة طويلة جداً (مثلاً سنة أو سنتين أو حسب الـ `compliance` بتاعك)، ممكن الـ `log files` دي مبقاش ليها أي قيمة أو فايدة، أو إن القانون بيقولك لازم تمسحها. هنا الـ `lifecycle rules` بتسمحلك تحدد:
>
>     *   "امسح الـ `log files` دي خالص من الـ `bucket` بعد فترة زمنية معينة (مثلاً 730 يوم، يعني سنتين)".
>
> ده بيساعدك تضمن إنك مش بتدفع فلوس على داتا مبقتش محتاجها، وفي نفس الوقت بتخليك ملتزم بأي متطلبات `retention`  خاصة بالـ `logs`.



## S3 Durability and Availability

### **الـ `S3 Durability` و `Availability`**

 الـ S3 بيوفرلك أكتر من storage class تقدر تستخدمها للـ objects بتاعتك. بس فيه ملحوظة مهمة جداً لازم تركز عليها: الـ storage class دي بتكون على مستوى الـ object الواحد، مش على مستوى الـ S3 bucket كله. يعني مش لازم الـ bucket كله يكون نوع واحد؛ أنت ممكن تحط كل object جوه نفس الـ bucket في storage class مختلفة حسب احتياجك.

. والـ `class` اللي بتختاره بيعتمد على قد إيه مهم إن الداتا تفضل موجودة وميحصلهاش حاجة (الـ `durability`)، وقد إيه محتاج توصلها بسرعة (الـ `availability`)، والفلوس اللي انت مستعد تصرفها.

> [!NOTE]
>
>  الـ `Durability` هي نسبة ضمان إن الداتا اللي رفعتها على `S3` مش هتضيع أو تتلف (`corrupted`) بمرور الوقت بسبب أي عطل في الهاردوير بتاع أمازون.



#### **الـ `Durability`**

`S3` بيقيس الـ `durability` كنسبة مئوية. يعني مثلاً، ضمان الـ `durability` اللي نسبته 99.999999999% لمعظم الـ `S3 classes` و `Amazon S3 Glacier`...

> ...ده معناه متوسط خسارة سنوية متوقعة بنسبة 0.000000001% من الـ `objects`. يعني مثلاً، لو خزنت 10 مليون `object` على `Amazon S3`، فالمتوسط بيقول إنك ممكن تخسر `object` واحد بس كل 10,000 سنة.
> المصدر: aws.amazon.com/s3/faqs ![طبعا.. أومال😂واحنا كأس العالم هيجيلنا في تاكسي😎 ومجموعة افيهات هتقتلك ضحك  #بوشكاش](assets/maxresdefault.jpg)

بمعنى تاني، وبشكل واقعي، تقريبًا مستحيل إنك تخسر داتا متخزنة على المنصات الأساسية بتاعة `S3` أو `Glacier` بسبب أي فشل في الـ `infrastructure`.

بس، هيبقى تصرف غير مسؤول إنك تعتمد على الـ `S3 buckets` بتاعتك كأنها النسخة الوحيدة من الداتا المهمة. في الأول والآخر، فيه احتمال حقيقي إن يحصل `misconfiguration` (إعدادات غلط)، أو الـ `account` بتاعك يتقفل، أو يحصل هجوم خارجي غير متوقع، وده ممكن يمنعك توصل للداتا بتاعتك للأبد. وكمان، مع إن الكلام ده ممكن يبان جنان دلوقتي، بس مش مستحيل نتخيل إن `AWS` نفسها في يوم من الأيام ممكن تقفل. كوداك وبلوك باستر كانوا مسيطرين على السوق بتاعهم في وقتهم، صح؟ لازم دايمًا تاخد `backup` للداتا بتاعتك في أماكن متعددة، وتستخدم خدمات وأنواع `media` مختلفة. هتتعلم إزاي تعمل كده في `Chapter 10`، اللي هو عن الـ `Resilient Architectures`.

معدلات الـ `durability` العالية اللي `S3` بيقدمها سببها الأساسي إنه بينسخ الداتا بتاعتك بشكل أوتوماتيكي على الأقل في تلاتة `availability zones`. ده معناه إنه حتى لو `facility` (منشأة) كاملة بتاعة `AWS` اتمسحت من على الخريطة فجأة، نسخ من الداتا بتاعتك هتسترجع من `zone` تانية.

انت ممكن توازن بين زيادة أو نقصان الـ `durability` قصاد مميزات تانية زي الـ `availability` والـ `cost` عشان توصل للتوازن الصح ليك. ومع إن كل الـ `classes` اللي بينصحوا بيها حاليًا متصممة عشان تتحمل فقدان الداتا بنسبة 99.999999999% ، معظمهم بيتخزنوا في تلاتة `availability zones` على الأقل. الاستثناء هو `S3 One Zone-IA`، اللي زي ما اسمه بيقول، بيخزن الداتا بتاعته في `zone` واحدة بس. والفرق ده بيظهر في الـ `availability` بتاعته اللي أقل شوية، ودي هنتكلم عنها دلوقتي



### **الـ `Availability`**

الـ `Object availability` بتتقاس هي كمان كنسبة مئوية؛ بس المرة دي، هي نسبة الوقت اللي ممكن تتوقع فيها إن `object` معين يكون متاح ليك فورًا أول ما تطلبه على مدار سنة كاملة.

الـ `Amazon S3 Standard class` مثلاً، بتضمن إن الداتا بتاعتك هتكون `available` وجاهزة كل ما تحتاجها بنسبة 99.99% من السنة. ده معناه إن هيكون فيه أقل من 9 ساعات `downtime` في السنة كلها. ولو حسيت إن الـ `downtime` عدى الحد ده في خلال سنة واحدة، ممكن تقدم طلب عشان تاخد `service credit`. على عكسه، الضمان بتاع الـ `durability` من أمازون معمول عشان يوفر حماية للداتا بنسبة 99.999999999%. ده معناه إن عمليًا مفيش فرصة إن الداتا بتاعتك تضيع، حتى لو ممكن في أوقات معينة متقدرش توصلها فورًا.

الـ **`S3 Intelligent-Tiering`** هي `storage class` جديدة نسبيًا بتوفرلك فلوس وفي نفس الوقت بتعمل `optimizing` للـ `availability`. مقابل رسوم `automation` شهرية، الـ `Intelligent-Tiering` بيراقب الطريقة اللي بتعمل بيها `access` للداتا اللي جوه الـ `class` دي مع مرور الوقت. وهو بينقل الـ `object` أوتوماتيك للـ `tier` الأرخص بتاع الـ `infrequent access` بعد ما يكون محدش عمله `access` لمدة 30 يوم متواصلة.

الجدول ده بيوضح ضمانات الـ `availability` لكل الـ `S3 classes`.

**جدول 3.1: ضمانات الـ `availability`  لتخزين `S3`**

| S3 Standard                 | S3 Standard-IA | S3 One Zone-IA | S3 Intelligent-Tiering |
| :-------------------------- | :------------- | :------------- | :--------------------- |
| **ضمان الـ `Availability`** | 99.99%         | 99.9%          | 99.5%                  |

![AWS S3: Different Types of Storage Types Available in S3 | by Amir Mustafa  | AWS in Plain English](assets/11t8zFevEzNDTf0l8oKp12A.png)



الأربعة `classes` دول كلهم يعتبروا في الـ `Tiers` "السخنة" والـ "WARM". يعني كلهم بيوفروا وصول فوري للداتا (`millisecond access`). محدش فيهم زي `Glacier` اللي بيحتاج دقايق أو ساعات عشان يرجعلك ملفاتك. الفرق الحقيقي بينهم بيظهر في **نموذج التسعير (`cost model`) والفلسفة ورا كل واحد**.

![Optimizing your AWS Infrastructure for Sustainability, Part II: Storage |  AWS Architecture Blog](assets/Figure-2.-Data-access-patterns-for-Amazon-S3.jpg)

#### **1. `S3 Standard` **

ده هو الـ `class` الافتراضي، والأغلى فيهم من حيث تكلفة التخزين الشهرية، والأبسط في الفهم.

*   **الفروقات الاساسية:**
    *   **مفيش `Retrieval Fee`:** دي أهم نقطة. تقدر تعمل `download` للداتا بتاعتك مليون مرة في الشهر، مش هتدفع مليم زيادة على عملية Retrieval نفسها (طبعًا لسه فيه تكلفة نقل الداتا `Data Transfer`, بس دي قصة تانية).
    *   **مفيش حد أدنى لمدة التخزين:** تقدر ترفع ملف وتمسحه بعدها بثانية. مش هتتحاسب على أي حاجة غير المدة البسيطة دي.
    *   **مفيش حد أدنى لحجم الـ `Object`:** ارفع ملف حجمه 1 بايت، هتتحاسب على 1 بايت.

*   **فلسفته:** "ادفع أكتر شوية في التخزين، وخد حريتك كاملة في الوصول للداتا زي ما انت عايز ومن غير أي تكاليف مفاجئة."

*   **حالات استخدامه (Active Data):**
    *   **(`Website Assets`):** الصور والـ `CSS` والـ `JavaScript` اللي كل زوار موقعك بيحملوها باستمرار.
    *   **(`Content Distribution`):** ملفات برامج أو فيديوهات الناس بتعملها `download` كتير.
    *   **الداتا اللي بتتحلل باستمرار:** أي داتا بتستخدمها في عمليات `Big Data Analytics` والـ `servers` بتقراها وتكتب عليها كتير.
*   **باختصار:** أي داتا "نشيطة" وبتستخدمها بشكل متكرر ويومي وبيكون عليها عملية قراءة او upload عنيفة .

---

#### **2. `S3 Standard-IA` (Infrequent Access)**

هنا اللعبة بتبدأ تتغير. ده معمول للداتا اللي مش بتوصلها كتير، بس لما تحتاجها، لازم توصلها فورًا.

*   **الفروقات الاساسية:**
    *   **تكلفة تخزين شهرية أرخص بكتير** من `S3 Standard`.
    *   **فيه رسوم استرجاع (`Retrieval Fee`):** دي هي Trade-off. هتوفر في إيجار التخزين الشهري، بس كل مرة هتيجي تعمل `download` لملف، هتدفع رسوم على كل جيجابايت بتسترجعه.
    *   **فيه حد أدنى لمدة التخزين (30 يوم):** لو مسحت الملف قبل 30 يوم، هتتحاسب على تكلفة تخزين الـ 30 يوم كاملة.
    *   **فيه حد أدنى لحجم الـ `Object` (128 كيلوبايت):** لو رفعت ملف حجمه 10 كيلوبايت، هتتحاسب على إنه 128 كيلوبايت.

*   **فلسفته:** "لو عندك داتا مهمة بس مش بتستخدمها كل يوم، حطها عندي بسعر رخيص. بس لما تحتاجها، هدفعك تمن إني أروح أجبهالك."

*   **حالات استخدامه (Inactive Data):**
    *   **`Backups` طويلة الأجل:** نسخ احتياطية بتحتفظ بيها لشهور أو سنين، ومش بتلمسها إلا لو حصلت كارثة.
    *   **مشاركة الملفات:** زي `Dropbox` شخصي. بترفع ملفات عشان تشاركها مع حد مرة واحدة وبعدين بتفضل موجودة.
    *   **الأرشيف اللي ممكن تحتاجه بسرعة:** داتا قديمة لازم تحتفظ بيها، بس ممكن مديرك يطلبها منك فجأة عشان يعمل تقرير.

---

#### **3. `S3 One Zone-IA`**

ده هو الأخ المتطرف لـ `Standard-IA`. بياخد نفس الفكرة بس بيزودها عشان يوصل لأقصى توفير ممكن في التكلفة.

*   **الفروقات الاساسية (غير الـ 1 AZ):**
    *   **أرخص تكلفة تخزين شهرية** في كل الـ `tiers` اللي بتوفر وصول فوري. هو أرخص من `Standard-IA`.
    *   نفس القيود بتاعة `Standard-IA` بالظبط: فيه رسوم استرجاع، وفيه حد أدنى للمدة والحجم.
    *   **الفرق  الفلسفي:** هنا انت بتقول لـ `AWS` بشكل صريح: "أنا موافق أضحي بالـ `durability` العالية ضد الكوارث اللي بتدمر داتا سنتر كامل في مقابل أقل سعر ممكن".

*   **فلسفته:** "عايز أرخص سعر على الإطلاق لداتا توصلها فورًا؟ أنا اختيارك. بس تبقى عارف إن الداتا دي لو ضاعت بسبب كارثة في الـ `AZ` الوحيد اللي هي فيه، دي مشكلتك انت."

*   **حالات استخدامه (Reproducible Data):**
    *   **النسخ الثانوية من الـ `Backups`:** عندك `backup` أساسي في `Standard-IA`، بس عامل نسخة تانية أرخص في `One Zone-IA` زيادة أمان.
    *   **الداتا اللي تقدر تعملها تاني بسهولة:** زي الـ `Thumbnails` أو الفيديوهات اللي معمولها `transcoding`. لو الملفات دي ضاعت، ممكن تشغل الـ `script` تاني وتعملها من أول وجديد من الملفات الأصلية.
    *   باختصار: أي داتا **مش حرجة**، أو **سهل تعملها من جديد**.

---

#### **4. `S3 Intelligent-Tiering`**

ده هو الحل الذكي اللي `AWS` عملته عشان تريحك من الحيرة دي كلها.

*   **الفروقات الاساسية:**
    *   **بيشتغل أوتوماتيك:** هو مش `class` ثابت، ده نظام ذكي بيحرك الداتا بتاعتك بين `tier` سخن (زي `Standard`) و `tier` بارد (زي `Standard-IA`) على حسب استخدامك.
    *   **مفيش retrieval:** الميزة الجبارة هنا إنك مش بتدفع `retrieval fee` لما توصل للداتا حتى لو هي في الـ `tier` البارد، لأن النظام هو اللي حركها، فبيعتبر ده جزء من الخدمة.
    *   **فيه رسوم مراقبة شهرية (`Monitoring Fee`):** دي تكلفة بسيطة بتدفعها مقابل الذكاء ده. `AWS` بتاخد فلوس عشان تراقب كل `object` وتشوف هيتحرك ولا لأ.

*   **فلسفته:** "انت مش عارف الداتا بتاعتك هتستخدمها كتير ولا قليل؟ سبهالي وأنا هوفرلك فلوس أوتوماتيك من غير ما توجع دماغك."

*   **حالات استخدامه (Unknown or Changing Access Patterns):**
    *   **الداتا اللي استخدامها متقلب وغير متوقع:** زي أرشيف صور المستخدمين على موقع، ممكن صورة قديمة فجأة تبقى `viral` والناس توصلها كتير.
    *   **الـ `Data lakes`:** لما تكون بتجمع كميات داتا ضخمة من مصادر مختلفة ومش عارف أنهي جزء منها اللي هيبقى مهم وهيتم تحليله بكثرة في المستقبل.
    *   **لما تكون عايز توفر فلوس بس معندكش وقت** أو خبرة كافية تعمل `life cycle policies` معقدة بنفسك.

### **مقارنة بين الـ `S3 Storage Classes`**

| الميزة                           | S3 Standard   | S3 Intelligent-Tiering | S3 Standard-IA | S3 One Zone-IA | S3 Glacier    | S3 Glacier Deep Archive |
| :------------------------------- | :------------ | :--------------------- | :------------- | :------------- | :------------ | :---------------------- |
| **مصممة للـ `Durability`**       | 99.999999999% | 99.999999999%          | 99.999999999%  | 99.999999999%  | 99.999999999% | 99.999999999%           |
| **مصممة للـ `Availability`**     | 99.99%        | 99.9%                  | 99.9%          | 99.5%          | 99.99%        | 99.99%                  |
| **ضمان الـ `Availability SLA`**  | 99.9%         | 99%                    | 99%            | 99%            | 99.9%         | 99.9%                   |
| **عدد الـ `Availability Zones`** | ≥ 3           | ≥ 3                    | ≥ 3            | 1              | ≥ 3           | ≥ 3                     |
| **أقل سعة للـ `Object`**         | N/A           | N/A                    | 128 KB         | 128 KB         | 40 KB         | 40 KB                   |
| **أقل مدة تخزين**                | N/A           | 30 يوم                 | 30 يوم         | 30 يوم         | 90 يوم        | 180 يوم                 |
| **رسوم التحميل**                 | N/A           | N/A                    | لكل جيجا بايت  | لكل جيجا بايت  | لكل جيجا بايت | لكل جيجا بايت           |

> [!IMPORTANT]
>
> ينفع عادي تنقل الـ `objects` بتاعتك من `storage class` لـ `storage class` تاني (يعني من `tier` لـ `tier` زي ما بنقول). بس هنا فيه ملحوظة مهمة أوي لازم تاخد بالك منها: فيه حاجة اسمها **`minimum storage duration`** لكل `storage class`. يعني لو نقلت `object` لـ `S3 Standard-IA` مثلاً، وقررت تنقله بعدها لـ `Glacier` قبل ما تعدي الـ 30 يوم بتاعته (اللي هي الـ `minimum storage duration` لـ `Standard-IA`)، `S3` هيحاسبك على الـ 30 يوم دول كاملين، حتى لو الداتا قعدت يوم واحد بس! دا شرط أساسي مينفعش تقل عنه.
>
> النقطة دي مهمة بالذات لو بتستخدم الـ `life cycle rules` اللي شرحناها. لو عايز العملية تتم بشكل أوتوماتيكي عن طريق الـ `lifecycle_configuration`، الـ `rule` بتاعك لازم يكون معمول بحيث يحترم الـ `minimum storage duration` دي، يعني الـ `transfer` مش هيحصل إلا بعد ما الـ `object` يقضي الـ `minimum storage duration` دي في الـ `tier` اللي هو فيها.
>
> طب افرض إنك مضطر ومش قادر تستنى المدة دي؟ هنا ممكن "تشغل دماغك" وتعملها بـ `tool` خارجية، زي الـ `AWS CLI` أو أي `SDK` (مثلاً إنك تنسخ الـ `object` من `storage class` لـ `storage class` تاني). بس خلي بالك، دي بتبقى للحالات الاضطرارية فعلاً، مش الوضع الطبيعي، لأن حتى في الحالة دي، أنت برضه هتدفع على الـ `minimum storage duration` كاملة للـ `storage class` الأولانية، مفيش مفر منها. `AWS` عاملة الشرط ده عشان تتأكد إنها بتغطي تكاليف التخزين والمعالجة حتى لو الداتا مش قعدت المدة كلها.

### **الـ `Eventually Consistent Data`**



عشان نفهم الموضوع ده بعمق، لازم نرجع لحتة مهمة جدًا قلناها قبل كده: الـ `Durability` العالية بتاعة `S3` (الـ 11 تسعة) سببها إنه بينسخ الداتا بتاعتك على الأقل في 3 داتا سنترز (`Availability Zones`) منفصلين عن بعض. وده تصرف عبقري عشان يحمي داتك من الكوارث.

**لكن...** عملية النسخ دي مش بتحصل في نفس اللحظة بالظبط. فيه فرق زمني بسيط جدًا (أجزاء من الثانية) على ما النسخة الجديدة من الداتا توصل لكل الداتا سنترز. والفرق الزمني البسيط ده هو اللي بيخلق مفهوم الـ **`Eventually Consistent`**.

تخيل إنك عندك ملف مهم اسمه `config.json` على `S3`. في لحظة معينة، قررت تعدل فيه ورفعت نسخة جديدة منه (`overwrite`).

**إيه اللي بيحصل ورا الكواليس في `AWS`؟**

1.  **الاستلام:** الـ `request` بتاعك بيوصل لأقرب داتا سنتر ليك (خلينا نسميه **DC-1**). الداتا سنتر ده بيكتب النسخة الجديدة من الملف على الديسكات بتاعته.
2.  **التأكيد:** أول ما `DC-1` يخلص كتابة، بيرد عليك ويقولك "تمام، الـ `upload` بتاعك نجح".
3.  **النسخ (Replication):** في نفس الوقت، `DC-1` بيبدأ يبعت النسخة الجديدة دي للداتا سنترز التانية (خلينا نقول **DC-2** و **DC-3**) عشان يضمنوا الـ `durability`. عملية النسخ دي بتاخد وقت، ممكن يكون `milliseconds` بسيطة.

**فين المشكلة بقى؟**

تخيل إن الـ `application` بتاعك، بعد ما عمل `upload` بجزء من الثانية، راح باعت `request` تاني عشان يقرأ نفس الملف `config.json`. الـ `Load Balancer` بتاع `AWS` ممكن يوجه الـ `request` ده لأي داتا سنتر من التلاتة.

*   **السيناريو السعيد:** الـ `request` يروح لـ **`DC-1`**. هيرجعلك النسخة الجديدة اللي لسه رافعها. كله تمام.
*   **السيناريو المحتمل (وهنا المشكلة):** الـ `request` يروح لـ **`DC-2`**، اللي ممكن تكون النسخة الجديدة لسه موصلتلوش. في الحالة دي، **`DC-2`** هيرجعلك **النسخة القديمة** من الملف!

ده بالظبط معنى الـ **`Eventually Consistent`**: يعني "في النهاية" (Eventually)، كل الداتا سنترز هتبقى متزامنة وهيكون عندها نفس النسخة الجديدة من الملف. بس فيه "فترة زمنية" قصيرة جدًا ممكن يحصل فيها عدم تطابق.

#### **إيه الفرق بين نوعين الـ Consistency اللي ذكرناهم؟**

`AWS` بتفرق بين حالتين عشان تخلي حياتنا أسهل:

1.  **`Eventual Consistency` (للتعديل والحذف):**
    ده بيحصل في حالتين:
    *   **`Overwrite PUTS`**: لما ترفع نسخة جديدة فوق ملف قديم.
    *   **`DELETES`**: لما تمسح ملف.
    في الحالتين دول، ممكن لو قريت الملف فورًا بعد التعديل تاخد النسخة القديمة، أو لو مسحته ممكن تلاقيه لسه موجود لثانية أو اتنين. الـ `Application` بتاعك لازم يكون متصمم وهو حاطط الاحتمال ده في الحسبان.

2.  **`Read-after-Write Consistency` (للملفات الجديدة):**
    `AWS` بتقدم الضمان ده عشان تطمنك لما ترفع ملف جديد لأول مرة.
    *   **`New Object PUTS`**: لما ترفع ملف **مش موجود قبل كده**.
    في الحالة دي، مفيش "نسخة قديمة" أصلًا في أي داتا سنتر عشان يحصل لخبطة. فأول ما `S3` يقولك "تمام الـ `upload` نجح"، هو بيضمنلك إن أي `read request` هييجي بعده فورًا هيشوف الملف الجديد ده. مفيش أي احتمالية إنك تلاقي الملف مش موجود.

*   لما ترفع ملف جديد، انت في أمان. تقدر تقراه وتستخدمه فورًا.
*   لما تعدل على ملف قديم أو تمسحه، لازم تبقى حذر. متصممش الـ `application` بتاعك على أساس إن التغيير ده هيسمّع في السيستم كله في نفس اللحظة. لو الموضوع ده حرج جدًا بالنسبة ليك، ممكن تحتاج تعمل `logic` بسيط في الكود بتاعك يستنى ثانية أو يعيد المحاولة لو ملقاش التغيير اللي هو متوقعه.





### **الـ `S3 Object Life Cycle`**

كتير من الشغل اللي هتعمله على `S3` غالبًا هيكون ليه علاقة بالـ `backup archives` . بس المشكلة في الـ `backup archives` دي، لو مصممها صح، إنك بتعمل منها نسخ جديدة بانتظام. مهم جدًا إنك تحتفظ بشوية من النسخ القديمة دي، بس في نفس الوقت هتبقى عايز تتخلص من وتمسح النسخ الأقدم عشان تسيطر على تكاليف التخزين بتاعتك.

الـ `S3` بيخليك تعمل كل ده بشكل أوتوماتيكي عن طريق الـ `features` بتاعته اللي هي `versioning` و `life cycle`.

#### **الـ `Versioning`**

في معظم الـ `filesystem environments`، لو حفظت ملف بنفس الاسم والمكان بتاع ملف موجود أصلًا، الملف الجديد هيعمل `overwrite` (هيكتب فوق) الملف الأصلي. ده بيضمن إنك دايمًا هيكون معاك أحدث نسخة، بس في المقابل هتخسر النسخ القديمة — بما فيهم النسخ اللي عملتلها `overwrite` بالغلط.

بشكل افتراضي، الـ `objects` على `S3` بتشتغل بنفس الطريقة دي. **بس** لو شغلت الـ `versioning` على مستوى الـ `bucket`، ساعتها النسخ القديمة اللي اتعملها `overwrite` هتفضل محفوظة ومتاحة ليك على طول. ده بيحل مشكلة إنك تفقد داتا قديمة بالغلط، بس بيخلق مشكلة تانية وهي احتمالية الـ `archive bloat` (إن archive يتضخم ويكبر زيادة عن اللزوم). وهنا بقى بييجي دور الـ `life cycle management` عشان يساعد.

دا مثال تافه على على versioning 

```terraform
resource "aws_s3_bucket" "s3_log_storage_bucket" {
  bucket = "my-s3-logs-destination-bucket-unique-123456789"
  force_destroy = true
  versioning {
    enabled = true
  }
}
resource "aws_s3_bucket_object" "just_file" {
  bucket = aws_s3_bucket.s3_log_storage_bucket.id
  key    = "file.txt"
  content = "this version 1"
  
}
resource "aws_s3_bucket_object" "just_file_1" {
  bucket = aws_s3_bucket.s3_log_storage_bucket.id
  key    = "file.txt"
  content = "this version 2"
  
}

resource "aws_s3_bucket_object" "just_file_2" {
  bucket = aws_s3_bucket.s3_log_storage_bucket.id
  key    = "file.txt"
  content = "this version 3"
  
}


```

![image-20250715153612320](assets/image-20250715153612320.png)

#### **الـ `Life Cycle Management`**

بالإضافة للـ `storage class` اللي اسمها `S3 Intelligent-Tiering` اللي اتكلمنا عنها قبل كده، انت ممكن تظبط `life cycle rules` بنفسك للـ `bucket`، والـ `rules` دي هتنقل الـ `object` بشكل أوتوماتيكي لـ `storage class` تانية بعد عدد معين من الأيام.

يعني مثلاً، ممكن تخلي الـ `objects` الجديدة تفضل في `S3 Standard class` أول 30 يوم، وبعدها تتنقل للـ `class` الأرخص اللي هي `One Zone-IA` لمدة 30 يوم كمان. ولو فيه قوانين أو التزامات تنظيمية (`regulatory compliance`) بتجبرك تحتفظ بالنسخ القديمة، ممكن بعدها الملفات دي تتنقل لخدمة التخزين الرخيصة وطويلة الأجل اللي هي `Glacier` لمدة 365 يوم كمان، قبل ما تتمسح بشكل نهائي.

> [!TIP]
>
> 
>
> لما بتيجي تستخدم الـ `life cycle rules`، لازم تكون فاهم إن `AWS` حطت شوية قيود مش بشكل عشوائي، لكن عشان تضمن إنك بتستخدم النظام ده بطريقة منطقية وفعالة من حيث التكلفة.
>
> أول نقطة هي فكرة استخدام الـ **`prefixes`** لتطبيق الـ `rules` على `objects` معينة. دي ميزة قوية جدًا بتديلك تحكم دقيق على مستوى الـ `bucket` الواحد. بدل ما تطبق سياسة واحدة على كل الملفات اللي في الـ `bucket`، انت بتقدر تقسمه لمناطق منطقية. يعني ممكن يكون عندك سياسة للـ `logs` وسياسة تانية مختلفة تمامًا للـ `backups`، وسياسة تالتة لملفات المستخدمين، وكل ده جوه نفس الـ `bucket`. ده بيخلي الإدارة مرنة جدًا وبيمنعك من إنك تحتاج تعمل عشرات الـ `buckets` لكل نوع ملف، وبالتالي بيقلل التعقيد الإداري عليك.
>
> النقطة التانية والمهمة هي **الحد الأدنى للمدة الزمنية**. إنك لازم تسيب الـ `object` في `class` معينة لمدة محددة (زي 30 يوم) قبل ما تنقله لـclass تاني ده شرط مرتبط مباشرةً بنموذج التسعير. الـ `storage classes` الرخيصة (زي الـ `Standard-IA`) بتوفرلك تكلفة تخزين قليلة على أساس إنك هتخزن الداتا دي ومش هتوصلها كتير. لو `AWS` سمحت بنقل الملفات دي بسرعة، الناس ممكن تستغل النظام وتنقل الملفات يوميًا عشان توفر كام مليم، وده بيعمل عبء تشغيلي على `AWS` وبيبوظ فكرة التسعير نفسها. فالشرط ده يعتبر بمثابة التزام منك. ولو خلفت الالتزام ده ومسحت الملف أو نقلته بدري، بتدفع غرامة بسيطة، وهي إنك بتتحاسب على تكلفة تخزين المدة دي كاملة.
>
> 
>
> النقطة الأخيرة، واللي بتعتبر أساسية في فهم الـ `life cycle management`، هي إن **مسارات النقل بين الـ `storage classes` مش مفتوحة على البحري**. يعني مينفعش تنقل `object` من أي `class` لأي `class` تانية بمزاجك. `AWS` حاطة نظام وقيود واضحة للموضوع ده، والقيود دي مش محطوطة بشكل عشوائي، دي مبنية على الفكرة الأساسية لدورة حياة الداتا.
>
> ![Optimizing your AWS Infrastructure for Sustainability, Part II: Storage |  AWS Architecture Blog](assets/Figure-2.-Data-access-patterns-for-Amazon-S3.jpg)
>
> الفكرة كلها قايمة على إن الداتا، بيقل استخدامك عليها او على حسب مصطلح aws بتبرد
>
> 1.  **Hot Tier:** أول ما الداتا و بتترفع، بتكون في قمة أهميتها. بيتم الوصول ليها كتير، ومحتاجينها تكون متاحة فورًا. عشان كده بتبدأ حياتها في `class` غالية وعالية الأداء زي **`S3 Standard`**.
>
> 2.  **Warm Tier:** بعد فترة (شهر مثلاً)، أهمية الداتا دي بتقل شوية. معدل الوصول ليها بيقل. هنا بييجي دور الـ `life cycle rule` عشان تنقلها بشكل منطقي لـ `class` ، زي **`S3 Standard-IA`**. هي لسه متاحة بسرعة، بس تكلفة تخزينها أقل.
>
> 3.  **(Cold Tier / Archive):** بعد شهور أو سنين، الداتا دي بتتحول لـArchive. نادرًا جدًا لما حد بيحتاجها، بس لازم تفضل متخزنة لأسباب قانونية أو التزامات تانية. هنا بتتنقل لأبرد وأرخص مكان ممكن، زي **`S3 Glacier`** أو **`Glacier Deep Archive`**.
>
>
> 
>
> القيود اللي `AWS` حطاها على مسارات النقل هي ببساطة عشان تجبرك تتبع المنطق ده. العملية دي عاملة زي شلال الماية، بتنزل من فوق لتحت، من الغالي للرخيص، من السخن للبارد. صعب جدًا، وفي معظم الحالات مستحيل...، إنك تطلع بالعكس في الـ `life cycle` دي بشكل أوتوماتيكي.
>
> 
>
> فمينفعش مثلًا تعمل `life cycle rule` تنقل `object` من `Glacier`  لـ `Standard-IA` . ده ملوش معنى في life cycle الداتا. لو حبيت ترجع تستخدم ملف قديم أوي تاني بشكل مستمر، المفروض الأول تعمل له `restore`من s3، وبعدها تقدر ترفعه تاني كـ `object` جديد في الـ `class`  اللي انت عايزها.
>
> مش شرط بترتيب يعني ممكن تروح **S3 Standard** الى **S3 Glacier** ولكن العكس المستحيل يحصل لازم تمشي بترتيب المعين ده او بتجاه hot الى  cold
>
> فالقيود دي مش تعقيدات، دي حماية ليك عشان تضمن إنك بتستخدم النظام صح، وبتوفر فلوس بالطريقة اللي `AWS` صممتها، وبتضمن إن عمليات نقل الداتا بتحصل بشكل سليم .

دا مثال بنقل logs على مستوى objects معينة عن طريق perfix معين 
```terraform
resource "aws_s3_bucket" "s3_log_storage_bucket" {
  bucket = "my-s3-logs-destination-bucket-unique-123456789"
  force_destroy = true
}

resource "aws_s3_bucket" "s3_source_data_bucket" {
  bucket = "my-s3-source-data-bucket-unique-123456789"
  force_destroy = true
}

resource "aws_s3_bucket_logging" "source_bucket_access_logging" {
  bucket        = aws_s3_bucket.s3_source_data_bucket.id
  target_bucket = aws_s3_bucket.s3_log_storage_bucket.id
  target_prefix = "s3-access-logs/"
}
resource "aws_s3_bucket_object" "upload_file_as_example" {
  bucket = aws_s3_bucket.s3_source_data_bucket.id
  key    = "file.txt"
  content = "This is an example file for S3 access logging."
  
}
resource "aws_s3_bucket_policy" "allow_s3_logging" {
  bucket = aws_s3_bucket.s3_log_storage_bucket.id

  policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Sid = "S3ServerAccessLogsPolicy",
        Effect = "Allow",
        Principal = {
          Service = "logging.s3.amazonaws.com"
        },
        Action = "s3:PutObject",
        Resource = "${aws_s3_bucket.s3_log_storage_bucket.arn}/*"
      }
    ]
  })
}
output "s3_log_storage_bucket_name" {
  value = aws_s3_bucket.s3_log_storage_bucket.id
  
}

resource "aws_s3_bucket_lifecycle_configuration" "log_lifecycle_full_tiers" {
  bucket = aws_s3_bucket.s3_log_storage_bucket.id

  rule {
    id     = "full-log-lifecycle-through-all-tiers"
    status = "Enabled"

    filter {
      prefix = "s3-access-logs/"
    }

    transition {
      days          = 30
      storage_class = "STANDARD_IA"
    }

    transition {
      days          = 60
      storage_class = "GLACIER"
    }

    transition {
      days          = 180
      storage_class = "DEEP_ARCHIVE"
    }

    expiration {
      days = 365
    }
  }
}

```



```mermaid
graph TD
    A["S3 Standard Day 0 to 29"] --> B["S3 Standard-IA Day 30 to 59"]
    B --> C["S3 Glacier Day 60 to 179"]
    C --> D["Glacier Deep Archive Day 180 to 364"]
    D --> E["Delete Object Day 365"]

```

عايز أضبط إعدادات الـ Terraform backend بحيث يتم نقل النسخ القديمة من الـ Terraform state في S3 تلقائيًا إلى Glacier بعد مرور 30 يوم من وقت ما تبقى نسخة قديمة (non-current version)

```terraform
resource "aws_s3_bucket" "terraform_backend_tfstae" {
  bucket = "terraform_backend_tfstae-unique-123456789"
  force_destroy = true
}

resource "aws_s3_bucket_versioning" "versioning" {
  bucket = aws_s3_bucket.terraform_backend_tfstae.id
  versioning_configuration {
    status = "Enabled"
  }
}
resource "aws_s3_bucket_lifecycle_configuration" "name" {
  
  bucket = aws_s3_bucket.terraform_backend_tfstae.id
  rule {
    id = "to_glacier_after_30_days"
    status = "enabled"
    filter {
      prefix = "terraform/"
    }
    noncurrent_version_transition {
      noncurrent_days = 30
      storage_class = "GLACIER"
    }
  }
  
  }
```





```mermaid
graph TD
    A["Current Version (terraform/state.tfstate)"] -->|New Version Uploaded| B["Becomes Non-Current"]
    B -->|After 30 Days| C["Transition to S3 Glacier"]

```



![image-20250626231452533](assets/image-20250626231452533.png)



### Accessing S3 Objects

لو مكنتش هتحتاج الداتا بتاعتك، مكنستش هتتعب نفسك وترفعها على `S3` أصلًا. عشان كده، لازم تفهم إزاي تعمل `access` للـ `objects` بتاعتك اللي على `S3`. والأهم من كده، إزاي تقفل الـ `access` ده وتسمح بيه بس للـ `requests` اللي بتمشي مع احتياجات البيزنس والأمان بتاعتك.

#### **الـ `Access Control`**

أول ما بتعمل `S3 bucket` جديد، هو وكل الـ `objects` اللي جواه بيكونوا مقفولين تمامًا على أي حد بره الـ `account` بتاعك، انت بس اللي بتقدر توصلهم. عشان تسمح لحد تاني يوصلهم، عندك 3 طرق أساسية:

 `Access Control List (ACL)`،
 أو `S3 Bucket Policies`، 
أو `Identity and Access Management (IAM) Policies`.

> [!CAUTION]
>
> فى حاجة اخيرة برده **`Block Public Access` : **
>
> عشان AWS تضمنلك أقصى درجات الأمان، عملت خاصية قوية جدًا اسمها **Block Public Access**. الفكرة الأساسية منها هي إنها تديلك القدرة إنك تقفل أي public access على الـ S3 bucket بتاعك وكل الـ objects اللي جواه، وده بيتم عن طريق مجموعة إعدادات. الإعدادات دي بتطبق على الـ bucket اللي بتظبطها عليه وعلى أي access points مرتبطة بيه.
>
> والـ Block Public Access ده ليه أعلى صلاحية، يعني كلامه هو اللي بيمشي. أهم نقطة فيه إنه بيتجاوز أي صلاحيات تانية ممكن تكون حاططها. يعني لو الـ ACL أو الـ Bucket Policy بتاعتك بتسمح بالوصول الـ public، والـ Block Public Access متفعل عشان يمنعه، الوصول هيتقفل فورًا.
>
> والخاصية دي موجهة بشكل أساسي للتحكم في الـ public access فقط، وهي عبارة عن أربعة options متقسمين بشكل واضح جدًا: فيه اتنين منهم موجهين للتحكم في الـ **ACLs**، واتنين تانيين موجهين للتحكم في الـ **Bucket Policy**، وده بيديك تحكم دقيق عشان تقفل كل منفذ ممكن يسبب public access غير مرغوب فيه.
> هشرح 2 بتوع acl مع acl و2 بتوع bucket poicy مع bucket policy
>
> ![image-20250715182156479](assets/image-20250715182156479.png)
>
> 

ولكن قبل ما اشرح acl او اي حاجة ثانية لازم نشرح حاجة اسمه **S3 Object Ownership**

ده إعداد على مستوى الـ S3 Bucket بيحدد مين هو "مالك" الـ objects اللي بتترفع جوه الـ bucket ده، ونتيجة لكده، بيحدد أنهي طريقة صلاحيات هتكون هي الأساسية (الـ ACLs ولا الـ Policies) او اللي شغالة
زمان، لو Account B رفع فايل في bucket بتاع Account A، كان الـ object ده بيفضل ملك Account B. ده كان بيعمل مشاكل كتير:

1. Account A (صاحب الـ bucket) مكنش يقدر يمسح الفايل أو يغير صلاحياته لأنه مش ملكه.
2. عشان Account A ياخد صلاحيات، كان لازم Account B وهو بيرفع الفايل يضيف ACL مخصوصة (bucket-owner-full-control). كانت عملية معقدة ومش مضمونة.
3. كان بيبقى عندك bucket واحد، لكن جواه objects بـ "ملاك" مختلفين، وده بيخلي إدارة الصلاحيات كابوس.

عشان كده AWS عملت الـ Object Ownership عشان تبسط الموضوع ده وتديلك اختيارات واضحة

**1. `ObjectWriter` (الوضع القديم - `Legacy`)**

الوضع ده، اللي اسمه ObjectWriter، هو الطريقة القديمة أو الـ legacy اللي كان S3 بيشتغل بيها، ومعناه إن "كاتب" الـ object أو اللي رفعه هو اللي بيفضل المالك الرسمي بتاعه. بالتطبيق العملي، لو Account B رفع فايل في bucket يملكه Account A، هيفضل الفايل ده ملكًا لـ Account B، وبالتالي Account A مش هيقدر يتحكم فيه. عشان Account A يقدر ياخد صلاحيات كاملة، لازم Account B وهو بيرفع الفايل يستخدم ACL صريحة زي 
bucket-owner-full-control عشان ينقل الملكية.

يبنا ان  الوضع ده معقد، لكنه ما زال مفيدًا في سيناريوهات محددة تتطلب استخدام الـ ACLs بشكل أساسي. على عكس الإعدادات الأحدث اللي بتقلل من دور الـ ACLs أو بتعطلها تمامًا، ObjectWriter بيدعم استخدامها بكامله. ده بيخليه ضروري في بعض التطبيقات المعقدة، زي أنظمة الـ multi-tenant اللي كل tenant فيها بيرفع داتا وعايز يحتفظ بملكية الـ objects بتاعته بشكل مستقل، أو في أي سيناريو بيتم فيه رفع ملفات من خارج الـ account وتحتاج فيه تحكم دقيق في الملكية على مستوى كل object على حدة.

#### **2. BucketOwnerPreferred ** (الوضع  Hybrid)

الوضع ده، اللي اسمه BucketOwnerPreferred، هو وضع "هجين" أو Hybrid  مصمم عشان يسهل الانتقال من الطرق القديمة للطرق الحديثة في إدارة S3. فكرته الأساسية إن "الأفضلية" في الملكية تروح لصاحب الـ bucket.

بالتطبيق العملي، لو account تاني رفع فايل عندك ومحددش أي ACL معينة في الـ request بتاعه، الملكية هتجيلك انت كصاحب bucket بشكل تلقائي، وده بيحل المشكلة الأساسية بشكل بسيط. في نفس الوقت، لو اللي بيرفع الفايل ده كان application قديم بيستخدم ACL صريحة زي bucket-owner-full-control، الـ ACL دي هتشتغل عادي جدًا والملكية هتتنقلك برضه.

والنقطة الجوهرية هنا هي إنه **بيدعم استخدام الـ ACLs بشكل كامل**. على عكس وضع BucketOwnerEnforced اللي بيعطلها تمامًا، BucketOwnerPreferred بيخليها شغالة وممكنة. ده بيخليه الاختيار المثالي في حالتين: أولًا، لو عندك workflows أو تطبيقات قديمة لسه بتعتمد على الـ ACLs في شغلها وعايز تضمن إنها متتعطلش. ثانيًا، وهو الأهم بالنسبة لأمثلتنا، لو انت نفسك محتاج تستخدم ACLs عشان تحقق هدف معين، زي إنك تخلي object واحد بس public-read من غير ما تفتح الـ bucket كله بـ policy. استخدام public-read ACL يتطلب إن الـ ACLs تكون شغالة، والوضع ده بيوفرلك المرونة دي. هو يعتبر مرحلة وسطية ممتازة بتديك أفضل ما في العالمين: بساطة الملكية التلقائية مع الحفاظ على قوة ومرونة الـ ACLs للحالات الخاصة.

**3. BucketOwnerEnforced**

الوضع ده، اللي اسمه BucketOwnerEnforced، هو أقوى وأحدث وضع لإدارة الصلاحيات في S3، وهو اللي AWS بتنصح بيه لكل الـ Buckets الجديدة وهو default وهو مش بيسمح باستخدام acl. فكرته الأساسية هي فرض قاعدة واضحة وصريحة: "صاحب الـ bucket هو المالك الوحيد والإجباري لكل object جواه، بدون أي استثناءات".

بالتطبيق العملي، لما بتفعل الوضع ده، **الـ ACLs بتتعطل تمامًا (gets disabled)** على مستوى الـ bucket كله، وS3 بيتجاهلها كأنها مش موجودة. نتيجة لكده، أي object بيترفع في الـ bucket ده، سواء انت اللي رفعته أو أي account تاني، بيصبح ملكك انت كصاحب bucket بشكل فوري وإجباري. مفيش حاجة اسمها object ليه owner مختلف.

والنقطة الجوهرية هنا هي إن إدارة الصلاحيات بتعتمد **100% وبشكل حصري على IAM Policies و Bucket Policies**. ده بيبسط إدارة الأمان بشكل كبير جدًا لأنه بيوحد كل قواعد الوصول في مكان واحد واضح، وبيلغي التعقيدات اللي كانت بتيجي من وجود ACLs متفرقة على ملايين الـ objects. هو بيقدم نموذج أمان أنظف وأسهل في المراجعة والإدارة، وبيكون الاختيار المثالي لأي سيناريو مش بيعتمد بشكل أساسي على workflows قديمة مرتبطة بالـ ACLs.



#### **1. `Access Control List (ACL)`****

الـ `ACL` هي `list` بسيطة من الـ `grants`. كل `grant` بيحدد حاجتين: `Grantee` (مين اللي هياخد الصلاحية، زي `AWS account` معين أو `group` متعرف عليه زي `AllUsers`)، و `Permission` (الصلاحية نفسها، زي `READ` أو `WRITE` أو `FULL_CONTROL`). **الـ `Scope` بتاعها:** على مستوى الـ `resource` نفسه. يعني تقدر تحط `ACL` على الـ `bucket`، و `ACL` تانية مختلفة تمامًا على كل `object` جواه بشكل منفصل. ده بيخلي إدارتها على `scale` كبير مش عملية ,**الـ `Primary Use Cases`:** استخدامها الأساسي دلوقتي هو في حالة واحدة: إدارة الـ `cross-account object ownership`.السيناريو: لو `AWS account` تاني عمل `upload` لـ `object` في الـ `bucket` بتاعك، الـ `object` ده بيفضل ملكه هو. عشان انت كصاحب `bucket` تاخد `full control` عليه، لازم الـ `account` التاني وهو بيرفع الـ `object` يحدد الـ `ACL` كـ `bucket-owner-full-control`.



### 

قبل ما نبدأ، أهم نقطة لازم تفهمها هي إن فيه `ACL` مستقلة بتتحط على **الـ `Bucket` ، و `ACL` تانية مستقلة بتتحط على **كل `Object`** جواه.

---

#### **أولًا: إعداد الـ `ACL` **

الـ `ACL` بتتكون من `grants`، وكل `grant` بيتكون من جزئين: `Grantee` و `Permission`.

##### **1. أنواع الـ `Grantees` (مين اللي بياخد الصلاحية)**

*   **`Owner`:** الـ `AWS account` اللي عمل الـ `resource`.
*   **`Specific AWS Account`:** `AWS account` تاني بتحدده بالـ `ID` بتاعه.
*   **`All Users` Group:** أي حد على الإنترنت (`public`).
*   **`Authenticated Users` Group:** أي حد عنده `AWS account` وعامل `login`.

##### **2. أنواع الـ `Permissions` **

صلاحيات الـ `ACLs` بسيطة، لكن معناها بيختلف تمامًا حسب هي محطوطة على إيه:

*   **`READ`:**
    *   **تأثيرها على الـ `Bucket`:** تسمح بعمل `List` للـ `objects` (عرض لستة أسماء الفايلات).
    *   **تأثيرها على الـ `Object`:** تسمح بقراءة محتوى الـ `object` نفسه (تحميله أو عرضه).

*   **`WRITE`:**
    *   **تأثيرها على الـ `Bucket`:** تسمح برفع `objects` جديدة (`PutObject`) ومسح `objects` موجودة (`DeleteObject`).
    *   **تأثيرها على الـ `Object`:** **لا تنطبق (Not Applicable)** لان object storage بحد ذاته مفيهاش تعديل

*   **`READ_ACP`:**
    *   **تأثيرها على الـ `Bucket`:** تسمح بقراءة الـ `ACL` بتاعة الـ `Bucket`.
    *   **تأثيرها على الـ `Object`:** تسمح بقراءة الـ `ACL` بتاعة الـ `Object`.

*   **`WRITE_ACP`:**
    *   **تأثيرها على الـ `Bucket`:** تسمح بتعديل الـ `ACL` بتاعة الـ `Bucket`.
    *   **تأثيرها على الـ `Object`:** تسمح بتعديل الـ `ACL` بتاعة الـ `Object`.

*   **`FULL_CONTROL`:**
    *   **تأثيرها على الـ `Bucket`:** كل صلاحيات الـ `Bucket` اللي فاتت.
    *   **تأثيرها على الـ `Object`:** كل صلاحيات الـ `Object` اللي فاتت.

---

#### **ثانيًا: الـ `Canned ACLs` **

دي اختصارات عملتها `AWS` عشان تسهل علينا الشغل. خلينا نشوف تأثير كل واحدة على الـ `Bucket` وعلى الـ `Object`.

*   **`private`**:
    *   **تأثيرها على الـ `Bucket`:** الـ `Owner` بس هو اللي ليه `FULL_CONTROL`. الـ `Bucket` بيكون مقفول تمامًا.
    *   **تأثيرها على الـ `Object`:** الـ `Owner` بس هو اللي ليه `FULL_CONTROL`. الـ `Object` بيكون مقفول تمامًا.

*   **`public-read`**:
    *   **تأثيرها على الـ `Bucket`:** أي حد (`AllUsers`) يقدر يعمل **`List`** للفايلات اللي جوه. (خطر لو أسماء الفايلات حساسة).
    *   **تأثيرها على الـ `Object`:** أي حد (`AllUsers`) يقدر **يقرأ ويحمل** الفايل نفسه.

*   **`public-read-write`**:
    *   **تأثيرها على الـ `Bucket`:** أي حد (`AllUsers`) يقدر يعمل `List` للفايلات، وكمان **يرفع فايلات جديدة ويمسح أي فايل**. (خطر كارثي).
    *   **تأثيرها على الـ `Object`:** **لا تنطبق (Not Applicable)**، لأن صلاحية `WRITE` ملهاش معنى على `object` موجود بالفعل. `S3` هيرجع `error` لو حاولت تطبقها.

*   **`authenticated-read`**:
    *   **تأثيرها على الـ `Bucket`:** أي حد عنده `AWS account` يقدر يعمل `List` للفايلات.
    *   **تأثيرها على الـ `Object`:** أي حد عنده `AWS account` يقدر يقرأ ويحمل الفايل.

*   **`bucket-owner-full-control`**:
    *   **تأثيرها على الـ `Bucket`:** **لا تنطبق (Not Applicable)**. ملهاش معنى على مستوى الـ `Bucket`.
    *   **تأثيرها على الـ `Object`:** ده هو الحل لمشكلة الـ `cross-account ownership`. لما `account` تاني يرفع `object` ويحط عليه الـ `ACL` دي، هو كده بينقل الـ `FULL_CONTROL` على الـ `object` ده لصاحب الـ `Bucket`.



#### تعالى ناخد شوية امثلة 

![image-20250723185241274](assets/image-20250723185241274.png)



هتلبس فى مشكلة  `Block Public Access` اللي انا قلت عليه فوق 

![image-20250723222513757](assets/image-20250723222513757.png)

### **1. `Block public access to buckets and objects granted through new public access control lists (ACLs)`**

في `Terraform` اسمه `block_public_acls`. وظيفته إنه يرفض أي `API call` مستقبلية بتحاول تطبق `ACL` جديدة تكون `public`. بالتطبيق ، لو أي `request` لرفع `object` جديد (`PutObject`) طلب إن الـ `ACL` بتاعته تكون `public-read`، الـ `request` ده كله هيتفشل من `S3` وهيرجع `error` بيقول `Access Denied`. الرفض هنا شامل، يعني مش بس الـ `ACL` هي اللي بتترفض، لأ ده الـ `object` نفسه مش بيترفع من أساسه. الإعداد ده لا يعمل بأثر رجعي، بمعنى إن أي `ACL` قديمة كانت `public` ستظل كما هي.

> [!NOTE]
>
> PutObject : لـ API call اللي بيستخدمه AWS S3 علشان ترفع ملف او objectجديد داخل bucket.

### **2. `Block public access to buckets and objects granted through any access control lists (ACLs)`**

في `Terraform` اسمه `ignore_public_acls`. الإعداد ده بيعطل فعالية أي `ACL` تمنح `public access`، بغض النظر إذا كانت الـ `ACL` موجودة حاليًا أو يتم محاولة إضافتها. وقت استقبال أي `request` للوصول،للـ `S3` هيتجاهل أي `public ACL` موجودة على الـ `object` كما لو كانت غير موجود. ودا يؤدي إلى رفض اي
 public access  للـ `object` حتى لو كان يمتلك `ACL` تسمح بده. 

### 

طب لو فهمته وجيت وشغلت هيطلع معاك نفس error 
![image-20250723185944985](assets/image-20250723185944985.png)

طب ليه عشان الترتيب هنا مش مزبوط خد فى بالك ان لازم aclيكون الاول يتحط وبعدها تحت block_public_acls و ignore_public_acls انت لازم تتحكم فى ترتيب بتاع التنفيذ
فى acls مع public access block انت لازم تاخد بالك فى التحكم فى الترتيب يعني ممكن تنفذ terraform على اكثر من مرة عشان توصل ل state اللي انت عايزه 

بص خد فى بالك برده حتى لو التزمت بالترتيب هتلبس فى مشكلة S3 Object Ownership 

![image-20250723223754780](assets/image-20250723223754780.png)

دا هيرجع بينا لمشكلة  ان الوضع defualt لل S3 Object Ownership هو  BucketOwnerEnforced 
 وانه Default من سنة 2023 هو BucketOwnerEnforced وبكد فهمت ليه باخد error 

![image-20250723211806757](assets/image-20250723211806757.png)

> [!IMPORTANT]
>
> 
>
> حتة مهمة جدًا في `Terraform`. لو أنا حذفت الـ `resource block` ده:
> ```terraform
> resource "aws_s3_bucket_ownership_controls" "example" {
>   bucket = aws_s3_bucket.just_example.id
> 
>   rule {
>     object_ownership = "BucketOwnerPreferred"
>   }
> }
> ```
> `Terraform` **مش هيرجع للوضع `default` بتاع الـ `provider`**. وده لأن في الحالة دي، `Terraform` بيبص على الكود الجديد ويلاقي إن الـ `resource` ده مبقاش موجود، فبيفهم من كده إنك بتقوله: "أنا مبقتش عايزك تدير الإعداد ده خالص، شيله من   الـ `state file` بتاعك". هو بيعتبرها إشارة لـ`stop managing` مش إشارة لـ "العودة للافتراضي" (`revert to default`).
>
> وبالتالي، هو مش هيبعت أي `API call` لـ `AWS` عشان يغير الإعداد ده، هو ببساطة هيشيل إيده من الموضوع وهيسيب الإعداد في `AWS` على **آخر وضع كان عليه**.
>
> **بس ده غلط ليه؟**
> زي ما انت قلت بالظبط، الدنيا هتشتغل معاك والـ `ACLs` هتفضل شغالة، لكنك كده بتعمل حاجة بتعاكس واحد من أهم مبادئ شغلنا، وهو إن **Single Source of Truth**. اللي بيحصل ده بيخلق حالة خطيرة جدًا اسمها **`Configuration Drift`** 
>
> النتيجة إن الإعداد في `AWS Console` هيفضل زي ما هو على آخر وضع كان عليه `BucketOwnerPreferred`
>
> ![image-20250723225523342](assets/image-20250723225523342.png)
>
> ... وبكده بقى عندك `Configuration Drift`. بقى الكود بتاعك بيقول حاجة (إنه مش بيدير الـ `ownership`)، والواقع في `AWS` بيقول حاجة تانية خالص (إن الـ `ownership` لسه متظبط عشان يسمح بالـ `ACLs`).
>
> **وده بيفتح الباب لمشاكل كبيرة في المستقبل:** أي مهندس جديد هيبص على الكود بتاعك مش هيفهم أبدًا ليه الـ `ACLs` شغالة، وهيعتبر إن فيه "شئ غريب" بيحصل.لو حد عمل أي تغيير تاني في المستقبل، النتيجة ممكن تكون غير متوقعة تمامًا لأن الكود مبقاش بيعبر عن الواقع.
>
> أي `scripts` أو `pipelines` بتعتمد على الكود ده عشان تاخد قرارات ممكن تفشل أو تعمل حاجات غلط.
>
> الحل الصح دايمًا هو إنك تخلي الكود يعبر عن الحالة النهائية اللي انت عايزها، مش إنك تمسح الـ `resource` وتسيبه "عايمة" 



```terraform
resource "aws_s3_bucket" "just_example" {
  bucket = "hallow-word-just-examples123"

}
resource "aws_s3_bucket_ownership_controls" "example" {
  bucket = aws_s3_bucket.just_example.id

  rule {
    object_ownership = "BucketOwnerPreferred"
  }
}


resource "aws_s3_bucket_public_access_block" "allow_access_to_public" {
  bucket = aws_s3_bucket.just_example.id
    block_public_acls       = false
    ignore_public_acls      = false
    
}
resource "aws_s3_bucket_acl" "allow_public_read" {
  bucket = aws_s3_bucket.just_example.id
  acl    = "private"
  depends_on = [ aws_s3_bucket_public_access_block.allow_access_to_public ]

}

resource "aws_s3_object" "file_private" {
  bucket = aws_s3_bucket.just_example.id
  
  key    = "just_prefix/private.txt"
  
  source = "private_file.txt" 
  
  acl    = "private"
  }

  resource "aws_s3_object" "file_public" {
  bucket = aws_s3_bucket.just_example.id
  
  key    = "just_prefix/public.txt"
  
  source = "public_file.txt" 
    acl    = "public-read"

  }
  

  output "public_file_url" {
  description = "The public URL for the public file."
  value       = "https://${aws_s3_bucket.just_example.bucket}.s3.${aws_s3_bucket.just_example.region}.amazonaws.com/${aws_s3_object.file_public.key}"
}

output "private_file_url" {
  description = "The URL for the private file (should return Access Denied)."
  value       = "https://${aws_s3_bucket.just_example.bucket}.s3.${aws_s3_bucket.just_example.region}.amazonaws.com/${aws_s3_object.file_private.key}"
}


```



دا المثال بالكام

خلي باللك فى فرق كبير    ` acl    = "public-read` اللي على bucket دي معناها ان اي حد يقدر يقراء اسماء files على s3 بس ميقدرش يمسحه وﻻ يعمل حاجة 

![image-20250724004125491](assets/image-20250724004125491.png) 

> [!WARNING]
>
> ---
>
> ### **إزاي تنادي على الفايل بتاعك في `S3`؟**
>
> النوع الأول هو الـ **S3 URI**، 
> وده يعتبر الـ identifier بتاع شغل AWS الداخلي. هو عبارة عن identifier مختصر بتستخدمه لما تكون بتتكلم مع S3 من جوه منظومة AWS نفسها، وعشان كده هو مش لينك تقدر تفتحه في browser، لكنه أشبه بـ local address بتفهمه خدمات AWS. تركيبته بسيطة جدًا، بتكون على صيغة s3://[bucket-name]/[object-key]
>
> ، زي المثال اللي في الكود s3://hallow-word-just-examples123/just_prefix/private.txt. أما استخدامه الأساسي فبيكون لما تشتغل بـ AWS CLI في أوامر زي aws s3 cp أو aws s3 ls، أو لما بتكتب كود بـ AWS SDKs زي Python أو Node.js، وكمان لما بتكون خدمة زي AWS Glue عايزة تقرأ داتا من S3. ده.
>
> ---
>
> النوع الثاني هو الـ **Amazon Resource Name (ARN)**، وده هو الـ identifier اللي بيكون globally unique لأي resource جوه AWS كلها، مش بس لـ S3. ومهمته الأساسية إنه يحدد الـ resource ده بشكل قاطع ومفيهوش أي لخبطة. بيتكون بصيغة ثابتة هي arn:[partition]:s3:::[bucket-name]/[object-key]
> ، زي المثال اللي في الصورة arn:aws:s3:::hallow-word-just-examples123/just_prefix/private.txt. أما استخدامه الرئيسي فبيكون في إدارة الصلاحيات (Permissions Management)، فلما بتيجي تكتب IAM Policy أو Bucket Policy عشان تدي صلاحيات لـ user أو role، لازم تستخدم الـ ARN في خانة الـ Resource لأن IAM مبيفهمش صيغة الـ S3 URI..
>
> ---
>
> النوع التالت والأكثر شيوعًا هو الـ **Object URL**، وده ببساطة هو اللينك العادي بتاع الـ browser اللي بنستخدمه كلنا على الإنترنت. هو عبارة عن HTTPS endpoint تقدر تاخده كوبي وتحطه في أي browser عشان تفتح الفايل مباشرةً. الأفضل دايمًا إنك تستخدم الصيغة اللي فيها الـ region عشان تتجنب أي مشاكل، وبتكون على شكل https://[bucket-name].s3.[region].amazonaws.com/[object-key]، زي المثال اللي في 
> الصورة https://hallow-word-just-examples123.s3.eu-west-1.amazonaws.com/just_prefix/private.txt. أما استخدامه فبيكون لأي وصول جاي من بره منظومة AWS، زي إنك تفتح الفايل في browser، أو تحط صورة في موقعك جوه <img> تاج، أو تبعت لينك لحد يحمل منه الفايل، وده طبعًا بيعتمد على إن الفايل يكون معموله public-read.
>
> ---
>
> 



> [!TIP]
>
> ![image-20250724004809361](assets/image-20250724004809361.png)
>
> **4. `Entity Tag (Etag)` **
>
> هو عبارة عن `hash` بيتعمل من محتوى الفايل. لو غيرت حرف واحد في الفايل ورفعته تاني، الـ `Etag` بتاعه هيتغير.
>
> *   **بستخدمه امتى؟**
>     *   **`Data Integrity`:** عشان تتأكد إن الفايل اللي نزل عندك هو هو نفس الفايل اللي على السيرفر ومحصلش فيه أي تلف.
>     *   **`Caching`:** الـ `Browsers` والـ `CDNs` بيستخدموه عشان يعرفوا إذا كان الفايل اتغير ولا لأ، فبدل ما يحملوه كل مرة، بيستخدموا النسخة اللي عندهم لو الـ `Etag` هو هو.

مثال ثاني رفع ملف للاسف مش هعرف اجربه او مش هيكون ليه لزمه 

```TERRAFORM

resource "aws_s3_object" "daily_report" {
  bucket = "bucket-name-in-account-a-1234"
  key    = "daily-reports/report-2024-07-28.csv"
  content = "col1,col2\nval1,val2"


  acl    = "bucket-owner-full-control"
}

```

لو انا مفعل object_ownership=objectwrite وعيز ادي صلاحية لصاحب bucket على كل object



> [!CAUTION]
>
> طب دلوقتي عيزين ندي امثلة بعيدا عن CANNED  
> هنا بقى بيظهر واحد من أهم عيوب ACL، ويمكن اعتباره "العيب القاتل" اللي بيحد من استخدامها: **إنك مش بتقدر تدي صلاحيات لـ IAM users أو IAM groups معينة**. نظام الـ ACL مش بيشوف  IAM في الـ account بتاعك أصلًا.
>
> وأقولك على حاجة، كل الجروبات اللي الـ ACL بتفهمها هما ثلاثة بس مفيش غيرهم: جروب اسمه **AllUsers Group** وده بيمثل أي حد في الدنيا على الإنترنت، وجروب تاني اسمه **AuthenticatedUsers Group**  وده بيمثل أي شخص مسجل في AWS وعامل login وجروب ثالث بيمثل خدمات logs  وبس كده، مفيش أي طريقة تانية تعمل بيها جروبات أو تدي صلاحيات مفصلة لمستخدمين معينين باستخدام ACL لوحدها فعشان كده اغلب الناس بتكتفى بي canned acl 
> طب ليه مينفعش acl مع iam users لانها بس بتتعامل مع id بتاع acount

![image-20250724040357018](assets/image-20250724040357018.png)

دا مثال عندنا نوعين CanoicalUser ودا اي root user وبحط id بتاعه وبحدد نوعه وبديله permission read و برده لو عايز احط جروب هما 3 بس وبستخدام uri عشان احدد جروب معين 

| الاسم                   | URI                                                         |
| ----------------------- | ----------------------------------------------------------- |
| **All Users**           | `http://acs.amazonaws.com/groups/global/AllUsers`           |
| **Authenticated Users** | `http://acs.amazonaws.com/groups/global/AuthenticatedUsers` |
| **Log Delivery**        | `http://acs.amazonaws.com/groups/s3/LogDelivery`            |

**Log Delivery** شوية خدامات من aws تقدر تاخد Logs منهم زي **S3 Server Access Logging** و**AWS CloudTrail** و **Elastic Load Balancing** و**Amazon CloudFront:**و**AWS Config**

بس خلي بالك aws_s3_object فى terraform مش بتدعم تدي grant على object وينفع aws cli

------

---

#### **2. `S3 Bucket Policies`**
دي `resource-based policy` بتتكتب بصيغة `JSON` وبتتعلق بالـ `bucket` نفسه. الـ `policy` دي عبارة عن `statements`، كل `statement` بيحدد:

*   `Effect`: إما `Allow` أو `Deny`.
*   `Principal`: مين اللي بتطبق عليه القاعدة (user, account, service, أو `*` للكل).
*   `Action`: إيه الـ `API calls` المسموحة أو الممنوعة (زي `s3:GetObject`).
*   `Resource`: القاعدة دي بتطبق على أنهي `resources` بالظبط (الـ `bucket ARN` أو `objects` معينة).
*   `Condition`: شروط إضافية لازم تتحقق، زي إن الـ `request` يكون جاي من `IP address` معين.

*   **الـ `Scope` بتاعها:**
    على مستوى الـ `bucket` بالكامل. هي أداة مركزية بتطبق سياسة واحدة على كل الـ `objects` اللي جوه الـ `bucket`يعني مينفع ترفع ملف بbaucket policy زي acl

*   **الـ `Primary Use Cases`:**
    *   إدارة الـ `cross-account access` بشكل مركزي.
    *   إعطاء `public access` لـ `bucket` كامل أو جزء منه، زي في حالة
          `hosting static website`.
    *   فرض قواعد أمان على مستوى الـ `bucket`، زي إجبار استخدام `HTTPS` (`aws:SecureTransport`) أو إجبار الـ `server-side encryption`.

فكر فيها كده: الـ `statement` كله هو اللي بيمثل الـ `if` statement، متقسم كالتالي:

**الجزء الأول: الشرط المركب (The `if` part)**
الشرط بتاعك مش بس الـ `Principal`. الشرط هو **كل دول مع بعض**:

*   **`if`** الـ `request` اللي جاي **مبعوت من** الـ `Principal` ده...
*   **`AND if`** الـ `request` ده **بيحاول يعمل** واحدة من الـ `Actions` دي...
*   **`AND if`** الـ `request` ده **متوجه للـ** `Resource` ده...
*   **`AND if`** الـ `request` ده **بيحقق كل** الـ `Conditions` الإضافية دي...

**لازم كل الشروط دي تتحقق** عشان `S3` يقول "أه، القاعدة دي ليها علاقة بالـ `request` ده".

**الجزء الثاني: النتيجة (The `then` part)**
لو كل الشروط اللي فاتت دي اتحققت، **`then`** `S3` هيبص على الـ `Effect`:

*   **`then Effect is "Allow"`**: ساعتها `S3` هيسمح بالـ `request` (طالما مفيش `Deny` صريح في حتة تانية).
*   **`then Effect is "Deny"`**: ساعتها `S3` هيرفض الـ `request` فورًا، ومش هيكمل يبص على أي حاجة تانية

> [!TIP]
>
> الـ **Principal** هو الجزء في الـ Policy اللي بيحدد **"مين"** الكيان اللي القاعدة بتنطبق عليه. لما بنكون بنتكلم عن IAM Roles، الـ Principal ده بيتعرف جوه حاجة  منفصلة اسمها **Trust Policy**، ومهمته ساعتها إنه يحدد مين مسموحله يستخدم الـ Role ده.
>
> الـ Principal ممكن ياخد أكتر من شكل، مش شرط يكون ARN بس. طريقة كتابته بتعتمد على **الـ key** اللي بتستخدمه:
>
> - لما يكون الـ key هو **AWS**، ساعتها القيمة لازم تكون ARN (زي ARN بتاع user أو role أو account).
> - وفعلًا، القيمة بتاعة الـ key اللي اسمه AWS ممكن تكون قيمة واحدة (string) أو list من الـ ARNs عشان تدي نفس الصلاحية لكذا identity مرة واحدة.
> - انت **لازم تحدد الـ key** بشكل صريح (AWS, Service, Federated...). لو محددتش key، الـ policy هتكون غلط. الطريقة الوحيدة اللي مش بتستخدم فيها key هي لما تحط Principal: "*" عشان تدي صلاحية للكل.
> - وزي ما الـ Principal بيقبل list، كمان الـ Action والـ Resource ممكن ياخدوا list من القيم عشان تطبق نفس القاعدة على أكتر من فعل أو resource مرة واحدة. (الـ Condition تركيبته مختلفة شوية لكنه بيقدر يتعامل مع قيم متعددة برضه).
>
> ``` terraform
>         Principal = {
>           AWS = [
> 
>             "arn:aws:iam::111122223333:user/taha", 
>             
> 
>             "arn:aws:iam::111122223333:role/AnalyticsServerRole", 
>             
> 
>             "arn:aws:iam::444455556666:root" 
>           ]
>         },
> //=================
>             Principal = {
>           AWS =  "arn:aws:iam::111122223333:user/taha", 
>             
> 
>             
>         },
> ```
>
> 
>
> **1. `AWS` Principal**
>
> *   **بيتعامل مع:** أي `identity` متعرفة بشكل مباشر جوه `IAM`.
> *   **القيمة بتاعته بتكون:**
>     *   **`ARN` بتاع `AWS Account` كامل:** زي `arn:aws:iam::111122223333:root`. ده بيدي صلاحية لكل الـ `identities` (users and roles) اللي جوه الـ `account` ده.
>     *   **`ARN` بتاع `IAM User` معين:** زي `arn:aws:iam::111122223333:user/taha-dev`.
>     *   **`ARN` بتاع `IAM Role` معين:** زي `arn:aws:iam::111122223333:role/EC2-Admin-Role`.
>
> *    لو عايز تدي `EC2 instance` حق إنها تعمل `access` على `S3`، **مينفعش تحط `ARN` مباشر للـ `EC2 instance`** في `policy`، لأن الـ `instances` مش بتعتبر `principals` ثابتة وليها `ARN` زي الـ `users` والـ `roles`. الطريقة الوحيدة والصحيحة هي إنك **لازم تدي للـ `EC2` دي `IAM Role`**، ومن خلال الـ `Role` ده هي اللي بتقدر تعمل `access` للـ `S3`. فالـ `policy` اللي هتكتبها على `S3 bucket` هتدي الصلاحية للـ `ARN` بتاع الـ `Role`، مش الـ `instance`.
>
> ---
>
> **2. `Service`**
>
> *   **بيتعامل مع:** خدمة من خدمات `AWS` نفسها، ككيان برمجي.
>
> *   **القيمة بتاعته بتكون:** اسم الـ `Service Principal`. ده بيكون على صيغة
>      `[service-name].amazonaws.com`.
>
> *   "Principal": { "Service": "ec2.amazonaws.com" }   مثلا دي معناها اسمح لاي ec2 مقدرش احدد اي ec2  لو عايز احدد لازم اروح للنوع الاول وادي ec2 ليه role ومن خلاله تقدر تعمل access
>
>     
>
> ---
>
> تمام، كلامك مظبوط وفي الصميم، والشرح ده بيوضح الفكرة الأساسية بشكل ممتاز. خلينا نعيد صياغته مع إضافة التوضيحات اللي انت طلبتها عشان يبقى دقيق 100%.
>
> ---
>
> **`Principal` من نوع `Federated`**
>
> الفكرة الأساسية هنا هي إنك عايز تدي صلاحيات لناس **معندهمش `IAM user`** في الـ `account` بتاعك. هويتهم موجودة في نظام تاني خالص بره `AWS`، زي حساباتهم في `Google` أو حساباتهم على الـ `network` بتاع الشركة (`Active Directory`).
>
> الـ `Principal` اللي من نوع **`Federated`** هو اللي بتخلي الـ `users` دول يقدروا يخشوا عالم `AWS` بتاعك ويشتغلوا بصلاحيات مؤقتة.
>
> **إزاي ده بيحصل؟**
> العملية بتعتمد على إنشاء **`IAM Role`** معين، والـ `Principal` ده بيستخدم **حصرًا** في **الـ `Trust Policy`** بتاعة الـ `Role` ده.
>
> الـ `Trust Policy` دي بتقول: "أنا كـ `Role`، بثق في الـ `Identity Provider (IdP)` ده، وهسمح لأي `user` يجيلي منه بـ `token` سليم إنه يستلف صلاحياتي بشكل مؤقت."
>
> ---
>
> **المثال: `Web Identity (OIDC)` - زي `Login with Google`**
>
> عندك `mobile app`، وعايز المستخدمين بتوعك يقدروا يعملوا `login` بحساب `Google` بتاعهم، وبعد ما يعملوا `login`، يقدروا يرفعوا صورهم على `S-S3 Bucket`.
>
> 
> 1.  **التجهيز:** انت بتروح على `IAM` وتعمل `Identity Provider` من نوع `OIDC` وتعرفه على `Google`. `AWS` بتديلك `ARN` للـ `IdP` ده (زي `arn:aws:iam::...:oidc-provider/accounts.google.com`).
> 2.  **بناء الثقة:** بتعمل `IAM Role` والـ **`Trust Policy`** بتاعته بيكون فيها الـ `Principal` ده:
>     ```json
>     "Principal": {
>       "Federated": "arn:aws:iam::111122223333:oidc-provider/accounts.google.com"
>     }
>     ```
> 3.  **العملية:** أي `user` هيعمل `login` بـ `Google` ويجي للـ `app` بتاعك بـ `token`، الـ `app` هياخد الـ `token` ده ويروح بيه لخدمة **`AWS STS`** (مش `S3` مباشرةً) ويطلب منها صلاحيات مؤقتة من الـ `Role` ده. بعد ما ياخد الـ `credentials` المؤقتة، يقدر يستخدمها عشان يكلم أي خدمة `AWS` (مش شرط `S3` بس) وينفذ الـ `actions` اللي متحددة في **الـ `Permissions Policy`** بتاعة الـ `Role`.
>
> 
>
> ---
>
> **4. `CanonicalUser`**
>
> * **بيتعامل مع:** `aws root account` فقط، لكن باستخدام الـ `ID` القديم بتاعه.
>
> * **القيمة بتاعته بتكون:** الـ `Canonical User ID` بتاع الـ `AWS Account`.
>
>   *   `"CanonicalUser": "79a59df900b949e55d96a1e698fbaced..."`
>
>   
>
> 
>
> 

بس قبل ما نخش نشرح Block Public Access الاثنين الخاصين ب**3.**
** `Block public access to buckets and objects granted through new public bucket or access point policies`**

في Terraform اسمه `block_public_policy`. وظيفته إنه يمنعك من *حفظ* أي `Bucket Policy` جديدة فيها `public access`. الـ `API call` نفسها اللي بتحاول تحفظ الـ` policy` (اللي هي PutBucketPolicy) هي اللي بتفشل، وS3 هيرجعلك `error` يمنع الـ `policy` دي إنها تتخزن أصلًا. هو عامل زي validator بيمنع الغلطة قبل ما تحصل، وبيضمن إن misconfiguration زي دي متقدرش تتسيف على الـ bucket بتاعك.

> [!TIP]
>
> `PutBucketPolicy` هو الـ API اللي بيخليك **تضيف أو تعدّل Policy كاملة** على الـ S3 bucket،



**`Block public and cross-account access to buckets and objects through any public bucket or access point policies`**

الإعداد ده، اللي اسمه restrict_public_buckets في Terraform، هو "شبكة الأمان" الأخيرة بتاعتك ضد أي policy عامة، خصوصًا القديمة منها.

فكرته مش إنه يمنعك تحفظ policy فيها public access زي أخوه (block_public_policy)، لأ، هو بيسيبك تحفظها عادي خالص. قوته الحقيقية بتبان بعد كده، وقت ما request حقيقي بيوصل للـ bucket.

لما S3 يجي يفحص الطلب ده، وأول حاجة يلاقيها إن الإعداد ده شغال، بيعمل حركة ذكية جدًا: وهو بيقرأ الـ Bucket Policy، بيقوم عامل "تجاهل" أو ignore لأي statement فيها بيدي public access (يعني أي statement فيه Principal: "*"). بيتعامل مع السطور دي كأنها مش موجودة في اللحظة دي.

وبعد ما يتجاهل الأجزاء الـ public دي، بيبص على اللي فاضل من الـ policy. لو ملقاش أي صلاحية تانية تسمح بالوصول، يبقى القرار النهائي هيكون Access Denied، والطلب هيترفض. وده بيحصل حتى لو الـ policy الأصلية اللي انت حافظها كانت بتقول اسمح بالوصول.

الميزة الجبارة هنا هي إنك بتقدر تأمّن bucket قديم عليه policies معقدة بشكل فوري، من غير ما تحتاج تفتح ملف الـ JSON المخيف ده وتخاطر إنك تبوظ الدنيا بالغلط. هو بيعطل تأثير الـ policy الخطرة من غير ما يمسحها.

لازم نعرف ايه actions اللي تتم على s3 فى actions كتير قوي احنا بس هنذكر اكثرهم 

**أولًا: `Actions` على مستوى الـ `Object` (للتعامل مع objects)**

دي أشهر حاجة هتستخدمها. الـ `Resource` بتاعها دايمًا بيكون `arn:aws:s3:::your-bucket/*`.

1.  **`s3:GetObject`**
    *   **دي بتاعة إيه؟** قراءة وتحميل محتوى الفايل. دي أهم صلاحية لو عايز تدي لحد صلاحية قراءة.
    *   عشان تفتح صورة في `browser`، تحمل `PDF`، أو أي `application` يقرأ داتا من فايل.

2.  **`s3:PutObject`**
    *   رفع فايل جديد أو عمل `overwrite` على فايل موجود بنفس الاسم.
    
3.  **`s3:DeleteObject`**
    
    ​	مسح فايل معين.

    
    
4.  **`s3:GetObjectAcl`**
    
    قراءة الـ `ACL` بتاعة فايل معين عشان تعرف مين مسموحله يوصله.
    
5.  **`s3:PutObjectAcl`**
    *   **دي بتاعة إيه؟** تعديل الـ `ACL` بتاعة فايل معين.

6.  **`s3:RestoreObject`**
    
    *   لو الفايل بتاعك متخزن في `storage class` زي `Glacier` ، الصلاحية دي بتخليك تقدر تعمل `restore` للفايل عشان تقدر تستخدمه.

---

### **ثانيًا: `Actions` على مستوى الـ `Bucket` (للتعامل مع الحاوية)**

دي بتتحكم في إدارة الـ `bucket` نفسه. الـ `Resource` بتاعها دايمًا بيكون `arn:aws:s3:::your-bucket`.

1.  **`s3:ListBucket`**

    *   **دي بتاعة إيه؟** عرض لستة بأسماء الـ `objects` اللي جوه الـ `bucket`.
    *   **استخدامها:** ضرورية جدًا عشان الـ `AWS Console` أو الـ `CLI` يقدروا يعرضولك محتويات الـ `bucket`.

2.  **`s3:DeleteBucket`**
    *   **دي بتاعة إيه؟** مسح الـ `bucket` بالكامل (لازم يكون فاضي الأول). صلاحية خطيرة جدًا.

3.  **`s3:GetBucketLocation`**
    *   **دي بتاعة إيه؟** بتخليك تعرف الـ `bucket` ده موجود في أنهي `region`. صلاحية بسيطة وغالبًا بتكون مسموحة.

4.  **`s3:GetBucketPolicy` / `s3:PutBucketPolicy` / `s3:DeleteBucketPolicy`**

    ​	للتحكم في الـ `Bucket Policy` نفسها (قراءة، تعديل، حذف). دي صلاحيات إدارية قوية.

5.  **`s3:GetLifecycleConfiguration` / `s3:PutLifecycleConfiguration`**

6.  **دول بتوع إيه؟** للتحكم في قواعد الـ `Lifecycle` اللي بتمسح الفايلات القديمة أو بتنقلها لتخزين أرخص.

> [!TIP]
>
> خلي بالك مش كله ينفع يتحط على object بعينة حجات زي**`s3:GetBucketPolicy`  `s3:PutBucketPolicy`  `s3:DeleteBucketPolicy`**و`s3:GetLifecycleConfiguration` `s3:PutLifecycleConfiguration`
>
> تقدر تقول s3:ListBucket معاها بس aws عمل ليه طريقة معين عشان تقدر تعملها على perfix معينه
>
> يعني لازم تحطه على bucket نفسه
>
> ```terraform
> resource "aws_s3_bucket_policy" "public_read_policy" {
>   bucket = aws_s3_bucket.public_read_bucket.id
> 
>   policy = jsonencode({
>     Version = "2012-10-17",
>     Statement = [
>       {
>         Effect    = "Allow",
>         Principal = "*",
>         Action    = "s3:GetObject",
>         Resource  = aws_s3_bucket.public_read_bucket.arn <==
>       }
>     ]
>   })
> }
> ```
>
> عشان تتطبق على على bucket كلها 
> طب لو عايز bucket وكل اللي جوها متشغلش دماغك وتخليه 
>
> ```terraform
>         Resource  = `${aws_s3_bucket.public_read_bucket.arn}*` <==
> ```
>
> مش هتشتغل ........ انت قصدك objects  بس 
>
> عشان تحدد كله لازم 
>
> ```
> Resource  = [`${aws_s3_bucket.public_read_bucket.arn}/*`,aws_s3_bucket.internal_docs_bucket.arn]
> ```
>
> 



---

**ثالثًا: الـ `Wildcards` **

عشان تسهل على نفسك، ممكن تستخدم `wildcards` (`*` و `?`) عشان تدي مجموعة صلاحيات مرة واحدة.

1.  **`s3:Get*`**
    *   **دي بتاعة إيه؟** بتدي كل الصلاحيات اللي بتبدأ بـ `Get` (زي `GetObject`, `GetObjectAcl`, `GetBucketLocation`...). طريقة سريعة عشان تدي صلاحيات قراءة شاملة.

2.  **`s3:Put*`**
    *   بتدي كل الصلاحيات اللي بتبدأ بـ `Put` (زي `PutObject`, `PutBucketPolicy`...).

3.  **`s3:*`**
    *   **دي بتاعة إيه؟** **بتدي كل الصلاحيات الممكنة على `S3`**. 





شوية امثلة 

```terraform
resource "aws_s3_bucket" "public_read_bucket" {
  bucket = "my-tf-policy-public-read-bucket"
}

resource "aws_s3_bucket_public_access_block" "public_read_access" {
  bucket = aws_s3_bucket.public_read_bucket.id

  block_public_policy     = false
  restrict_public_buckets = false
}

resource "aws_s3_bucket_policy" "public_read_policy" {
  bucket = aws_s3_bucket.public_read_bucket.id

  policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Effect    = "Allow",
        Principal = "*",
        Action    = "s3:GetObject",
        Resource  = "${aws_s3_bucket.public_read_bucket.arn}/*"
      }
    ]
  })
}
```



1. **if** الـ **Principal** هو **"\*"**...
   - **معناه:** لو الشخص اللي بيطلب الـ request هو **أي حد في العالم على الإنترنت** (سواء عنده account أو لأ، anonymous).
2. **AND if** الـ **Action** هي **"s3:GetObject"**...
   - **معناه:** لو الفعل اللي بيحاول يعمله هو "قراءة أو تحميل object" (فايل).
3. **AND if** الـ **Resource** هو **".../my-tf-policy-public-read-bucket/\*"**...
   - **معناه:** لو الفايل اللي بيحاول يوصله موجود **جوه** الـ bucket ده 
     (علامة /* معناها "أي object بداخل الـ bucket") وﻻ حظ اننا بستخدام arn اللي هو amazon resource name

**الجزء الثاني: النتيجة (then)**

- **then** الـ **Effect** هو **"Allow"**.
  - **معناه:** لو كل الشروط اللي فاتت دي اتحققت، يبقى النتيجة هي **"السماح بالطلب"**.



---

**تقييد منع من `IP Address` معين فقط**

 عندك `bucket` فيه ملفات داخلية بتاعة الشركة، ومش عايز أي حد يوصلها إلا الموظفين اللي شغالين من مكتب الشركة (يعني من الـ `IP` بتاع الشركة).

**فكرتها إيه؟**
هنا بنستخدم الـ `Condition` بلوك عشان نحط شرط على الـ `IP` اللي جاي منه الـ `request`.

```terraform
variable "office_ip_range" {
  description = "The public IP address range for the company office."
  type        = string
  default     = "203.0.113.0/24"  
}

resource "aws_s3_bucket" "internal_docs_bucket" {
  bucket = "my-company-internal-docs-bucket"
}

resource "aws_s3_bucket_policy" "ip_restriction_policy" {
  bucket = aws_s3_bucket.internal_docs_bucket.id

  policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Effect = "Deny",
        Principal = "*",
        Action = "s3:*",
        Resource = [
          aws_s3_bucket.internal_docs_bucket.arn,
          "${aws_s3_bucket.internal_docs_bucket.arn}/*"
        ],
        Condition = {
          NotIpAddress = {
            "aws:SourceIp" = var.office_ip_range
          }
        }
      }
    ]
  })
}
```
*   **Effect: "Deny"`**: احنا هنا بنستخدم سياسة الرفض، وهي أقوى.
*   **`Condition: { NotIpAddress: ... }`**: ده الشرط . معناه: "لو الـ `IP` بتاع الـ `request` **مش** في الرينج ده...".
*   (`Deny`) أي حد (`*`) من إنه يعمل أي حاة (`s3:*`) على الـ `bucket` ده، **بشرط إن** الـ `IP` بتاعه مش هو الـ `IP` بتاع الشركة".
*   محدش هيعرف يوصل للـ `bucket` ده إلا من جوه شبكة الشركة.



---

### **إجبار كل الملفات المرفوعة إنها تكون `Encrypted`**

**السيناريو:** عايز تضمن إن كل فايل بيترفع على الـ `bucket` بتاعك لازم يكون معموله `Server-Side Encryption` عشان الأمان والـ `Compliance`. لو حد حاول يرفع فايل عادي، ترفضه.

**فكرتها إيه؟**
هنستخدم `Deny` statement تاني، بس المرة دي الشرط هيكون على الـ `header` بتاع الـ `request`.

**الكود:**
```terraform
resource "aws_s3_bucket" "encrypted_uploads_bucket" {
  bucket = "my-encrypted-uploads-bucket"
}

resource "aws_s3_bucket_policy" "force_encryption_policy" {
  bucket = aws_s3_bucket.encrypted_uploads_bucket.id

  policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Effect = "Deny",
        Principal = "*",
        Action = "s3:PutObject",
        Resource = "${aws_s3_bucket.encrypted_uploads_bucket.arn}/*",
        Condition = {
  StringNotEquals = {
    "s3:x-amz-server-side-encryption" = "aws:kms"  
  }

        }
      }
    ]
  })
}
```
*   **`Action: "s3:PutObject"`**: القاعدة دي بتطبق على عمليات الرفع بس.
*   انا هنا اجبرته على نوع تشفير معين اللي هو 
*   محدش هيقدر يرفع فايل على الـ `bucket` ده إلا لو حدد صراحةً إنه عايز يستخدم `Server-Side Encryption`.











ترتيب bucket policy بيحصل ازاي 

1.  **هل فيه أي `Deny` صريح؟**
    *   أول حاجة `IAM` بيدور عليها هي أي `statement` يكون الـ `Effect` بتاعه `"Deny"` وينطبق على الـ `request` ده.
    *   لو لقى **واحد بس**، بيوقف عملية التقييم كلها فورًا ويرجع بنتيجة **`Deny`**. مش بيبص على أي `Allow` موجود في أي حتة تانية.

2.  **لو مفيش `Deny`، هل فيه أي `Allow` صريح؟**
    *   لو `IAM` دور وملقاش أي `Deny` صريح، بيكمل ويدور على `statement` يكون الـ `Effect` بتاعه `"Allow"` وينطبق على الـ `request`.
    *   لو لقى **واحد على الأقل**، يبقى النتيجة هي **`Allow`**.

3.  **لو مفيش لا `Deny` ولا `Allow`؟**
    *   لو `IAM` دور في كل الـ `policies` المطبقة (سواء `Bucket Policy` أو `IAM Policy` أو غيرها) وملقاش أي `statement` ينطبق على الـ `request` ده أصلًا، يبقى النتيجة هي **`Deny`** بشكل افتراضي. ده اسمه **`Implicit Deny`**.

---













---

### **إيه هو الـ `Presigned URL`؟**

تخيل إنك عندك فايل مهم جدًا ومقفول عليه (`private`) في الـ `S3 bucket` بتاعك. دلوقتي، فيه `user` معين عايز يحمل الفايل ده، لكن الـ `user` ده معندوش `AWS account` ولا `access keys` ولا أي حاجة.

**الحل إيه؟**
هنا بيجي دور الـ `Presigned URL`.
الـ `Presigned URL` هو عبارة عن **"لينك مؤقت بصلاحيات مؤقتة"**.

لما بتعمل `Presigned URL`، انت في الحقيقة بتعمل الآتي:
1.  **بتاخد اللينك العادي بتاع الفايل.**
2.  بتضيف عليه **"توقيع رقمي" (`Signature`)** معمول باستخدام الـ `access keys` **بتاعتك انت**.
3.  التوقيع ده بيكون فيه معلومات مهمة:
    *   مين اللي وقّع على اللينك ده (انت).
    *   إيه هي الصلاحية اللي انت بتديها (زي `GetObject` - تحميل الفايل).
    *   **اللينك ده هينتهي امتى** (زي بعد 10 دقايق).



---

### **إزاي بيشتغل عمليًا؟ (المثال بتاعك)**

الأمر ده:
```bash
aws s3 presign s3://MyBucketName/PrivateObject --expires-in 600
```
بيعمل الآتي:
1.  **`aws s3 presign`**: بتقول للـ `CLI` أنا عايز أعمل `Presigned URL`.
2.  **`s3://MyBucketName/PrivateObject`**: بتقوله إيه هو الفايل اللي أنا عايز أعمله اللينك.
3.  **`--expires-in 600`**: بتحدد مدة صلاحية اللينك بالثواني (600 ثانية = 10 دقايق). لو محطتش دي، الافتراضي بيكون ساعة (3600 ثانية).

**الـ `output` اللي هيطلعلك هيكون لينك طويل جدًا شكله كده:**
```
https://MyBucketName.s3.us-east-1.amazonaws.com/PrivateObject?AWSAccessKeyId=AKIAIOSFODNN7EXAMPLE&Expires=1678886400&Signature=vjbyPxybd...
```
بص على الـ `parameters` اللي في الآخر (`AWSAccessKeyId`, `Expires`, `Signature`). دي هي اللي فيها كل معلومات الصلاحية المؤقتة وخلي بالك دا بيحصل من cli او sdk 

لو انت فاهم jws هتكون فاهم دي ايه بزبط وكويس قوي عملية singnature تمت ازاي

العملية كلها بتتم بشكل كامل على جهازك أو السيرفر بتاعك (client-side) من غير ما تحتاج تكلم AWS في لحظتها. اللي بيحصل هو إن الـ SDK بيجمع كل المعلومات الأساسية زي اسم الـ Bucket والـ Object Key  بالإضافة
 للـ AWSAccessKeyId بتاعك، ومدة الصلاحية (Expires) اللي بتحدد امتى اللينك هينتهي. بعد كده، كل المعلومات دي بتتحط في string واحد منظم جدًا . وبعدها  بتيجي أهم خطوة، وهي إن الـ SDK بيقوم بعملية hash للـ string ده كله، وبيستخدم **الـ Secret Access Key بتاعك** كمفتاح سري لعملية الـ hash دي. فالـ Signature اللي بتطلع في الآخر هي فعلًا ناتج تشفير كل المعلومات دي مع الـ Secret Key، وبعدين كل الأجزاء دي بتتجمع عشان تكوّن الـ URL النهائي.



---

### ** `Static Website Hosting` على `S3`**

الفكرة دي عبقرية وبتحل مشاكل كتير. ببساطة، انت بتقدر تحول `S3 bucket` عادي لـ `web server` كامل يقدر يستضيف موقعك، بس بشرط إن الموقع ده يكون `static`.

#### **1. يعني إيه `Static Website` أصلاً؟**

عشان نفهمها، لازم نعرف الفرق بينها وبين الـ `Dynamic Website`:
*   **الـ `Static Website` (الموقع الثابت):** ده موقع كل صفحاته وملفاته (HTML, CSS, JavaScript, صور) بتكون جاهزة ومتخزنة زي ما هي. لما `user` بيطلب صفحة، `S3` بياخد الفايل زي ما هو ويبعته للـ `browser`. الـ `browser` بتاع المستخدم هو اللي بيقوم بكل الشغل عشان يعرض الصفحة. فكر فيها كأنك بتسلم "كتاب مطبوع وجاهز".
*   **الـ `Dynamic Website` (الموقع الديناميكي):** ده موقع بيحتاج `server-side code` (زي PHP, Python, Node.js) عشان "يبني" الصفحة في كل مرة `user` بيطلبها. السيرفر بيجيب داتا من `database` ويحطها في `template` وبعدين يبعت الـ `HTML` النهائي للـ `browser`. فكر فيه كأنه "شيف بيطبخلك أكلة مخصوصة ليك".

`S3` يقدر يستضيف النوع الأول بس، لأنه خدمة تخزين، مش سيرفر يقدر يشغل كود `Python` أو `PHP`.





Amazon Elastic File System

![EFS icon](https://icon.icepanel.io/AWS/svg/Storage/EFS.svg)

  `Amazon Elastic File System` أو اختصارًا `EFS`. دي خدمة تخزين ملفات كده في الـ `AWS`، بس مش أي تخزين! دي بقى بتتميز إنها `automatically scalable`، يعني بتزيد السعة بتاعتها لوحدها كده أوتوماتيك مع زيادة استخدامك، مش محتاج أنت تزودها يدويًا. وكمان دي `shareable file storage`، يعني ممكن أكتر من `Linux instance` يدخلوا عليها في نفس الوقت ويشوفوا نفس الملفات ويتعاملوا معاها، كإنها `folder` مشتركة كده.

**طب وإزاي بتوصلها وبتستخدمها في إيه؟**
هي مصممة أساسًا عشان تدخل عليها من جوه الـ `Virtual Private Cloud (VPC)` بتاعك، وده بيحصل عن طريق حاجة اسمها `Network File System (NFS) mounts` بتعملها على الـ `EC2 Linux instances` بتاعتك. يعني الـ `instance` بتاعك بيشوف الـ `EFS` ده كإنها `folder` عادية عنده يقدر يحط فيها ملفات ويقرأ منها. وممكن كمان لو عندك `servers` بتاعتك في الـ `on-premises` (يعني في مكانك مش في الـ `cloud`)، توصلها بـ `EFS` عن طريق الـ `AWS Direct Connect connections` عشان تنقل الملفات بتاعتك أو تخزنها هناك. الهدف الأساسي من `EFS` هو إنها تسهل عليك خالص إنك تعمل `secure` (آمن)، و`low-latency` (يعني استجابة سريعة ومفيش تأخير)، و`durable file sharing` (يعني مشاركة ملفات موثوقة ومتقفلة ضد الأعطال) بين عدد كبير من الـ `instances` بتاعتك.

**بالنسبة لاستخداماتها الشائعة والسرعة بتاعتها:**
استخدامات `EFS` كتير أوي، تنفع لأي حاجة محتاجة `shared file storage`، زي مثلاً ملفات المواقع اللي أكتر من `web server` بيقرا منها، أو بيئة `development/test` اللي فيها `developers` كتير شغالين على نفس الـ `codebase`، أو حتى في حلول الـ `big data analytics` لما يكون عندك `datasets` كبيرة وعايز `compute instances` كتير توصلها في نفس الوقت. كمان بتنفع لـ `container storage` أو الـ `home directories` للمستخدمين في بيئات العمل.

أما بقى للـ `speed` والأداء، الـ `EFS` بتقدم أداء كويس بـ `low-latency` (زمن وصول قليل)، وده مهم عشان التطبيقات تشتغل بسرعة. وهي مش بتديلك "سرعة واحدة ثابتة" لكل حاجة، لأ ده فيها خيارات للـ `performance modes` عشان تختار اللي يناسب الـ `workload` بتاعك:

تمام، كلامك مظبوط 100%. أنا شرحت الـ `modes` بس مادتش الأرقام والمقاييس اللي على أساسها بتختار. ده جزء مهم جداً.

الـ `EFS` مش زي الـ `EBS` مثلاً اللي بتختار فيها عدد `IOPS` أو `throughput` معين مباشر، الـ `EFS` ليها طريقة مختلفة شوية في تحديد الأداء لأنها `shared file system` وبتعتمد على عوامل كتير.

لكن خلينا نوضحلك الأرقام والمقاييس الأساسية اللي بيستخدمها الـ `EFS` عشان تختار صح:

### **1. Performance Modes (دي بتحدد إزاي الـ I/O هيتعامل مع الـ file system):**

**هنا AWS مش بتديلك أرقام ثابتة لـ `read/write rates` بالـ `MB/s` أو الـ `IOPS` لكل `instance`.** ليه؟ لأن دي `shared file system`، والأداء الكلي بيتوزع على كل الـ `instances` اللي بتستخدمها. لكن بتقولك إيه اللي بتعمله كل `mode` وبتناسب أي `workload`:

*   **`General Purpose` `performance mode` (الـ default):**
    *   **ده لأيه:** لمعظم الـ `file workloads` العادية اللي مش محتاجة أداء خرافي، زي الـ `web servers`، الـ `CMS`، بيئات الـ `development`، الـ `container storage`، والـ `home directories`.
    *   **إيه ميزته:** بيوفر `latency`  قليل جداً (أقل من `millisecond`) لمعظم عمليات الـ `file operations`. يعني لما `instance` بيطلب ملف، الرد بيجيله بسرعة.
    *   **معدل الـ `read/write` (تحديداً):** AWS مبتديلكش رقم معين ثابت هنا، لكن بتقولك إنه بيعمل `scale` لآلاف عمليات الـ `I/O operations` في الثانية، وأداءه بيزيد مع زيادة عدد الـ `instances` اللي بتستخدمه.

*   **`Max I/O` `performance mode`:**
    *   **ده لأيه:** للـ `workloads` اللي محتاجة `high throughput` (معدل نقل بيانات عالي جداً) وتكون `highly parallel` (يعني عدد كبير جداً من الـ `instances` بيشتغلوا ويقرأوا ويكتبوا في نفس الوقت على الـ `file system`)، زي الـ `big data analytics`، الـ `media processing workflows`، أو أي حاجة فيها `hundreds or thousands of EC2 instances` بيعملوا `access` لـ `files` كتير جداً.
    *   **إيه ميزته:** بيوفر `higher aggregate throughput` (إجمالي معدل نقل بيانات أعلى بكتير) وعدد `IOPS` أعلى بكتير على مستوى الـ `file system` كله. لكن ممكن يكون فيه `latency` أعلى شوية لعمليات الـ `I/O` الفردية مقارنة بالـ `General Purpose` عشان الـ `overhead` بتاع التعامل مع عدد كبير من الـ `clients`.
    *   **معدل الـ `read/write` (تحديداً):** برضه مفيش رقم ثابت لكل `instance`، لكن هو مصمم عشان يستوعب `hundreds of thousands of I/O operations per second` على مستوى الـ `file system` ككل.

### **2. Throughput Modes (دي بقى اللي بتتحكم في معدل نقل البيانات بالـ MB/s):**

**هنا بقى بتلاقي أرقام فعلية تقدر تختار على أساسها:**

*   **`Bursting Throughput mode` (الـ default):**
    *   **ده لأيه:** لمعظم الـ `workloads` اللي الـ `throughput` بتاعها مش ثابت، وبيبقى فيه فترات هدوء وفترات `burst` (زيادة مفاجئة).
    *   **إزاي بيتحسب الأداء:** بيعتمد على حجم الـ `storage` اللي أنت بتستخدمه في الـ `EFS`، وبيتحسب كالتالي:
        *   **Baseline throughput:** بتديلك **50 MB/s لكل TB (تيرابايت) من البيانات المخزنة**. يعني لو عندك 1TB بيانات، هتاخد 50 MB/s. لو 2TB، هتاخد 100 MB/s.
        *   **Bursting:** الـ `EFS` بيديلك `burst credits` كده، بتخليك تقدر توصل لأداء أعلى بكتير من الـ `baseline` لفترات محدودة (بتوصل لـ 100 MB/s لكل TB، وفي `minimum burst rate` لـ 100 MB/s حتى لو حجمك صغير). لما بتستخدم الـ `burst`, الـ `credits` دي بتخلص. لما مابتستخدمش الـ `burst`، الـ `credits` بتزيد تاني.

*   **`Provisioned Throughput mode`:**
    *   **ده لأيه:** للـ `workloads` اللي محتاجة `consistent and predictable throughput` (معدل نقل بيانات ثابت ومضمون)، وعارف بالظبط هتحتاج كام `MB/s`، ومش عايز تعتمد على `burst credits` أو على حجم الـ `storage`.
    *   **إزاي بيتحسب الأداء:** أنت اللي بتحدد الرقم اللي عايزه. يعني أنت بتعمل `provision` لـ `throughput` معين بالـ `MB/s` (مثلاً 100 MB/s، 500 MB/s، 1000 MB/s، وهكذا). AWS هتضمنلك إنك هتاخد الـ `throughput` ده دايماً، بغض النظر عن حجم البيانات المخزنة عندك.
    *   **الفرق:** ده بيبقى أغلى من الـ `Bursting Throughput` لأنك بتدفع على الـ `throughput` اللي أنت اخترته حتى لو ما استخدمتوش كله.



1.  **قرر `Performance Mode`:**
    *   لو استخدامك عادي، وتطبيقك بيعتمد على `latency` قليل لعمليات فردية: اختار `General Purpose`.
    *   لو تطبيقك فيه `high concurrency` و `parallel access` من عدد كبير من الـ `instances` وعايز `throughput` كلي عالي جداً: اختار `Max I/O`.

2.  **قرر `Throughput Mode`:**
    *   لو الـ `throughput` بتاعك متغير، وفيه فترات هدوء وفترات ذروة، وحجم بياناتك هيزيد مع الوقت، وعايز توفر: اختار `Bursting Throughput`.
    *   لو عندك `workload` محتاج `throughput` ثابت ومضمون ومش عايزه يتأثر بحجم الـ `storage` أو الـ `burst credits`: اختار `Provisioned Throughput` (وهتدفع أكتر).









---

**`Amazon FSx`: **

![Amazon FSx | AWS Cheat Sheet](assets/Amazon-FSx.jpg)

، `Amazon FSx` دي مش مجرد خدمة تخزين ملفات عادية وخلاص زي `EFS`، لأ دي حاجة متخصصة أكتر بكتير. فكر فيها  بتجيبلك **File Systems قوية ومعروفة** بره الـ `AWS cloud`، وتوفرها لك كـ `fully managed service` جوه الـ `AWS`. يعني الـ `AWS` هي اللي بتدير كل حاجة من تحت لتحت وأنت بتستخدم النظام وخلاص

الـ `FSx` بتنزل في أربع أنواع مختلفة، كل نوع فيهم معمولة لـ `use case`  معين ومحتاج `features`  أو أداء خاص:

### **1. `FSx for Lustre` **

![Lustre file system architecture | Download Scientific Diagram](assets/images.png)

*   **إيه هو ده؟**
    
*   ![FSx for Lustre icon](assets/FSx-for-Lustre.svg+xml)
    
*   `Lustre` ده في الأساس `open source distributed filesystem` (نظام ملفات موزع ومفتوح المصدر) معروف جداً في عالم الـ `High-Performance Computing (HPC)`. هو معمول خصيصاً عشان يدي للـ `Linux clusters` (مجموعات كبيرة من سيرفرات الـ `Linux`) القدرة إنها توصل لأنظمة ملفات `high-performance`  بشكل غير عادي.
    
*   **لماذا تستخدمه (مميزاته الرئيسية)؟**
    *   **السرعة الخرافية:** ده يعتبر من أسرع حلول الـ `file storage` اللي ممكن تاخدها للـ `Linux` على `AWS`.
    *   **`Throughput` عالي جداً:** بيوفر معدلات نقل بيانات (قراءة وكتابة) عالية بشكل مهول، وده مهم لما يكون عندك `datasets`  ضخمة جداً وعايز توصلها وتتعامل معاها بسرعة فائقة.
    *   **مصمم للـ `parallel access`:** يعني مئات أو آلاف الـ `instances` يقدروا يوصلوا لنفس الملفات في نفس الوقت بأداء ممتاز.

*   **هينفعك في إيه؟:**
    *   **`High-Performance Computing (HPC)`:** يعني شغل المحاكاة العلمية المعقدة، التحليلات الهندسية الضخمة.
    *   **`Machine Learning`:** لما تكون بتعمل `training` لـ `models` كبيرة جداً بتحتاج تقرا وتكتب كميات ضخمة من البيانات بسرعة.
    *   **`Media Processing`:** زي شغل الـ `rendering` أو معالجة الفيديوهات الـ `4K` أو الـ `8K` اللي بتحتاج `access` سريع جداً لملفات الميديا الضخمة.

### **2. `FSx for Windows File Server`**

![FSx for WFS icon](assets/FSx-for-WFS.svg+xml)

*   **إيه هو ده؟**
    ده بيقدملك نفس فكرة مشاركة الملفات اللي بيقدمها `EFS`، بس الفرق الجوهري إنه **مصمم خصيصاً للـ `Windows servers`**. يعني لو كل السيرفرات والتطبيقات بتاعتك شغالة `Windows`، ده هيكون اختيارك الأول لتخزين الملفات المشتركة.

*   **لماذا تستخدمه (مميزاته الرئيسية)؟**
    *   **تكامل أصيل مع `Windows`:** بيشتغل بسلاسة تامة مع كل أدوات وخدمات الـ `Windows` اللي أنت متعود عليها.
    *   **دعم `SMB`:** ده الـ `protocol` الأساسي اللي الـ `Windows` بتستخدمه عشان تشارك الملفات. فبيخلي الـ `Windows servers` بتاعتك توصل للملفات دي كأنها `network drive` عادي جداً.
    *   **دعم `NTFS`:** ده نظام الملفات اللي الـ `Windows` بتستخدمه لإدارة الملفات والـ `permissions` (الصلاحيات). `FSx for Windows File Server` بيدعم الـ `NTFS permissions` بشكل كامل، وده مهم جداً للتحكم في مين يقدر يوصل لإيه.
    *   **تكامل مع `Microsoft Active Directory`:** لو عندك `Active Directory` (سواء `managed` في `AWS` أو `on-premises`)، تقدر تستخدمه عشان تدير صلاحيات الوصول للملفات دي، وده بيبسط جداً إدارة المستخدمين في بيئات الشركات.

*   **هينفعك في إيه؟:**
    *   **`Home Directories`:** لو عندك موظفين كتير بيشتغلوا على `Windows` وعايز مكان واحد يحطوا فيه ملفاتهم الشخصية المشتركة.
    *   **`Shared Storage` لتطبيقات الـ `Windows`:** أي تطبيق `Windows` بيحتاج `shared file storage` (زي `CMS` أو `CRM` أو `ERP` خاص بالـ `Windows`).
    *   **`Migration` لأحمال العمل من `on-premises`:** لو عندك `file servers` على `Windows` في الـ `data center` بتاعك وعايز تنقلهم للـ `cloud` بنفس الـ `experience`.



---

**`Amazon FSx for OpenZFS`: **

![FSx for OpenZFS icon](assets/FSx-for-OpenZFS.svg+xml)

يا صاحبي، `FSx for OpenZFS` دي بتجيبلك `capabilities` (إمكانيات) `file system` قوي ومشهور جداً اسمه `OpenZFS`، ومعروف بقوته في إدارة البيانات وكمان الـ `high performance` بتاعه، وتخليهولك
 `fully managed service` (خدمة مدارة بالكامل) جوه الـ `AWS`. هنا بقى لازم تفهم نقطة جوهرية: **ده مش `distributed file system` (نظام ملفات موزع) زي `Lustre`** اللي اتكلمنا عليه، اللي بيتوزع على كذا `server` عشان يديك `massive power`. لأ، `FSx for OpenZFS` ده بيكون `single` (واحد بس)، لكنه `highly scalable file system instance` (يعني نظام ملفات واحد لكنه قوي جداً وبيكبر معاك في الحجم والأداء لدرجة ضخمة). فكر فيه كأنه `super-powerful` و `enterprise-grade` 
و `single-node file server` (سيرفر فايلات قوي جداً ومستوى مؤسساتي وبيشتغل على node واحدة)، بس طبعاً `managed` بالكامل من `AWS`. هو في الأساس `integrated volume manager` 
وأيضاً `file system`، يعني بيدير مساحات التخزين بتاعته وبيوفر الـ `file system` فوقيها.

**ليه تستخدمه؟**
مميزاته الرئيسية بتخلي الـ `applications` بتاعتك تشتغل بكفاءة غير عادية. أولاً، بيوفر `ultra-high performance` (أداء عالي جداً) مع `low latency` (زمن وصول قليل)، وده معناه سرعة قراءة وكتابة ممتازة للفايلات. ثانياً، `data integrity`  فيه على أعلى مستوى؛ `OpenZFS` معروف جداً بمميزاته اللي بتضمن إن الـ `data` بتاعتك متتلفش، لو فيها أي `corruption` (تلف)، ولو فيه `redundancy` بيعمل `self-healing` (إصلاح ذاتي) للـ `data` التالفة لوحده. ثالثاً، الـ `snapshots` و `clones` السريعة بتاعته؛ تقدر تاخد `snapshots`  من الـ `file system` بتاعك في ثواني معدودة مهما كان حجمه، ودي مفيدة جداً للـ `backups` أو عشان ترجع لأي `point-in-time copy` سابقة بسرعة، والـ `clones` بتخليك تعمل نسخ فورية من الـ `snapshots` دي للـ `testing` أو الـ `development` وده بيوفر وقت ومساحة عشان بيستخدم `Copy-on-Write`. رابعاً، فيه `data compression`  و `deduplication` (إزالة التكرار)، ودي `features` بتساعدك توفر في مساحة التخزين بشكل كبير، ده غير `Thin Provisioning` اللي بيخلي الـ `file system` يستهلك مساحة على قد احتياجه الفعلي بس.

**هينفعك في إيه؟**
`FSx for OpenZFS` ده مثالي للـ `workloads` زي: `media processing` (شغل الـ `video editing` والـ `rendering` اللي بيحتاج `fast access` لـ `large files` مع ضمان الـ `data protection`)؛ `Electronic Design Automation (EDA)` (شغل تصميم الرقائق الإلكترونية اللي بيولد كميات ضخمة من الملفات الصغيرة والمعقدة اللي بتحتاج أداء عالي جداً)؛ `machine learning` (خاصة في مراحل الـ `data preparation` و الـ `feature engineering` و الـ `model training`، اللي بتحتاج `fast` و `reliable access` لـ `datasets` ضخمة)؛ وكمان `container image stores` (عشان تخزن الـ `container images` بتاعتك وتعملها `serve` بسرعة للـ `container orchestrators` زي `ECS` أو `EKS`)؛ وأخيراً، للـ `dev/test environments` بفضل الـ `snapshots` و الـ `clones` السريعة اللي بتخليك تعمل `provision` لبيئات كتيرة جداً في ثواني.

---









AWS Storage Gateway

![Guidance for Getting Started with AWS Storage Tape Gateway](assets/getting-started-with-aws-storage-tape-gateway.18a9eb5317697226218e91cd74d0e9441ce71721.png)

---

موضوع إنك تربط الـ `backup`  والـ `archiving` اللي عندك في شركتك (يعني الـ `local operations` بتاعتك) مع خدمات التخزين اللي في الـ `cloud`  ده ممكن يبقى `complicated` أوي ووجع دماغ. هنا بقى بيجي دور الـ `AWS Storage Gateway`، ده بيديلك `software gateway appliances`. ودي ببساطة عبارة عن أجهزة (ممكن تكون `software` بس) متخصصة بتسطبها عندك في الـ `on-premises`، وبتشتغل كـ `gateway` بين الـ `on-premises` بتاعك وبين الـ `AWS cloud`.
 الـ `appliance` ده ممكن يكون `hardware appliance` حقيقي عندك `on-premises`، أو `virtual machines` أنت بتشغلها على `VMware ESXi`، أو `Microsoft Hyper-V`، أو `Linux KVM`، أو حتى على `VMware Cloud on AWS`. وممكن كمان يكون `EC2 images` يعني `instance` عادي خالص في الـ `AWS`.

الـ `appliance` ده بقى بيبقى فيه `multiple virtual connectivity interfaces`، عشان أي `local device` عندك يقدر يوصل بيه. فالـ `local devices` اللي عندك في مكانك بتقدر توصل بالـ `appliance` ده، كأنها بتوصل بـ `physical backup device` عادي، زي الـ `hard disk` عادي مثلاً، وبتبدأ تبعتله الـ `data` بتاعتك.

الـ `data` نفسها مش بتتخزن عندك في الـ `appliance` ده بشكل دائم، لأ دي بتتخزن وتترفع مباشرة على `AWS cloud storage services` محددة زي:

*   **`S3`:** وده الأساسي للـ `file gateway` ولتخزين الـ `virtual tapes`.
*   **`S3 Glacier`:** وده للأرشفة طويلة الأمد لما تستخدم `tape gateway`.
*   **`EBS snapshots`:** لما تستخدم `volume gateway` في وضع `stored` أو `cached`، فالـ `snapshots` بتترفع للـ `S3` واللي ممكن ترجعها كـ `EBS volumes`.

يعني هو بيركز على خدمات `S3` (بما فيها `Glacier`) و الـ `EBS snapshots` عشان يوفرلك حلول `backup` و `archiving` و `on-premises file access` بطريقة `seamless`. والميزة كمان إن الـ `data` دي ممكن تفضل موجودة في `local cache` (ذاكرة تخزين مؤقتة) عندك في الـ `appliance`، عشان لو حبيت ترجع لأي `data` بسرعة تبقى `locally available` ومش محتاج تنزلها من الـ `cloud` كل مرة. فكأنها `bridge` كده بيربطلك الـ `storage` اللي عندك بـ `cloud storage` بتاع الـ `AWS` بسلاسة.







الـ `AWS Snow Family` (عائلة Snow)، دي بقى خدمة جامدة جداً معمول عشان لو عندك `large data sets` (مجموعات بيانات ضخمة) وعايز تنقلها للـ `cloud`. لأن ساعات إنك تنقل `terabyte-scaled` أو حتى `petabyte-scaled data` على `Internet connection` عادي، بياخد `far too much time` و `bandwidth` لدرجة إنه ميبقاش `practical` خالص. فلو أنت بتفكر تنقل كميات البيانات دي عشان تعملها `backup` أو تستخدمها `actively` جوه الـ `AWS`، يبقى إنك تطلب `Snow device` ده ممكن يكون أحسن `option` ليك.

**إزاي بتشتغل الـ Snow Family؟**
لما بتطلب `device` من الـ `AWS`، هما بيشحنولك `physical device` (جهاز ملموس) بالحجم المناسب للـ `data` بتاعتك. أنت بقى بتنقل الـ `data` بتاعتك على الـ `device` ده. ولما بتخلص وتبقى جاهز، بتقدر تبعت الـ `device` ده تاني لـ `Amazon`. وبعد كده، `Amazon` هي اللي بتنقل الـ `data` بتاعتك على الـ `buckets` بتاعتك في الـ `S3`.

**استخدامات تانية (Edge Locations):**
ده غير إن الـ `Snow devices` دي مش بس بتنقل الـ `data` فيزيائياً، بعضها كمان ممكن يستخدم في `connected edge locations`. يعني ممكن تشغل عليها `workloads` في بيئات standerd أو مش متوفر فيها `enterprise-ready hardware` بشكل عادي، عشان الـ `design` بتاعها `rugged and secure`.

**أنواع الـ Snow Devices:**

1.  **`Snowcone`:**
    *   ده أصغر `device` في الـ `Snow Family`. حجمه كده حوالي `9×6×3-inch` (بالتقريب حجم علبة مناديل كبيرة).
    *   بيجي بـ `22 TB` من الـ `usable storage` (مساحة تخزين ممكن تستخدمها).
    *   فيه `server` بـ `four vCPUs` و `4 GB of usable memory`.
    *   وبيدعم `RJ45` (كابل شبكة) و `Wi-Fi connectivity`.

2.  **`Snowball Edge`:**
    *   ده أكبر وأتقل شوية من الـ `Snowcone`، وبيجي بـ `two profiles` (نوعين):
        *   **`Snowball Edge Storage Optimized`:** ده بتاع التخزين بالأساس. فيه `80 TB` من الـ `usable storage`، و `40 vCPUs`، و `80 GB of memory`.
        *   **`Snowball Edge Compute Optimized`:** ده بتاع الـ `compute` (المعالجة) بالأساس. مساحة التخزين فيه أقل شوية `42 TB`، لكن فيه `52 vCPUs` و `208 GB of memory`. الـ `Compute version` ده طبعاً معمول أساساً لـ `edge computing workloads` (أحمال العمل اللي بتشتغل على حافة الشبكة).

3.  **`AWS Snowmobile`:**
    *   ده بقى مش `device`، ده `shipping container` بطول `45-foot`، يعني  تريلا!
        ![AWS retires Snowmobile truck-based data transfer service - DCD](assets/Snowmobile-AWS-truck-data-delivery.original.jpg)
    *   هو `waterproof`  و `tamper-resistant` 
    *   معمول أساساً للـ `petabyte` أو `exabyte data migrations`، يعني لنقل كميات بيانات `massive` (ضخمة جداً) اللي بتتقاس بالـ `petabyte` و الـ `exabyte`.
    *   الفكرة إنك بتوصل الـ `container` ده بالـ `local network` بتاعك عشان تنقل الـ `data` بتاعتك عليه. ولما بتخلص، الـ `AWS` هي اللي بتسوق الـ `container` ده لـ `AWS region` وهتعمل `upload` للـ `data` لـ `account resources` بتاعتك.

**الأمان والـ Compliance:**
كل الـ `Snow devices` دي بتكون `256-bit encrypted`، وكمان `HIPAA-compliant`، يعني آمنة جداً ومطابقة للمواصفات اللي ليها علاقة بالبيانات الحساسة.

**إمتى تختار الـ Snow Family؟ (حسابات بسيطة):**
إنك تختار أحسن `method` عشان تنقل الـ `data` لـ `AWS account` بتاعك، محتاج شوية `arithmetic` (حسابات بسيطة). لازم تعرف سرعة الـ `upload speeds` الحقيقية اللي بتاخدها من الـ `Internet connection` بتاعك، وكمان قد إيه من الـ `bandwidth` ده مبيبقاش مستخدم في عمليات تانية.

في الـ `chapter` اللي فات، ممكن تكون عرفت عن `Multipart Upload` و `Transfer Acceleration` عشان تنقل `larger objects` لـ `S3 bucket`. بس فيه `objects` بتكون `so large` لدرجة إن إنك تعملها `upload` على الـ `Internet connection` اللي عندك مبيبقاش `practical`. فكر كده: لو الـ `Internet service provider (ISP)` بتاعك بيديلك `10 MB/second`، وعلى افتراض إن مفيش حد تاني بيستخدم الـ `connection` ده، عشان تعمل `upload` لـ `one-terabyte archive` هتاخدلك حوالي **`10 days`**!

فلو أنت بجد محتاج تنقل الـ `data` دي لـ `cloud`، يا إما هتستثمر في
 `expensive AWS Direct Connect connection` (وده حل مكلف)، أو هتبدأ تتعرف على `AWS Snowball device` (أو، لو كميات الـ `data` `really massive`، يبقى الـ `AWS Snowmobile`).







 بص بقى على `AWS DataSync` ده. ده بقى متخصص أوي في إنه ينقل الـ `on-premises data stores` (يعني الداتا اللي عندك في مكانك) للـ `AWS account` بتاعك بأقل `fuss` ممكن. هو بيشتغل عادي خالص على الـ `regular Internet connection` بتاعك، عشان كده هو مش مفيد أوي زي الـ `Snowball` للـ `really large data sets` (لأن الـ `Snowball` ده معمول للكميات اللي أكبر بكتير ومش هتنفع مع الإنترنت العادي). بس بقى الـ `DataSync` ده `much more flexible`، لأنه مش بيخليك مقيد بإنك تبعت الـ `data` بتاعتك للـ `S3` بس (زي ما كنت مقيد مع الـ `Snowball`)، أو مقيد بـ `RDS` لو كنت بتستخدم 
`AWS Database Migration Service`. الـ `DataSync` بيديلك حرية إنك ترمي الـ `data` بتاعتك في **أي `service`** جوه الـ `AWS account` بتاعك.

يعني ده معناه إنك ممكن تعمل بيه حاجات زي:
*   تنقل الـ `old data` بتاعتك بسرعة و`securely` من الـ `datacenter` بتاعك اللي بيكلفك كتير، وتحطها في `S3` أو `Glacier` الأرخص بكتير.
*   تنقل الـ `data sets` دي مباشرة للـ `S3`، أو الـ `EFS`، أو الـ `FSx`، وهناك بقى تقدر الـ `EC2 instances` بتاعتك تعملها `process` و `analyze` عادي.
*   تقدر تستخدم `power` (قوة) أي `AWS service` على أي نوع من الـ `data` بتاعتك، وده كله بيبقى جزء من `automated system` سهل إنك تعمل له `configure`.

وأداءه مش قليل على فكرة! الـ `single DataSync task` ممكن يتعامل مع `external transfer rates` بتوصل لغاية `10 Gbps` (طبعاً لو الـ `connection` بتاعك يستحمل السرعة دي)، وده غير إنه بيديلك `encryption` عشان الداتا تبقى `secure`، وكمان بيعمل `data validation` عشان تتأكد إن الداتا وصلت صح ومفيهاش غلطات.
