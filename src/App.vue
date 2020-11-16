
<template>
     <div id="app">
      <div id="wrapper">
				<!-- Main -->
					<div id="main">
						<div class="inner">

							<!-- Header -->
								<header id="header">
									<a href="index.html" class="logo"><strong>Young Social Makers</strong></a>
									<ul class="icons">
										<!-- <li><a href="#" class="icon brands fa-twitter"><span class="label">Twitter</span></a></li>-->
										<li><a href="https://www.facebook.com/frank.kroondijk" target="_blank" class="icon brands fa-facebook-f"><span class="label">Facebook</span></a></li>
										<li><a href="https://www.linkedin.com/in/frankkroondijk/" target="_blank" class="icon brands fa-linkedin"><span class="label">LinkedIn</span></a></li>
										<li><a href="https://www.instagram.com/frankkroondijk/" target="_blank" class="icon brands fa-instagram"><span class="label">Instagram</span></a></li>
										<!-- <li><a href="#" class="icon brands fa-medium-m"><span class="label">Medium</span></a></li>		-->
                   						<li><nav id="nav"><router-link to="/login"><div id="un">{{loginLabel}}</div></router-link></nav></li>
									</ul>
								</header>

              					<router-view/>

								<footer id="footer" style="text-align:center">
									<p class="copyright">&copy; Young Social Makers</p>
								</footer>
						</div>
					</div>

				<!-- Sidebar -->
				<section>
					<div id="sidebar">
						<div class="inner">
							<div id="logo" style="width:100%;text-align:center">
							<img src="images/ysm-logo.png" style="max-width:100px; ">
							</div>
							<!-- Search 
								<section id="search" class="alt">
									<form method="post" action="#">
										<input type="text" name="query" id="query" placeholder="Search" />
									</form>
								</section>
							-->
							<div id="cMenu" style="display:none;">

							</div>

							<!-- Menu -->
              				<div id="nav">
								<nav id="menu">
									<header class="major">
										<h2>Menu</h2>
									</header>
									<ul>
                        
										<li><router-link to="/">Home</router-link></li>
										<li><router-link to="/about">About</router-link></li>
										<li><router-link to="/projects" >Projects</router-link></li>
										<li><router-link to="/softskills" >Soft skills</router-link></li>
										<li><router-link to="/courses" >Courses</router-link></li>
										<!--
										<li><a href="index.html">Homepage</a></li>
										<li><a href="generic.html">Generic</a></li>
										<li><a href="elements.html">Elements</a></li>
										<li>
											<span class="opener">Submenu</span>
											<ul>
												<li><a href="#">Lorem Dolor</a></li>
												<li><a href="#">Ipsum Adipiscing</a></li>
												<li><a href="#">Tempus Magna</a></li>
												<li><a href="#">Feugiat Veroeros</a></li>
											</ul>
										</li>
										<li><a href="#">Etiam Dolore</a></li>
										<li><a href="#">Adipiscing</a></li>
										<li>
											<span class="opener">Another Submenu</span>
											<ul>
												<li><a href="#">Lorem Dolor</a></li>
												<li><a href="#">Ipsum Adipiscing</a></li>
												<li><a href="#">Tempus Magna</a></li>
												<li><a href="#">Feugiat Veroeros</a></li>
											</ul>
										</li>
										-->
									</ul>
								</nav>
              				</div>
						</div>
                	</div>
					</section>
							<!-- Section 
								<section>
									<header class="major">
										<h2>Ante interdum</h2>
									</header>
									<div class="mini-posts">
										<article>
											<a href="#" class="image"><img src="images/pic07.jpg" alt="" /></a>
											<p>Aenean ornare velit lacus, ac varius enim lorem ullamcorper dolore aliquam.</p>
										</article>
										<article>
											<a href="#" class="image"><img src="images/pic08.jpg" alt="" /></a>
											<p>Aenean ornare velit lacus, ac varius enim lorem ullamcorper dolore aliquam.</p>
										</article>
										<article>
											<a href="#" class="image"><img src="images/pic09.jpg" alt="" /></a>
											<p>Aenean ornare velit lacus, ac varius enim lorem ullamcorper dolore aliquam.</p>
										</article>
									</div>
									<ul class="actions">
										<li><a href="#" class="button">More</a></li>
									</ul>
								</section>
							-->
							<!-- Section 
								<section>
									<header class="major">
										<h2>Get in touch</h2>
									</header>
									<p>Sed varius enim lorem ullamcorper dolore aliquam aenean ornare velit lacus, ac varius enim lorem ullamcorper dolore. Proin sed aliquam facilisis ante interdum. Sed nulla amet lorem feugiat tempus aliquam.</p>
									<ul class="contact">
										<li class="icon solid fa-envelope"><a href="#">information@untitled.tld</a></li>
										<li class="icon solid fa-phone">(000) 000-0000</li>
										<li class="icon solid fa-home">1234 Somewhere Road #8254<br />
										Nashville, TN 00000-0000</li>
									</ul>
								</section>
							-->
							<!-- Footer 								<footer id="footer">
									<p class="copyright">&copy; Purposeful learning labs. All rights reserved.</p>
								</footer>
-->

						</div>
            							<!-- Footer -->

					</div>

</template>

<script>
//import something from "./assets/js/something.js"
//import "./assets/js/jquery.min.js"
import sideBar from "./assets/js/main.js"
import "./assets/css/main.css"
//import "./assets/css/sss.css"


//import AccountInfo from "@/components/AccountInfo.vue";

export default {
	 data() {
		 return {
			 user: {},
			 loginLabel:'Login'
		 }
   	}
 ,
 methods: {

},
  mounted() {
    
    //alert('hier');
	sideBar();
	

  },
 components: {
   //AccountInfo
 },

   methods: {
      sideBar
      
   },
   created() 
    {
		
	if(localStorage.user)
    {
	  this.user = JSON.parse(localStorage.user);

	  if(this.user.Name)
	  {
		  this.loginLabel = this.user.Name;
	  }
    }
		
    }
}

</script>


