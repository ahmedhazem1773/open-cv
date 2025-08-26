  1. هنكون blank image عن طريق نكون array  و تكون 3 ابعاد زي الصورة و كده نخلي قيم بتاعتها صفر عشان تبقي لونها اسود
  2. هتلاقي كمان هنا مستخدم 16bit عشان لما دورت كل ده قدام في عمليات الطرح مثلا مثلا و الmaping  و كدعه هنحتاج رينج كبير من الارقام و الارقام السالبة كمان لكن لا تقلق كده كده opencv بتظبط الدنيا بحيث الارقام تبقي زي ما هي من0 ل255 
```python
blank_img = np.zeros(shape=(512,512,3),dtype=np.int16)
plt.imshow(blank_img)
plt.show()
```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Figur.png]]
3. ممكن اعملها بطريقة تانية لو عايزها حجم صورة معينة عن طريق func اسمها `np.full`
4. هي بتاخد الابعاد فا هنباصي shape  بتاع الصورة الصليةو بعد كده  القيم  هنملاها ب0  الي هو الاs,] و بعد كده نوع الالوان هنختار `unit8` كده بقت خلفية بيضاء من 3channels  كلها 0 
```python
black_background = np.full(water_mark_rgb.shape, 0, dtype=np.int16)
#first para is image as array
plt.imshow(black_background)
plt.show()
```
## 1.Drawing rectangle
1. هي func بتعدل علي الصورة ذات نفسها يعني مفيش داعي استلمها و كده في نفس الvari 
2. هي بتاخد paramters 
	1. اول واحد هو مصفوفة الصورة 
	2. بتاخد tuple لاحداثيات (x,y) بتاعت نقطة top left بتاعت المستطيل 
	3. بتاخد بردوا احداثيات بتاعتbottom left بتاعت المستيطل 
	   -هتلاحظ ان x  من نقطة الاصل ماشي من صفر لاعلي قيمة لكن y  بادء من اعلي قيمة ليه لحد الصفر يعني نقطة الاصل (zero, maxvalue )- 
	4. بتاخد الحاجة الرابعة اللون في شكل tuple  من 3 عناصر intger  و انت تدخل الكود بتاعه 
   `(R ,G , B)`
	5. بعد كده الخامس السمك بيتحسب بالبيكسل بس خلي بالك لو مثلا  انت قولت 10 بيكسل هتلاقي عند مكان الحدود بتاعت الشكل الي حددتها هتلاقي 5 فوق و5 تحت لكن هو مش بيعمل fill لشكل لو عايز اديله fill  احط قيمة بسالب 1
3. خلي بالك لو الصورة قريبة من الحواف و كده لان لو السمك بتاعك كبير هيتاكل بسبب ان ده اخر الصورة 
```python 
blank_img = np.zeros(shape=(512,512,3),dtype=np.int16)
cv2.rectangle(blank_img,pt1=(384,45),pt2=(500,128),color=(0,0,255),thickness=6) # i can don't use key word to assign values with keeping the order but to be readable
plt.imshow(blank_img)
plt.show()
```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Figure_3.png]]
```python
blank_img = np.zeros(shape=(512,512,3),dtype=np.int16)
cv2.rectangle(blank_img,pt1=(300,45),pt2=(416,128),color=(0,0,255),thickness=-1)
plt.imshow(blank_img)
plt.show()
```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Figure_6.png]]
4. تقدر كمان تطبقها اكتر مرة  و هي كده كده بتحفظ التغيرات اول باول و مش شرط التغيرات تكون ورا بعض لا عادي ممكن يكون في النص ما بينهم اكواد بس طبعا هتطبق علي لما يجي دورها في flow 
```python
blank_img = np.zeros(shape=(512,512,3),dtype=np.int16)
cv2.rectangle(blank_img,pt1=(384,45),pt2=(500,128),color=(0,0,255),thickness=6)
cv2.rectangle(blank_img,pt1=(100,45),pt2=(216,128),color=(20,170,180),thickness=-1)
plt.imshow(blank_img)
plt.show()
```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Figure_9.png]]
## 2. Drawing circle
1. نفس خصائص بتاعت المستطيل في كل حاجة بس هتختلف فقط في parameters  و مش كلها
	1. الصورة
	2. مركز الدايرة
	3. نصف القطر 
	4. اللون
	5. السمك 
```python
blank_img = np.zeros(shape=(512,512,3),dtype=np.int16)
cv2.circle(blank_img , center=(250,400) , radius=50 , color=(255,0,0), thickness=4)
cv2.circle(blank_img , center=(400,400) , radius=50 , color=(255,0,0), thickness=-1)
plt.imshow(blank_img)
plt.show()
```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Figure_45 1.png]]
## 3. Drawing line
1. نفس الكلام كذلك بتاع الاشكال الي فاتت زي المستطيل بالظبط بس الفرق ان النقط الي بتباصيها هي بداية و نهاية الخط 
2. بس خلي بالك هنا السمك لو حطيت قيمة سالبة هيطلعلك ايرور لان اكيد ده ملهوش مساحة 
```python
blank_img = np.zeros(shape=(512,512,3),dtype=np.int16)
cv2.line(blank_img , pt1=(0 , 512) , pt2=(140 , 300), color=(180,180,100) , thickness=5)
cv2.line(blank_img , pt1=(512,256) , pt2=(256 , 256), color=(40,255,100) , thickness=5)
plt.imshow(blank_img)
plt.show()
```

