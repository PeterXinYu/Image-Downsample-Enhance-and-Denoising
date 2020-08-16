# Image-Enhance-and-Denoising
## 1: Spatial and Intensity Resolution

### (a) Please make a program to downsample an image to 1/2, 1/4 and 1/8, and implement a function (DIY) to recover the original size by interpolation (bilinear or bicubic), you can refer Section 2.4 of Digital Image Processing 3rd by Gonzalez. 

1. **Interpolation**

- **Interpolation** is the process of using known data to estimate values at unknown locations. 
- **Nearest-neighbor interpolation** algorithm selects the value of the nearest point and does not consider the values of neighboring points at all, yielding a piecewise-constant interpolant.
- **bilinear interpolation** is an extension of linear interpolation for interpolating functions of two variables (e.g., x and y) on a rectilinear 2D grid.
- **bicubic interpolation** is an extension of cubic interpolation for interpolating data points on a two-dimensional regular grid. The interpolated surface is smoother than corresponding surfaces obtained by bilinear interpolation or nearest-neighbor interpolation.

2. **Adoped Method:** Bilinear Interpolation

- **Algorithm Description**  
  We have a source image and intend to get an image through downsampling or upsampling, which refered as a destination image.We assumes that the coordinate of a pixel in destination image is (i + u,j + v), in which i and j are integer parts, and u and v are decimal parts.Therefore, the intensity of the pixel in destination is determined by the intensities of the four pixels whose coordinates are (i,j), (i + 1,j), (i,j + 1), (i + 1,j + 1) in source image. The formula is f(i+u,j+v) = (1-u)(1-v)f(i,j) + (1-u)vf(i,j+1) + u(1-v)f(i+1,j) + uvf(i+1,j+1),in which f(i,j) represents the intensity of pixel of coordinate (i,j) in source image.<br> <center>![figure1](bilinear_interpolation.jpg)</center><br>For example, we assume that the size of source image is 4 * 4 and that the size of destination image is 5 * 5. Assuming the coordinate of pixel in destination image is (1,1), we can get the cooresponding coordinate in source image is (0.75,0.75). In this case i=0,j=0,u=0.75 and v=0.75. Therefore, the intensity of pixel in (0.75,0.75) is determined by the intensities of pixels in (0,0),(0,1),(1,0),(1,1). Because (0.75,0.75) is near from (1,1) relatively, the pixel in (1,1) has more power, as we can see u*v=0.75 * 0.75 in the formula. Meanwhile, pixel in (0,0) has less power, as we can see (1-u)(1-v) = 0.25 * 0.25 in the formula.
- **Algorithm Step**  
  - First, through the sizes of source image and destination image, we can get the proportion between two images.
  - Second, we correspond the pixel in destination image to the pixel in source image.
  - Third, we get the x,y coordinate of four pixels: i,j,u,v.
  - Forth, using formula of bilinear interpolation, we get the intensity of the pixel in destination image.
  - Fifth, iterate the second setp until we get all intensities pf pixels in destination image.

3. **Downsampling and Recovery**

- We have three original images.  
  <br>

<table>
    <tr>
        <td ><center><img src="/images/face.png" width="200">face</center></td>
        <td ><center><img src="/images/cameraman.png" width="200">cameraman</center></td>
        <td ><center><img src="/images/crowd.png" width="200">crowd</center></td>
    </tr>
</table>

<br>
Then we make a program which use bilinear interpolation to downsample three images to 1/2, 1/4 and 1/8 respectively.Then, we make recovery from image which has downsample to 1/2, 1/4, 1/8 to original size.
<br>
<img src="/images/downsample_face.png" width=700>
<img src="/images/recovery_face.png" width=700>
<img src="/images/downsample_cameraman.png " width=700>
<img src="/images/recovery_cameraman.png" width=700>
<img src="/images/downsample_crowd.png" width=700>
<img src="/images/recovery_crowd.png" width=700>
<br>

4. **Conclusion**

- The more details a image has, the more blurred the image is after downsampling.
- The less size a image has, the more diffcult the image recover to the original size.
- The downsampling and recovery are not reversible. Although the new image is made by downsampling of the original image, the recovery of new image cannot completely result in the original iamge.

### (b) Please make a program to reduce the quantization level of an image to 1/2, 1/4 and 1/8 and compare the results with (a).

1. **Quantization**

- **Quantization** is the process of constraining an input from a continuous or otherwise large set of values (such as the real numbers) to a discrete set (such as the integers). 
- **Quantization**, involved in image processing, is a lossy compression technique achieved by compressing a range of values to a single quantum value. 

2. **Adopted Method**

