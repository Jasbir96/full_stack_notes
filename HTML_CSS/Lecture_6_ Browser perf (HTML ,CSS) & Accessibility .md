# Lecture_HTML/CSS-6: Browser perf (HTML ,CSS)

---

## Agenda
* Performance and perceived performance
	* Lighthouse
	* 3 webvitals
* Compression vs minification vs uglification
* Multimedia Performance
	* Srcset attribute in image tag
	* Lazy attribute




**Asking Question:**
What do you understand by the term performance?


## Performance:
* Load Time is Less
* Responsive to Use
* Scalable
* Reliable

## Perceived Performance
* Reliable
* Responsive
* load fast

**Asking Question:**
Can you tell me when a user thinks an application is fast but truly its not?
**Ans:** Loading just the skeleton UI, loader, progress bar make the user less worry about the performance. 

# peformance  Metrics
There are a few things that can't be visually observed in a webpage but are important. They are:
* **Performance:** How fast is the website load time and how quick are the interactions
* **Accessibility:** Features that make a website usable (like assistive devices, it's easy for people with disabilities to use as well)
* **Best Practice in Coding:** Readability, a good naming convention, Code Quality, Code Coverage (How much lines of code actually contribute to the actual website, there should be no code present that is not being used in the website) 
* **SEO:** Search Engine Optimization

To observe these parameters in a website, go on to inspect webpage, and to the Lighthouse tab.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/308/original/upload_6e90fabbbaf90c57d3af16a547be177c.png?1695150223)

As the performance is not good, the above website is not really usable. Our focus should be on getting as better as possible in all these 4 metrics.


## core vitals

**Performance Metrics also known as Web Vitals**
There are three metrics on Google by which if a web page is performant or not.

Let's consider with the example of an webpage,

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/016/original/upload_1d4d2e0f8221393a552f3b3d2319b384.png?1696310902)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/017/original/upload_cb0e12ed3b8994b0dd4a8eeb3fbdbe53.png?1696310925)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/018/original/upload_6a06a6c4acd330885dc23c54f531c09c.png?1696310970)

Lets discuss the 3-core web vitals in details:
* User interactivity
* Search ranking increases

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/019/original/upload_4fb49b7de5ca484c335f98f7ce79e83a.png?1696310999)

**LCP:** measures the time from when a user first interacts with a page (that is, when they click a link, tap on a button, or use a custom, JavaScript- powered control) to the time when the browser is actually able to begin processing event handlers in response to that interaction.

**What elements are the candidates for LCP?**
* `<img>` elements
* `<image>` elements inside an `<svg>` element.
* `<video>` elements with a poster image(the poster image load time is used.)
* background image
* block level element

**Cumulative Layout Shift (CLS)**    

CLS is a way to measure and quantify how much various pieces of content (text, images, ads, video, etc.) jumps around as the page loads. 
**Note**: Just add height

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/020/original/upload_348a28fa70ee55f488c8fd59822c6839.png?1696311030)
    
Let's take an example as follows:

```html
<!DOCTYPE html>
<html>

<head>
    <style>
        /* CSS to hide the image initially */
        .hidden-image {
            opacity: 0;
            transition: opacity 0.5s;
        }

        /* CSS to show the image abruptly */
        .visible-image {
            opacity: 1;
        }
    </style>
    <script>
        window.addEventListener('DOMContentLoaded', (event) => {
            // Show the image abruptly after a delay
            setTimeout(() => {
                document.querySelector('.hidden-image').classList.add('visible-image');
            }, 2000);
        });
    </script>
</head>

<body>
    <h1>Website with Cumulative Layout Shift (CLS) Example</h1>
    <img src="https://source.unsplash.com/random/300" alt = "Example Image" class = "hidden-image"
    height = "600">
    <div class = "shifting-element">
        <h1>This text appears without layout shift.
Lorem ipsum dolor sit, amet consectetur adipisicing elit. Ullam, delectus quis incidunt itaque hic nam voluptate exercitationem in sequi esse expedita ipsum minus et officiis vel, sint odio laudantium est.
        </h1>
    </div>
</body>

</html>
```

**As the image comes the text move to the below:**

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/021/original/upload_c53bb3a7355d90e931ecf8748085faae.png?1696311085)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/022/original/upload_faa0ef496b4deb9d5d5faf68dcdcd18f.png?1696311103)

**Asking Question:**
How to manage the layout shift?
**Ans:** By giving the height for the upcoming image we can manage the layout shift.

**First Input Delay (FID):** measures the time from when a user first interacts with a page (that is, when they click a link, tap on a button, or use a custom, JavaScript-powered control) to the time when the browser is actually able to begin processing event handlers in response to that interaction.

