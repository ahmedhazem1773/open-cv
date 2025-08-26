1. يعني ايه blending يعني انت تسيح صورتين علي بعض و تخلي واحدة فيهم اظهرة اكتر و هكذا و ده بيتم في func اسمها addweight  شبه بتاعت الشبكات العصبية الي بتبقي عن طريق weight و كده   ![[Pasted image 20250816175734.png]]
2. الالفا و البيتا دي قيم من صفر ل1 انت بتدد بيها اني صورة تظهر و كده و الجاما معرفش بتاعت ايه فا بنحطها صفر
## 1. if 2 images has same size
1. اول حاجة هخلي الصورتين الي معايا قد بعض عشان نمشي النظرية
2. و طبعا نحولهم لrgp  عشان هنعرضهم عن طريق plt 
3. نعمل vari  عشان ياخد الصورة الجديدة 
4. خلي بالك الصورة التانية بتتحط فوق الصورة الاولي
5. الfunc   دي مش بتشتغل غير علي الصور الي نفس المقاس و ده شئ بديهي
6. خد بالك من حتة تعديل المقاس ان انا هنا ماباصيها x ,y  عادي لكن لما تاخدهم كوبي من img.shape متنساش ان هي بتعرض المصفوفة y,x  عشان cv2  هي الي عاملة كده
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")
water_mark=cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\do-not-copy-160137_1280.jpg")
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
water_mark_rgb=cv2.cvtColor(water_mark , cv2.COLOR_BGR2RGB)
water_mark_rgb=cv2.resize(water_mark_rgb,(1280,853)) #this size of first image
blended_img = cv2.addWeighted(src1=img_rgb,alpha=1,src2=water_mark_rgb,beta=0.2,gamma=0)
plt.imshow(blended_img)
plt.show()
```
![[Figure_12.png]]
## 2. how to paste small image on large one
1. الاول نحدد نقطة starting point بتاعت small_img الي هي top left  عشان دي المكان الي هتبدأ فيها الصورة الصغيرة
2. نحدد النقطة bottom right  الي هتبقي جمع مقاسات الصورة الصغيرة علي starting point 
3. بعد كده بما ان الصورة الكبيرة عبارة عن مصفوفة خلاص ممكن نستبدل بيكسلات الصورة الكبيرة ببيكسلات الصورة الصغيرة فا كده كأنك عملت pasting للصورة
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")
water_mark=cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\do-not-copy-160137_1280.jpg")
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
water_mark_rgb=cv2.cvtColor(water_mark , cv2.COLOR_BGR2RGB)
small_img=cv2.resize(water_mark_rgb,None,None,0.3 , 0.3)
large_img= img_rgb
large_img[150:150+small_img.shape[0],500:500+small_img.shape[1]]=small_img
# u notice i path startingpoint of y axis and ending point that is 
#ending = starting + y size of small imge
#notice y axis has index 0 and x axis has index 1
plt.imshow(large_img)
plt.show()
```
![[Figure_0.png]]
## 3. How cut off part from img
1. نفس الي عملناه نوعا ما بس العكس نحدد الجزء الي عايزينه و نحفظه في vari 
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
cut_off= img_rgb[550:750, 600:800]
plt.imshow(cut_off)
plt.show()
```
![[Figure_01.png]]
## 4. How to creat a mask
1. خلي بالك الطريقة مع الحاجات الي فيهاش الوان كتير و الخلفية لونها مختلف و واحد 
2. نحدد الحتة الي عايزين نحط عليها mask  ونعملها cut
3. بنحول الصورة الاول لgray scale هتلاقي الصورة بقت الخلفية سودة و الي عايز اقطعه ابيض
4. اخلي الي عايز اقطعه باللون الابيض و الخلفية بالاسود و ده عن طريق func  اسمها `bitwise_not` بتخليك تعكس الالوان
5. هتلاحظ ان الصورة بقت بعد لان البعد التالت هو ele  واحد عشان بقت channel واحدة بسبب gray scale 
6. بعد كده هنسخدم ان احنا نعمل تقاطع مابين الصورة دي والصورة الي فيها الوان عن طريق `bitwise_or` 
7. اعمل paste للmask  مع الصورة الي قطعتها و بعد كده اعملها paste تاني  في نفس الصورة الاصية من مكان ما قطعتها
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")
water_mark=cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\do-not-copy-160137_1280.jpg")
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
water_mark_rgb=cv2.cvtColor(water_mark , cv2.COLOR_BGR2RGB)
img_hsv = cv2.cvtColor(img_rgb, cv2.COLOR_BGR2HSV)
print(img_rgb.shape, water_mark_rgb.shape) 
#output:(853, 1280, 3) (1280, 1277, 3)
plt.imshow(img_rgb)
plt.show() # image1
plt.imshow(water_mark_rgb)
plt.show() #image2
large_img= img_rgb
cut_off= img_rgb[800-384:800, 600:600+383]
plt.imshow(cut_off)
plt.show() #image3
small_img=cv2.resize(water_mark_rgb,None,None,0.3,0.3)
imgingray=cv2.cvtColor(small_img, cv2.COLOR_BGR2GRAY)
inv_img=cv2.bitwise_not(imgingray)
plt.imshow(imgingray, cmap="gray")
plt.show() #image4
inv_img=cv2.bitwise_not(imgingray)
plt.imshow(inv_img, cmap="gray")
plt.show() #image5
print(inv_img.shape)
#output: (384, 383)
up_mask= cv2.bitwise_or(small_img,small_img , mask=inv_img)
print(up_mask.shape)
#output: (384, 383 ,3)
print(cut_off.shape)
#output: (384, 383 ,3)
plt.imshow(up_mask)
plt.show() #image6
added_imgs= cv2.bitwise_or(up_mask,cut_off)
plt.imshow(added_imgs)
plt.show() #image7
large_img[800-384:800, 600:600+383]=added_imgs
plt.imshow(large_img)
plt.show() #image8
```
![[Figure_a.png]]
![[Figure_b.png]]
![[Figure_c.png]]
![[Figure_d.png]]
![[Figure_e.png]]
![[Figure_f.png]]
![[Figure_j.png]]
![[Figure_i.png]]