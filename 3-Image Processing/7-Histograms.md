1.  هي حاجة بتشوف الdistrubtion بتاع كمية متصلة 
2. ممكن نستخدم ده عشان مقتدير الالوان في الصورة
![[Pasted image 20250819094420.png]]
3. هي بتاخد الصورة بس جوا قوس بتاع list بس بما ان هي تبع اوبن نباصي الصورة ام bgr و الchannels  الي عايز نعرضها نباصي الindex  بتاعها و بعد كده في الmask  ده عشان لو عايز اعمل الحوار ده علي جزء معين علي الصورة فا اعملها none و بعد كده hostsize  ده العرض بتاع محور اكس عايزه يعرض لحد كام ملهوش دعوة القيم قيمتها الفعلية ايه و الرينج ده بتحدد بيه ان انت عايزه يقرا قيم تتراوح من كام لكام 
4. و بما ان هي plot فا هنستخدم plot.plot  هي بتاخد الplot الي كوناه و ممكن تاني parameter  نحدد بيه لون الgraph   سواء بقي اول حرف من اللون او تكتب اللون كله  من الوان دي لكن لو مكتبش الdefault هو اللون الازرق
![[Pasted image 20250819104307.png]]
```python
img_2=cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\output4.png")
hist_values = cv2.calcHist([img_2],channels=[0],mask=None,histSize=[256],ranges=[0,256])
plt.plot(hist_values , color= "b")
plt.show()
```
![[Figure_53.png]]
5. ممكن استخدم لوب عشاغن اعرض ال3 الوان
6. هتلاقي ان انا مستخدم حاجة اسمها enumerate هي بتخلي ترجع الindex و ال value بتاعت الindex 
7. هتلاحظ بردوا حاجة اسمها xlim  دي بتحدد ليميت بتاع عرض الداتا منت كام لكام
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\output4.png")
color = ('b','g','r')
for i,col in enumerate(color):
    histr = cv2.calcHist([img],[i],None,[256],[0,256])
    plt.plot(histr,color = col)
    plt.xlim([0,200])
plt.title('Blue Bricks Image')
plt.show()
```
![[Figure_54.png]]
## 1. Masking
1. الاول هنعمل صورة لونها اسود نفس مقاس الصورة الي عايزين ز بعد كده نحدد الجزء الي عايزين نفسه و نعمله في الصورة السودة و نخليه ابيض و بعد كده نعمل بيه ماسك مرة للنسخة الrgb عشان كل ده اشوف انا قصيت ايه 
2. لازم الماسك يكون نفس مساحة الصورة و بنباصي الصورة العادية bgr  عشان دي تبع مكتبة opencv  فا هي عارفة الترتيب
3. بعد كده هباصي عادي كل حاجة بس ابصي الماسك بتاعي 
```python
img_1=cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\output1.png")
img_1_rgb=cv2.cvtColor(img_1,cv2.COLOR_BGR2RGB)
mask = np.zeros(img_1.shape[:2], np.uint8)
mask[400:600, 250:550] = 255
plt.imshow(mask , cmap="gray")
plt.show()
masked_img_rgb =cv2.bitwise_and(img_1_rgb,img_1_rgb,mask=mask)
plt.imshow(masked_img_rgb)
plt.show()
hist_values_masked = cv2.calcHist([img_1],channels=[2],mask=mask,histSize=[256],ranges=[0,256])
plt.plot(hist_values_masked , "red")
plt.show()
```
![[Figure_55.png]]
![[Figure_56.png]]
![[Figure_57.png]]
## 2. Histogram Equalization
1. هي بتستخدم في انك تعلي الكونتراست بتاع الصورة عن طريق المبدأ ده هي بتعمل ايه بتشوف اعلي قيمة عندك و اقل قيمة في الchannel  وبعد كده علي اساسه كل القيم الي واقعة ما بينهم تعمل لكل ده زي stretch ان اقل قيمة تبقي صفر و اعلي قيمة هنحولها ل255
2. هتلاقي القيم واقعة في النص و الخط الاسود ده بيمثل تدرج القيم في الchannel الواحدة فا هتلاقي اقل قيمة كانت عند120 تقريبا و اعلي قيمة عند 200 تقريبا فا انت لما عملت stretch خليت التدرج يبقي معادلة خطية نوعا ما 
3. عيب الحوار ده ان لو الصورة عندك فيها noise  ممكن ده يزود الnoise 
![[Pasted image 20250819132249.png]]
4. اول حاجة هنعمل هنجيب الصورة و نحولها لgrayscale  و بعد كده نجيب histgram بتاعها عشان اعرف اعلي قيمة و اقل قيمة
5. بعد كده هنستخدم المعادلة الي هتعمل equalize  علي طول 
```python
img_4=cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\laura-college-K_Na5gCmh38-unsplash.jpg", 0)
plt.imshow(img_4 , cmap="gray")
plt.show()
hist_values_2 = cv2.calcHist([img_4],channels=[0],mask=None,histSize=[256],ranges=[0,256])
plt.plot(hist_values_2)
plt.show()
eq_img=cv2.equalizeHist(img_4)
plt.imshow(eq_img , cmap='gray')
plt.show()
hist_values_3 = cv2.calcHist([eq_img],channels=[0],mask=None,histSize=[256],ranges=[0,256])
plt.plot(hist_values_3)
plt.show()
```
![[Pasted image 20250819134620.png]]
![[Pasted image 20250819135138.png]]
6. ينفع نعمل كده في الصور الالوان بس لازم نحولها لhsv و نخليها تتعامل مع الv ,و بعد ما تتعامل معاها نرجعها تاني لنظام rgb
```python
img_5=cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\bro-takes-photos-fEbukGB0bBY-unsplash.jpg")
img_5_hsv= cv2.cvtColor(img_5 , cv2.COLOR_BGR2HSV)
img_5_hsv[:,:,2]=cv2.equalizeHist(img_5_hsv[:,:,2])
img_backed=cv2.cvtColor(img_5_hsv, cv2.COLOR_HSV2RGB)
plt.imshow(img_backed)
plt.show()
```
![[Pasted image 20250819141152.png]]



