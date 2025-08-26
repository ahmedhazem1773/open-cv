1. السموزنج ممكن يفيد لو صورة فيها noise
2. خلي بالك في methodes  لكتير للاتنين دول بس هناخد يدوبك شوية منهم
3. و دول بيساعدوا لما نطبقهم في edge detection بيساعد انك تعرف تحدد الobj  كويس فا الي هو نعمل بلور الاول و بعد كده نعمل سموزينج و بعد كده نطبق edge detection 
![Pasted image 20250817204746](Pasted%20image%2020250817204746.png)
4. في حاجة اسمها gamma correction  دي علي حسب قيمتها بتخلي الصورة افتح ولا اغمق زي الي موجودة في الالعاب 
5. و ده لينك بيشرح فيه kernel  ال methodes  الي فيه الي مستخدمة في البلور و السموز [Image Kernels explained visually](https://setosa.io/ev/image-kernels/)
6. ان انت بتشوف البيكسل الي بتختاره و هم بقعد كده بيشوف 8 بيكسلات الي حاوليه و بعد كده توصلوا لمعمالات بتتضرب في البيكسلات دي و تتجمع ان ده البلور و العملية دي اسمها kerbel convolution
![Pasted image 20250817210843](Pasted%20image%2020250817210843.png)
7. هتلاقي كمان ان المبدأ بيستخدم في كذا حاجة زي الcontrast و هكذا
8. اول حاجة func  بتاعت الاس هنستخدمها للجاما لو الرقم اقل من 1 كده بفتح لكن اكبر كده بغمق و هي بترجع الصورة 
9. هنا كمان هتلاحظ ام احنا حولنا الارقام لfloat  عشان نعرف نقسمهم علي 255  و هتلاقي `np.float32 ` هي الوحيدة شغالة و هتلاقي زي رسالة بتقولك في ارقام طلعت من الرينج بتاع0ل255 بس عادي
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
img=cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\output4.png").astype(np.float32)/255
img_rgb = cv2.cvtColor(img , cv2.COLOR_BGR2RGB)
plt.imshow(img_rgb)
plt.show()
img_gamma_1= np.power(img_rgb ,2)
img_gamma_2= np.power(img_rgb ,0.5)
plt.imshow(img_gamma_1)
plt.show()
plt.imshow(img_gamma_2) 
plt.show()
```
![output.3png](output.3png.png)
![Figure_20 2](Figure_20%202.png)
![Figure_23](Figure_23.png)
10. هنكتب علي الصورة تيكست بس عشان تلاحظ فرق البلور و كده بس مش اكتر
```python
font = cv2.FONT_HERSHEY_COMPLEX
cv2.putText(img_rgb,text='bricks',org=(10,600), fontFace=font,fontScale= 8,color=(255,0,0),thickness=4)
plt.imshow(img_rgb)
plt.show()
```
![Figure_24](Figure_24.png)
11. هنبدا بابسط نوع الي هو low bass filter  الي بتكون الkernel  بتاعته عبارة عن 1 مقسومة علي  مساحة الkernel (هي هنا مساحة تبان لكن هي مقسومة علي مجموع الkernel ) و المساحة بتاعته عبارة عن ارقام فردية اس 2 يعني العرض و الطول يكون رقم فردي و كل ما مساحة الكيرنل تزيد كل ما كمية البلور تزيد
12. بعد كده هسنتخدم func  اسمها `cv2.filter2D` و هي 2d  عشان معاك 2 من الarray الصورة و الkernel بتاخد الصورة و بعد كده حاجة اسمها الdepth  دي باصي فيها -1 عشان تخلي الصورة الي داخلة زي الي طالعة في العمق  و بعد كده الkernel
```python
img=cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\output4.png").astype(np.float32)/255
img_rgb = cv2.cvtColor(img , cv2.COLOR_BGR2RGB)
font = cv2.FONT_HERSHEY_COMPLEX
cv2.putText(img_rgb,text='bricks',org=(10,600), fontFace=font,fontScale= 8,color=(255,0,0),thickness=4)
kernel= np.ones((5,5) , dtype=np.float32)/25 #array of ones divided by sum of elements
blured_image=cv2.filter2D(img_rgb,-1 ,kernel)
plt.imshow(blured_image)
plt.show()
```
![Figure_25.png](Figure_25.png)
13. هتلاقي كمان func blur  جاهزة و ليها قيم خاصة بيها للkernel  كل الي عليك بس تباصي الصورة و تحدد مقاس الkernel 
```python
img=cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\output4.png").astype(np.float32)/255
img_rgb = cv2.cvtColor(img , cv2.COLOR_BGR2RGB)
font = cv2.FONT_HERSHEY_COMPLEX
cv2.putText(img_rgb,text='bricks',org=(10,600), fontFace=font,fontScale= 8,color=(255,0,0),thickness=4)
blured_image=cv2.blur(img_rgb, ksize=(5,5))
plt.imshow(blured_image)
plt.show()
```
![Figure_25 1](Figure_25%201.png)
14. في بعد كده median & gaussian bluring  معتمد علي متوسط قيم البيكسلات علي حسب لو جاوس بيبقي علي حسب  قيمة السيجما لكن المادين في مقاس الfilter الي هتحدده , هتلاقي في جاوس بتحدد قيمة السيجما و كمان هتلاحظ ان المادين مش بيعمل بلور كويس بس بيستخدم ان انت تشيل الnoise  من الصورة عشان لو عايز تبين حاجات و كده عشان detection بس في مقابلها ممكن تخسر تفاصيل بس هي مش هتمك زي مثلا حشائش في الخلفية و هكذا
```python
img=cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\output4.png").astype(np.float32)/255
img_rgb = cv2.cvtColor(img , cv2.COLOR_BGR2RGB)
font = cv2.FONT_HERSHEY_COMPLEX
cv2.putText(img_rgb,text='bricks',org=(10,600), fontFace=font,fontScale= 8,color=(255,0,0),thickness=4)
blured_image_1= cv2.GaussianBlur(img_rgb, ksize=(5,5),sigmaX=8)
blured_image_2= cv2.medianBlur(img_rgb, ksize=3) #it tahes only 1d and powerd it to 2
plt.imshow(blured_image_1)
plt.show()
plt.imshow(blured_image_2)
plt.show()
```
![Figure_27](Figure_27.png)
![Figure_28](Figure_28.png)
شوف الفرق هنا لما جربنا نشيل الnoise  هتلاقي في تفاصيل اختفت زي العشب و عوض ده نوعا ما عن طريق اكبر مساحة الصورة قبل ما اشيل الnoise عشان تعمل generate للبيكسلات هي و تزود دقة الصورة 
![Pasted image 20250818142024](Pasted%20image%2020250818142024.png)
15. في فلتر تاني اسمه bilateral filter  ده بيشيل الnoise  و بيحافظ علي sharpe  بتاعت edges  بس هي بطيئة مقارنة بالي فاتممكن اديها بصة علي notebook  بتاعت الفيديو ده 



 