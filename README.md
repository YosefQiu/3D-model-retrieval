---
[3D model retrieval base on hand draw sketch](https://yongfengqiu.xyz/2021/06/20/3D-model-retrieval-based-on-hand-drawn-sketch/)

With the rapid development of CPU and GPU, 3D models are not only becoming more and more complex and rich in details, but also widely used in animation, machinery, medical and other fields. The number of 3D models is also increasing. The classification and retrieval of 3D models has become an important research direction. Although many scholars have done different researches on 3D model retrieval technology and proposed many different model retrieval algorithms, there are still many problems to be solved.  
      
In order to improve the accuracy of retrieval, firstly, the 3D model is rendered according to the appropriate spatial position change, and then the 2D view set is obtained according to the fixed projection method. Each model selects 6 2D views as the feature view set of 3D model. Secondly, feature vectors are extracted from sketch and 2D view sets, and weighted sets of global view features and D2 descriptors are constructed by Zernike moments and Fourier descriptors as integrated feature descriptors. By integrating feature descriptors to serve as feature vectors of user's hand drawn sketches and 2D views of the model, similarity evaluation is performed to retrieve 3D models.
     
<!--more-->
In this project, I focus on two things.
- 3D model projection
- 3D model retrieval

#### 3D model projection
Fist, Lets talk about projection. Because the sketch drawn by the user is two-dimensional and the model is three-dimensional, it is necessary to reduce the dimension of the three-dimensional model. Avoid the additional time overhead caused by dimension disaster in subsequent similarity calculation. In addition, when drawing 2D sketches, different users have actually reduced the dimension of objects in 3D space. What is drawn is the projection of an object with a fixed angle of view. Therefore, in order to adapt to different users, they may draw the same object from different perspectives. Considering the efficiency of the retrieval system, the system only obtains the projections of six different perspectives of the same model, and forms the optimal view set of the model. While including the perspective of the object drawn by the user as much as possible, the additional time overhead caused by excessive redundancy of the view set is reduced.
    
The projection system of 3D model in this paper adopts OpenGL and OpenCV as geometric modeling platform, imgui as UI interface and C++ language. A simple renderer is implemented. The main functions are: reading and real model; Rotate, scale and move the model; Add different lighting effects to the model (horizontal light, point light, spotlight); Different material effects; Projection of 3D model.
![src](https://github.com/YosefQiu/image/blob/master/uPic/model_prj.gif?raw=true)
#### 3D model retrieval
Second, Lest talk about retrieval. In this part, It is most important to consider which feature descriptor to choose and use. I make up a feature descriptor called Integrated featrue descriptor by Fourier descriptor and Zernike moments. Finally, I use Integrated feature descriptor and D2 shape descriptor as a model's feature descriptor.
##### Fourier descriptor
So what is fourier descriptor? The influence of Fourier descriptor is used to describe the contour data information of the expressed image, which has the characteristics of parallel movement, rotation and scale constancy. For an image, the image contour information is obtained by Fourier descriptor. Its essence is the problem of spatial and frequency domain transformation. The contour information of the image is obtained by Fourier transform of the pixels in the image. As a result of the low-pass filtering of the Fourier descriptor, the low-frequency component of the Fourier descriptor captures the general shape characteristics of the object, and the high-frequency component captures finer details, and the Fourier descriptor does not consider the spatial position.
   
$$
S(t) = X(t)+Y(t)
$$
   
$$
\mathcal{F}\left[S(t)\right] = \sum^{T-1}_{t = 0}S(t)e^{-j_2\pi k\frac{t}{T}}
$$
##### Zernike moments
The moment of the image generally describes the overall global characteristics of the modified image, and provides a lot of different kinds of mathematical and geometric feature data information for the image, such as size, specific location, distribution direction and specific shape. Zernike moment is an orthogonal moment, which is an operation function orthogonalized according to various Zernike formulas. Zernike moment has the following characteristics: perfection, orthogonality, rotation, no bending deformation. Zernike moment is a complex moment. The module of Zernike moment is usually used as a feature to express the specific shape of an object.
##### D2 Shape descriptor
![img](https://github.com/YosefQiu/image/blob/master/uPic/形状描述符.jpg?raw=true) 
According this image, we can know what is D2 descriptor. Random two points are connected on the surface of the experimental model, and the probability of the effective actual distance between the two points is dispersed to form the specific shape characteristics of D2.
#### Result
- Retrieve chair
![img](https://github.com/YosefQiu/image/blob/master/uPic/椅子检索.gif?raw=true)   
- Retrieve table
![img](https://github.com/YosefQiu/image/blob/master/uPic/桌子检索.gif?raw=true)   
