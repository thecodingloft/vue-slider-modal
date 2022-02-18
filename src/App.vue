<template>
  <section class="course__slider">


    <!-- Start Test Slider -->

    <!-- End Test Slider -->
    
    <div class="course__category">
      <h1 class="category__heading">International Cuisine</h1>
    </div>

    <!-- Category: International Cuisine -->
    <div class="category__slider mySlider">
      <div class="slider__container">

          <div class="inner" ref="inner">
            <transition-group tag="ul" name="list" appear mode="out-in">
              <li class="course__slide list-item" v-for="course in coursesInternational" :key="course.id">
                <a class="list" href="#" @click='openModal(course)'>
                  <img v-bind:src=course.image v-bind:alt=course.name>
                </a>
              </li>
            </transition-group>

          </div>
        
          <button class="slider__button btn--custom btn--white prev " @click="changeSlideInternational(-1)">&#10094;</button>
          <button class="slider__button btn--custom btn--white next" @click="changeSlideInternational(1)">&#10095;</button>
      
      </div>
      

    </div>

    <div class="course__category">
    <h1 class="category__heading">Pastry & Baking</h1>
    </div>

    <!-- Category: Pastry & Baking -->
    <div class="category__slider">
      <div class="slider__container">

          <div class="inner" ref="inner">
            <transition-group tag="ul" name="list" appear mode="out-in">
              <li class="course__slide list-item" v-for="course in coursesPastry" :key="course.id">
                <a class="list" href="#" @click='openModal(course)'>
                  <img v-bind:src=course.image v-bind:alt=course.name>
                </a>
              </li>
            </transition-group>

          </div>
        
          <button class="slider__button btn--custom btn--white prev " @click="changeSlidePastry(-1)">&#10094;</button>
          <button class="slider__button btn--custom btn--white next" @click="changeSlidePastry(1)">&#10095;</button>
      
      </div>
      

    </div>

    <div class="course__category">
    <h1 class="category__heading">Special Topics</h1>
    </div>

    <!-- Category: Specialty Courses -->
    <div class="category__slider">
      <div class="slider__container">

          <div class="inner" ref="inner">
            <transition-group tag="ul" name="list" appear mode="out-in">
              <li class="course__slide list-item" v-for="course in coursesSpecialty" :key="course.id">
                <a class="list" href="#" @click='openModal(course)'>
                  <img v-bind:src=course.image v-bind:alt=course.name>
                </a>
              </li>
            </transition-group>

          </div>
        
          <button class="slider__button btn--custom btn--white prev " @click="changeSlideSpecialty(-1)">&#10094;</button>
          <button class="slider__button btn--custom btn--white next" @click="changeSlideSpecialty(1)">&#10095;</button>
      
      </div>
      

    </div>

   
    <!-- Modal Box -->
    <div class="modal__wrapper" @click="closeModal()">
      <div class="modal__box">
      <a href="#" @click='closeModal'>X</a>
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

    console.log(this.coursesInternational)

    const slides = document.querySelectorAll(
    console.log(slides)

    slides.forEach((slide) => {
      slide.addEventListener('mousedown', () => {
        console.log('mouse')
      })
    })
  },

  methods: {
    openModal: function(course) {
      var modalWrapper = document.querySelector('.modal__wrapper')
      modalWrapper.style.visibility = 'visible'
      
      this.selectedCourse = course
      this.selectedCourseImage = course.image
      this.selectedCourseName = course.name
      this.selectedCourseDescription = course.description
      this.selectedCourseDifficulty = course.difficulty
      this.selectedCourseRecipes = course.recipes

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
      widthStep = this.cardWidth * (-2)
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
    },
    setupTouch: function() {
      console.log('touch setup')
    }
  },
}


</script>

<style>
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

.course__slider {
  background: rgb(242, 242, 242);
  padding-top: 20px;
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
  max-width: 90%;
  padding-top: 0px;
}

.slider__container {
  display: flex;
  justify-content: center;
  position: relative;
  overflow: hidden;
  max-width: 100%;
  background: rgb(156,156,156);
  background: radial-gradient(circle, rgba(156,156,156,1) 0%, rgba(56,56,56,1) 0%, rgba(156,156,156,1) 100%);
}

.slider__container::before,
.slider__container::after {
  display: block;
  content: "";
  position: absolute;
  top: 0;
  width: 25%;
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
  box-shadow: 1 10 10 10 rgba(0,0,0,1);
}

.course__slide:hover,
.course__slide:focus {
  transform: scale(1.02)
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
  top: 0;
  left: 0;
  background: rgba(192,192,192, 0.5);
  width: 100vw;
  height: 100vh;
  z-index: 11;
}

.modal__box {
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


/* Mobile Adjustment */

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

/* TESTING SLIDER STUFF */

#myElement {
  background: silver;
  height: 300px;
  text-align: center;
  font: 30px/300px Helvetica, Arial, sans-serif;
}

</style>

