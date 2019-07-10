we're gonna make a machine learning program that determines based on the background the user picks to output either dark or light text.

We'll start by installing brain.js which is an npm module that lets us set up a neural network.

We're gonna go ahead and import it using

`const brain = require("brain.js")`

and set up the network

`const network = new brain.NeuralNetwork()`

`network.train()`

so now we need to give it some training data. Traditionally programming if we wanted to determine if a color is light or dark we'd start by programming an algorithm and you'd say hey let's take the rgb values and let's idk add them together and if that number gets past a certain point let's say that that's a light background. But there's also edge cases you'd need to apply like yellow would need dark text and so on.

A neural network doesn't work that way at all. you simply give it an array of values or an array of inputs. and each input has two things an input: and an output: .

So we give it historical data of hey for these values historically here's what the output has been. Think of it like flashcards to teach a toddler the difference between cats and dogs. You get a stack of flashcards of different cats and dogs and say hey this is a cat, this is a dog and eventually they get that this is a cat and this is a dog. This is exactly how a neural network functions. It simulates the behavior of a human brain.

So in this particular case brain.js expects values between 1 and 0 and an input can be 1 or more values and an output can also be 1 or more values. 

So like you can train it to say hey when you give it 1 that's an output of 1 and 0 then that's an output of 0 and then our neural network can tell the difference between 1s and 0s. Though that's pretty useless it should give you an idea.

~~~~
 network.train([ 
    {input: [1], output:[1]},
    {input: [0], output:[0]},
]) 
~~~~

Something more useful might be hey let's try to find the difference between children and adults. We'll have 1 be hey that's an adult.
We'll have the first person be like 60 inches 150 pounds and maybe a 30 incher idk how much a kid weighs maybe 60 pounds.

~~~~
 network.train([
    {input: [60, 150], output:[1]},
    {input: [30, 60], output:[0]},
    {input: [30, 60], output:[0]},
]) 
~~~~

so now we have some real world data and that's considered trained. so down in the results we can give it some other information. Let's say we have a 70 inch 200 pounder and that should tell us 1 hey that's an adult!

So let's actually try running this here and save that result here.

`const result = network.run()`
` console.log(result) `

So now we get a result saying it's about 31% chance it's an adult. Now a 31% chance guess that we're dealing with an adult isn't very high. So let's try and give it some more training data. Maybe a 73 incher 250 pounder and that's an adult.

` {input: [73, 250], output:[1]}, `

so now we run it and we can see that it thinks there's 67% chance that it thinks this is an adult. So our network became a lot more accurate with the same data because we gave it more historical data points. So your network is only as good as youre able to create your data. Most of the trick is coming up with data that can accurately predict what the output or outcome will be.

Another thing here we can do is change the values in the array into objects like height: or weight: and then say adult: 1

` {input: {height: 60, weight: 150}, output:{adult: 1}}, `

now the input will look a little differently. It'll now say adult: 92% certainty which is more clear.

So lets try and apply this to a problem of if this is a light or dark color? We'll change the inputs to rgb

` {input: {r, g, b}, output: {dark: 1}} `

and we can easily convert rgb into something between 0 and 1 for brain.js by dividing them by 255 because rgb colors are between 0 and 255. So if we divide by 255 then 0 stays 0, 120 is in the middle and 255 becomes 1.

So I've gone ahead and done that and made this rgb dataset by just picking out some colors and dividing all the values by 255 and saying if it's a light color or dark color.

For a test I'll strip this last value here and comment it out using \\ . I know that this is a light color so I'll input it in the network and see what result we get. With just 5 training values we get a 95% certainty that it's dealing with a light color! Wow!

So let's go ahead and make an html ` touch index.html` and make an input for colors and a div with some example text.

~~~~
<input type="color" value="#ff0000"/>
<div id="example">Example Text</div>
~~~~

I'll also make a css file and add a bit of styling just to make it easier to see the changes.

~~~~
body {
    font-family: helvetica neue;
  }
  #example {
    padding: 100px;
    background: #ff0000;
    color: white;
    font-size: 36px;
  }
  ~~~~

So let's copy all this code and plug it into our html and it should work to change your styling.

so now I need to also change the text when the background changes. So I'll set a varaiable for that to get the values from the rgb numbers as well as console.log that change just to check if it's working.

~~~~
const rgb = getRgb(e.target.value);
console.log(rgb);
~~~~

So now when look at that in the web console I'll see the values changing.
So now I'll console log the results instead and I'll see the light/dark outputs now.

` console.log(result); `

So we could say hey if light is larger than dark then we know it's light. but I can also have brain.js do that for me by saying 
` const result = brain.likely(rgb, network) `

so now all we have to do at this point is say something like if result is dark return white text otherwise return black text. This is an example of ternary in javascript.
` example.style.color = result === "dark" ? "white" : "black" `

and now we get the expected results. Our text changes based on if the background color is dark or light. Now within only a little while we've solved a problem that would've actually been pretty complicated and hard to solve with a traditional algorithm.