![[open cv/2-Image Basics with OpenCV/images_and_videos/Figure_11.png]]
## 4. Drawing Text
1. هو شبه بتاعت رسم الخط بس بتختلف في  parameters 
	1. الصورة
	2. الجملة الي عايزها 
	3. نقطة بداية كتابة الخط بتبقي لو بصينا للجملة ان هي مستطيل فا النقطة دي بتمثل left bottom 
	4. نوع الfont 
	5. المقاس بتاع الجملة 
	6. لونها 
	7. السمك
	8. حاجة مش فاهم بتاعت ايه 
2. الخطوط الي فيها قليلة جدا 
```python 
blank_img = np.zeros(shape=(512,512,3),dtype=np.int16)
font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(blank_img,text='Hello',org=(10,500), fontFace=font,fontScale= 4,color=(255,255,255),thickness=2,lineType=cv2.LINE_AA)
plt.imshow(blank_img)
plt.show()
```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Figure_31.png]]
### addtion
1. لو انت عايز تستخدم خطوط انت محملها و تتعامل مع الخط بشكل احسن في مكتبة اسمها Font  خد بالك من اسمها عشان في واحدة شبهها 
2. خد بالك هتغير هي بردوا نسخ بتاعت opencv و numpy 
```
pip install Font
```
3. هتعمل import لfunc بتعملك تعديل علي الصورة عشان تكتب الكلمة 
```python
from Font.funcs import putTTFText
import cv2
```
4. هي func  بترجع مصفوفة الصورة وبتاخد parameters 
![[open cv/2-Image Basics with OpenCV/images_and_videos/Pasted image 20250815204151.png]] 
```python
renderedImage = putTTFText(blank_img, "Hello world!", (0, 512), r"D:\fonts\Blaka,Cairo,IBM_Plex_Sans_Arabic,Kufam,Lalezar,etc (4)\Cairo\Cairo-VariableFont_slnt,wght.ttf", 1000)
#forth paramter for path , org for top left
plt.imshow(renderedImage)
plt.show()
```
## 5. Drawing Polygons 
1. بنعمل array  بتحمل arrays  كل واحد حاملة احداثيات و كل واحدة بتمثل مكان الرأس بتاع الشكل بالبلدي عددهم بيساوي عدد الرؤوس او الاضلاع 
2. هحتاج احولهم ل3 ابعاد عشان الصورة من 3 و هي سطمبة واحدة `(-1,1,2)`
```python
vertices = np.array([[100,300]],dtype=np.int32)
pts = vertices.reshape((-1 , 1 ,2))
print(pts.shape)
print(pts)
```
	Output:
		(4, 1, 2)
		[[[100 300]]
		 [[200 200]]
		 [[400 300]]
		 [[200 400]]]
3. يعد كده هتاخد parameters
	1. الصورة
	2. المصفوفة الي عملناها من 3 ابعاد هنحطها في [ ] 
	3. حاجة اسمها isclosed دي عشانلو عايز اقفل الشكل فا احط True
	4. اللون
	5. السمك
4. الطريقة دي مش بتشتغل علي نظام int16  بتشتغل علي int32 
5. خلي بالك السمك مش بياخد قيم سالبة عشان تعمل fill
```python
blank_img = np.zeros(shape=(512,512,3),dtype=np.int16)
vertices = np.array([[100,300]],dtype=np.int32)
pts = vertices.reshape((-1 , 1 ,2))
cv2.polylines(blank_img,pts=[pts],isClosed=True,color=(255,0,0),thickness=5)
plt.imshow(blank_img)
plt.show()
```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Figure_43.png]]
6. لو عايز اعمل fill للشكل ده في func بتعمل كده و ملهاش دعوة هي بالstroke
```python
vertices = np.array([[100,300]],dtype=np.int32)
pts = vertices.reshape((-1 , 1 ,2))
cv2.fillConvexPoly(blank_img, pts,color=(255,0,0))
plt.imshow(blank_img)
plt.show()
```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Figure_32.png]]
6. فا انت ممكن تعمل حركة حلوة انك تعمل polygone  ده ب thikness لون و بعد كده لما ترسم الpolygone  تاني بس fill  ختار لون تاني و هكذا
```python 
vertices = np.array([[100,300]],dtype=np.int32)
pts = vertices.reshape((-1 , 1 ,2))
cv2.polylines(blank_img,pts=[pts],isClosed=True,color=(255,255,255),thickness=6)
cv2.fillConvexPoly(blank_img, pts,color=(255,0,0))
plt.imshow(blank_img)
plt.show()
```
![[open cv/2-Image Basics with OpenCV/images_and_videos/Figure_33.png]]
وده لينك دوكيمنتاشن لو عايز تعرف اكتر بس نشخة اقدم شوية 
[Drawing Functions — OpenCV 3.0.0-dev documentation](https://docs.opencv.org/3.0-beta/modules/imgproc/doc/drawing_functions.html#fillpoly