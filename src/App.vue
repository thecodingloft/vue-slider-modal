<template>

  <section class="course__slider">

    <h1 class="slider__heading">Browse Our Courses</h1>

    <!-- Category: International Cuisine -->
    <div class="course__category">
      <h1 class="category__heading">International Cuisine</h1>
    </div>
    <div class="category__slider">
      <div class="slider__container">

          <div class="inner" ref="inner">
            <transition-group tag="ul" name="list" appear mode="out-in">
              <li class="course__slide list-item" v-for="course in coursesInternational" :key="course.id">
                <div class="list" @click='openModal(course)'>
                  <img v-bind:src=course.image v-bind:alt=course.name>
                </div>
              </li>
            </transition-group>

          </div>
        
          <button class="slider__button btn--custom btn--white prev " @click="changeSlideInternational(-1)">&#10094;</button>
          <button class="slider__button btn--custom btn--white next" @click="changeSlideInternational(1)">&#10095;</button>
      
      </div>
    </div>

    <!-- Category: Pastry & Baking -->
    <div class="course__category">
      <h1 class="category__heading">Pastry & Baking</h1>
    </div>
    <div class="category__slider">
      <div class="slider__container">

          <div class="inner" ref="inner">
            <transition-group tag="ul" name="list" appear mode="out-in">
              <li class="course__slide list-item" v-for="course in coursesPastry" :key="course.id">
                <div class="list" @click='openModal(course)'>
                  <img v-bind:src=course.image v-bind:alt=course.name>
                </div>
              </li>
            </transition-group>

          </div>
        
          <button class="slider__button btn--custom btn--white prev " @click="changeSlidePastry(-1)">&#10094;</button>
          <button class="slider__button btn--custom btn--white next" @click="changeSlidePastry(1)">&#10095;</button>
      
      </div>
    </div>


    <!-- Category: Specialty Courses -->
    <div class="course__category">
      <h1 class="category__heading">Special Topics</h1>
    </div>
    <div class="category__slider">
      <div class="slider__container">

          <div class="inner" ref="inner">
            <transition-group tag="ul" name="list" appear mode="out-in">
              <li class="course__slide list-item" v-for="course in coursesSpecialty" :key="course.id">
                <div class="list" @click='openModal(course)'>
                  <img v-bind:src=course.image v-bind:alt=course.name>
                </div>
              </li>
            </transition-group>

          </div>
        
          <button class="slider__button btn--custom btn--white prev " @click="changeSlideSpecialty(-1)">&#10094;</button>
          <button class="slider__button btn--custom btn--white next" @click="changeSlideSpecialty(1)">&#10095;</button>
      
      </div>
      

    </div>

   
    <!-- Modal Box -->
    <div class="modal__wrapper">
      <div class="modal__box">
      <a href="#" @click.prevent='closeModal'>X</a>
      <img class="modal__image" v-bind:src=selectedCourseImage alt="">
      <h1> {{ selectedCourseName }}</h1>
      <h4 class="italic"> {{selectedCourseDescription}} </h4>
      <p class="bold primary">Difficulty</p>
      <p class="italic"> {{selectedCourseDifficulty}} </p>
      <p class="bold primary">Recipes included </p>
      <p class="italic"> {{ selectedCourseRecipes }}</p>
      <button class="btn--custom btn--primary" href="">Sign Up Now</button>
      </div>
    </div>
    
  </section>

