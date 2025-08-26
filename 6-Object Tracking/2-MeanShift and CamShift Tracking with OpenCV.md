1. الmean shift  هو الي مسئول عن طريق الwindow  الي بتدور بيها علي object  وكده بس مقاس الwindow بيبقي ثابت فا مثلا لو الجسم بيتحرك و بيقر من الكاميرا او بيلف فا المساحة تصغر المسئول عن ده الcamshift 
2. اول حاجة بناخد اول فريم من الفيديو او اللايف كاميرا الي هو الفريم الي قبل العرض ده 
3. بعد كده ندخل cascade  بتاع الوشوش و نطلع الوشوش 
4. بعد كده انا عايز اعمل تراك لوش واحد هاخد اول وش في الarray  ال بيبقي عبارة عن احداثيات الtop left  و العرض و الطول بتاع الdetection  عشان تجيب بيهم الbottom right 
5. انا اخد منها اول وش و اخزن القيم بتاعته في tuple  و سميها track_window 
6. اعمل بقيroi(regin of intrest) ده اقص مقاس الوش من الفريم 
7. احول بعد كده roi  لنظام hsv  و بعد كده هنعمل histogram  لchannel بتاعت الhue و اخد القيم الي من صفر ل180 و اعمل بعد كده نورماليز من صفر ل255  بس كده محافظ علي الرانج بتاعي احنا بنعمل كده عشان هو من صفر ل360 و احنا بنتعامل مع 8bit 
8. احدد الكارتيريا بتاعت meanshift  سواء عدد iteration  او ابسلون 
9. بعد كده ادخل في الwhile loop  بتاعت الفيديو عشان اشغله و كده و اخد الفريمات 
10. بعد كده اخد الفريم الجديد و احوله لhsv 
11. بعد كده  استخدم حاجة اسمها cv2.backprojection دي بتاخد الاتي 
	1. الصورة بتاعت الفريم الجديد بس هنباصي نسخة hsv  في شكل list عشان لو عوزت تبعت اكتر من صورة 
	2. نحدد الchannels  بتاعت الصورة الي هنقارنها مع مع الhistogram  الي هباصيها 
	3. ال histogram الي عملنها بتاع roi 
	4. نباصي range بتاع الhistogram لكن النورماليز بيغير في القيم لكن مش في اقصي قيمة لخط الاكس 
	5. الscale factor بتاع الاوتبوت   هنخليه 1
12. هو بيعمل ايه ان هو ان هو يقارن الhist  الي انت بتباصيها بالصورة في channel الي حدتها و يحدد اكتر مكان له احتمالية يكون شبه الhist ده 
13. بترجع صورة بنفس مقاس الصورة الي دخلتها بس في channel  واحدة محددة اكتر مكان محتمل تكون هي زي الي كان بيحصل في الmatching template 
14. بعد كده استخدم الmeanshift الي بتباصي ليها 
	1. الصورة بتاعت backproj
	2. مقاس الويندو بتاع الفريم القديم 
	3. كارتيريا الي انا عملتها
15. هي بتحاول تلاقي  متوسط او مركز الثقل بتاع المكان المحتمل الجدبد و  بعد كده بتحاول تحسب  بتاع الشيفت الي حصل للصورة عن طريق تبص في الbackproj  و تبص لاحداثيات المربع القديم  و بتحاول تغيير موقع المربع بنفس مقاساته في المكان الجديد و بعد كده بترجع هي مكان المربع الجديد 
16. و استخدم مربع الجديد علي الفريم الجديد بتاعي و كده عملت obj tracking 
17. بالنسبة لcamshift ابقي اشوف الفيديو
```python
import numpy as np

import cv2

  

# Capture a video stream

cap = cv2.VideoCapture(0)

  

# take first frame of the video

ret,frame = cap.read()

  
  

# Set Up the Initial Tracking Window

  
  

# We will first detect the face and set that as our starting box.

face_cascade = cv2.CascadeClassifier(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\haarcascades\haarcascade_frontalface_default.xml")

face_rects = face_cascade.detectMultiScale(frame)

  

# Convert this list of a single array to a tuple of (x,y,w,h)

(face_x,face_y,w,h) = tuple(face_rects[0])

track_window = (face_x,face_y,w,h)

# set up the ROI for tracking

roi = frame[face_y:face_y+h, face_x:face_x+w]

  
  

# Use the HSV Color Mapping

hsv_roi =  cv2.cvtColor(roi, cv2.COLOR_BGR2HSV)

  

# Find histogram to backproject the target on each frame for calculation of meanshit

roi_hist = cv2.calcHist([hsv_roi],[0],None,[180],[0,180])

  

# Normalize the histogram array values given a min of 0 and max of 255

cv2.normalize(roi_hist,roi_hist,0,255,cv2.NORM_MINMAX)

  
  

# Setup the termination criteria, either 10 iteration or move by at least 1 pt

term_crit = ( cv2.TERM_CRITERIA_EPS | cv2.TERM_CRITERIA_COUNT, 10, 1 )

  

while True:

    ret ,frame = cap.read()

    if ret == True:

        # Grab the Frame in HSV

        hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

        # Calculate the Back Projection based off the roi_hist created earlier

        dst = cv2.calcBackProject([hsv],[0],roi_hist,[0,180],1)

        # Apply meanshift to get the new coordinates of the rectangle

        ret, track_window = cv2.meanShift(dst, track_window, term_crit)

        # Draw the new rectangle on the image

        x,y,w,h = track_window

        img2 = cv2.rectangle(frame, (x,y), (x+w,y+h), (0,0,255),5)

        cv2.imshow('img2',img2)

        k = cv2.waitKey(1) & 0xff

        if k == 27:

            break

    else:

        break

cv2.destroyAllWindows()

cap.release()
```

