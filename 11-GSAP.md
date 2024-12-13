# GSAP

GSAP Stands for GreenSock Animation Platform and it's a cool Frontend tool to handle animations.
Let's start.

## Basics

GSAP has three main methods:

- `gsap.to()`
- `gsap.from()`
- `gsap.fromTo()`

Each of these methods will create a **Tween** and optionally a **Timeline**.

A Tween can change a single propery of a single object over time.

### `gsap.to()`

`gsap.to()` accepts two parameters: first the object to animate, and second the parameters of the animation.
The first parameter can be a string, that will trigger a `document.querySelectorAll()`.

An example is:

```javascript
gsap.to('.start', { x: 750, rotation: 360, fill: 'yellow', duration: 3 });
```

This example will select all elements with a start class, translate them in the X axys by 750 pixels, rotate them, and slowly changing the fill colore to yellow.
Cool, right? But very simple.

You can specify the duration, but if not, the default duration will be 500 milliseconds, so half of a second.

You can override deault values by using:

```javascript
gsap.default({
	duration: 1,
});
```

The animations will stransform each object 60 times per second, so that each animation will be super fluid.

For best performance, it's metter to animate CSS trasnform properties (x, y, scale, skew, rotation, etc). Other properties will still work (of course) but are not hardware accelerated (like color, border-radius, etc.).

### `gsap.from()` and `gsap.fromTo()`

`gsap.from()` has the same API of `gsap.to()`, but, it'll animate the object FROM the value you give TO it's original values. Basically, it'll reversed.
`gsap.fromTo()`, as maybe you already understood, will allow to specify where to begin, and where to end. For this reason, it accepts 3 parameters: the target, the properties to change for the **from** animation, and finally the properties to change for the **to** animation.

### Tween Controls

When you run a `gsap.to()` function or similar, it'll return the **Tween** object. This object has various properties, but also methods that will allow you to completely control the tween, like:

- `play()`: Will play the animation.
- `pause()`: Will pause the animation.
- `reverse()`: Will reverse the animation.
- `restart()`: Will reset the animation.

## Timelines

You can combine multiple tweens together in a **Timeline**. I fyou have sequential animation, and maybe you change the duration of the first one, you have to change all the other animations according to the new duration.
A timeline solves this problem.
To create a timeline you can do like this:

```javascript
gsap
	.timeline()
	.from('#demo', { opacity: 0, duration: 1 })
	.from('#title', { opacity: 0, scale: 0, ease: 'back' })
	.from('#freds img', { stagger: 0.1, y: 160, duration: 0.8, ease: 'back' })
	.from('#time', { xPercent: 100, duration: 0.2 });
```

In this code, every animation will be executed in direct succession.
This means if you change the duration of the first animation, all the other will be executed in order.

> Note: The **xPercent** parameter tells gsap to move the target on the x axis based on a percentage value. In this case, it'll move the element on the right by 100% of its width.

One cool thing you can do, is to execute an animation in a specific time in the timeline.
To do that, you can do like this:

```javascript
gsap
	.timeline()
	.from('#demo', { opacity: 0, duration: 1 })
	.from('#title', { opacity: 0, scale: 0, ease: 'back' }, '+=1');
```

In the above example, the second animation will be executed 1 second _after_ the previous animation is finished.
You can specify the amount of time in the third parameter of each tween.

To tell that the tween should start _at the same time_ of the previous animation, you can use the symbol "<".

To tell that the tween should start _at a specific second_, you can symply type a number.

To tell that the tween should start _at the same time_, but _a little bit after_ the previous animation is finished, you can type the symbol "<" followed by the amount of delay (e.g "<0.5").

### Debugging

You can debug timelines by adding label names to a specific point in the timeline, like this:

```javascript
const animation = gsap
	.timeline()
	.from('#demo', { opacity: 0, duration: 1 })
	.add('test')
	.from('#title', { opacity: 0, scale: 0, ease: 'back' }, '+=1');

animation.play('test');
```

By doing this, the animation will play immediately starting from where the label "test" is.
This can be very useful to debug and skip the inital part of the whole animation.

Another cool way is to use the `GSDevTools` plugin. It's required a Club subscription to use it, but you
can try if for free on CodePen.

