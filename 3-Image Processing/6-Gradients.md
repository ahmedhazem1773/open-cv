1. من الحاجات المهمة الي تفيد في obj detection 
2. هو عبارة تغير في اتجاه معين في اللون او كثافة الصورة
3. فا هنقعد نكتشف basic sobel-feldman operators  و الي معروفة بي sobel و في المستقبل هنتوسع اكتر فيها في detection 
4. هي بتخليك تشوف الحاجات الي مثلا افقية او رأسية او الاتنين  و هي مبنية علي مبدأ تشوف المشتقة او التغيير في قيم البيكسلات فا لو التغير كبير اكيد ده edge  لو التغيير صغير يبقي الاتنين نفس المستوي او نفس اللون فا  مفيش edge 
5. دي kernels  بتاعتها واحدة بتكشف عن الخطوط الافقية و التانية عن الرأسية  مضروبة في مصفوفة البيكسل و المجاورة ليها
![Pasted image 20250818175642](open%20cv/3-Image%20Processing/images%20&%20videos/Pasted%20image%2020250818175642.png)
6. هي func بتاخد الصورة و قيمة الdepth  و المشتقة من الدرجة الكام في اتجاه الx و اتجاه الy و بعد كده مقاس الkernel بتاعتك و بنبقي شغالين علي gray scale  عشان يعرف يقارن ما بين الابيض و الاسود اسهل
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
from PIL import Image
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\soduko.png",0)
plt.imshow(img , cmap="gray")
plt.show()
sobelx= cv2.Sobel(img , cv2.CV_64F,1 ,0 , ksize=5)
plt.imshow(sobelx , cmap="gray")
plt.show()
sobely= cv2.Sobel(img , cv2.CV_64F,0 ,1, ksize=5)
plt.imshow(sobely , cmap="gray")
plt.show()
```
![soduko](open%20cv/3-Image%20Processing/images%20&%20videos/soduko.png)
![Figure_46](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_46.png)![Figure_47](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_47.png)
7. ممكن بعد كده اجيب قيمة الوتر بما ان معايا قيم الy و الx الي هو اربع كل واحد و اجمعهم و اخد الجذر التربيعي و  كده عملت edge detection  و لو جربت اعمل اشتقاق في الاتجاهين و و ربعته و رجعته تاني عشان اشيل القيم السالبة هتلاقي النتيجة سيئة  و ممكن اعمل زي اعمل للنتيجتين اجمعهم مع بعض زي كان ان انا بعمل paste لدي فوق دي و ادي كل واحدة فيهم نص و لو عايز احسن النتيجة ممكن استخدم threshold  بدل ما اجيب القيم الموجبة  
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\soduko.png",0)
sobelx= cv2.Sobel(img , cv2.CV_64F,1 ,0 , ksize=5)
sobely= cv2.Sobel(img , cv2.CV_64F,0 ,1, ksize=5)
sobel1=np.power(np.power(cv2.Sobel(img , cv2.CV_64F,1 ,1, ksize=5),2),0.5)
sobel2=((sobelx**2)+(sobely**2))**0.5
plt.imshow(sobel1 , cmap="gray")
plt.show()
plt.imshow(sobel2 , cmap="gray")
plt.show()
sobel3=cv2.addWeighted(sobelx,0.5 , sobely , 0.5,0)
plt.imshow(sobel3 , cmap="gray")
plt.show()
sobel4=np.power(np.power(sobel3,2),0.5)
plt.imshow(sobel4 , cmap="gray")
plt.show()
```
![Figure_48](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_48.png)
![Figure_48_combine_by](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_48_combine_by.png)![Figure_50](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_50.png)
![Figure_49](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_49.png)
8. في بردوا حاجة اسمها laplace operator  بتبقي شكل المعادلة بتاعتها كده بتباصي فيها بس الصورة و الdepth  
![Pasted image 20250818201526](open%20cv/3-Image%20Processing/images%20&%20videos/Pasted%20image%2020250818201526.png)
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\soduko.png",0)
laplacian1 = cv2.Laplacian(img,cv2.CV_64F)
plt.imshow(laplacian1 ,  cmap="gray")
plt.show()
laplacian2=np.power(np.power(laplacian1,2),0.5)
plt.imshow(laplacian2 ,  cmap="gray")
plt.show()
```
![Figure_51](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_51.png)
![Figure_52](open%20cv/3-Image%20Processing/images%20&%20videos/Figure_52.png)