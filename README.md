# Mapping the Sky: GPS Receiver Animation

[![dev.to](https://img.shields.io/badge/dev.to-0A0A0A?style=for-the-badge&logo=devdotto&logoColor=white)](https://dev.to/rodrigoantunes/mapping-the-sky-gps-receiver-animation-1ne2)

## Demo

[![CodePen](https://img.shields.io/badge/Codepen-000000?style=for-the-badge&logo=codepen&logoColor=white)](https://codepen.io/rodrigoant/pen/BagdXmY)

## Post 

I love the "Something in 100 Seconds" series from Fireship, and the recent episode, "**Scala in 100 Seconds**," caught my attention. Not only was the content on Scala interesting, but there was also a sponsor video that intrigued me, which you can see at [2:51](https://youtu.be/I7-hxTbpscU?si=g-0oBy5gjWDSGxxi&t=171).

I wondered if I could recreate the animation featured in that video, so I gave it a try. [Here](https://codepen.io/rodrigoant/pen/BagdXmY) is my attempt:

### HTML

For the first time, I used a `<datalist>` element to display ticks below the input range element. It doesn’t look exactly like the original, but it’s a close approximation using a native solution.

```html
    <input type="range" min="0" max="100" value="0" list="markers">

    <datalist id="markers">
      <option value="0"></option>
      <option value="10"></option>
      <option value="20"></option>
      <option value="30"></option>
      <option value="40"></option>
      <option value="50"></option>
      <option value="60"></option>
      <option value="70"></option>
      <option value="80"></option>
      <option value="90"></option>
      <option value="100"></option>
    </datalist>
```

This gives us these little markers

![datalist](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/i1e4bk8te7uz2xcmgqln.png)

For the animatable elements, I decided to apply the images via the `style` attribute so that my CSS is responsible only for positioning:

```html
<div class="container"
  style="background-image: url('https://res.cloudinary.com/rodrigoantunes/image/upload/v1723209932/stars.jpg')">

  <div class="globe"
    style="background-image: url('https://res.cloudinary.com/rodrigoantunes/image/upload/v1723210573/flatmap.jpg');">
  </div>

  <div class="satellite"
    style="background-image: url('https://res.cloudinary.com/rodrigoantunes/image/upload/v1723213814/particules.png');">
  </div>
  
</div>

```

### CSS

The CSS is mostly straightforward, but I’d like to highlight the use of `aspect-ratio`. This allowed me to avoid hardcoding many values and keep my code DRY:

```css
.globe {
  height: 130px;
  /* width: 130px; */
  aspect-ratio: 1;
}
```
Or when using `width: 100%` but still wanting to maintain the aspect ratio without forcing it with hardcoded equations:

```css
.container {
  width: 100%;
  /* height: 333px; */
  aspect-ratio: 3/2;
}
```

### Javascript

Here I had to set initial values to prevent the animations from "jumping" when the input element is triggered. I also performed some logical operations to make the range from 0 to 100 behave like in the video, converting some values to negative:

```js
const range = document.querySelector('input[type=range]');

// Initial values
const initialValue = parseInt(range.value, 10);
const initialContainerPosition = initialValue / -3;
const initialGlobePosition = initialValue;
const initialSatellitePosition = -initialValue - 40;

```

Then, it was just a matter of adjusting and refining the positions and directions:

```js
range.addEventListener('input', (e) => {
  const containerPosition = e.target.value / -3;
  const globePosition = e.target.value;
  const satellitePosition = -e.target.value - 40;

  container.style.backgroundPositionX = `${containerPosition}%`;
  globe.style.backgroundPositionX = `${globePosition}%`;
  satellite.style.transform = `translateX(${satellitePosition}%)`;
});

```

It's not an exact replica, but I'm happy with the result using native HTML, CSS, and JavaScript. Feedback and suggestions are welcome!