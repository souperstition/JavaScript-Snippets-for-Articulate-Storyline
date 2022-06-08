# JAVASCRIPT SNIPPETS FOR ARTICULATE STORYLINE

This is a collection of JavaScript that can be used in Articulate Storyline to make modifications otherwise unavailable within the software. The code currently focuses on making visual modifications, but suggestions are welcome. These snippets have been tested in Articulate Storyline 3; if you are able to test in other versions please feel free to contribute your findings. 

## HOW TO USE 

Add a new trigger on the desired slide (often the first slide of your project):

![create trigger](./images/01.%20create%20trigger.png)

Next, change the first field to "Execute JavaScript" and click "Add/Edit JavaScript":

![execute javascript](./images/02.%20execute%20javascript.png)

You can then copy and paste any of the code you find here in the window that appears. Now, anytime you preview your project it will tell you that JavaScript preview is not available. You will have to publish your project to see any of the changes.

If you have any trouble using this code, the .story files are in the project demos folder! Feel free to download them yourself and see if it helps solve your issue.

## THE SNIPPETS

- [Fullscreen Background Image](#1-fullscreen-background-image)
- [Fullscreen Button](#2-fullscreen-button)
- [Get Rid of the Player Border and Colors](#3-remove-the-border-and-player-background)

---

## 1. Fullscreen Background Image 

[demo](https://itsdani.me/sl/fullscreen-bg/story.html)

![fullscreen-bg](./images/03.%20fullscreen-bg.png)

**What it does:** Takes the background image you have added to your slide, removes it, and makes it the background image of the browser window, giving the project a fullscreen effect. The rest of the content is unchanged. 

I have seen similar solutions which require you to make your slide backgrounds transparent, but I appreciate this solution preserving the original slide background so the thumbnail generated by Storyline includes the slide background.

**NOTE:** This solution assumes that you are using the same background for each slide. This action will only take effect on the first slide, and any additional background images will also be removed.

```
const slideContainer = document.querySelector('#slide-window');

const bgImg = slideContainer.querySelector('image');

const href = bgImg.href.baseVal;

bgImg.remove();

document.body.setAttribute('style', `background: url(${href}); background-repeat: no-repeat; background-size: cover`);
```
[[top]](#javascript-snippets-for-articulate-storyline)


## 2. Fullscreen Button 

[demo](https://itsdani.me/sl/fullscreen-button/story.html)

![fullscreen-button](./images/04.%20go-fullscreen.png)

**What it does:** Adds a fullscreen button to your project. The styles can be changed to match the colors in your project, but do note that hover effects are unavailable without additional JavaScript.

**NOTE:** This does not resize your project. It basically does the same thing as pressing F11 on your keyboard, with one important difference: If the project is embedded; for example, in an LMS, it will allow the project to be seen in full screen without the LMS wrapper. I also find that having a button is much easier than having to tell students they can press f11, etc.. Not to mention mobile devices which don't have this option! 

Also make sure that in your Player settings under "Other", you have it set to scale the player to fill the browser window. Otherwise your browser window will go full screen, but the project will stay the same size.

```
let fullscreen;
let fsBtn = document.createElement('button');
fsBtn.innerHTML = 'Go Fullscreen';

// STYLES: 
fsBtn.setAttribute(
	'style',
	'position: relative; 
     top: 20px; 
     left: 100%; 
     transform: translateX(calc(-100% - 20px)); 
     z-index: 11; 
     border: 1px solid #fff; 
     border-radius: 5px; 
     font-size: 1.5rem; 
     padding: 0.5rem 0.7em; 
     background-color: rgba(0,0,0,0.2); 
     color: #fff; 
     font-family: Verdana, Geneva, Tahoma, sans-serif; 
     -webkit-font-smoothing: antialiased; 
     cursor: pointer; 
     transition: all .3s;'
);

// this class lets us know the button has been added so that if we revisit this slide we don't get a bunch of buttons
fsBtn.classList.add('added'); 

// if we don't already have a button on the page, go ahead and add it:
if (!document.body.querySelector('.added')) { 
	document.body.appendChild(fsBtn); 
	fsBtn.addEventListener('click', function(e) {
		e.preventDefault();
		if (!fullscreen) {
			fullscreen = true;
			document.documentElement.requestFullscreen();
            // if we are in Fullscreen mode, the text should say Exit Fullscreen
			fsBtn.innerHTML = 'Exit Fullscreen'; 
		} else {
			fullscreen = false;
			document.exitFullscreen();
            // if we are not in Fullscreen mode, the text should say Go Fullscreen
			fsBtn.innerHTML = 'Go Fullscreen'; 
		}
	});
}

```

[[top]](#javascript-snippets-for-articulate-storyline)

## 3. Remove the Border and Player Background

**What it does:** Gets rid of the border and background caused by the player window. Yes, this can be done in the player settings, but it takes a long time to do so. This bit of code added to your project takes care of it for you. You still have to make sure any unwanted buttons are turned off in the settings.

```
const frame = document.querySelector('#frame');
frame.setAttribute(
	'style',
	`border-radius: 0;
	border: none;
	background-color: transparent;
	width: 100%;
	height: 100%;`
);
```

[[top]](#javascript-snippets-for-articulate-storyline)