</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      scrollStart: 0,
      showModal: false,
      selectedCourse: {},
      selectedCourseImage: "",
      selectedCourseName: "",
      selectedCourseDifficulty: "",
      selectedCourseDescription: "",
      selectedCourseRecipes: "",
      currentSlide: 0,
      cardWidth: 350,
      innerStyles: {},
      step: '',
      courses: [],
      coursesInternational: [],
      coursesPastry: [],
      coursesSpecialty: [],
        }
      },

  async mounted () {

    await fetch('https://gist.githubusercontent.com/veecoco/0653f11aac43c1dc61c0fada39517545/raw/2593b138111b2b962db6c2f03bbf0518a143cc8e/course-info.json')
      .then(res => res.json())
      .then(data => {
        this.coursesInternational = data.filter((course) => {
          return course.category === 'international'
        })
        this.coursesPastry = data.filter((course) => {
          return course.category === 'pastry'
        })
        this.coursesSpecialty = data.filter((course) => {
          return course.category === 'specialty'
        })
        })
      .catch(err => console.log(err.message))

  },
  
  methods: {
    openModal: function(course) {

      const courseSlider = document.querySelector('.course__slider')

      // set the course slider width to 100vh to prevent moving
      courseSlider.style.width = '100vw'

      var modalWrapper = document.querySelector('.modal__wrapper')
      modalWrapper.style.visibility = 'visible'
      
      // dynamically fill the properties shown in the modal
      this.selectedCourse = course
      this.selectedCourseImage = course.image
      this.selectedCourseName = course.name
      this.selectedCourseDescription = course.description
      this.selectedCourseDifficulty = course.difficulty
      this.selectedCourseRecipes = course.recipes

    // closing the window when clicking outside
    window.onclick = function(event) {
      if(event.target.className == 'modal__wrapper') {
        modalWrapper.style.visibility = 'hidden'
      }
    }

    },
    
    closeModal: function() {
      
      var modalWrapper = document.querySelector('.modal__wrapper')
        modalWrapper.style.visibility = 'hidden'
    },

    setStep: function() {
      var innerContainers
      var widthStep 
      // move the inner container to the left so the first slide moves left, too
      innerContainers = document.querySelectorAll(".inner")
      widthStep = this.cardWidth * (-1.5)
      innerContainers.forEach((container) => {
        container.style.transform = `translateX(${widthStep}px)`
      })
    },
    changeSlideInternational: function(n) {
      console.log(this.coursesInt, this.courses)

      if (n === 1) {

        var addedSlide = this.coursesInternational.shift()
        setTimeout(() => this.coursesInternational.push(addedSlide), 100)
        
      }
      
      if (n === -1) {

        var addedSlide = this.coursesInternational.pop()
        setTimeout(() => this.coursesInternational.unshift(addedSlide))

      }
    },
    changeSlidePastry: function(n) {

      if (n === 1) {

        var addedSlide = this.coursesPastry.shift()
        setTimeout(() => this.coursesPastry.push(addedSlide), 100)
        
      }
      
      if (n === -1) {

        var addedSlide = this.coursesPastry.pop()
        setTimeout(() => this.coursesPastry.unshift(addedSlide))

      }
    },
    changeSlideSpecialty: function(n) {

      if (n === 1) {

        var addedSlide = this.coursesSpecialty.shift()
        setTimeout(() => this.coursesSpecialty.push(addedSlide), 100)
        
      }
      
      if (n === -1) {

        var addedSlide = this.coursesSpecialty.pop()
        setTimeout(() => this.coursesSpecialty.unshift(addedSlide))

      }
    }
  },
}


</script>

<style>
body {
  padding: 0;
  margin: 0;
}

#app {
  font-family: 'Open Sans', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  padding: 0;
  margin: 0;
}

.bold {
  font-weight: bold;
}

.italic {
  font-style: italic;
}

.primary {
  color: #de8311;
}

.btn--custom {
  padding: 10px 20px;
  border: none;
  border-radius: 50px;
  margin: 10px;
}

.btn--primary {
  background: #de8311;
  color: white;
}

.btn--white {
  background: #fff;
  color: #494949;
  box-shadow: 0px 0px 15px 1px rgba(0, 0, 0, .4);
}

.slider__heading {
  font-size: 2rem;
  color: #de8311
}

.course__slider {
  background: rgb(242, 242, 242);
  background: white;
  padding-top: 30px;
  padding-bottom: 30px;
  overflow: hidden;
  position: relative;
}

.category__heading {
  padding: 20px 0;
}

.category__slider {
  display: inline-flex;
  overflow: hidden;
  text-align: center;
  max-width: 100%;
  padding-top: 0px;
}

.slider__container {
  display: flex;
  justify-content: center;
  position: relative;
  overflow: hidden;
  max-width: 100%;
  -webkit-box-shadow: 0 0 5px 0px rgba(0,0,0,.2) inset;
  -moz-box-shadow: 0 0 5px 0px rgba(0,0,0,.2) inset;
  box-shadow: 0 0 15px 0px rgba(0,0,0,.2) inset;
  background: #f2f2f2;
}

