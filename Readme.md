# Assignment 2 reprot

## Genetic algoritm results:
![](https://i.imgur.com/HdHQQwf.gif)
![](https://i.imgur.com/dYMXywi.gif)
![](https://i.imgur.com/pAPGq6J.gif)
![](https://i.imgur.com/G7GXwdE.gif)
![](https://i.imgur.com/VXxTCLy.gif)
![](https://i.imgur.com/NbXlOmD.gif)
![](https://i.imgur.com/82EDLNV.gif)


## Results
## What is the representation in your algorithm?
Vector with Image objects* <br>
*objects are stored as images to reduce time taken by mutation and selection phases.

### Chromosomes
1. Chromosomes are represented as image objects with geometric figures drawn
2. New chromosomes are represented as empty images 
```python
target_image = Image.open('test.png')
target_image = np.array(target_image, np.int16)
empty_image = Image.new("RGB", (512,512), (255,255,255))
fit = fitness_function(target_image, empty_image)
new_population = [Gene(empty_image, target_image, fitness_value)]
```

## Which selection mechanism is being used?
### fitness based selection mechanism
Replace with a member of a sample of mutations if better than parent
For every chromosome fitness value is calculated and only n-best survives
```python
def get_the_best(population, n=1):
    parents = sorted(population, key=lambda x: fitness_function(target, x))[:n]
    return parents
```
#### Example 
For image: <img src="https://i.imgur.com/19PdNuf.png" width=100>

The first from this results will be chosen as it covers more image and as a result reduces error and has better fitness value.

<img src="https://i.imgur.com/GoChmYz.png" width=100></img><img src="https://i.imgur.com/TqkrVGF.png" width=100></img><img src="https://i.imgur.com/ZcOTlfk.png" width=90></img>

## Which image manipulation techniques have you applied?

### Plotting ellipsis
<img src="https://i.imgur.com/vQEcUdO.png" width=200></img>
### Plotting triangles
<img src="https://i.imgur.com/rDfAoHk.png" width=200></img>

## What is the fitness function?
sum of module of differences for each collor at each pixel which relfects how different images are
```python
def fitness_function(target, generated):
    target_pixels = target
    generated_pixels = np.array(generated, np.int16)
    return np.sum(np.abs(generated_pixels - target_pixels))
```
#### Example 
For image: <img src="https://i.imgur.com/19PdNuf.png" width=100>

Generated images:

<img src="https://i.imgur.com/Jd6oKER.png" width=100></img><img src="https://i.imgur.com/7eu76cr.png" width=100></img>

Fitness values:

<p style="word-spacing: 40px;">3993054 740396</p>

## Explain the Crossover function.
![](https://i.imgur.com/JAOTRpL.png)







## Explain Mutation criteria.
Every parent produces 55 new chromosomes
### Mutation is done for every parent as following:
1. Random points are chosen
2. Color is chosen from original image
3. ellipse/triangle is drawn
4. fitness value is recalculated
5. results are saved as new chromosomes
#### Example 
Using ellipses
<img src="https://i.imgur.com/K8QHYxG.gif" width=300></img>
Using triangles
<img src="https://i.imgur.com/gPfVi0i.gif" width=300></img>

For me, ellipses produced better results, so I used them to produce images


## What is art for you/How would you define art/what is your perception of art?
**Art** - any manifestation of personality that is expressed in written or oral form.

**My perception of art** - painting that are appreciated for their beauty.

## Describe the artistic aspect of your output images/why do you consider the output image as a piece of art?
1. Images have smooth shapes and soft transitions between colors that looks pretty
2. Building blocks of generated images are ellipses and triangles that looks like brush strokes that artists use

## Provide examples of images.
![](https://i.imgur.com/6bYJoA1.png)
![](https://i.imgur.com/gu311xS.png)
![](https://i.imgur.com/8pIE3R0.png)
![](https://i.imgur.com/xyPSPMc.png)
![](https://i.imgur.com/VUnmEVN.png)
![](https://i.imgur.com/9EPtzlm.png)
![](https://i.imgur.com/a01kZ4t.png)


