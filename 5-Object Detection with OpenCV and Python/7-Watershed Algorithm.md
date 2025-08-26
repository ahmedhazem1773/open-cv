1.  هي خوارزمية بتحول الصورة لtopographical maps حيث الحاجات الي فيها مثلا المرتفعات بتبقي الحاجات الشدة بتاعتها في اللون عالي و الحاجات الي بتبقي اثل بتبقي مثلا وديان
2. فا احنا هنستخدمها عشان segmention  و كده 
3. ممكن يحصل زي merging  لحاجاتين مع بعض مع ان هم مش نفس  الشئ
4. فا هنا يجي دور edge  يفصل الحاجات عن بعض 
5. و بتستخد فصل الخلفية عن المقدمة في بعض الحالات الي بتبقي صعبة لبعض الخوارزميات زي مثلا coins كتير جنب بعض مثلا  لان مثلا ممكن خوارزيمة تشوفهم كلهم جسم واحد عشان الخلفية لونها مختلفة عنهم 
![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Pasted image 20250822161831.png]]
6. ممكن نستخدم seeds  خاصة بينا بحيث نوضح ان ده جسم لوحده و هكذا
## 1. Without watershed 
 1. اول حاجة نعمل بلور عشان نشيل الnoise و كمان نشيل تفاصيل مش مهمة لينا زي مثلا الصورة او الكتابة  الي علي العملة  هتلاحظ ان احنا استخدمنا كيرنيل مقاسها كبير عشان الصورة مقاسها كبير 
 2. بعد كده نخليها gray scale 
 3. بعد كده نطبق binary threshold و هنستخدم inv  بحيث نخلي foreground  يبقي لونه ابيض 
 4. بعد كده نطلع الcontours 
 ```python
 import cv2

import numpy as np

import matplotlib.pyplot as plt

def display (img , cmap = None) :

    plt.imshow(img , cmap= cmap)

    plt.show()

sep_coins = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\pennies.jpg")

sep_coins_rgb= cv2.cvtColor(sep_coins, cv2.COLOR_BGR2RGB)

display(sep_coins_rgb)

sep_blur = cv2.medianBlur(sep_coins_rgb,25)

display(sep_blur)

gray_sep_coins = cv2.cvtColor(sep_blur,cv2.COLOR_BGR2GRAY)

display(gray_sep_coins,cmap='gray')

ret, sep_thresh = cv2.threshold(gray_sep_coins,160,255,cv2.THRESH_BINARY_INV)

display(sep_thresh,cmap='gray')

contours, hierarchy = cv2.findContours(sep_thresh.copy(), cv2.RETR_CCOMP, cv2.CHAIN_APPROX_SIMPLE)

# For every entry in contours

for i in range(len(contours)):

    # last column in the array is -1 if an external contour (no contours inside of it)

    if hierarchy[0][i][3] == -1:

        # We can now draw the external contours from the list of contours

        cv2.drawContours(sep_coins_rgb, contours, i, (0, 0, 255), 15)
	if hierarchy[0][i][3] != -1: # i show 2 images if i draw internal or not

	    # Draw the Contour

        cv2.drawContours(sep_coins_rgb, contours, i, (0, 0, 255), 15)

display(sep_coins_rgb)
 ```
 ![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_85.png]]
 ![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_86.png]]
 ![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_87.png]]
 ![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_88.png]]
 ![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_89.png]]
 ![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_90.png]]
 5. لو خد بالك ان هنا قاريهم جسم واحد دي حاجة و الحاجة التانية ممكن تقولي استخدم erosion  عشان افصلهم بس المشكلة هيلعب عند ك في مساحة هتقل و التحديد هيبقي اصغر من المساحة
