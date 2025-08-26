## 1.image obj
1. بتخليك تعرف تحفظ صورة في variable  من النوع image obj عن طريق `vari=Image.open(path)`
2. بنباصي جواها path بتاع الصورة الي عايزين نتعامل معاها و ناخد بالنا ان ممكن يحصل ايرور بسبب backslash لان هي من escaping char فا عشان نبطل مفعولها يا اما نكتب وؤا كل باكسلاش نكتب باكسلاش زيها بحيث تبقي زي كده \\ او الطريقة التانية انك تكتب قبل علامة التنصيص بتاعتpath حرف r 
3. لو عايز تشوف الصورة الي بتتعامل معاها و كده في method اسمها `vari.show()` بتخليك تشوف الصورة في عارض الصور الي عندك
```python
pic = Image.open(r'C:\Users\EG.LAPTOP\Downloads\855b056d-e67e-4fa8-8de8-4c2402c43b92.jpeg')
pic.show()
```
![[open cv/1-NumPy and Image Basics/images_and_videos/Pasted image 20250815171821.png]]
4. لو استخدمت `type()`func هيكتبلك نوع امتداد الصورة
5. مكتبة numpy متعرفش تتعامل مع الصورة كده لازم نحولها لarray عن طريق
   `newarrayvari= np.asarray(imagevari)`
   خلي بالك index اول واحد يمثل y  وتاني index بيمثل x
6. لو عايز اعرض الصورةفي شكل graph لازم اباصي الarray بتاعت الصورة.( بص علي الكود هتفهم قصدي عند حتة الي تخص النقطة دي )
```python
pic = Image.open(r'C:\Users\EG.LAPTOP\Downloads\855b056d-e67e-4fa8-8de8-4c2402c43b92.jpeg')
print(type(pic))
newarray = np.asarray(pic)
print(newarray.shape)
#this is for point num 6
plt.imshow(newarray) #هنا بباصي المصفوفة بتاعت الصورة
plt.show() # دي لازم تتكتب بعد الكود الي فوق بتشتغل عليها وبتعرضلك الصورة بتاعت المصفوفة دي
plt.show() #this will not run becuse there the closest one is plt.show()
```
	Output:
	<class 'PIL.JpegImagePlugin.JpegImageFile'>
	(736, 736, 3)
	بعد كده هتعرض الصورة بتاعتك في graph في عارض خاص بالمكتبة و الكود مش هيكمل شغل غير لما تقفل الصفحة دي 
	ملاحظة: خلي بالك  plt.show() عشان هي بتشتغل علي اقرب plt.imshow(newarray) فوقيها لو قابلت اقرب حاجة كانت plt.show()  فا هي مش هتشتغل بس مش هتعمل ايرور
![[open cv/1-NumPy and Image Basics/images_and_videos/Screenshot 2025-08-15 165401.png]]
7. لو عايز اطلع الصورة في لون واحد بس اصفر اللونين التانيين لو طفيت لون واحد بس هتطلع الصورة ميكس ما بين درجات اللونين التانيين زي لو طفيت الاخضر هيطلع درجات من البنفسجي (دمج ما بين الازرق و الاحمر)
```python
pic = Image.open(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")
newarray = np.asarray(pic)
red_array = newarray.copy()
red_array[: , : ,1] =0 #turn green to zero for every pixel
red_array [: , : ,2] =0 #turn blue to zero for every pixel
plt.imshow(red_array)
plt.show()
```
![[open cv/1-NumPy and Image Basics/images_and_videos/Figurered.png]]
8. لكن لو جيت استخدم slicing  عشان كده مش بصفر اللونين التانيين كده  هيطلع حاجة اسمها  pseudocolor ده نظام لوني المهم اقدر منه اطبق منه حاجة اسمها colormap  ده بيخليني زي اختار الثيم بتاع الصورة الالوان الي طابع عليها ايه لان انا كده بتعامل مع channel واحدة و نظام color map ده بيبقي من 0 ل255  زي مثلا gray scale لو انا حددت channel red  فا انا كده هعرف المناطق الفاتحة متشبعة باللون الاحمر و الغامقة قليل فيها اللون الاحمر و هكذا بقي مع colormaps بشوف الصفر بتاعها لونه ايه و 255 لونه ايه و اختار channel الي اتعامل معاه و كده و ده بيفيد  مثلا مع الناس الي عندها عمي الوان و تطبيقات اخري  
9. اقدر ادي للgraph  ده عنوان  عن طريق mthod اسمها `plt.title("string")` و اكتبها ما بين `plt.im` و `plt.show`  
10. اقدر كمان اطلع colorbar  يمثلي cmap  بتاعتي ان الصفر لونه ايه و 255 لونه ايه `plt.colorbar()` , الdefualt لو محددتش cmap للصورة هو  Viridis  و نفس النظام زي نقطة رقم 9 يتكتب فين 
```python
pic = Image.open(r"C:\Users\EG.LAPTOP\Downloads\WhatsApp Image 2025-08-13 at 20.14.36_568770bd.jpg")
newarray = np.asarray(pic)
plt.imshow(newarray[:,:,0] , cmap="gray")
plt.title("Image with gray Colormap")
plt.colorbar()
plt.show()
```
	note: cmap takes only keywords that builtin the library as string after more deep ,  u will be able creat your own colormap and give it name to use it with cmp 
![[open cv/1-NumPy and Image Basics/images_and_videos/Figure_2.png]]
