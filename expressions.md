# Expressions

## 1. Impact shake (decaying hit at a marker)

Apply to **Position** of a camera null or adjustment precomp. Add a layer marker where the impact happens.

```js
// Sliders on this layer: "Amp" (e.g. 25), "Freq" (e.g. 18), "Decay" (e.g. 7)
amp = effect("Amp")("Slider");
freq = effect("Freq")("Slider");
decay = effect("Decay")("Slider");
n = 0;
if (marker.numKeys > 0) {
  n = marker.nearestKey(time).index;
  if (marker.key(n).time > time) n--;
}
if (n == 0) {
  value;
} else {
  t = time - marker.key(n).time;
  y = amp * Math.sin(freq * t * 2 * Math.PI) / Math.exp(decay * t);
  x = (amp * 0.35) * Math.cos(freq * 0.8 * t * 2 * Math.PI) / Math.exp(decay * t);
  value + [x, y];
}
```

## 2. Seamless loop of keyframed motion

```js
loopOut("cycle");
```

For a loop that has to repeat with no jump, make the first and last keyframe identical, then use:

```js
loopOut("pingpong");
```

## 3. Wheel spin tied to horizontal movement

Apply to **Rotation** of a wheel layer parented to the car.

```js
// Slider "Wheel Diameter (px)"
d = effect("Wheel Diameter (px)")("Slider");
carX = thisComp.layer("CAR").transform.position[0];
startX = thisComp.layer("CAR").transform.position.valueAtTime(inPoint)[0];
((carX - startX) / (Math.PI * d)) * 360;
```

## 4. Auto fade in/out (no keyframes)

Apply to **Opacity**.

```js
fadeIn = 0.4; fadeOut = 0.4;
Math.min(linear(time, inPoint, inPoint + fadeIn, 0, 100),
         linear(time, outPoint - fadeOut, outPoint, 100, 0));
```

## 5. Subtle handheld drift

Apply to **Position** for a gentle, premium handheld feel on static shots.

```js
wiggle(0.6, 6);
```

## 6. Speed-ramp friendly time remap smoothing

Apply to **Time Remap** after setting two keyframes, to ease the ramp.

```js
if (numKeys > 1) {
  t1 = key(1).time; t2 = key(numKeys).time;
  v1 = key(1).value; v2 = key(numKeys).value;
  ease(time, t1, t2, v1, v2);
} else value;
```
