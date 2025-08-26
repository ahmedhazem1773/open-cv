1. هو بيتبع الحركة الظاهرية للobj بين الفريمات الناتجة يا اما من حركة الobj او الكاميرا 
2. عنده كام فرضية ان شدة بتاعت البيكسلات كلون متتغيرش يعني مثلا مش بيتبع لمبة بتتحرك وهي بتطفي و تولع مثلا  و ان البيكسلات الماجورة ليها نوعا ما نفس الحركة 
3. هي بتعمل ايه الاول بتاخد نقط معطاة ليها و الفريم و بعد كده هتحاول تدور علي نفس النقط في الفريم الي بعده و بالنسبة للنقط الموضوع راجع لليوزر لو عايز هو يغذيها يعني ممكن نعمل detect face  و بعد كده نديها للمسثود و هس هتعامل تراك للوش بناء علي النقط الي انا اديتهالها بس هو مش هيعرف يفرق الحركة هل الكاميرا هي بتتحرك عشان كده الجسم يبان يتحرك ولا العكس ا ن الجسم هو الي بيتحرك الfunc الي builtin في opencv2 الي هي lucas_kanade هي بس هتابع النقط الي حدتتها في الفيديو مش كله ان هي تتابعها مش كل النقط الي في الفيديو  و ده اسمه sparse feature set 
4. فا في builtin algorithm اسمها  gunner farneback عشان بتحسب حاجة اسمها dense opyical flow هي بتشوف flow بتاع كل النقاط و هتلون الحاجة الي  مش بتتحرك باللون الاسود 
## 1. lucas 
1. الاول كده هتلاقينا ان احنا عملنا dic  بالparamters بتاعت الكورنر و لوكاس 
2. عملنا ان احنا خدنا اول فريم قبل ما نعرض البث وده بسبب مفيش while loop  و حولنها لgray scale و جبنا منها الكورنرز الي هي النقط الي عايزه يتابعها وانا هنا محدد عددهم 
3. بعد كده عملنا ماسك لونه اسود نفس شكل بتاع الفريم الي خدناه 
4. بعد كده هنعمل while loop  بتاعتنا الي هنعرض بيها البث بتاعنا و عملنا بردوا من الفريمات دي منها gray scale 
5. بعد كده هنستخدم pylk  الي هي func  بس معاها حاجة اسمها pyramed level  ده انت بتحدد عايز تتعامل مع الصورة جودة كاملة ولا نصها و هكذا وكل ما نعلي في الليفيل  الكسر بيزداد ب2 اس الليفل يعني ليفيل 0  يبقي الصورة كامة ليفيل 2 يبقي ربع حودة الصورة و  هكذا و كده كده بتعمل بلور بنسبة 
6. هي بتاخد الاتي :
	1. الفريم الي خدناه قبل البث  الgray scale
	2.  الفريم الحالي الg
	3. الكورنرز الاولوية الي طلعناها 
	4. بعد الكورنرز القادمة او نقطة النهاية بس معندناش فا نحطها none 
	5. بعد كده حاجة اسمها winsize بس خلي بالك المقاسات الصغيرة بتخليك حساس مع الnoise  و بس ممكن متعرفش تحس بالحركات القوية حركة سريعة جدا او راحت بعيد و الكبيرة ممكن تعرفش تحس الحركات الصغيرة و تفتكر الobj  ثابت 
	6. بعد كده maxlevel  بتاع الpyramed  الي قولنا عليها 
	7. حاجة اسمها كرايتيريا هنباصي واحدة تخص الايبسلون  بتاعت التقريب الي مناسبة معايا و العد و نعمل ما بينهم or  و بعد كده العدد الي انا عايزه من iteration  و بعد كده قيمة epslon و هي هتوقف لما توصل لحاجة من الاتنين  عموما كل ده عشان تدور علي نفس النقاط في الفريم التالي 
