1. هتلاقي في CV ان في التطبيقات معتمدة علي ان الصورة تبقي gray scale  عشان كل ده تعرف تحدد  ال shapes  و في حاجة بتستخدم كمان الباينري بمعني يا ابيض يا اسود فقد
2. الthresholding هو في الاساس بس طريقة بسيطة بتستخد ملفصل الصورة لاجزاء مختلفة و هي بتحول الصور لابيض يا اسود و ده عن طريق ان احنا نحول الصورة الاول لgray scale و بعد كده نحدد قيمة الي بتبقي اسمها threshold  و بعد كده نشوف قيم البيكسل لو اعلي من القيمة دي تخليها لون ابيض لو اقل تبقي اسود
3. ده لينك لو عايز تقرأ عن الموضوع اكتر [[image processing|Thresholding (image processing|[image processing) - Wikipedia]] - Wikipedia]]%20-%20Wikipedia))
![[open cv/3-Image Processing/images_and_videos/Pasted image 20250817151600.png]]
![[open cv/3-Image Processing/images_and_videos/Pasted image 20250817151609.png]]
4. ممكن نحول الاصورة لgray scale  بطريقة لما ناخدها اساسا و دي الصورة الاصلية
![[open cv/3-Image Processing/images_and_videos/output1.png]]
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\output1.png",0 )
plt.imshow(img_rgb, cmap="gray")
plt.show()
```
![[open cv/3-Image Processing/images_and_videos/Figure_21.png]]
5. هي  الfunc  بتاخد الصورة و قيمةthreshold  بتاعتك و maxvalue  الي انت عازيها في الحالة دي هتبقي 255 عشان اي حاجة اعلي من قيمة thrshold  تتحول للmaxvalue و نوع الthreshold  ايه
6. هي ال func  دي بترجع قيمة الthreshold  و الصورة 
7. لو عايز طريقة threshold تبقي العكس في حوار الابيضو الاسود اقدر اختار ده من نوع threshold
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\output1.png",0 )
threshold_value , threshold_image = cv2.threshold(img , 127 , 255 , cv2.THRESH_BINARY)
plt.imshow(threshold_image , cmap="gray")
plt.show()
threshold_value , threshold_image_inv = cv2.threshold(img , 127 , 255 , cv2.THRESH_BINARY_INV)
plt.imshow(threshold_image_inv , cmap="gray")
plt.show()
```
![[open cv/3-Image Processing/images_and_videos/Figure_22.png]]![[open cv/3-Image Processing/images_and_videos/Figure_100.png]]
8.  في نوع تاني`THRESH_TRUNC` دي بتخلي اي قيمة اعلي من thresholdvalue  تتحول للthreshold و الاقل يفضل زي ما هو 
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\output1.png",0 )
threshold_value , threshold_image = cv2.threshold(img , 127 , 255 , cv2.THRESH_TRUNC)
plt.imshow(threshold_image , cmap="gray")
plt.show()
```
![[open cv/3-Image Processing/images_and_videos/Figure_13.png]]
9. دي بقية الانواع 
![[open cv/3-Image Processing/images_and_videos/Pasted image 20250817175454.png]]
10. هتلاقي `THRES_TOZERO` هتلاقي ان اي قيمة اعلي من threshold تفضل زي ما هي والاقل يبقي صفر و في عكسها
```python
img = cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\output1.png",0 )
threshold_value , threshold_image = cv2.threshold(img , 127 , 255 , cv2.THRESH_TOZERO)
plt.imshow(threshold_image , cmap="gray")
plt.show()
threshold_value , threshold_image_inv = cv2.threshold(img , 127 , 255 , cv2.THRESH_BINARY_TOZERO_INV)
plt.imshow(threshold_image_inv , cmap="gray")
plt.show()
```
![[open cv/3-Image Processing/images_and_videos/Figure_14.png]]
![[open cv/3-Image Processing/images_and_videos/Figure_15.png]]
11. لو خد بالك ان هنا بنكون شغالين بقيمة threshold ثابتة ده اسمهاglobal بس فل نفترض لو الاضاءة مكانتش مساعدة او اي حاجة فا احنا ممكن نعمل زي threhold  قيمته ==متكيفة== علي كثافة بتاعت البيكسلات و تعالي نطبق ده و نطبق تطبيقات حقيقية في العالم الخارجي عشان كده هنستخدم حاجة اسمها
adaptive threshold 
12. اولا بس في كود بيتحكم في شاشة عرض بتاعت الplot  ممكن تعملها بالماوس بس قولت اكتبهاو ابقي ادور وراها
```python
def show_pic(img):
    fig = plt.figure(figsize=(10,10)) # inch x inch
    ax = fig.add_subplot(111)
    ax.imshow(img,cmap='gray')
    plt.show()