- **Algorithm Description**  
  As usual, the value of gray level of an digital image equals to the integer power of 2, and 8 bits is most commonly used. Therefore, an image which is quantized to 256 levels is an 8 bits image. In this problem, what we intend to do is reduce the gray level of 8 bits image to 7 bits image, 6 bits image, and 5 bits, which respectively contains 128 gray levels, 64 gray levels, and 32 gray levels. On the condition, the algotithm is that every 2,4,8's gray level should be combined by one gray level.For example, in 1/2 reduction of quantization,0 level gray pixel and 1 gray pixel should both convert to 0 gray level pixel, which is deneoted by [0,1]->0.Therefore, for all of pixels in an image, we execute the same process: [0,1]->0,[2,3]->2,...,[244,255]->244. As result, an 8 bits image become a 7 bits image.
- **Algorithm Step**  
  The value of an image's bits is denoted $k$, and every pixel's value of intensity,or gray level, is restored as a binary number.Therefore, every 1/2 reduction of the quantization an image make, every 1 bit the binary number of all pixels shift left.

3. **Implementation**

- The followings are different images which have different $k$ values after reducing the quantization level of three orginal images.
  <img src="/images/down_quant_face.png" width=800>
  <img src="/images/down_quant_cameraman.png" width=800>
  <img src="/images/down_quant_crowd.png" width=800>

4. **Conclusion**

- The image named face has the least details; the image namde cameraman has middle details; theimage named crow has most abundant details. Observe that isopreference curves tend to become more vertical as the detail in the image increases. This result suggests that for
  images with a large amount of detail only a few intensity levels may be needed.<br><img src=/images/isopreference.png width=250><br>For example, the isopreference curve in figure corresponding to the crowd is nearly vertical.This indicates that, for a fixed value of N, the perceived quality for this type of image is nearly independent of the number of intensity levels used. Also, this conclusion can be proved in the section Implementation. As we can see, with the quantization changing, the change of image "face" is most significant after k=5, but the change of image "crowd" is unsignigicant except k=1.

## 2: Image Enhance and Denoising

### (a) Implement the histogram equalization technique discussed in Section 3.3.1 of Digital Image Processing 3rd by Gonzalez.

1. **Histogram Equalization**

- **Histogram equalization** is a method in image processing of contrast adjustment using the image's histogram. 
- **Histogram equalization** usually increases the global contrast of many images, especially when the usable data of the image is represented by close contrast values. Through this adjustment, the intensities can be better distributed on the histogram. This allows for areas of lower local contrast to gain a higher contrast. Histogram equalization accomplishes this by effectively spreading out the most frequent intensity values. 

2. **Adopted Method**

- **Algorithm Description**  
  For discrete values, we work with probabilities and summations instead of probability density functions and integrals (but the
  requirement of monotonicity stated earlier still applies). Recall that the probability of occurrence of intensity level in a digital image is
  approximated by<br><br><center>$$P_r(r_k)=\frac{n_k}{MN}$$</center><br>where $MN$is the total number of pixels in the image, and denotes the number of pixels that have intensity $r_k$.As noted in the
  beginning of this section, $p_r(r_k)$with $r_k\in[0,L-1]$is commonly referred to as a normalized image histogram.<br><br>The discrete form of the transformation is<br><center>$$s_k=T(r_k)=(L-1)\displaystyle \sum^{k}_{j=0}{p_r(r_j)},k=0,1,2,...,L-1$$</center><br>where, as before, $L$ is the number of possible intensity levels in the image (e.g., 256 for an 8-bit image). Thus, a processed (output)
  image is obtained by using this formula to map each pixel in the input image with intensity $r_k$ into a corresponding pixel with level $s_k$ in
  the output image, This is called a _histogram equalization_ or _histogram linearization transformation_. 
- **Algorithm Step**  
  - First,calculate the histogram of intensities of origial image.
  - Second, use the formula talked above for every pixel.
  - Third, repeat the first and second processes for R,G,B channels.

3. **Implementation**

- Original image is a dark image, and we show its histogram diagram.
- After histogram equalization, we show a lighter image and its histogram diagram.

<img src = /images/histogram_euqalization.png width=600>

4. **Conclusion**  

- We note in the dark image that the most populated histogram bins are concentrated on the lower (dark) end of the intensity scale.An image with low contrast has a narrow histogram located typically toward the middle of the intensity scale. Finally, we see that the components of the histogram of the high-contrast image cover a wide range of the intensity scale, and the distribution of pixels is not too far from uniform, with few bins being much higher than the others. Intuitively, it is reasonable to conclude that an image whose pixels tend to occupy the entire range of possible intensity levels and, in addition, tend to be distributed uniformly, will have an appearance of high contrast and will exhibit a large variety of gray tones. 

### (b) Please implement the sharpening spatial filters in Section 3.6 and apply to the image Spatial Filtering.png.

1. **Spatial Filter**  

