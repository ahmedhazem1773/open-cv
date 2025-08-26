1. هي عبارة عن زي ان هي بتعمل تحديد للحدود الخارجية و الداخلية عن طريق الفرق ما بين الخلفية و الشكل الي في المقدمة
2. هي بتشتغل مع الصورة الي بتبقي عبارة عن لbinary افضل عشان كده بتلاقي البعض يعمل ايه يحول الصورة لgray scale و يعمل threshold  عشان تبقي عبارة عن ابيض و اسود 
3. هي بتاخد الاتي :
	1. الصورة
	2. انت عايز تحدد externl & internal  ولا exrernal  بس و هكذا
	3. طريقة الapproximation 
4. هي بترجع contours فس شكل list  وحاجة اسمها hierarchy دي array فيها و يتميز للcontours بحيث تعرف مثلا دول خارجي مش داخلي دول تبع obj a  و دول obj b بتبقي عموما 4 elements 
5. طبعا عشان كده هنستخدم لوب عشان نرسم الcontours و كده 
6. بعد كده جوا اللوب في func هنرسم بيها  علي مثلا خلفية سودة بتاخد الاتي 
	1. الصورة الي عايز ترسم عليها 
	2. الcontours بتاعتك 
	3.  هتباصي index  بتاع contour  الي عايز تلرسمه بعد كده اللون و بعد كده السمك 
7. خد بالك في حزار الرسم ان الخلفية المثلا الي حدد ترسم عليها خلتها تاخد shape بتاع one channel  ولا 3 channel  عشان ل انت عايز تحدد بالالوان و كده 
```python
import cv2

import numpy as np

import matplotlib.pyplot as plt

img = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\internal_external.png")

img2 = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

plt.imshow(img2 , cmap="gray")

plt.show()

contours, hierarchy = cv2.findContours(img2, cv2.RETR_CCOMP, cv2.CHAIN_APPROX_SIMPLE)

print(hierarchy.shape)

print(hierarchy)

# Draw External Contours

  

# Set up empty array

external_contours = np.zeros(img.shape )

# Create empty array to hold internal contours

image_internal = np.zeros(img2.shape )

  

# For every entry in contours

for i in range(len(contours)):

    # last column in the array is -1 if an external contour (no contours inside of it)

    if hierarchy[0][i][3] == -1:

        # We can now draw the external contours from the list of contours

        cv2.drawContours(external_contours, contours, i, (255,0,0), -1)

    # If third column value is NOT equal to -1 than its internal

    if hierarchy[0][i][3] != -1:

        # Draw the Contour

        cv2.drawContours(image_internal, contours, i, 255, -1)

print(external_contours.shape)

plt.imshow(external_contours )

plt.show()

print(image_internal.shape)

plt.imshow(image_internal , cmap="gray" )

plt.show()
```
![internal_external](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/internal_external.png)
![Figure_78](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Figure_78.png)
![Figure_79](open%20cv/5-Object%20Detection%20with%20OpenCV%20and%20Python/images%20&%20videos/Figure_79.png)
