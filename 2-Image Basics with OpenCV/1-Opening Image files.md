## 1. cv2.imread(path)
1. هي زيها `image.open(path)` بس الفرق هنا ان هي بتحول الصورة علي طول array بدل ما استخدم numpy عشان احول الصورة
2. بس عيبها ان انت لو دخلت path  غلط هي مش هتجيبلك ايرور بس vari  مش هيتسجل فيه حاجة و هيبقي ملهوش datatype و هتطلع ليك رسالة في تيرمنال ان الباث الي المدخله غلط
3. خلي بالك لو جيت تعمل عرض للصورة و كده عن طريق plt هتلاقي الوان فيها غريبة و ده بسبب ان plt بتقرا الارقام rgb  علي عكس cv2  كاتبة الارقام بترتيب bgr  فا كده لما plt  تقرا هتقرا قيم الازرق ان هي احمر و العكس 
```python
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
import cv2
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")
print(type(img))
img2 = cv2.imread("some/wrong/path.jpg")
print(type(img2))
print(img.shape)
plt.imshow(img)
plt.show()
```
	Output:
	<class 'numpy.ndarray'>
	[ WARN:0@0.023] global loadsave.cpp:275 cv::findDecoder imread_('some/wrong/path.jpg'): can't open/read file: check file path/integrity 
	<class 'NoneType'>
	(853, 1280, 3)
![[open cv/2-Image Basics with OpenCV/images_and_videos/Figure_1.png]]
4. عشان نحل المشكلة دي هنستخدم func  موجودة في cv2 بتعمل switch
  `cv2.cvtColor(src image , code)`
  مكان الcode  ده بيخليك تحدد التغير و كده بتاع الالوان و خد بالك حرف (C) بتاع كلمة color  كابيتال
  5. خلي بالك لما تحرك الماوس علي البلوت هتلاقيه بيقرأه عادي بترتيب rgb  
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB) # there are multible codes python show u to act on channels
plt.imshow(img_rgb)
plt.show()
``` 
5.  تقدر كمان تخلي الصورة gray scale   عن طريق 
 `img_gray = cv2.imread(path,cv2.IMREAD_GRAYSCALE)`
 بع كده ارجع استخدم plt.imshow  بس بردوا لازم احدد cmap بتاعها gray عشان الdefault بتاعها Viridis  
```python
img_gray = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg",cv2.IMREAD_GRAYSCALE)
plt.imshow(img_gray , cmap= "gray")
plt.show()
```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Pasted image 20250815165952.png]]
## 2. Resize the image 
1. في func  بتخليك تلعب في الابعاد بتاعت االصورة
2. بس خلي بالك هي بتاخد vari بتاعت الصورة  و tuple فيها الابعاد الجديدة الي انا عايزها (x,y)  ممكن تلعب في الحجم بتاعها و كده لو الصورة واضحة ممكن تعلي عددالبيكسلز هو هيعمل generate  للبيكسلز الي واقعة في النص و العكس صحيح لو قلقت عن مقاسها هيضحي ببيكسلات بحيث يحافظ علي اقرب شي لشكل الصورة 
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
sized_img =cv2.resize(img_rgb,(8000,6000)) #(image , (x,y))
plt.imshow(sized_img)
plt.show()
```
3. لو عايز اتعامل مع مع نسب بدل ما استخدم الارقام و مش هحفظ حجم كل صورة ممكن استخدم نفس func  تاني
4. في paramter الخامس ده مسئول عن معالجة البيكسلات الزيادة او الي هتتحذف ازاي و كده فا ملناش دعوة بيها دلوقتي
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
x_axis= 2
y_axis=2
sized_img =cv2.resize(img_rgb, None ,None ,x_axis ,y_axis ) #sec none for parameter that can replaced by existing variable to recive image
#or 
#sized_img =cv2.resize(img_rgb ,None,fx=x_axis ,fy= y_axis )
plt.imshow(sized_img)
plt.show()
```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Pasted image 20250815170230.png]]
## 3. flip images 
1. هي  func بتخليك تشقلب الصورة سواء رأسيًا او أفقيًا او الاتنين بتباصي فيها الصورة و رقم بيحدد الصورة تتشقلب ازاي `cv2.flip(src . int)`  وهنا كلمة src اقصد بيها مصفوفة بتاعت الصورة 
2. لو كتبت رقم 0 هتتشقلب حول المحور الافقي لو كتبت 1 هتتشقلب حول المحور الرأسي لو كتبت -1 هتتشقلب في المحورين
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
fliped_img = cv2.flip(img_rgb , 1 )
plt.imshow(fliped_img)
plt.show()
```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Pasted image 20250815170311.png]]
## 4. save image 
1.دي func  مش لازم احطها في متغيير و هي `cv2.imwrite("nameofimage.xxx" ,src)`
2. هي بتحط الصورة في working directory بتاعتك
3. خد بالك ان هي لما بتخزن المصفوفة بتقراها bgr فا مثلا بعد ما خلص تعديل علي الصورة و كده ارجعها تاني بنظام بتاع opencv 
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
cv2.imwrite("this name of image.jpg" , img) #image will be saved noramal becuse img vari in BGR color
cv2.imwrite("this name of image.jpg" , img_rgb)#image will be saved switched becuse img vari in RGB color

```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Pasted image 20250815170418.png]]
![[open cv/2-Image Basics with OpenCV/images_and_videos/Pasted image 20250815170528.png]]
## 5. open image in window instead of plt
```python 
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")
cv2.imshow('window_name',img)
# Wait for something on keyboard to be pressed to close window.
cv2.waitKey()
#the paramedter is waited ms 
```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Pasted image 20250815172452.png]]
1. عيبها ممكن تعمل ايرور لما تعرضها و كده ليها حل حصلت ابحث علي stackoverflow  عن حاجة اسمها 0xff 
```python
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
1. الحاجة التانية ان الصورة لما تتعرض هتتعرض بالحجم بتاعها و متعرفش تكبر ولا تصغر الصفحة فا لو جود الصورة اعلي من دقة الشاشة ممكن متعرفش تقفلها و كده 