show_pic(img)
```
13. لو معانا صورة زي دي مثلا ممكن نفكر فيها ان احنا نحول الصورة ان اي حاجة معمولة بالحبر اسود غير كده ابيض بس لو جربنا الطريقة العادية هتبقي نتيجة غير مرضية لو استخدمنا مثلا رقم 127 و ممكن حد يقولي طيب ما نشوف اللون الابيض هنا بيتراوح في رقم كام ونعملها يبقي كده الموضوع اتحسن بس انت هنا هتضطر تشوف بنفسك فل نفترض بعد ما حاولت تشوف لقيت اقل قيمة هي 200 هتلاقي انك نسيت بتاعت جملة we enable your OCD  هنختار 170 و هنلاقي النتيجة اتحسنت بس طبعا مش معقول كل شوية نقعد ندور و نجرب و بتاع و هنا يجي دور adaptive threshold
![[open cv/3-Image Processing/images_and_videos/output3.png]] 
![[open cv/3-Image Processing/images_and_videos/Figure_16.png]]![[open cv/3-Image Processing/images_and_videos/Figure_17.png]]
![[open cv/3-Image Processing/images_and_videos/Figure_18 1.png]]
14. دلوقتي لو بصينا علي func  بتاعت adaptive 
`threshold_img=cv2.adaptiveThreshold(src, maxValue, adaptiveMethod, thresholdType, blockSize, C)`
15. هتلاقي ان هي بتاخد الصورة و maxvalue و بتختارطريقة adaptive  هم 2 موجودين في المكتبة جاوسين و المتوسط هشرحهم بعدين  و بعد كده طريقةthreshold output  الي هو انت عايزها pinary  ولا العكس و بعد كده blocksize  ده مساحة عدد البيكسلات المجاورة الي انت عايز تبص عليه بتبقي مربع الرقم ده و الرقم ده لازم يكون فردي و بعد كده c  ده رقم بتطرحه من threshold  الي هتطلع من طريقة الadaptive و بيكون غالبا موجب بس ينفع صفر او سالب و ده عن طريق التجربة و قيمة adaptive threshold في الطريقة بتبقي عن طريقة  متوسط للبيكسلات من قيمة المساحة ناقص الc لكن التانية معتمدة علي طريقة جاوسين و تستخدم standard deviation  و هكذا
16. بعد كده نقعد نعمل نتيجتين و نعملهم blending  مع بعض و كل نتيجة تطلع اعملها blend  مع نتيجة الblend  و هكذا لحد عدد iterations مناسب 
```python
img_2= cv2.imread(r"C:\Users\EG.LAPTOP\Downloads\output3.png",0)
threshold_img_1=cv2.adaptiveThreshold(img_2, 255, cv2.ADAPTIVE_THRESH_MEAN_C,cv2.THRESH_BINARY, 11, 10)
blended_img=cv2.addWeighted(src1=threshold_img_1,alpha=0.2,src2=img_2,beta=1,gamma=0)
plt.imshow(blended_img , cmap="gray")
plt.show()
```
![[open cv/3-Image Processing/images_and_videos/Figure_19.png]]