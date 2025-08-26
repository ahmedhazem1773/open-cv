1. هي ابسط  فورم مستخدمة في obj detection  وهي ببساطة بتعمل ايه معاها صورة صغيرة (template) بتمر بيها علي صورة كبيرة و تعمل بيه matching  وهي جواها بتفرق في الmethodes  الي مستخدمة في الmatching 
2. خلي بالك ان هي بتmatch  الحاجة الي في نفس مقاسها بالظبط مش اي حاجة شبها لكن بscale مختلف 
3. قبل ما نبدأ في func اسمها `eval` هي بتعمل ايه بتباصي جوها string  هي بتعمل ماتشينج لو في func بنفس الاسم و لو استخدم الاسم ده هيتشغل عادي الاهم بس يكون الاسم مكتوب صح عشان هيعمل ايرور
```python
myfunc1=eval("sum")
print(myfunc1)
print(myfunc1([1,2,3]))
myfunc2=eval("enumerate")
print(myfunc2)
myfunc3=eval("np.power")
print(myfunc3)
print(myfunc3(2,3))
```
	Output:
	<built-in function sum>
	6
	<class 'enumerate'>
	<ufunc 'power'>
	8
4. خلي بالك الmehtodes  الي هنستخدمها هي النتيجة هتطلع في one channel  الي هي الحراري 4 طرق بتظهر ده ان الحتة الي فيها ماتشنج بتبقي  maximum value و طريقتين بتبقي minimum value و النقطة الي بتظهرها بتبقي top_left  بتاعت المستطيل 
5. احنا هنستفاد من النقطة دي ان احنا نعمل مستطيل بيوري فين الماتشنج ده 
6. هتلاقي في func بتاعت الtemp  الي هي `cv2.matchTemplate` احنا بنباصي الصورة و بعد الtemp  و بعد كده اسم ال methode
7. ممكن تلاقي بعض الmdthodes  تخرف منك علي الرغم من ان هي دي نسخة من دي
```python
import cv2

import numpy as np

import matplotlib.pyplot as plt

img= cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")

img_nose=img[550:800 ,610:900]

img_rgb=cv2.cvtColor(img , cv2.COLOR_BGR2RGB)

img_nose_rgb=cv2.cvtColor(img_nose , cv2.COLOR_BGR2RGB)
plt.imshow(img_rgb)

plt.show()
plt.imshow(img_nose_rgb)

plt.show()

y, x,channels = img_nose_rgb.shape

# All the 6 methods for comparison in a list

# Note how we are using strings, later on we'll use the eval() function to convert to function

methods = ['cv2.TM_CCOEFF', 'cv2.TM_CCOEFF_NORMED', 'cv2.TM_CCORR','cv2.TM_CCORR_NORMED', 'cv2.TM_SQDIFF', 'cv2.TM_SQDIFF_NORMED']

for m in methods:

    # Create a copy of the image

    full_copy = img_rgb.copy()
    # Get the actual function instead of the string
    method = eval(m)
    # Apply template Matching with the method

    res = cv2.matchTemplate(full_copy,img_nose_rgb,method)

    # Grab the Max and Min values, plus their locations

    min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(res)

    # Set up drawing of Rectangle

    # If the method is TM_SQDIFF or TM_SQDIFF_NORMED, take minimum

    # Notice the coloring on the last 2 left hand side images.
    if method in [cv2.TM_SQDIFF, cv2.TM_SQDIFF_NORMED]:
        top_left = min_loc   #x , y
    else:
        top_left = max_loc
    # Assign the Bottom Right of the rectangle
    bottom_right = (top_left[0] + x, top_left[1] + y)
    # Draw the Red Rectangle
    cv2.rectangle(full_copy,top_left, bottom_right, 255, 10)
    # Plot the Images
    plt.subplot(121) #this means one row 2 colomns fst plot
    plt.imshow(res)
    plt.title('Result of Template Matching')
    plt.subplot(122) #this means one row 2 colomns sec plot
    plt.imshow(full_copy)
    plt.title('Detected Point')
    plt.suptitle(m) #title for hole plot
    plt.show()
    print('\n')
    print('\n')
```
![Figure_59](Figure_59.png)
![Figure_60](Figure_60.png)
![Figure_61](Figure_61.png)
![Figure_62](Figure_62.png)
![Figure_63](Figure_63.png)
![Figure_64](Figure_64.png)
![Figure_65](Figure_65.png)
![Figure_66](Figure_66.png)
8.  دلوقتي شكل عندنا لو فيه كذا obj  نفس الشكل اكيد مش هينفع ندور علي اعلي قيمة عشان هتجيب صورة واحدة بس كده