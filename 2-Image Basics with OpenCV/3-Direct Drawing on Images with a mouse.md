1. هنستخدم حاجة اسمها CallBacks بتخلينا نربط الصور بحاجة اسمهاevent funcs  في opencv و لاحقا مع الفيديوهات كمان
2. اول حاجة هتلاحظ عملنا func  بتاخد الحدث الي هيبقي من الماوس و احداثيات الماوس و بعد كده قولنا لو الحدث ده بيساوي الكليك شمال يعمل الدايرة الي موجودة و هتلاقي لما تكتب كلمة cv2.EVENT  هتلاقي احداث كتير اختار الي يناسبك  اغلابها يا ماوس اما زراير معينة بس بتبقي مع flags  الي انت عامله و تقريبا عشان تشغلها انت بتعمل hold  للزرار 
3. اللون هنا المرة دي للاسف bgr  عشان بنعرض عن طريق opencv باستخدام   `cv2.imshow` مش plt  زي درس الي فات بتاع الاشكال
4. كوننا المصفوفة و نظام الالوان ده `np.unit8` اقرب حاجة دقيقة و شغالة مع الfunc دي
5. بعد كده محتاجين نحدد اسم الويندو الي هتظهر
6. بعد كده الfunc عن هي ترجع حركة الماوس بنباصي فيها اسم الويندو بالظبط و draw_circle  بس هتلاحظ ان احنا باصيناها as a pointer  مش func
7. تقدر تضيف اكتر من حدث و تخلي كل واحد يعمل حاجة معينة
8. ممكن تستخدم حتة الفلاج ان هو طول ما انت ماشي  يعمل دواير و كده 
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
# Create a function based on a CV2 Event (Left button click)
def draw_circle(event,x,y,flags,param):
    if event == cv2.EVENT_LBUTTONDOWN:
        cv2.circle(img,(x,y),100,color=(1,0,255), thickness=-1 )
    elif event == cv2.EVENT_RBUTTONDOWN:
        cv2.circle(img,(x,y),100,(0,0,255),-1)
    elif flags == cv2.EVENT_FLAG_SHIFTKEY:
        cv2.circle(img,(x,y),100,(0,255,0),-1)

# Create a black image

img = np.zeros((512,512,3), np.uint8)

# This names the window so we can reference it

cv2.namedWindow(winname='my_drawing')

# Connects the mouse button to our callback function

cv2.setMouseCallback('my_drawing',draw_circle)

  

while True: #Runs forever until we break with Esc key on keyboard

    # Shows the image window

    cv2.imshow('my_drawing',img)

    # EXPLANATION FOR THIS LINE OF CODE:

    # https://stackoverflow.com/questions/35372700/whats-0xff-for-in-cv2-waitkey1/39201163

    if cv2.waitKey(20) & 0xFF == 27:

        break

# Once script is done, its usually good practice to call this line

# It closes all windows (just in case you have multiple windows called)

cv2.destroyAllWindows()
```
![[Pasted image 20250816135855.png]]
9. خلي بالك انك ممكن نعمل مستطيل بال drag  بالماوس عن طريق الlogic  ده 
10. بس للاسف هتلاحظ ان بيقعد يعمل نسخ كتير تح النسخ الي بتعملها و هتلاحظ ده لو حركت الماوس في اتجاه غير المفترض فيه و مينفعش مثلا لو حركت ايديك جامد و عايز يكون الحجم صغير خلاص مش هتشوف ده (النص الشمال هتلاحظ ده و اخر واحدة في النص اليمين)
```python
import cv2
import numpy as np
# Create a function based on a CV2 Event (Left button click)
drawing = False # True if mouse is pressed
ix,iy = -1,-1
# mouse callback function
def draw_rectangle(event,x,y,flags,param):
    global ix,iy,drawing,mode
    if event == cv2.EVENT_LBUTTONDOWN:

        # When you click DOWN with left mouse button drawing is set to True

        drawing = True

        # Then we take note of where that mouse was located

        ix,iy = x,y

  

    elif event == cv2.EVENT_MOUSEMOVE:

        # Now the mouse is moving

        if drawing == True:

            # If drawing is True, it means you've already clicked on the left mouse button

            # We draw a rectangle from the previous position to the x,y where the mouse is

            cv2.rectangle(img,(ix,iy),(x,y),(0,255,0),-1)

  

    elif event == cv2.EVENT_LBUTTONUP:

        # Once you lift the mouse button, drawing is False

        drawing = False

        # we complete the rectangle.

        cv2.rectangle(img,(ix,iy),(x,y),(0,255,0),-1)
```
![[20250816-1138-05.1889909.mp4]]
 