1. هنعمل زي ما كنا هنعمل بس مش هنحط 0 عشان نوصل default كاميرا لا هنباصي الpath بتاع الفيديو الي عايزين نشغله و خلي بالك لو الpath  غلط مش هيطلع ايرور بس هيبقي vari  فاضي ملهوش نوع
2. بعد كده نفس كل حاجة تقريبا بس خلي بالك هو هيعرض الفيديو بسعرة كبيرة لان هو بيقرأه كملف مش بيقرأه للمستخدم و كدهو ده مفيد مثلا في ان انت مش محتاج تقعد ساعة عشان البرنامج يشوف فيديو ساعة مثلا 
3. فا عشان نتحكم في ده هنستخدم مكتبة time  هي مكتبة builtin  و بعد كده نستخدم func اسمها time.sleep و احط جواها 1 علي عدد الفريمات الي عايزها تمشي بيه
4. وخلي بالك كل ما تزود المقام الفيديو يسرع
```python
import cv2

import time

# Same command function as streaming, its just now we pass in the file path, nice!

cap = cv2.VideoCapture('student_capture.mp4')

  

# FRAMES PER SECOND FOR VIDEO

fps = 30

  

# Always a good idea to check if the video was acutally there

# If you get an error at thsi step, triple check your file path!!

if cap.isOpened()== False:

    print("Error opening the video file. Please double check your file path for typos. Or move the movie file to the same location as this script/notebook")

  

# While the video is opened

while cap.isOpened():

    # Read the video file.

    ret, frame = cap.read()

    # If we got frames, show them.

    if ret == True:

  

         # Display the frame at same frame rate of recording

        # Watch lecture video for full explanation

        time.sleep(1/fps)

        cv2.imshow('frame',frame)

        # Press q to quit

        if cv2.waitKey(25) & 0xFF == ord('q'):

            break

    # Or automatically break this whole loop if the video is over.

    else:

        break

cap.release()

# Closes all the frames

cv2.destroyAllWindows()
```