## 2. With Watershed
1. هنعمل بردوا بلور 
2. هنعملها gray scale 
3. بعد كده threshold  و بردوا binary_inv بس هتلاقي ممكن النتيجة مش كويسة فا هسنتخدم ميثود تانية اسمها ==Otsu==  هتلاقيها هي هي نفس الكود بس الفرق ان احنا هنخلي الthreshold  بيساوي صفر و هنيجي عندbinary_inv  نجمع عليها الكود ده `cv2.THRESHOLD_OTSU` 
4. هنعمل خطوة اختيارية بس هي بتبقي مهمة في الصور الاكثر تعقيدا ان احنا نشيل الnoise عن طريق الmorphology  هنستخدم الopen هتحس ان مفيش فرق 
5. هتلاقي المشكلة بتاعتنا ان هو شايف ان هم جزء واحد مع بعض فا  احنا هسنتخدم حاجة اسمها distance transform  هي بتعمل ايه لما تلاقي صورة الخلفية اسود و هي ابيض فا بتعمل في اللون الابيض تدرج فيه بحيث يبقي القلب هو الابيض و كل ما نبعد بنروح لللون الاسود هتلاقيها بتاخد  الصورة و طريقة الcalc  و رقم ليه علاقة بالماسك هتحط الdefault  الي هي يا 3 يا 5 
![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Pasted image 20250822192504.png]]
6. بعد كده هتستفاد من ده ان انا اعمل threshold  تاني عشان افصل الcoins  عن بعضها و  بس هتلاقيها تدي نقط صغيرة كده فا كده معايا الخلفية الي متأكد منها و الforeground  بس هستخدمهم عشان اجيب المساحات الغير معروفة عشان بعد كده اوقع markers  بحيث يعرف ان دي segments  مختلفة  و بعد كده اجمع الmarkers  دي مع الunknown area عشان الكمبيوتر مش عارف هي جسم واحد ولا لا  بحيث يبقي عندي الshape  لونه مختلف شوية عن الخلفية عشان كده ضيفت 1 للخلفية بتاعت markers  و الmarkers  لونها مختلف بردوا 
7. بعد كده هستخدم watershed  ابصي فيها الصورة الاصلية بتاعتنا و  الصورة الي فيها بتاعت الmarkers  دي مع الunknown  ا  فايدة بان عملت markers  ان watershed  هتشتغل عشان تعرف ان دول 6 segments   مش واحد  ان هي تربطها بالصورة الاصلية زي ما اكانلي قولتلها خلي بالك 6 اجسام وانتي اتصرفي و خلي بالك انا لما بصيت الصورة بصيت الاصلية فا طلعت نتيجة كويسة علي عكس خوسيه كان باصي الصورة الي فيها بلور عالي فا الشكل طلع شوية بعافية
8. بعد كده نستخدم الcontours و هي خلاص كده جابت ان هم 6 من غير ما نستخدم حتي نجيب الحدود الداخلية عشان مفيش 
```python
def display (img , cmap = None) :

    plt.imshow(img , cmap= cmap)

    plt.show()

sep_coins = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\pennies.jpg")

sep_coins_rgb= cv2.cvtColor(sep_coins, cv2.COLOR_BGR2RGB)
blured= cv2.medianBlur(sep_coins_rgb,35)

gray = cv2.cvtColor(blured,cv2.COLOR_BGR2GRAY)

ret, thresh = cv2.threshold(gray,0,255,cv2.THRESH_BINARY_INV+cv2.THRESH_OTSU)

display(thresh,cmap='gray')

# noise removal

kernel = np.ones((3,3),np.uint8)

opening = cv2.morphologyEx(thresh,cv2.MORPH_OPEN,kernel, iterations = 2)

display(opening,cmap='gray')

# sure background area

sure_bg = cv2.dilate(opening,kernel,iterations=3)

display(sure_bg,cmap='gray')

# Finding sure foreground area

dist_transform = cv2.distanceTransform(opening,cv2.DIST_L2,5)

ret, sure_fg = cv2.threshold(dist_transform,0.7*dist_transform.max(),255,0)

display(dist_transform,cmap='gray')

display(sure_fg,cmap='gray')

# Finding unknown region

sure_fg = np.uint8(sure_fg)

unknown = cv2.subtract(sure_bg,sure_fg)

display(unknown,cmap='gray')

# Marker labelling

ret, markers = cv2.connectedComponents(sure_fg)

# Add one to all labels so that sure background is not 0, but 1

markers = markers+1

# Now, mark the region of unknown with zero

markers[unknown==255] = 0

display(markers,cmap='gray') #before watershed

markers = cv2.watershed(sep_coins_rgb,markers)

display(markers)

contours, hierarchy = cv2.findContours(markers.copy(), cv2.RETR_CCOMP, cv2.CHAIN_APPROX_SIMPLE)

  

# For every entry in contours

for i in range(len(contours)):

    # last column in the array is -1 if an external contour (no contours inside of it)

    if hierarchy[0][i][3] == -1:

        # We can now draw the external contours from the list of contours

        cv2.drawContours(sep_coins_rgb, contours, i, (255, 0, 0), 10)

display(sep_coins_rgb)
```
![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_91.png]]
![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_92.png]]
![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_93.png]]
![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_94.png]]
![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_95.png]]
![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_96.png]]
![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_97.png]]
![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_98.png]]
![[open cv/5-Object Detection with OpenCV and Python/images_and_videos/Figure_99.png]]

 
 
 
 
 