7. هي بعد كده بترجع مكان النقط الجديد في الفريم الجديد و الstatus دي Array و  بتاع الايرور لو حصل ايرور 
8. الstatus دي عبارة عن array  من الvectors و كل عنصر من الvector  بيبقي يساوي واحد لو وجد في flow غير كده بتبقي 0
9. فا احنا هنعمل ايه ناخد من النقط الجديدة  النقط الي ليه flow  بمعني كده vector  فا هنستخدم status جوا الindexing  لو النقط الي ليها vector  هنحطها في array  نسميها good_news و نعمل نفس النظام للنقط القديمة كده نسميها good_prev 
10. بعد كده هنعمل زي دمج باستخدام zip  بحيث يحط اي 2 element  ليهم نفس الindex  في tuple  واحدة و كده بقوا element  واحد في مكان بتاعهم لكن مع بعض في tuple واحدة 
11. استخدمهم بعد كده في for loop   استخدم enumerate  عشان ارجع الindex  كمان عشان استخدمه 
12. بعد كده هنستخدم نقط القديمة و الجديدة عشان نرسم خطوط علي الماسك ابو خلفية سودة الي عملناها تمثل ان اتجاه الحركة و كده و الازاحة قد ايه بس خلي بالك لو النقط دي float مش هتعرف ترسم بيها فا  محتاج تحولها لint
13. و نرسم دايرةصغيررة زي نقطة علي الفريم بحيث باستخدام النقط الحديثة بحيث نوري ايه الobj  الي بيتتبعه 
14. بعد كده نعمل زي دمج ما بين الماسك و الفريم الي مرسوم عليه النقط و نعرض الصورة دي بقي للسمتخدم او الفريم ده الي متعدل عليه ده البث بتاعك يعني الفرق ان انت كنت مشغل البث لكن مش معروض للمستخدم  و نعمل بقي الكونديشن عشان نعرف نطلع من while loop  و كده عادي كأنك بتقفل الcapture  زي ما اتعلمنا 
15. بس مننساش نعمل update للفريم السابق عشان  الفريم الحالي هو الي هيبقي في الiteration  الجاية الفريم القديم  و النقط بتاعته هي هتبقي القديمة 
```python
import numpy as np

import cv2

# Parameters for ShiTomasi corner detection (good features to track paper)

corner_track_params = dict(maxCorners = 20,

                       qualityLevel = 0.3,

                       minDistance = 7,

                       blockSize = 7 )

# Parameters for lucas kanade optical flow

lk_params = dict( winSize  = (200,200),

                  maxLevel = 2,

                  criteria = (cv2.TERM_CRITERIA_EPS | cv2.TERM_CRITERIA_COUNT, 10,0.03))

# Capture the video

cap = cv2.VideoCapture(0)

  

# Grab the very first frame of the stream

ret, prev_frame = cap.read()

  

# Grab a grayscale image (We will refer to this as the previous frame)

prev_gray = cv2.cvtColor(prev_frame, cv2.COLOR_BGR2GRAY)

  

# Grabbing the corners

prevPts = cv2.goodFeaturesToTrack(prev_gray, mask = None, **corner_track_params)

  

# Create a matching mask of the previous frame for drawing on later

mask = np.zeros_like(prev_frame)

  
  

while True:

    # Grab current frame

    ret,frame = cap.read()

    # Grab gray scale

    frame_gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # Calculate the Optical Flow on the Gray Scale Frame

    nextPts, status, err = cv2.calcOpticalFlowPyrLK(prev_gray, frame_gray, prevPts, None, **lk_params)

    # Using the returned status array (the status output)

    # status output status vector (of unsigned chars); each element of the vector is set to 1 if

    # the flow for the corresponding features has been found, otherwise, it is set to 0.

    good_new = nextPts[status==1]

    good_prev = prevPts[status==1]

    # Use ravel to get points to draw lines and circles

    for i,(new,prev) in enumerate(zip(good_new,good_prev)):

        # x_new,y_new = new.ravel()

        x_new,y_new= [round(x)for x in new.ravel()]

        # x_prev,y_prev = prev.ravel()

        x_prev,y_prev= [round(x) for x in prev.ravel()]

        # Lines will be drawn using the mask created from the first frame

        mask = cv2.line(mask, (x_new,y_new),(x_prev,y_prev), (0,255,0), 3)

        # Draw red circles at corner points

        frame = cv2.circle(frame,(x_new,y_new),8,(0,0,255),-1)

    # Display the image along with the mask we drew the line on.

    img = cv2.add(frame,mask)

    cv2.imshow('frame',img)

    k = cv2.waitKey(30) & 0xff

    if k == 27:

        break

    # Now update the previous frame and previous points

    prev_gray = frame_gray.copy()

    prevPts = good_new.reshape(-1,1,2)

cv2.destroyAllWindows()

cap.release()
```
## 2. Dense Optical Flow in OpenCV
1. هي تقريبا نفس الخطوات 
2. اول حاجة ناخد اول فريم و نحوله لg
3. بعد كده هنعمل ماسك بنفس مقاس الفريم  بس هنسميهhsv  عشان يبقي عبارة عن القيم دي و هنحط في saturation  نخليه 255 
4. بعد كده نعمل while loop  عشان البث  ونتحرك من فريم للتاني و ناخد الفريم ونخليه بردوا g
5. بعد كده هنستخدم الfunc  بتاعت dense  ويتاخد الاتي 
	1. الفريم القديم الg
	2. الفريم الجديد الg
	3. الflow  بس معندناش واحد جاهز فا هنحطها none 
	4. دي بتحدد بيها pyramed scale   مش الليفل هنا لما يطلع من ليفل للتاني يقل قد ايه و القيمة default  هي 0.5 يعني كل scale  هو نص الي قبله 
	5. عدد الليفيل الي عايزها انا كام و الdefaut 3 
	6. ده win size و بنحدد بيه القماس بس بعد واحد و الdefault 15 
	7. عدد الiteration  الي هيتعمل في كل level و الdefault 3
	8. عايز معادلة كثيرة الحدود من الدرجة الكام بتبقي الdefault 5 or 7
	9. سيجما بتاعت standard deviation بتبقي 1.2 لل5 و 1.5 لل7 
	10. دي للفلاجز و انا هحطها صفر 