.slider__container::before,
.slider__container::after {
  display: block;
  content: "";
  position: absolute;
  top: 0;
  width: 10%;
  height: 100%;
  z-index: 5;
}

.slider__container::before {
  left: 0;
  background:  linear-gradient(to right, rgba(255,255,255,.5) 0%, rgba(0,0,0,0) 100%);
}

.slider__container::after {
  right: 0;
  background:  linear-gradient(to left, rgba(255,255,255,.5) 0%, rgba(0,0,0,0) 100%);
}

.inner {
  position: relative;
  display: inline-flex;
  width: 100%;
  white-space: nowrap;
  transition: transform 1s;
  transform: translateX(-350px);
}

.course__slide {
  position: relative;
  display: inline-flex;
  margin: 10px 20px;
  padding: 0;
  background: white;
  border-radius: 10px;
  width: 350px;
  box-shadow: 0px 5px 15px 0px rgba(0,0,0,.3);
}

.course__slide img {
  width: 100%;
  border-radius:10px;
}

.course__slide:hover,
.course__slide:focus {
  transform: scale(1.02);
  cursor: pointer;
}

.course__slide a {
  text-decoration: none;
  color: #2c3e50;
}

.course__slide img {
  width: 100%;
  height: auto;
}

.slider__button {
  cursor: pointer;
  position: absolute;
  top: 40%;
  z-index: 10;
  font-size: 18px;
}

.slider__button.prev {
  left: 0%;
}

.slider__button.next {
  right: 0%;
}

.modal__wrapper {
  position: fixed;
  display: flex;
  visibility: hidden;
  justify-content: center;
  align-items: center;
  text-align: left;
  top: 0px;
  left: 0;
  background: rgba(192,192,192, 0.5);
  width: 100vw;
  height: 100vh;
  z-index: 11;
}

.modal__box {
  position: relative;
  background: white;
  display: inline;
  flex-direction: vertical;
  position: relative;
  padding: 20px;
  border-radius: 10px;
  min-width: 25%;
  max-width: 35%;
  z-index: 50;
}

.modal__image {
  width: 100%;
}

.modal__box a {
  text-decoration: none;
  position: absolute;
  right: 10px;
  top: 10px;
}

.modal__box a:hover {
  text-decoration: underline;
  color: #de8311;
}

/* Transition styles */

.list-item {
  transition: opacity 0.5s linear
}

.list-enter-from, .list-leave-to {
  opacity: 0;
  transform: scale(0)
}

.list-enter-active,
.list-leave-active {
  transition: all .4s ease;
  height: 0;
}

.list-move {
  transition: all .4s ease-out;
}

/* Swiper */

.swiper {
        width: 100%;
        height: 100%;
      }

.swiper-slide {
        text-align: center;
        font-size: 18px;
        background: #fff;

        /* Center slide text vertically */
        display: -webkit-box;
        display: -ms-flexbox;
        display: -webkit-flex;
        display: flex;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        -webkit-justify-content: center;
        justify-content: center;
        -webkit-box-align: center;
        -ms-flex-align: center;
        -webkit-align-items: center;
        align-items: center;
      }

.swiper-slide img {
        display: block;
        width: 100%;
        height: 100%;
        object-fit: cover;
      }

.swiper {
        margin-left: auto;
        margin-right: auto;
      }


/* Mobile Adjustment */

@media (min-width: 1800px) {
  .category__slider {
    max-width: 90%
  }
}

@media (max-width: 768px) {
  .course__slide {
    width: 80%;
  }

  .slider__container::before,
  .slider__container::after {
  width: 15%;
}
  
  .inner {
    transform: translateX(-84%)
  }

  .modal__box {
    min-width: 60%;
    max-width: 75%;
    padding: 10px 20px;
  }

  .modal__image {
    max-width: 100%;
  }
}

@media (max-width: 400px) {
  .course__slide {
    width: 80%;
  }
  
  .inner {
    transform: translateX(-85%)
  }

  .slider__button {
    transform: scale(0.8)
  }

  .modal__box {
    min-width: 60%;
    max-width: 80%;
    padding: 10px 20px;
  }

  .modal__image {
    max-width: 90%;
  }
}

</style>