### CSS Optimizations

When you perform Javascript animations there are some CSS tricks you can use to optimize the animation, make it smoother, and clean.
One of these optimizations is to use the CSS property "will-change". The **will-change** property will
tell the browser that a specific property will change at some point, so that the browser can optimize
it before the animation starts.

```css
.item {
	will-change: transform;
}
```

Another trick regards scaling elements. If you want to animate an element to make it bigger, you'll notice there will be some
anti-aliasing pixels around texts and corners. To avoid it, you can set the initial scale of the element
to a smaller size, and animate it to make scale it at 1 (original size).

```css
.item {
	will-change: transform;
	transform: scale(0.8);
}
```

### Defaults

In your timeline you can set default values for every animation that will happen. For example, if you want
to make a "fade in" animation on every object you animate in the timeline, you can write the following code:

```javascript
gsap.timeline({ opacity: 0, ease: 'back' });
```

### FOUC

FOUC (Flash Of Unstyles Content) is a phenomenon that happens when the content of your page loads before
any style and Javascript gets loaded.
This can be a problem of your animations, because they can eventually start before your page fully loaded.
To avoid this problem, you can set the property **visibility** to none to the main container of your animations,
put your content inside of the `window.addEventListener('load')` callback, and finally reset the visibility
to **visible** when the animation starts.
To do that, GSAP provides a parameter called **autoAlpha**.
If you tween from an autoAlpha of 0 the visibility will be set to "inherit" and the opacity will animate from 0 to 1.

```css
#demo {
	visibility: hidden;
}
```

```javascript
const tl = gsap.timeline({ defaults: { opacity: 0, ease: 'back' } });

window.addEventListener('load', () => {
	tl.from('#demo', { ease: 'linear', autoAlpha: 0 }) // here it'll start
		.from('h1', { x: 80, duration: 1 })
		.from('h2', { x: -80, duration: 1 }, '<')
		.from('p', { y: 30 }, '-=0.2')
		.from('button', { y: 50 }, '-=0.4')
		.from('#items > g', { scale: 0, stagger: 0.1, transformOrigin: '50% 50%' }, '-=0.5');
});
```

## Text animations

### Text

To perform text animations, GSAP provides the `Text` Plugin.
It's very easy to use. All you will have to do, is first register the plugin, and then you'll have
now available a new property called **text**. In this property you can write the text you want to animate.

```javascript
gsap.to('p', {
	text: 'typewriter effect with GSAP 3', // New property
	ease: 'power1.in',
	duration: 2,
});
```

The tween amove will create a typewriter effect by making appear each letter sequentially.
You can further customize the behaviour of the text if you use an object instead of a simple string.

If the `p` tag already has some text, the tween will sequentally replace each letter with the new value,
creating a cool "matrix like" animation.

### SplitText

The `SplitText` plugin makes it easy to break apart the text in an HTML element so that each character,
word, and/or line is wrapped in its own div tag.

The downside is that it's part of the Club API (so you must pay).

An Example is the following:

```javascript
const split = new SplitText('p', { type: 'chars' });
gsap.timeline().from(split.chars, { opacity: 0, stagger: 0.05, y: 50, ease: 'back' });
```

You can split the text by:

- **chars**
- **words**
- **lines**

## Keyframes

In CSS Keyframes there is the cool possibility to define what value an object should have based on
percentages, meaning that on a specific percentage of the animation the object should have specific values.
On GSAP this is also possible, and allows to have full controls over an animation.
You can specify keyframes in the `keyframes` property.

```javascript
gsap.to('.slime', {
	keyframes: {
		'25%': { y: 0 },
		'50%': { y: -100, ease: 'sine' },
		'75%': { y: 0, ease: 'sine.in' },
		'100%': { x: 320, ease: 'none' },
	},
	duration: 3,
	stagger: 0.4,
});
```

## Special properties

As we already seen, the object with the properties accepts also some special properties. The one we saw so far is the **duration** property, but there are also several other, like:

