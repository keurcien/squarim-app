# squarim-app

Make images square again!
https://squarim.web.app/

If you need square images but can't afford to crop them, **squarim** may help you! 
You can use squarim in the following cases:

### 1. When the background is plain and the subject is centered

<img src="https://user-images.githubusercontent.com/11570911/181007239-a04d6604-95e6-41f8-a460-10de4443c77e.jpeg" height="200">

The app will simply stretch the edges of the image (by default, 50px on the left and 50px on the right), using some basic interpolation. With nowadays resolution, the default settings should work:

<img src="https://user-images.githubusercontent.com/11570911/181008219-63d73f9b-d237-4f46-af35-8716f57ac8f7.jpeg" height="200">

### 2. When the background is plain and the subject if off-centered

<img src="https://user-images.githubusercontent.com/11570911/181009274-9bfb422c-afd0-4aca-a42c-2a405bbf710f.jpeg" height="200">

Set the left value or the right value to 0, depending on the position of the subject:

<img src="https://user-images.githubusercontent.com/11570911/181009099-6a4a956c-1703-48ff-820a-09e42ae7d87c.jpg" height="200">

### 3. When the background is textured

Check the mirror checkbox:

![squarim_mirroir](https://user-images.githubusercontent.com/11570911/181009972-f9f8bb0f-e244-469c-89c3-278508ea157a.png)

### 4. When the background is...more complex

Check the auto checkbox. This will combine an object algorithm with the seam-carving algorithm to expand the image.

<img width="360" src="https://user-images.githubusercontent.com/11570911/181010635-321d657d-8a88-48a7-b1b5-f80122c702f2.png">



