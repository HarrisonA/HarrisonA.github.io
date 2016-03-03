---
layout: post
title: Displaying multiple Angular views on page load
date: 2016-2-12 00:00:00

---

Using the ui-router with Angular, multiple views can be displayed in a single state.  
This can be done using "relative" naming or "absolute" naming.  This post will
show one way it can be done using "absolute" naming.
<br>

A few notes:
<br>
1. The code snippets below were added to my existing code.
<br>
2. This is recommended for users who have some minimal experience with AngularJS.
<br>
3. This is not the only way to implement multiple views using ui-router.

<br>
Include a ui-router CDN.  Add a ui-view to the body.

(My index.html file)
{% highlight HTML %}

<script src="https://cdnjs.cloudflare.com/ajax/libs/angular-ui-router/0.2.18/angular-ui-router.js"></script>


<body>
  <div ui-view></div>
.
.
.
</body>
{% endhighlight %}

<br><br>
Create the HTML file where the multiple views are contained.
This will be the template url for the main state.

(My mainDisplay.html file)

{% highlight HTML %}
<div class='nav' ui-view="nav"></div>
<div class='side' ui-view="side"></div>
<div class='map' ui-view="map"></div>

{% endhighlight %}



<br>
Set the state configuration for the ui-router
(My app.js file)


{% highlight javascript %}
// inject ui.router into the module
angular.module('nameOfYourApp', ['ui.router'])


.config(function ($stateProvider, $urlRouterProvider) {
  $stateProvider
  .state('main', {
        url: '/',

        views: {
          '@': {  // unnamed ui-view element thats in the index.html
            templateUrl: '/main/mainDisplay.html',
          },

          'nav@main': { templateUrl: '/navBar/nav.html', controller: 'navCtrl' },
          //'nav' is the view thats in 'main' states template

          'side@main': { templateUrl: '/sideBar/sideBar.html', controller: 'sideCtrl' },
          //'side' is the view thats in 'main' states template

          'map@main': { templateUrl: '/map/map.html', controller:'mapCtrl' },
          //'map' is the view thats in 'main' states template

        },
      })

  // Default route if a valid route is not provided
  $urlRouterProvider.otherwise('/');  

});

{% endhighlight %}

Notice that the "@" key in the "views" property, has nothing on either side of it.
If nothing is to the left of the @ symbol, then the ui-view is unnamed.  If nothing is
to the right of the symbol, then it will look for that unnamed ui-view in the index.html.

The views key "nav@main" tells the ui-router to look for a ui-view element named "nav" and that
element is located in a state template named "main".  Note the "main" template of my example
is located at "/main/mainDisplay.html".





References:
<br>
<a href="http://slides.com/timkindberg/ui-router#/11/5">http://slides.com/timkindberg/ui-router#/11/5</a>
<br>
