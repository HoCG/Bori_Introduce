<template>
  <div id="carousel_container">
    <div class="carousel_wrap">
      <div class="carousel_box">
        <div class="carousel_list clearfix">
          <div class="carousel_content carousel01">
            <p>1</p>
          </div>
          <div class="carousel_content carousel02">
            <p>2</p>
          </div>
          <div class="carousel_content carousel03">
            <p>3</p>
          </div>
          <div class="carousel_content carousel04">
            <p>4</p>
          </div>
          <div class="carousel_content carousel05">
            <p>5</p>
          </div>
        </div>
        <!-- // .carousel_list -->
      </div>
      <!-- // .carousel_box -->
      <div class="carousel_btn_box">
        <button type="button" class="carousel_btn_prev">Prev</button>
        <button type="button" class="carousel_btn_next">Next</button>
      </div>
      <!-- // .carousel_btn_box -->
      <ul class="carousel_pagination"></ul>
      <!-- // .carousel_pagination -->
    </div>
    <!-- // .carousel_wrap -->
  </div>
  <!-- // .container -->
</template>
<script>  // carousel parent dom

export default {
  mounted() {
    const carouselList = document.querySelector('.carousel_list');
    const carouselContents = document.querySelectorAll('.carousel_content');  // each carousel dom
    const carouselBtnNext = document.querySelector('.carousel_btn_next'); // next button
    const carouselBtnPrev = document.querySelector('.carousel_btn_prev'); // prev button
    const pagination = document.querySelector('.carousel_pagination');
    const carouselLen = carouselContents.length;  // carousel length
    const carouselWidth = 400; // carousel width
    const carouselSpeed = 300; // carousel speed
    const startNum = 0; // initial carousel index (0 ~ 4)
    carouselList.style.width = carouselWidth * (carouselLen + 2) + "px";
    // Copy first and last carousel
    let firstChild = carouselList.firstElementChild;
    let lastChild = carouselList.lastElementChild;
    let clonedFirst = firstChild.cloneNode(true);
    let clonedLast = lastChild.cloneNode(true);

    // Add copied carousels
    carouselList.appendChild(clonedFirst);
    carouselList.insertBefore(clonedLast, carouselList.firstElementChild);
        // Add pagination dynamically
    let pageChild = '';
    for (var i = 0; i < carouselLen; i++) {
      pageChild += '<li class="dot';
      pageChild += (i === startNum) ? ' dot_active' : '';
      pageChild += '" data-index="' + i + '"><a href="#"></a></li>';
    }
    pagination.innerHTML = pageChild;
    const pageDots = document.querySelectorAll('.dot'); // each dot from pagination

    carouselList.style.transform = "translate3d(-" + (carouselWidth * (startNum + 1)) + "px, 0px, 0px)";

    let curIndex = startNum; // current carousel index (except copied carousel)
    let curCarousel = carouselContents[curIndex]; // current carousel dom
    curCarousel.classList.add('carousel_active');
    /** Next Button Event */
    carouselBtnNext.addEventListener('click', function() {
      if (curIndex <= carouselLen - 1) {
        carouselList.style.transition = carouselSpeed + "ms";
        carouselList.style.transform = "translate3d(-" + (carouselWidth * (curIndex + 2)) + "px, 0px, 0px)";
      }
      if (curIndex === carouselLen - 1) {
        setTimeout(function() {
          carouselList.style.transition = "0ms";
          carouselList.style.transform = "translate3d(-" + carouselWidth + "px, 0px, 0px)";
        }, carouselSpeed);
        curIndex = -1;
      }
      curCarousel.classList.remove('carousel_active');
      pageDots[(curIndex === -1) ? carouselLen - 1 : curIndex].classList.remove('dot_active');
      curCarousel = carouselContents[++curIndex];
      curCarousel.classList.add('carousel_active');
      pageDots[curIndex].classList.add('dot_active');
    });

    /** Prev Button Event */
    carouselBtnPrev.addEventListener('click', function() {
      if (curIndex >= 0) {
        carouselList.style.transition = carouselSpeed + "ms";
        carouselList.style.transform = "translate3d(-" + (carouselWidth * curIndex) + "px, 0px, 0px)";
      }
      if (curIndex === 0) {
        setTimeout(function() {
          carouselList.style.transition = "0ms";
          carouselList.style.transform = "translate3d(-" + (carouselWidth * carouselLen) + "px, 0px, 0px)";
        }, carouselSpeed);
        curIndex = carouselLen;
      }
      curCarousel.classList.remove('carousel_active');
      pageDots[(curIndex === carouselLen) ? 0 : curIndex].classList.remove('dot_active');
      curCarousel = carouselContents[--curIndex];
      curCarousel.classList.add('carousel_active');
      pageDots[curIndex].classList.add('dot_active');
    });

    /** Pagination Button Event */
    let curDot;
    Array.prototype.forEach.call(pageDots, function (dot) {
      dot.addEventListener('click', function (e) {
        e.preventDefault();
        curDot = document.querySelector('.dot_active');
        curDot.classList.remove('dot_active');
        
        curDot = this;
        this.classList.add('dot_active');

        curCarousel.classList.remove('carousel_active');
        curIndex = Number(this.getAttribute('data-index'));
        curCarousel = carouselContents[curIndex];
        curCarousel.classList.add('carousel_active');
        carouselList.style.transition = carouselSpeed + "ms";
        carouselList.style.transform = "translate3d(-" + (carouselWidth * (curIndex + 1)) + "px, 0px, 0px)";
      });
    });
  }
}

</script>
<style>
  html, body { box-sizing: border-box; padding: 0; margin: 0; text-align: center; }
  *, *:before, *:after { box-sizing: inherit; }
  .clearfix:after { content: ''; display: block; clear: both; float: none; }
  .title { margin-bottom: 0; text-align: center; font-size: 30px; color: #333; }
  .link, .link:visited { display: inline-block; margin: 20px 0; color: #555; text-decoration: none; font-weight: bold; }
  .link:hover, .link:focus { color: #9fd6c2; }
  /* container - body */
  #container { width: 1000px; margin: auto; }
  .carousel_wrap { position: relative; width: 400px; margin: auto; padding-bottom: 30px; }
  .carousel_box { width: 100%; margin: auto; overflow-x: hidden; }
  .carousel_content { display: table; float: left; width: 400px; height: 400px; }
  .carousel_content > p { display: table-cell; vertical-align: middle; text-align: center; font-size: 100px; font-weight: bold; color: #555; }
  .carousel_content.carousel01 { background: #ddbdff; }
  .carousel_content.carousel02 { background: #9fd6c2; }
  .carousel_content.carousel03 { background: #abe2f7; }
  .carousel_content.carousel04 { background: #f08c78; }
  .carousel_content.carousel05 { background: #fbdb65; }
  .carousel_btn_box > button { position: absolute; top: 50%; margin-top: -45px; width: 60px; height: 60px; font-size: 16px; color: #999; background: none; border: 1px solid #ddd; cursor: pointer; }
  .carousel_btn_box > .carousel_btn_prev { left: -100px; }
  .carousel_btn_box > .carousel_btn_next { right: -100px; }
  .carousel_pagination { position: absolute; left: 50%; bottom: 0; list-style: none; margin: 0; padding: 0; transform: translateX(-50%); }
  .carousel_pagination .dot { display: inline-block; width: 15px; height: 15px; margin: 0 5px; overflow: hidden; background: #ddd; border-radius: 50%; transition: 0.3s; }
  .carousel_pagination .dot.dot_active { background: #333; }
  .carousel_pagination .dot a { display: block; width: 100%; height: 100%; }
</style>