6. بترجع flow   ده عبارة عن array ابعاده الطول و العرض بتوع الصورة و 2 elements  في البعد التالت مش 3 زي الالوان و ال2 ele  الاول فيهم ده بيمثل الازاحة الي حصلت في بعد x  و التاني الازاحة في بعد y 
7. بعد كده هستخدم ازاحة الx , y  دول احولهم للصورة القطبية  بfunc builtin في opencv بتباصي فيها الx بعد كده الy و بعد كده احدد نوع الدرجات radian ولا degree و اخليها درجات و هي هترجع القيمة و الزاوية 
8. هستخدم الحوار ده في الماسك  ان اخلي الزاوية تتحكم في اللون الي هو hue و القيمة يتحكم في value
9. هحط نص قيم الزاوية بحيث مش عايز الوان كتير و كمان متلفش و ترجع نفس اللون لا انا عايز مثلا position  بتاع الobj في اقصي الشمال لون و اقصي اليمين لون تاني مش نفس اللون 
10. هتلاقي قيم بتاعت الوصةر القطبية هنعملها normlize بحيث تبقي من صفر ل255 
11. بعد كده يا باشا حول الصورة دي bgr  عشان بعد كده نعرضها وبس كده يعمنا 
12. و نعمل update  للفريم القديم 
```python 
# Capture the frame

cap = cv2.VideoCapture(0)

ret, frame1 = cap.read()

  

# Get gray scale image of first frame and make a mask in HSV color

prvsImg = cv2.cvtColor(frame1,cv2.COLOR_BGR2GRAY)

  

hsv_mask = np.zeros_like(frame1)

hsv_mask[:,:,1] = 255

  

while True:

    ret, frame2 = cap.read()

    nextImg = cv2.cvtColor(frame2,cv2.COLOR_BGR2GRAY)

    # Check out the markdown text above for a break down of these paramters, most of these are just suggested defaults

    flow = cv2.calcOpticalFlowFarneback(prvsImg,nextImg, None, 0.5, 3, 15, 3, 5, 1.2, 0)

    # Color the channels based on the angle of travel

    # Pay close attention to your video, the path of the direction of flow will determine color!

    mag, ang = cv2.cartToPolar(flow[:,:,0], flow[:,:,1],angleInDegrees=True)

    hsv_mask[:,:,0] = ang/2

    hsv_mask[:,:,2] = cv2.normalize(mag,None,0,255,cv2.NORM_MINMAX)

    # Convert back to BGR to show with imshow from cv

    bgr = cv2.cvtColor(hsv_mask,cv2.COLOR_HSV2BGR)

    cv2.imshow('frame2',bgr)

    k = cv2.waitKey(30) & 0xff

    if k == 27:

        break

    # Set the Previous image as the next iamge for the loop

    prvsImg = nextImg

  

cap.release()

cv2.destroyAllWindows()
```
![[Recording 2025-08-23 175357.mp4]]