- **Spatial filtering** is used in a broad spectrum of image processing applications
- The name **filter** is borrowed from frequency domain processing where “filtering” refers to passing, modifying, or rejecting specified frequency components of an image.For example, a filter that passes low frequencies is called a lowpass filter. The net effect produced by a lowpass filter is to smooth an image by blurring it. We can accomplish similar smoothing directly on the image itself by using spatial filters
- **Spatial filtering** modifies an image by replacing the value of each pixel by a function of the values of the pixel and its neighbors

2. **Adopted Method**:Laplacian Operator

- **Algorithm Description**  
  Because the Laplacian is a derivative operator, it highlights sharp intensity transitions in an image and de-emphasizes regions of slowly
  varying intensities. This will tend to produce images that have grayish edge lines and other discontinuities, all superimposed on a dark,
  featureless background. Background features can be “recovered” while still preserving the sharpening effect of the Laplacian by adding
  the Laplacian image to the original. As noted in the previous paragraph, it is important to keep in mind which definition of the Laplacian
  is used. If the definition used has a negative center coefficient, then we subtract the Laplacian image from the original to obtain a
  sharpened result. Thus, the basic way in which we use the Laplacian for image sharpening is <br><br><center>$$g(x,y)=f(x,y)+c[\nabla^2f(x,y)]$$</center><br>where f(x, y) and g(x, y) are the input and sharpened images, respectively. We let c = -1 if the Laplacian kernels in <br><img src=laplacian-1.png width=300><br> is used, and c = 1 if <br><img src=laplacian1.png width=300><br> is used.
- **Algorithm Step**  
  - First, set the kernel.  
  - Second, caculate the sum of products between every value in kernel and every nine values of intensity in original image.
  - Third, caculate the value of every pixel of the original image to get laplacian.
  - Forth, caculate the sum of original image and laplacian to get the new image.
  - Fifth, restrict the values of intensity of this new image between 0 and 255.

3. **Implementation**

<img src=/images/sharpening_image.png width=700>

4. **Conclusion**

- The detail in this image is unmistakably clearer and sharper than in the original image. Adding the Laplacian to the original image restored the overall intensity variations in the image. Adding the Laplacian increased the contrast at the locations of intensity discontinuities. The net result is an image in which small details were enhanced and the background tonality was reasonably preserved. 

### (c) Please apply frequency filtering in Section 5.2.4 and 5.4.2 to denoise the given image Frequency Filtering.png. Hint: you can use matlab image processing toolbox but you have to consider which functions should be applied to the image in order to make good recovery.

1. **Frequency Filtering**  
   Filtering techniques in the frequency domain are based on modifying the Fourier transform to achieve a specific objective, and then
   computing the inverse DFT to get us back to the spatial domain
2. **Adopted Method**  

- **Ideal Lowpass Filters**  
  A 2-D lowpass filter that passes without attenuation all frequencies within a circle of radius from the origin, and “cuts off” all frequencies outside this, circle is called an ideal lowpass filter (ILPF); it is specified by the transfer function<br><br><center>$$H(u,v)=\begin{cases}
  1,\quad if D(u,v)\leq D_0 \\\\
  0,\quad if D(u,v)>D_0
  \end{cases}$$</center><br>where is a positive constant, and $D(u,v)$ is the distance between a point $(u, v)$ in the frequency domain and the center of $P*Q$the
  frequency rectangle.
- **Gasussian Lowpass Filters**  
  we can express the Gaussian transfer function in the same notation as other functions in this section:<br><br><center>$$H(u,v)=e^{-D^2(u,v)/2D_0^2}$$</center><br>where $D_0$ is the cutoff frequency. When $D(u,v)=D_0$ the GLPF transfer function is down to 0.607 of its maximum value of 1.0
- **Median Filter**  
  Median Filter's kernel is <br><br><center>$$median\begin{bmatrix}1 & 1 & 1\\1 & 1 & 1\\1 & 1 & 1\end{bmatrix}$$</center><br>
- Algorithm Step
  - First, do FFT for the original image.
  - Second, do FFTshift to get the image of frequency domain to decide which filter should be used.
  - Third, make the filter in frequrncy in frequency domain.
  - Forth, do IFFTshift to get the image, and then do IFFT for it to get the denoising image.

3. **Implementation**

<br><center><img src=/images/denoise1.png width=600></center><br>

<br><center><img src=/images/denoise2.png width=600></center><br>

<br><center><img src=/images/denoise3.png width=600></center><br>

<br><center><img src=/images/denoise4.png width=600></center><br>

5. **Conclusion**

- In this case, for periodic noise, Gaussian Filter and Ideal Filter have similiar result.
- For periodic noise, although Median Filter can denoise the image, the processed image is also blurred.
- For frequency filtering, we should recognise which part represnts the noise and which filter should we use.
