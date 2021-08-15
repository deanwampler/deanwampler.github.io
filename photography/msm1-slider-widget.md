---
layout: page
style: no-image
title: Astrophotography with the MSM Star Tracker
tagline:
---

<script src='https://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js'></script>

<style>
*, *:before, *:after {
  box-sizing: inherit;
}
.msm1-slider-container {
  position: relative;
  width: 900px;
  height: 600px;
  border: 2px solid white;
  opacity: 1.0;
}
.msm1-slider-container .img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-size: 900px 100%;
}
.msm1-slider-container .background-img {
  background-image: url("https://deanwampler.github.io/assets/images/MilkyWayStacked.jpg");
}
.msm1-slider-container .foreground-img {
  background-image: url("https://deanwampler.github.io/assets/images/MilkyWayOneImage.jpg");
  width: 50%;
}
.msm1-slider-container .slider {
  position: absolute;
  -webkit-appearance: none;
  appearance: none;
  width: 100%;
  height: 100%;
  background: rgba(242, 242, 242, 0.0);
  outline: none;
  margin: 0;
  transition: all 0.2s;
  display: flex;
  justify-content: center;
  align-items: center;
}
.msm1-slider-container .slider:hover {
  background: rgba(242, 242, 242, 0.0);
}
.msm1-slider-container .slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 6px;
  height: 600px;
  background: white;
  cursor: pointer;
}
.msm1-slider-container .slider::-moz-range-thumb {
  width: 6px;
  height: 600px;
  background: white;
  cursor: pointer;
}
.msm1-slider-container .slider-button {
  pointer-events: none;
  position: absolute;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background-color: white;
  left: calc(50% - 18px);
  top: calc(50% - 18px);
  display: flex;
  justify-content: center;
  align-items: center;
}
.msm1-slider-container .slider-button:after {
  content: "";
  padding: 3px;
  display: inline-block;
  border: solid #5D5D5D;
  border-width: 0 2px 2px 0;
  transform: rotate(-45deg);
}
.msm1-slider-container .slider-button:before {
  content: "";
  padding: 3px;
  display: inline-block;
  border: solid #5D5D5D;
  border-width: 0 2px 2px 0;
  transform: rotate(135deg);
}
</style>

This widget compares a single exposure vs. 10 exposures stacked together. All exposures were taken with the Sony A7RIV and the Sony 24mm F1.4 GM lens, F1.4, 15 seconds, ISO 800. The stacked image was created using [Starry Sky Stacker](https://sites.google.com/site/starryskystacker/home). My camera was mounted on a [Move Shoot Move Star Tracker](https://www.moveshootmove.com/collections/sifo-rotator/products/sifo-rotator-for-star-tracking-time-lapse-panorama-photography).

See my Medium blog post for more details.

<div class='msm1-slider-container opaque'>
  <div class='img background-img'></div>
  <div class='img foreground-img'></div>
  <input type="range" min="1" max="100" value="50" class="slider" name='slider' id="slider">
  <div class='slider-button'></div>

  <script id="rendered-js" >
    $("#slider").on("input change", (e)=>{
      const sliderPos = e.target.value;
      // Update the width of the foreground image
      $('.foreground-img').css('width', `${sliderPos}%`)
      // Update the position of the slider button
      $('.slider-button').css('left', `calc(${sliderPos}% - 18px)`)
    });
  </script>
</div>

Credits:

* Slider widget adapted from [here](https://levelup.gitconnected.com/how-to-create-a-before-after-image-slider-with-css-and-js-a609d9ba77bf)
* Images, &copy; 2021, Dean Wampler, All Rights Reserved. You can find them on Flickr:
    - [Single Image](https://www.flickr.com/photos/deanwampler/51350918396/in/album-72157719618117355/)
    - [Stacked Image](https://www.flickr.com/photos/deanwampler/51351936525/in/album-72157719618117355/)
    

  