FID measures the interactivity and value should be less than 100ms.

## compression example

Let's see how a basic compression effects your application:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/023/original/upload_977c08a77ea1d8e975337e8ab6f0bc6e.png?1696311127)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/024/original/upload_698740bc5156eda75ebc5ba4a6413c93.png?1696311150)

*To show such a higher size if image is not a good parctise. We can use a compresses version of the image.*

## minification example

- **Minification:** Removing unnecessary characters like whitespace and comments from files (e.g., JavaScript, CSS, HTML) to reduce their file sizes and improve loading times.
- Most frameworks include a minification feature that reduces file sizes.
- However, when working with vanilla HTML and CSS, we can use extensions like "minifyall" to remove unnecessary white spaces.
- These spaces are primarily for human readability and don't affect how browsers or compilers interpret code.
- Minifying code is essential when preparing it for production.
- The first step in this process is eliminating white spaces, directly reducing file size.
- This reduction in file size can have a significant impact on network performance and loading times.

As shown below the style.css has size 4.9 kb and after the minification it reduces to 3.6kb.

Extension is (**MINIFYALL**):

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/025/original/upload_26dba67f9877321b0fd0c708a9723112.png?1696311183)



![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/026/original/upload_cca71097ca7ed9ca1a69436e5679adb3.png?1696311206)



![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/027/original/upload_6e601f4e6195e1c5b1b97d75a8342ed9.png?1696311235)

Usually these things are done by the framework, but we should have a knowledge about these things.

* **Minification:** remove white space from files ex : js , css,html
* **Ugilification:** changing name of functions and variable to minimise character used
* **Compression:** reducing size of assets using a compression techique, ex: compressing assets like img,video
    - Generally either gzip or brotli for compression
    - CDN(Content Delivery Network):
        - serve our assets
        - apply compression

***Ideal workflow for a file involves minificatication->uglification->compression***



**Images:**
* Multiple screen sizes and multiple type of images
* Usually Bandwidth of Web Users > Tab Users > Mobile Users.

**Asking Question:**
Is it sensible to send the same density or quality image to all types of users?
**Ans:** There is inbuilt way in HTML called srcset to solve the above problem.

* High pixel dense screen and low pixel dense screen so to solve these problems we have srcset tool with us.

* Lets take an example as shown below of three images of large, medium and small size:

```html
<!-- it will help in saving bandwidth -->
<!DOCTYPE html>
<html lang = "en">

<head>
  <meta charset = "UTF - 8">
  <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
  <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
  <title>Document</title>
</head>
<style>
  img {
    width: 100%;
  }
</style>

<body>
  <h1>Responsive Image Example</h1>
  <!-- browser -> choice -> decide when to load which image
  easily -> bandwidth
  -->
  <img src = "./Capture.jpg" alt = ""
  srcset = "
  ./Capture_large.jpg 3x,
  ./Capture_medium.jpg 2x,
  ./Capture_small.jpg 1x"
  >

  <img src = "./Capture.jpg" alt = "" srcset = "
    ./Capture_large.jpg 1200w,
    ./Capture_medium.jpg 900w,
    ./Capture_small.jpg 400w">
</body>

</html>
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/028/original/upload_46d0dd69ed59837d13968a43d4b5d85a.png?1696311384)


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/029/original/upload_bd8b48526d3a3e001409924b993b4ba0.png?1696311413)



![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/030/original/upload_f33832abcdfbe59270ec99b6c1ecca6b.png?1696311485)

*As the DPR changes the appropriate image gets loaded using srcset.*

**Asking Question:**
Can you explain me when you should use the plain image and when the jpg image?
**Ans:** Png is usually for the transparent images and the jpg for the not transparent, and the webp is the most compressed.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/032/original/upload_f7764b77833aa8172f6e38760ba173ab.png?1696311536)

Lets undrestand this using an example as follows:

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
</head>
<style>
    img {
        width: 100%
    }
</style>

<body>
    <h1>Picture Tag Example</h1>
    <!-- 2 sources 

        
1-> fallback -> img-->
    <picture>
        <source srcset = "Capture.webp
        " type = "image/webp">
        <source srcset = "Capture_small.jpg 1x,
              Capture_medium.jpg  2x,
              Capture_large.jpg  3x">
        <img src = "Capture.jpg" alt = "Example Image" />

    </picture>
</body>

</html>
<!-- half -> webpage -> images  -->
```

There are two sources and one fallback, try to get th ewebp image if not loaded then get from the other three based on the size, or if this is not possible then try to get the default image.

