1.  هتلاقي ان احنا هنستخدم ملف مدرب ان هو ازاي بيكشف عن حاجة عن طريق اختبارات  عديدة 
2. بعد كده هسنتخدم الcascade classifer  منه methodes  هنباصي فيها الصورة و 2 parameters  ممكن تحطهم و ممكن لا الاول هو scalefactor  ده الي مسئول ان الصورة تقل بمقدار كام في المية في كل step في ال process  كل ما كان الرقم صغير زي 1.01 كده النتيجة هتكون كويسة بس بطيئة لكن لو زادت زي 1.3 كده هتبقي اسرع لكن كفاءة اقل و الي بعده ده مسئول ان هو ميعملش اخطاء زي ان هو يعمل 2 مربعين علي الوش بس لو زودته ممكن يصلح ده بس ممكن يفقد يشوف وشوش تانية
3. هي هترجعلك احداثيات top left  بتاعت المسطتيل و الطول و العرض الي هتسخدمهم انك ترسم بيهم علي الصورة مربع مكان ما لقي هو الوش 
```python 
def display (img , cmap = None) :

    plt.imshow(img , cmap= cmap)

    plt.show()

nadia = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\Nadia_Murad.jpg",0)

denis = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\Denis_Mukwege.jpg",0)

solvay = cv2.imread(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\solvay_conference.jpg",0)

face_cascade = cv2.CascadeClassifier(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\haarcascades\haarcascade_frontalface_default.xml")

def detect_face(img):

    face_img = img.copy()

    face_rects = face_cascade.detectMultiScale(face_img , scaleFactor=1.2 , minNeighbors=5)

    for (x,y,w,h) in face_rects:

        cv2.rectangle(face_img, (x,y), (x+w,y+h), (255,255,255), 10)

    return face_img
result = detect_face(denis)
display(result)


```
1. ممكن نعمل ده مع الفيديو عادي و هنتعامل مع الفريم ذات نفسه 
```python
def display (img , cmap = None) :

    plt.imshow(img , cmap= cmap)

    plt.show()
face_cascade = cv2.CascadeClassifier(r"D:\udemy\open cv\Udemy - Python for Computer Vision with OpenCV and Deep Learning 2021-3\1 - Course Overview and Introduction\Computer-Vision-with-Python\DATA\haarcascades\haarcascade_frontalface_default.xml")
def detect_face(img):

    face_img = img.copy()

    face_rects = face_cascade.detectMultiScale(face_img , scaleFactor=1.2 , minNeighbors=5)

    for (x,y,w,h) in face_rects:

        cv2.rectangle(face_img, (x,y), (x+w,y+h), (255,255,255), 10)

    return face_img

cap = cv2.VideoCapture(0)

  

while True:

    ret, frame = cap.read(0)

    frame = detect_face(frame)

    cv2.imshow('Video Face Detection', frame)

    c = cv2.waitKey(1)

    if c == 27:

        break

cap.release()

cv2.destroyAllWindows()
```