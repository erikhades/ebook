Copyright© Anshul 2019

This book is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0) license.
##  Preface
A fast site is a good user experience[UX] and a satisfying UX leads to higher conversion rates.
People like fast sites. Loading time has been a major contributing factor to page abandonment.
Along the way i will provide sample code and quick tutorial wherever possible and explanations as detailed as possible. 
This book at core focuses on improving quality of production builds.

This book tries to showcases real world trends and practices to improve the performance of website and webapps.
## Target Audience
This book is targeted to professional senior developers wishing to improve the performance of their website and webapps. I'll try my best to keep everything simple and cover as many aspects as possible.
##  Prerequisite
Whilst this book is targeted at both beginners and senior developers, a basic understanding of JavaScript fundamentals is assumed. I will try to provide links wherever possible for advance topics.
## Note To Readers
I am neither affilated or endorsed by any organisation or entity to promote their services or products. Their may be better alternatives available that i might not know, i always welcome feedbacks and suggestions.
All Chapter are mostly independent and you can start from whichever you find most interesting.
I intend to publish all my books for free digitally, surely i would appreciate if you help me buy coffee.

* * *

# Table Of Contents
* Introduction
* Chapter 1:  Images
* Chapter 2:  Javascript


* * *

# Introduction
The current web has evolved a lot in past decade from viewing static site to playing AAA 3D games in browser. Over 4.33 billion people were active internet users as of July 2019. In this over growing economy website visibility and high conversion rates is highly important. Search engine like Google among various other criteria also rank websites on the basis of site speed. In the study "The Need for Mobile Speed" it was found that 53% of mobile site visits are abandoned if the page took longer than 3 seconds to load. Another research showed a 2 second delay during transaction resulted in shopping cart abandonment rates upto 87% and 79% shoppers dissatisfied with website performance are less likely to buy from the same site again.

A 1 second delay in page response can result in a 7% reduction in conversion [ If an e-commerce site is making $100K per day, a mere 1 second could potentially cost $2,500,000 in lost sales every year]. So there is a constant need in making the web faster and easier accessible to everyone.

# Images
A picture is worth thousand of words. According to the [HTTP Archive](https://httparchive.org), 60% of the data transferred to fetch a web page is images composed of JPEGs, PNGs and GIFs. As of July 2017, images accounted approximately 0.45MB of the content loaded for every 1.0MB average site.

Image optimization consists of:
* Choosing Right Format
* Proper Resizing
* Image Compression
* Lazy Loading
* Priority Fetching and Caching

[Chrome Lighthouse](https://developers.google.com/web/tools/lighthouse) and [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/) are few tools that will help in auditing the website for best performance practices.

##  Choosing Right Format
Larger file size have more information but that information may simply be noises or undetectable to human eye.

**Remember: Higher file size doesn't always imply higher image quality.**
### Raster vs Vector Graphics
**Raster Graphics** represent images by encoding the values of each pixel within a rectangular grid of pixels. These are used where photorealism is necessary. Example: JPEG or PNG

**Vector Graphics** represent images made using points, lines and polygons offering high resolution and zoom independency. Example: [SVG](https://en.wikipedia.org/wiki/Scalable_Vector_Graphics)

#### JPEG:
JPEG originated in 1992 and trace its footsteps in phones, digital cameras and webcams. 45% of the images seen on sites crawled by HTTP Archive are JPEG. JPEG is a lossy compression algorithm that discard information in order to save space.
#### JPEG Compression Modes:
**Baseline[Sequential] JPEG**:  Users see the top of image with more of it revealed as the image loads.

**Progressive JPEG[PJPEG]**:  Progressive JPEGs divide given image into number of scans. First scan showing blurry or low quality image. Each scan of an image progressively adds an increasing level of detail.

**Lossless**: It can be achieved by optimizing an image's [Huffman tables](https://en.wikipedia.org/wiki/Huffman_coding) or removing [EXIF data](https://en.wikipedia.org/wiki/Exif) added by digital cameras. [Jpegoptim](https://github.com/tjko/jpegoptim) and [Mozjpeg](https://github.com/mozilla/mozjpeg) are few tools which support lossless JPEG compression.
####  Progressive JPEGs
Currently these are widely being used in the industry. So i decided to cover it specially.
PJPEGs allows users to see roughly what an image is when only part of the file is received, allowing them to make a call on whether to wait for it to fully load.

PJPEGs have higher compression as compared to baseline JPEGs, consuming 3-10% less bandwidth for images over 10KB. This is due to the fact that in baseline JPEGs blocks are encoded one at a time, whereas in PJPEGs the [Discrete Cosine Transform coefficients](https://en.wikipedia.org/wiki/Discrete_cosine_transform) across more than one block can be encoded together and the option of each scan having its own dedicated optional Huffman table leads to better compression.

**PJPEGs in Production**
* **[Medium](https://jmperezperez.com/medium-image-progressive-loading-placeholder)**: The popular publishing platform use PJPEGs to load images as they're scrolled into the viewport.
* [**Facebook**](https://engineering.fb.com/ios/faster-photos-in-facebook-for-ios): They used PJPEGs in their iOS app which resulted in data usage reduced by 10% and image load time 15% faster.
* **[Yelp](https://engineeringblog.yelp.com/2017/06/making-photos-smaller.html)**:  It resulted in 4.5% of their image size reduction.
* **Pinterest**:  Image heavy social network uses an infinte [masonry grid](https://masonry.desandro.com) to hold images and progressive loading technique to load their images. A placeholder with dominant color is initially used for each pin.
* **Twitter**:  They use it with a baseline quality of 85%.

**Disadvantages Of PJPEGs**

PJPEGs are slower to decode than baseline JPEGs raising a concern on lower end mobile devices with limited resources. For smaller images PJPEGs  can be larger than their baseline counterparts.

For some users PJPEGs can be disadvantageous as it can become hard to tell when an image is completely loaded.

**Creating PJPEGs**

[ImageMagick](https://imagemagick.org/index.php) and [imagemin](https://github.com/imagemin/imagemin) supports exporting Progressive JPEGs.
```
//  Using imagemin 
const gulp = require('gulp');
const imagemin = require('gulp-imagemin');

gulp.task('images', function () {
    return gulp.src('images/*.jpg')
        .pipe(imagemin({
            progressive: true
        }))
        .pipe(gulp.dest('dist'));       
});
```