- **delay**: the seconds to wait before executing the animation.
- **repeat**: the number of times you want to repeat the animation (note: default is 0. Repeat = 1 will make it repeat once).
- **repeatDelay**: the amount of time to wait for each repetition.
- **yoyo**: it's a boolean value. If true, the animation will go back and forth.
- **ease**: change the easing function of the animation. A "none" ease value is equals to "linear". You can check the [ease visualizer here](https://gsap.com/docs/v3/Eases)
- **stagger**: if the selector in the first parameter targets more than one element, with the stagger property you can add a delay for each element. It's a number value, which corresponds to the delay in seconds from the last animated object (of the same tween). It can also be an object with custom parameters.
- **paused**: boolean value. If true, the animation won't play.
- **overwrite**: boolean value. If true, it'll kill eventual animations playing on the same target.
- **onComplete**: a callback that gets executed whenever the animation is completed.
- **onCompleteParams**: an array of values which will correspond to the parameters that you will get access to in the _onComplete_ callback.
- **onRepeat**: a callback that gets executed whenever the animation is repeated.
- **onRepeatParams**: an array of values which will correspond to the parameters that you will get access to in the _onRepeat_ callback.
- **onReverseComplete**: a callback that gets executed whenever the reverse of the animation is completed.
- **onReverseCompleteParams**: an array of values which will correspond to the parameters that you will get access to in the _onReverseComplete_ callback.
- **onStart**: a callback that gets executed whenever the animation starts.
- **onStartParams**: an array of values which will correspond to the parameters that you will get access to in the _onStart_ callback.
- **onUpdate**: a callback that gets executed whenever the animation is updated.
- **onUpdateParams**: an array of values which will correspond to the parameters that you will get access to in the _onUpdate_ callback.

## Getters and Setters

When you create a tween or a timeline, you get some functions that you can execute on the returned object.
Each method will return the same animation object, so that functions can be chained.

- `pause()`: will pause the animation.
- `paused()`: returns true if the animation is paused.
- `paused(value)`: where _value_ is a boolean value. It sets the paused state to the given value.
- `progress()`: gets the progress of the animation (returns a number between 0 and 1).
- `progress(value)`: sets the progress of the animation.
- `time()`: gets the position of the playhead.
- `time(value)`: where _value_ is a number. Set the position of the playhead.
- `timeScale()`: gets the current timescale.
- `timeScale(value)`: where _value_ is a number. Makes animation play by \[value]x as fast.
- `duration()`: gets the current animationn duration.
- `duration(value)`: where _value_ is a number. Set the animationn duration.
- `reversed()`: gets true if the animation is playing on reverse.
- `reversed(value)`: where _value_ is a boolean. It sets the _reversed_ state to the given value.
- `getChildren()`: gets the children of the animation, so it'll return a list of tweens.

## Methods

With gsap you can use the following methods:

- `gsap.killTweensOf(selector, property?)`: kills the specified property on a specific object obtained through the selector. If no property is specified, it kills the whole tween.
- `gsap.utils.wrap()`: you can use this function to giev the properties of an animations different values based on their position.
- `gsap.registerEffect()`: create re-usable and customizable effects that you can call directly on timelines.

### `gsap.utils.wrap()` example

```javascript
gsap.timeline().from(chars, {
	y: gsap.utils.wrap([-100, 100]),
	rotation: -100,
	opacity: 0,
	stagger: { each: 0.05 },
});
```

### `gsap.registerEffect()` example

```javascript
// We register the "rainbow" effect
gsap.registerEffect({
	name: 'rainbow',
	effect: (targets, config) => {
		let split = new SplitText(targets, { type: 'chars' });
		let tl = gsap.timeline();
		tl.from(split.chars, { opacity: 0, y: -100, stagger: 0.05 });
		tl.to(split.chars, { color: gsap.utils.wrap(['aqua', 'pink', 'yellow']), stagger: 0.05 }, 0);
		return tl;
	},
});

// We use the "rainbow" effect on a target
gsap.effects.rainbow('h1');

// We can add it to a timeline
gsap.timeline().add(gsap.effects.rainbow('h1'));
```

#### Extend timeline

Or, you can set `extendTimeline` to **true** and use it directly on the timeline:

```javascript
// We register the "rainbow" effect
gsap.registerEffect({
	name: 'rainbow',
	extendTimeline: true, // We added this
	effect: (targets, config) => {
		let split = new SplitText(targets, { type: 'chars' });
		let tl = gsap.timeline();
		tl.from(split.chars, { opacity: 0, y: -100, stagger: 0.05 });
		tl.to(split.chars, { color: gsap.utils.wrap(['aqua', 'pink', 'yellow']), stagger: 0.05 }, 0);
		return tl;
	},
});

// Now we can simply write this
gsap.timeline().rainbow('h1');
```

#### Configuration

You can make your effects highly customizable. First of all, you can put **default values**, and combine
them with the configuration object (the second parameter).

```javascript
// We register the "rainbow" effect
gsap.registerEffect({
	name: 'rainbow',
	extendTimeline: true, // We added this
	defaults: {
		y: -100,
	},
	effect: (targets, config) => {
		let split = new SplitText(targets, { type: 'chars' });
		let tl = gsap.timeline();
		tl.from(split.chars, {
			opacity: 0
			 y: config.y, // we now use the config value
			 stagger: 0.05
			});
		tl.to(split.chars, { color: gsap.utils.wrap(['aqua', 'pink', 'yellow']), stagger: 0.05 }, 0);
		return tl;
	},
});


// Now we can simply write this
gsap.timeline()
	.rainbow('h1');
	.rainbow('h2', { y: 0 }); // we customize the "y" parameter on the effect
```

## Erase values

To erase CSS values on an object, you can use the `gsap.set()` method in combination with the **clearProps** CSS plugin.

```javascript
gsap.set('.box', { clearProps: 'backgroundColor,width' });
```

In the above example you specifically reset the _backgroundColor_ property and the _width_ property.
If you want to erase **all** inline styles, you can write "all" instead:

```javascript
gsap.set('.box', { clearProps: 'all' });
```

You can actually also use the `clearProps` property in your tweens. It will be executed whenever the
tween ends its execution.

## immediateRender

`immediateRender` is a special GSAP proeprty that is automatically set to `true` on `.from()` and
`.fromTo()` animations. Basically, when it is set to true, it'll immediately set the values you put
on the animation when the tween or timeline starts.
If you put false, the values won't be set until the tween actually starts.

Setting it to `true` may cause undesired results when you have multiple `.from()` tweens on the same
properties of the same object. The solution is to set in to false on any subsequent `from()` or
`fromTo()` tweens.

## QuickSetter

Sometimes you simply want to use the `gsap.set` method to set a property value on an object, and that's it.
This kind of scenario can be when you want to set, for example, a scale property on an element based
on the position of the mouse in a plane, so based on a mousemove event.
To do that, you can use the `gsap.quickSetter` method, which is almost **200%** more performant.

```javascript
const setScale = gsap.quickSetter('.box', 'scale');

demo.addEventListener('mousemove', (e) => {
	setScale(e.offsetX);
});
```

## Utility methods

GSAP offers some utility methods to make life easier. We already saw the `wrap()` method. Now we'll
focus on the other main ones.

- `gsap.utils.mapRange(inMin, inMax, outMin, outMax, valueToMap)`: maps values from an input range to their equivalent in an output range (e.g. from a range of 0, 600, to a range of 0, 4).
- `gsap.utils.snap()`: snaps a given value to a specific amount.
- `gsap.utils.pipe()`: allows you to pass a value through a "pipeline" of GSAP's utility function. Each function will modify the value it receives and pass it down to the next pipeline.

### Utilities example

```javascript
const mapRange = gsap.utils.mapRange(0, 600, 0, 360);
const snap = gsap.utils.snap(45);
const setRotation = gsap.quickSetter('.box', 'rotation', 'deg');

const pipe = gsap.utils.pipe(mapRange, snap, setRotation);

demo.addEventListener('mousemove', (e) => {
	pipe(e.offsetX);
});
```

## functions as parameters

GSAP allows to put a function as a value for a property. The result returned by this function will
be the actual value.
With the function you can customize the result of the animation for each specific item. In fact,
it accepts 3 parameters: the index of the current animating item, the target (actual DOM element)
and the list of targets of the current animation.

Here's an example:

```javascript
gsap.to('.box', {
	y: function (index, target, targets) {
		return index * 20;
	},
});
```

Knowing this, you can also use some predefined functions to create cool animations on a list of items.
This helper function can be, for example, based on an ease curve.
You can find most of them in the `gsap.utils` object.

One of these is the `gsap.utils.distribute` function. In the next example, we'll create a "rubberbander"
animation thanks to this function:

```javascript
gsap.registerPlugin(SplitText);
let mySplit = new SplitText('h1', { type: 'chars' });

let scaleDistributor = gsap.utils.distribute({
	base: 0.2,
	amount: 1.5,
	ease: 'power1',
	from: 'center',
});

let distanceDistributor = gsap.utils.distribute({
	base: -200,
	amount: 400,
	ease: 'none',
});

gsap.from(mySplit.chars, {
	scale: scaleDistributor,
	x: distanceDistributor,
	opacity: 0,
	stagger: {
		each: 0.01,
		from: 'center',
	},
});
```

## Timeline methods

With timelines you can create advance timeline configurations.

- `addPause(position, callback, callackParams)`: With addPause() you can interrupt a timeline at a specific point, making easier
  to create elements like "next" button.
- `add(name)`: with the add() method you can label a specific point in a timeline.
- `tweenTo(name)`: if you used the "add()" method, you can put your custom name in the first parameter
  and the timeline will animate to that specific point.
- `tweenFrom(name)`: if you used the "add()" method, you can put your custom name in the first parameter
  and the timeline will animate to that specific point.
- `tweenFromTo(name, name2)`: if you used the "add()" method, you can put your custom name in the first parameter
  and the timeline will animate to that specific point.
- `labels`: this getter returns all the labels you put in the timeline, which will be an object having
  the name of the label as a key and the time where it is positioned as a value.

## ScrollTrigger

**ScrollTrigger** is a GSAP plugin that allows to control an animation based on the scroll of the mouse.
It's almost essential, in modern websites, to have this plugin, and it's completely free.

Why it is needed? Animations are usually not triggered by some events like click, or mouseenter, or
stuff like that. Instead, are usually played when the object enters the screen.

This plugin helps controlling this. Adding it, will allow to set a new `scrollTrigger` property on the
configuration object of a tween.

Example:

```javascript
gsap.to(target, {
	duration: 3,
	x: '-50vw',
	rotation: -360,
	ease: 'linear',
	scrollTrigger: startTarget, // the element that will start the animation once it enters the screen.
	// it can also be the same target of the animation
});
```

This simple configuration will make the animation start once the `startTarget` element enters the screen.

But, you can further customize the scrollTrigger options if you use an object:

```javascript
gsap.to(target, {
	duration: 3,
	x: '-50vw',
	rotation: -360,
	ease: 'linear',
	scrollTrigger: {
		trigger: startTrigger,
		markers: true, // will add some markers in the screen to see visaully where the trigger is.
		start: 'top 75%', // it says that the animation starts when the trigger enters 75% of the screen from the top.
		end: 'bottom 25%', // it says that the animation ends when the trigger enters 25% of the screen from the bottom.
		// events: onEnter onLeave onEnterBack onLeaveBack
		// options: play, pause, resume, reset, restart, complete, reverse, none
		toggleActions: 'restart pause reverse reset',
		pin: true, // will apply a "sticky" position to the animated element
		pinSpacing: true // It's true by default. It will NOT make the content after the element appear until the animationj ended.
		scrub: true // the progress of the animation will depend on the scroll position
		once: true // once the animation ends, it won't be executed ever again (it invokes the `kill()` method to clean up).

})
```

### Parallax Scrolling

```javascript
gsap.defaults({ ease: 'none', duration: 1 });

const tl = gsap.timelime({
	scrollTrigger: {
		trigger: '.wrapper',
		start: 'top 25%',
		end: '+=100',
		toggleActions: 'restart none none reverse',
		scrub: 0.5,
		pin: true,
	},
});

tl.from('.background', { y: 50 });
tl.from('.middleBackground', { y: 150 }, 0);
tl.from('.foreground', { y: 250 }, 0);
tl.from('.text', { y: 500 }, 0);
```