**Asking Question:**
Lets say you are building an image gallery, being a web developer can you tell me when a user visit your screen how many images you should load?

**Ans:** You should only load the visible image and other should be lazy loaded. 

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/033/original/upload_c226587b2c1ba0f7aa3ca33c5d302f45.png?1696311654)

you can go the below example for this:
```html
<!DOCTYPE html>
<html>
<style>
    h1 {
        text-align: center;
    }

    img{
        display: block;;
    }
</style>

<body>
    <h1>Image Gallery</h1>
    <h2>Lorem ipsum dolor sit amet consectetur adipisicing elit. Accusantium, voluptatibus nesciunt eligendi molestiae ipsum neque debitis temporibus iure, eos, perferendis expedita velit beatae repellat hic saepe ea modi aspernatur maiores? Sit animi et corrupti repellat non nemo quaerat blanditiis incidunt id consectetur pariatur distinctio, possimus doloremque excepturi totam accusamus tempora. Perspiciatis quidem ipsam explicabo nemo architecto placeat a consequatur natus nulla illum id, quia ipsum vitae repellendus harum maiores labore molestiae illo nostrum, velit numquam voluptates commodi officiis consectetur! Dolor quia officiis placeat consequatur dolorem deleniti non ducimus facilis, deserunt veritatis veniam doloremque debitis cupiditate tempore. Delectus, maiores eaque! Culpa labore ea, quis cupiditate consequuntur recusandae, ipsa expedita placeat amet ad est facilis temporibus aliquid optio repudiandae aspernatur. Quae vitae optio dolorum consectetur tempora ea error commodi nobis quibusdam beatae voluptatibus doloribus iusto, voluptatem nam, repellat minima molestias, suscipit eaque qui iure odio quasi atque. Quaerat aliquam maxime ad recusandae dolor officiis. Nam, reprehenderit! Officiis quidem placeat quos labore eaque? Distinctio deserunt quasi aut molestiae ratione, laboriosam hic illo quo sunt? Totam maiores perspiciatis eaque eveniet, sed suscipit debitis asperiores, accusantium eos sequi quidem. At, voluptate minus magni recusandae officia corporis soluta provident culpa ullam, distinctio laudantium tempora vero. Tempore temporibus id facilis pariatur. Unde aliquam non corporis fugit sed porro ab dolores placeat. Hic natus molestias commodi pariatur nulla dolor assumenda deleniti nemo laborum consectetur beatae voluptatibus dignissimos soluta expedita, obcaecati placeat earum nesciunt cum maxime nam id ratione impedit suscipit cumque? Nostrum ut voluptate consequatur libero non quae quibusdam. Numquam facere aperiam exercitationem laborum similique itaque sed eum, labore repudiandae laboriosam sit, debitis corrupti repellat excepturi nulla voluptate nisi harum atque aliquam non fugiat facilis sint? Nostrum magni dolorum tempore exercitationem atque architecto officia odio ipsa accusantium quam cum facilis voluptas inventore sint, velit ut quasi deleniti aliquam!</h2>
    
       <h2>Inview</h2>
            <img src = "https://source.unsplash.com/random/600" alt = "Image 1"  height = "300">
            <img src = "https://source.unsplash.com/random/500" alt = "Image 2"  height = "300">
            <img src = "https://source.unsplash.com/random/200" alt = "Image 3"  height = "300">
            <img src = "https://source.unsplash.com/random/250" alt = "Image 3"  height = "300">
            <img src = "https://source.unsplash.com/random/300" alt = "Image 4"  height = "300">
     
       <!-- lazy loaded Images  reduce bandwidth-->
            <img src = "https://source.unsplash.com/random/301" alt = "Image 5" 
             loading = "lazy" height = "300">
            <img src = "https://source.unsplash.com/random/302" alt = "Image 6"  
            loading = "lazy" height = "300">
            <img src = "https://source.unsplash.com/random/303" alt = "Image 7" 
             loading = "lazy" height = "300">
            <img src = "https://source.unsplash.com/random/304" alt = "Image 8"  
            loading = "lazy" height = "300">
    
    </div>
</body>

</html>
```
**Explanation:**
This HTML code snippet displays a set of images within an "Inview" section. 
1. Section Title: The section is titled "Inview" with an `<h2>` heading.
2. Images: There are eight images in total, each with different source URLs and the `height` attribute set to 300 pixels.
3. Lazy Loading: The last four images utilize the `loading="lazy"` attribute, which defers their loading until they enter the user's viewport, saving bandwidth.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/034/original/upload_893c01d211fa28256e1a40a54d164189.png?1696311788)

---
