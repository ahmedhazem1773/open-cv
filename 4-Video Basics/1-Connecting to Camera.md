1. اهم حاجة تتأكد ان يبقاش في كذا حاجة شغالة من الاكواد عشان ميحصلش مشاكل لما يحاول يوصل للكاميرا و نفس النظام يبقاش كذا kernel شغال 
2. لو عايز كاميرا تعرض عادي ابعت علي طول الframe  بدل من احولها 
```python
## PUT THIS ALL IN ONE CELL!

  

import cv2
# Connects to your computer's default camera

cap = cv2.VideoCapture(0)

# Automatically grab width and height from video feed

# (returns float which we need to convert to integer for later on!)

width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))

height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))

while True:
    # Capture frame-by-frame
    ret, frame = cap.read()
    # Our operations on the frame come here
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    # Display the resulting frame
    cv2.imshow('frame',gray)
    # This command let's us quit with the "q" button on a keyboard.
    # Simply pressing X on the window won't work!
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
# When everything done, release the capture and destroy the windows
cap.release()
cv2.destroyAllWindows()
```
3. لو عايز بقي احفظ الstream كفيديو اشوف الpath الي عايز اوديه و بعد كده اسمه الفيديو او من غير path  و هيتحفظ في wd و بعد كده طلابقة الحفظ ليها كود معتمدة علي نوع os  بتاعك لو اوبنتو يبقي XVID لو ويندوز DIVX و ده لينك بيشرح codec  ده [[https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_gui/py_video_display/py_video_display.html#saving-a-video]] و بعد كده عدد الفريمات في الثانية انت عايزه كام و بعد كده العرض و الطول الي انا عايزه
```python
import cv2
cap = cv2.VideoCapture(0)
# Automatically grab width and height from video feed

# (returns float which we need to convert to integer for later on!)

width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))

height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
# MACOS AND LINUX: *'XVID' (MacOS users may want to try VIDX as well just in case)

# WINDOWS *'VIDX'
#this is new line added
writer = cv2.VideoWriter('student_capture.mp4', cv2.VideoWriter_fourcc(*'VIDX'),24, (width, height))
## This loop keeps recording until you hit Q or escape the window
## You may want to instead use some sort of timer, like from time import sleep and then just record for 5 seconds.
  
while True:

    # Capture frame-by-frame

    ret, frame = cap.read()
    # Write the video
	#this is new line added
    writer.write(frame)
    # Display the resulting frame
    cv2.imshow('frame',frame)
    # This command let's us quit with the "q" button on a keyboard.
    # Simply pressing X on the window won't work!
    if cv2.waitKey(1) & 0xFF == 27:

        break
cap.release()
#this is new line added
writer.release()
cv2.destroyAllWindows()
```
